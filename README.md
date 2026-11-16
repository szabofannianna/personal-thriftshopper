# Online thrift assistant

A personal second-hand shopper for Vinted, built as a Claude plugin.

You keep a wishlist in Notion. A few mornings a week the assistant searches your Vinted site in several languages, throws out everything that breaks your rules (over budget including fees, wrong size, fake leather in any language), ranks what's left by a taste it learns from your favourites, and gives you **one Notion page of photos** with a line on why each piece fits. Every week it looks at what you hearted, skipped or rejected and gets better at ranking.

It does not buy, message sellers, or touch your Vinted account. It reads public listing pages in your own browser.

## What you need

- A Claude plan with the **Claude desktop app** (its built-in browser runs the searches)
- **Notion** connected to Claude (stores your wishlist, finds and learnings)
- **Claude in Chrome**, logged in to Vinted in Chrome (only for reading your favourites, once a week)
- **Gmail** connected (optional — for a short "new finds" email with the link)
- Your computer awake with the Claude app open when the runs are scheduled

## Get started

Say **"set up the thrift assistant"**. It will:
1. check your connections,
2. ask for your Vinted country, currency, email and schedule,
3. create your Notion workspace (Wishlist, Finds, Shown listings with a photo gallery, Favourites, Learnings, Taste profile, Settings),
4. optionally import your Vinted favourites as a taste anchor,
5. schedule the runs and offer a first search.

Then add items to the **Wishlist** table — each needs a max price. Colour names, hex codes and reference photos help a lot.

## Skills

| Skill | Say | What it does |
|---|---|---|
| setup-thrift-assistant | "set up the thrift assistant" | One-time setup |
| find-thrift-picks | "find my thrift picks" | One search run → Notion photo page + email |
| learn-my-taste | "learn my taste" | Weekly: favourites sync, reactions, learnings, monthly taste profile |

## How it decides

- **Your wishlist rules are hard.** Budget (price incl. buyer protection, converted to your currency), sizes, must-have materials and exclusions are never relaxed.
- **Your taste only steers the order.** Favourites and learnings rank what already passed your rules.
- **It shows you each listing once.** Anything already shown or already in your favourites is skipped.
- **It says when it fails.** A blocked or broken run is reported as failed, never as "nothing new".

## Teaching it

- Heart things on Vinted as you normally would.
- In Notion → Shown listings → Photos, tick **Not for me** and add a few words of why.
- Set Outcome to **Bought** when you buy something.
- Read and veto anything on the **Learnings** page.

## Limits

- Runs only while your computer is awake with the Claude app open.
- Vinted has no public API; the assistant reads its normal pages slowly, for you only. If Vinted changes its pages, runs may fail until the plugin is updated.
- Colours and materials judged from photos are estimates and are marked as such.
- Photos are shown in Notion, not in email: Gmail can't display Vinted images.

## Privacy

Everything stays in your own Notion, Gmail and browser. The plugin sends nothing anywhere else. It reads your Vinted favourites (read-only) to learn your taste.

## License

MIT
