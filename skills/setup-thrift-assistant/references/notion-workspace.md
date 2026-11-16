# Notion workspace

Create everything under one parent page, **"Online thrift assistant"**. Use the Notion connector's create-database tool with these SQL-style schemas, then record every resulting database URL, data source (`collection://…`) and page URL on the Settings page.

## 1. Wishlist (database)

```
CREATE TABLE ("Item" TITLE,
 "Status" SELECT('Active':green, 'Paused':gray, 'Found':blue),
 "Description" RICH_TEXT COMMENT 'In your own words: what it is, feel, style',
 "Max price" NUMBER,
 "Currency" SELECT(<user currency>, 'EUR', <site currency>),
 "Sizes" MULTI_SELECT('XS', 'S', 'M', 'L', 'XL', 'One size'),
 "Colors" RICH_TEXT COMMENT 'Colour names or hex codes; attach reference photos',
 "Color photos" FILES,
 "Must have" RICH_TEXT COMMENT 'Hard requirements, e.g. real leather, natural fibre',
 "Exclude" RICH_TEXT COMMENT 'Disqualifiers, e.g. leather look, acrylic',
 "Strictness" SELECT('Strict':red, 'Balanced':yellow))
```
Add shoe sizes or numeric sizes as options when the user's items need them.

## 2. Shown listings (database) — every listing ever shown

```
CREATE TABLE ("Listing" TITLE,
 "Wishlist item" RELATION('<wishlist data source id>', DUAL 'Shown listings'),
 "Link" URL, "Vinted ID" RICH_TEXT, "Price" NUMBER,
 "Currency" SELECT(<user currency>),
 "Country" RICH_TEXT, "Match" SELECT('Strong':green, 'Possible':yellow),
 "Why it matches" RICH_TEXT, "First seen" DATE,
 "Not for me" CHECKBOX, "Reason" RICH_TEXT COMMENT 'Why not? The assistant learns from this',
 "Outcome" SELECT('Hearted':pink, 'Not for me':red, 'Ignored':gray, 'Bought':green),
 "Outcome set" DATE)
```
Add a **gallery view** named "Photos": `SORT BY "First seen" DESC; SHOW "Listing", "Price", "Wishlist item", "Match", "Why it matches", "Not for me"`. Card preview = page content (each row's first block is its photo).

## 3. Favourites (database)

```
CREATE TABLE ("Item" TITLE, "Vinted ID" RICH_TEXT, "Link" URL, "Brand" RICH_TEXT,
 "Type" SELECT('Sweater/knit', 'Bag', 'Dress', 'Jacket/coat', 'Top', 'Bottoms', 'Shoes', 'Accessory', 'Other'),
 "Size" RICH_TEXT, "Condition" RICH_TEXT, "Price" NUMBER,
 "Wishlist item" RELATION('<wishlist data source id>'),
 "Status" SELECT('Favourited':green, 'Removed':gray),
 "First seen" DATE, "Counts for taste" CHECKBOX)
```

## 4. Pages

- **Finds** — intro line: "One page per run, newest at the top. Tick 'Not for me' in Shown listings → Photos to teach the assistant." Inside it, create **"Sample – how each run will look"** using the format in the find-thrift-picks skill's `references/finds-page-format.md` with one illustrative listing (clearly labelled as an example, no real Vinted IDs).
- **Taste profile** — "Your aesthetic, read from your Vinted favourites. An anchor for style, not a shopping list: the Wishlist has the hard rules. Edit freely." Then empty sections: Aesthetic, Favourites that relate to wishlist items, Recurring themes not on the wishlist, What changed since last month.
- **Learnings** — use the template in the learn-my-taste skill's `references/learning-rules.md`.

## 5. Settings (page)

Plain key: value lines, one per line, so other skills can read them reliably:

```
Vinted site: www.vinted.<tld>
Site currency: <code>
Budget currency: <code>
Search languages: <comma-separated, max 5>
Notify email: <address or none>
Schedule: <days and time, time zone>
Wishlist data source: collection://…
Shown listings data source: collection://…
Shown listings database URL: https://…
Favourites data source: collection://…
Finds page ID: …
Sample page URL: https://…
Taste profile page URL: https://…
Learnings page URL: https://…
First run done: no
```
Add a line at the top: "Used by the Online thrift assistant. Edit values here if something changes; don't rename this page."
