---
name: find-thrift-picks
description: Runs one search for the Online thrift assistant — searches the user's Vinted site for their active Notion wishlist items in several languages, drops anything that breaks their hard rules (budget incl. fees, size, fake materials, exclusions), ranks the rest by learned taste, publishes a Notion photo page of new finds and emails a short note with the link. Use when the user says "find my thrift picks", "run the thrift assistant", "check Vinted for my wishlist", "any new finds?", or when a scheduled task asks for it.
---

# Find thrift picks

One run = one Notion photo page of NEW listings + one short email + rows recorded in Shown listings. Work quietly; the outputs are what the user reads. Photos matter most: the user decides by looking.

## 0. Load settings

Search Notion for the page titled **"Settings"** under **"Online thrift assistant"** and read every key. If it is missing, stop and offer to run the setup-thrift-assistant skill. Use only the site, currencies, languages and IDs from Settings.

## 1. The core rule

- The **Wishlist** defines what the user wants. Its criteria — item type, budget, sizes, colour, must-have material, exclusions — are HARD rules, never relaxed.
- **Favourites, Taste profile and Learnings** only steer RANKING among listings that already pass the rules. A faux-suede favourite may teach a silhouette; faux suede is still excluded where the wishlist demands real suede.

## 2. Read the context

- Wishlist rows with Status = Active. Skip rows without Max price (mention them in the email).
- Shown listings: all Vinted IDs (never show twice), plus "Not for me" rows with Reason (avoid similar listings when the reason applies).
- Favourites: Vinted IDs (never show) and rows with Status = Favourited and Counts for taste = checked (style anchor).
- Taste profile and Learnings pages. Apply "Active learnings" weighted by confidence; ignore "Rejected by you".

## 3. Search

Use the built-in browser (`mcp__remote-devices__Claude_Browser__*`). Never log in there. Follow `references/vinted-reading.md` exactly — it records what works and what silently fails.
- Per active item: 2–4 short queries across the Settings search languages (translate the item's essence: type + colour + material). Prefer wordings that Learnings say produced hearted items. First results page only, `order=newest_first`.
- Collect per listing: Vinted ID, title-attribute text (title, brand, condition, size, two prices), photo URL (full, signed, unmodified).
- Check only promising candidates in detail (≤ 40 per run).
- If Vinted shows a CAPTCHA or block page, stop searching and report a FAILED run.

## 4. Filter (hard rules)

Drop a listing if any applies:
- Its Vinted ID is in Shown listings or Favourites.
- Price including buyer protection, converted to the budget currency at today's rate, exceeds Max price. Look up the rate; if impossible use a recent known rate and say so in the email. Never compare unconverted prices.
- A stated size doesn't match the item's Sizes (ignore for bags/accessories).
- It matches an Exclude term or a fake-material term in any language — see `references/material-terms.md`.
- It closely resembles a "Not for me" listing whose reason applies again.
Must-have material not stated or clearly shown → at most "Possible". Colour judged from photos is an estimate — say so.

## 5. Rank and cap

Order passing listings by: wishlist fit → Active learnings → closeness to favourites/taste profile → price. Cap per item: **5 Strong + 3 Possible** (Possible only for Strictness = Balanced). If Settings says `First run done: no`, cap at **3 per item in total**, then set it to `yes` after publishing. Every shown listing must have a photo URL.

## 6. Publish

1. **Duplicate guard**: if the Finds page already has a child page created in the last 30 minutes, stop — this is a duplicate firing.
2. **Notion photo page**: create a child page under Finds titled `<Weekday d Mon> – N finds`, in the exact format of `references/finds-page-format.md`. If nothing passed, create no page.
3. **Email** (if Notify email isn't "none" and Gmail is connected): plain-text, subject `New thrift finds: N – <weekday date>` or `No new thrift finds – <date>`. Body: one line per wishlist item (count + top 1–2 titles), the link to today's page, the Photos gallery link. Never put images in the email — Gmail cannot display Vinted photos.
4. **Record**: for each shown listing add a Shown listings row (Listing, Wishlist item, Link, Vinted ID, Price in budget currency incl. fees, Currency, Country if known, Match, Why it matches — include brand, size, material, colour, fit and era words, the learning job reads this — First seen = today, Outcome empty) and put the photo `![](<url>)` as the row page's first block so the Photos gallery shows it. If publishing failed, record nothing.

## 7. Failure honesty

If the browser is unreachable, Vinted blocks the search or a step fails, say clearly that the run FAILED and why — by email if possible, and in the reply. Never present a failure as "no new finds".
