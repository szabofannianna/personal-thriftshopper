---
name: setup-thrift-assistant
description: Sets up the Online thrift assistant for a new user — creates the Notion workspace (Wishlist, Finds, Shown listings, Favourites, Learnings, Settings), asks for their Vinted country, currency and email, checks which seller countries their Vinted site shows, and schedules the search and learning runs. Use when the user says "set up the thrift assistant", "start my Vinted shopper", "install/configure the online thrift assistant", or when another skill of this plugin finds no Settings page.
---

# Set up the Online thrift assistant

Walk a new user through setup in plain language. Never mention file paths, IDs or schemas unless asked. Ask questions with AskUserQuestion where available.

## Step 1 — Check what is connected

The assistant needs:
- **Notion** connector (required): stores everything.
- **Claude app's built-in browser** (required): runs the anonymous Vinted searches. Present when `mcp__remote-devices__Claude_Browser__*` tools exist; the computer must be linked.
- **Claude in Chrome** (required for taste learning): reads the user's Vinted favourites where they are logged in.
- **Gmail** connector (optional): sends a short "new finds" email with the link. Without it, the Notion page is the only output.

Check each by looking for its tools (load deferred tools with ToolSearch). Tell the user in one short list what is ready and what is missing, and how to connect anything missing. Do not continue without Notion and a browser that can reach Vinted.

## Step 2 — Ask the essentials

Ask in one round:
1. **Vinted country** — the site they buy on. See `references/vinted-sites.md` for domains, currencies and languages.
2. **Budget currency** — the currency they think in (default: the site's currency).
3. **Email for notifications** — default to the connected Gmail address; allow "no email".
4. **Schedule** — which days and roughly what time (default: Tue/Thu/Sat around 10:00, their local time). Mention that the computer must be awake with the Claude app open at that time.
5. **Where in Notion** — a page they name, or a new private page.

## Step 3 — Check which countries the site shows

Open `https://<domain>/catalog?search_text=<a common item in the site's language>&order=newest_first` in the built-in browser. Extract listing titles (see the find-thrift-picks skill's `references/vinted-reading.md`). Judge from title languages and seller locations which countries appear (e.g. vinted.hu shows Polish, Romanian, Greek, Lithuanian sellers). Store the resulting **search languages** (site language + English + the languages seen, max 5). Tell the user what you found in one line.

## Step 4 — Create the Notion workspace

Under the chosen page create a parent page **"Online thrift assistant"** and inside it everything in `references/notion-workspace.md`: the five databases, the Finds page with its sample, the Learnings page, the Taste profile page and the **Settings** page. Fill Settings with the answers from Steps 2–3 and the URLs/IDs of every database and page created. Other skills find everything through the Settings page — it must be complete.

Then ask the user to add 1–3 wishlist items right away (in Notion, or tell you and you add the rows), each with a **max price** — rows without a budget are skipped by the search.

## Step 5 — Optional favourites import

Offer to import their Vinted favourites now as the taste anchor ("it takes a few minutes and needs Chrome open with you logged in to Vinted"). If yes, run the learn-my-taste skill in favourites-only mode. If no, the first weekly learning run does it.

## Step 6 — Schedule the runs

Use the session's scheduling tool (load it with ToolSearch; it creates "scheduled tasks"). Create two tasks that require the user's computer:
- **Thrift picks** on the chosen days/time, prompt: `Run the Online thrift assistant: find new thrift picks for my wishlist (find-thrift-picks skill).`
- **Taste learning** weekly on Sunday at a similar time, prompt: `Run the Online thrift assistant weekly taste learning (learn-my-taste skill).`
Use a slightly off-the-hour minute (e.g. 9:52). If no scheduling tool exists, explain that the user can run "find my thrift picks" manually and set up a scheduled task later.

## Step 7 — First run and wrap-up

Offer to run the first search now (it uses the small first batch: max 3 finds per item). Finish with 3–4 lines: where the Notion workspace is, when runs happen, how to add items (edit the Wishlist table), how to give feedback (tick "Not for me" with a reason; heart things on Vinted).
