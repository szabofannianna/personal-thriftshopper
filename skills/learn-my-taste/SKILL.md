---
name: learn-my-taste
description: Runs the Online thrift assistant's weekly learning — syncs the user's Vinted favourites into Notion through their logged-in Chrome, records how they reacted to listings the assistant showed (hearted, not for me, ignored), writes evidence-based learnings and hit rates, and refreshes the taste profile monthly. Use when the user says "learn my taste", "sync my Vinted favourites", "update my taste profile", "what have you learned about my taste?", or when a scheduled task asks for the weekly taste learning. Also used by setup for a favourites-only first import.
---

# Learn my taste

Read-only on Vinted: never click hearts, buy, message or change anything there.

## 0. Load settings

Read the **Settings** page under **"Online thrift assistant"** in Notion (see the setup skill). If it's missing, stop and offer setup. If called from setup in favourites-only mode, do Part A only, then rewrite the Taste profile (Part D) and stop.

## Part A — Sync favourites

1. Use **Claude in Chrome** (`mcp__claude-in-chrome__*`; read the claude-in-chrome skill first). Chrome is where the user is logged in to Vinted. Never use the built-in browser for this — it stays logged out.
2. Open `https://<site>/member/items/favourite_list` in a new tab and load ALL favourites exactly as described in the find-thrift-picks skill's `references/vinted-reading.md` (slow load, real mouse-wheel scrolling, output-truncation workaround). Close the tab afterwards.
3. If Chrome is unreachable, Vinted shows a login page, or 0 favourites load, change nothing. Email the user (if Gmail and a notify address exist) `Thrift assistant: taste update failed – <date>` with one line saying why, and stop.
4. Sync the **Favourites** database, keyed on Vinted ID:
   - New favourite → add a row (Item, Vinted ID, Link, Brand, Type, Size, Condition, Price incl. fees, Wishlist item relation only when clearly the same kind of item, Status = Favourited, First seen = today, Counts for taste = checked; unchecked only for obvious non-clothing such as books).
   - Row no longer among favourites → Status = Removed. Never delete rows.
   - Sanity check first: if the scrape found under 70% of current Favourited rows, assume loading failed — mark nothing Removed and mention it.
   - Never change "Counts for taste" on existing rows; the user controls it.

## Part B — Record reactions

In **Shown listings**, for each row with an empty Outcome:
- "Not for me" ticked → Outcome = Not for me.
- Its Vinted ID is now in Favourites (Status = Favourited) → Outcome = Hearted.
- First seen 14+ days ago and neither → Outcome = Ignored.
- Otherwise leave it empty.
Set "Outcome set" = today when setting Outcome. Never overwrite an Outcome the user set (e.g. Bought).

## Part C — Learn

Read the **Learnings** page first and keep its structure, the user's edits and the "Rejected by you" section. Follow `references/learning-rules.md` to update: hit rate per wishlist item, active learnings with evidence and confidence, and suggestions the assistant may not apply itself.

## Part D — Taste profile (first run of each month, or after a favourites-only import)

Rewrite the **Taste profile** page from Favourites rows with Status = Favourited and Counts for taste checked. Keep its framing — an aesthetic anchor, not shopping rules — and any lines the user added. Sections: "Last refresh: <date> (<N> favourites)", Aesthetic (eras/vibe, silhouettes, colour palette, materials, recurring brands, price zone, sizes), Favourites that relate to wishlist items, Recurring themes not on the wishlist, What changed since last month.

## Email

Send one short plain-text email (subject `What your thrift assistant learned – <date>`) ONLY if something material changed: a learning reached medium/high confidence, a new suggestion appeared, or a hit rate moved a lot. Max ~8 lines with the Learnings link. Otherwise no email.
