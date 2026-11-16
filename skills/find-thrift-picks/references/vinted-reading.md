# Reading Vinted — what works

Learned the hard way while building the original agent. Follow it literally.

## Access

- There is no official public API. Community "Vinted API" wrappers and MCP servers broke when Vinted changed its internal endpoints (they returned 404 for every search). Don't use them.
- Read Vinted's normal web pages in the Claude app's **built-in browser**, as a person would. Vinted blocks data-centre connections, so cloud fetches fail; the user's own computer works.
- Searches run logged OUT in the built-in browser. Only the favourites sync (learn-my-taste) uses the logged-in Chrome.
- Be gentle: one page at a time, about a second between item checks, no parallel requests, never solve CAPTCHAs.

## Search results

URL: `https://<site>/catalog?search_text=<url-encoded query>&order=newest_first`

After the page loads, run in javascript_tool:

```js
const out = new Map();
for (const a of document.querySelectorAll('a[href*="/items/"]')) {
  const id = (a.href.match(/items\/(\d+)/) || [])[1];
  if (!id || out.has(id)) continue;
  let el = a, img = null;
  for (let k = 0; k < 6 && el && !img; k++) { img = el.querySelector && el.querySelector('img[src*="vinted.net"]'); el = el.parentElement; }
  out.set(id, { t: a.getAttribute('title') || '', img: img ? img.src : '' });
}
[...out].map(([id, v]) => ({ id, ...v }))
```

- The link's `title` attribute holds: title, brand, condition, size, item price, and price including buyer protection (that second price is what the user pays).
- Photo URLs are signed (`…/f800/<hash>.webp?s=<signature>`). Keep them exactly; changing the extension, size or dropping `?s=` gives 404.
- Long tool outputs get truncated (~1,000 characters in some browser tools). Return compact fields, or batch over several calls.

## Item details

While on any page of the same Vinted site, fetch the item HTML same-origin instead of navigating:

```js
const h = await fetch('/items/<id>').then(r => r.text());
```

Read `<meta property="og:image" content="…">` (main photo; replace `&amp;` with `&`), and the attribute block: material, colour, size, condition, upload time, description. Labels are in the site's language (e.g. Hungarian: Anyag = material, Szín = colour, Állapot = condition, Méret = size).

## Where photos can and can't be shown

- Vinted photos load in a normal browser, including from other sites, and in **Notion** image blocks.
- They do **not** load in **Gmail** (Google's image proxy is refused). Never rely on images in email.

## Favourites page (logged-in Chrome only)

URL: `https://<site>/member/items/favourite_list`
- Loads slowly: poll until more than 2 item links exist (30+ seconds is normal).
- JavaScript scrolling does NOT trigger loading more items. Use real mouse-wheel scrolls via the computer tool (scroll down 5–10 ticks over the grid, ~3 s waits), in batches, counting unique IDs after each batch. Stop when the count stops growing for 3 batches and the footer is visible without a spinner.
- To read a long list despite output truncation, replace `document.body.innerHTML` with a `<main><article><pre>` of `id|title` lines and read it with get_page_text. Close the tab afterwards.
