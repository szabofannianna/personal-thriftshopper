# Testing sequence — Online thrift assistant

Goal: find out whether **someone who isn't Fanni** can install the plugin, set it up and get useful finds without help. Do this before submitting to the plugin directory.

Work through the rounds in order. Don't move on until the round's **exit check** passes. Log every problem in the table at the bottom.

---

## Round 0 — Package check (10 min, Fanni)

- [ ] Install the plugin from this repo in a fresh Claude session.
- [ ] The three skills appear: setup-thrift-assistant, find-thrift-picks, learn-my-taste.
- [ ] Each trigger phrase loads the right skill. Try "set up the thrift assistant", "find my thrift picks" and "learn my taste".
- [ ] The README alone tells a stranger what they need before installing.

**Exit check:** all three skills trigger from their phrases, without naming the skill.

---

## Round 1 — Clean-room run on your own account (1 hour, Fanni)

Run everything **separately from your live setup**. Your current scheduled tasks and the "Vinted wants" Notion page must stay untouched.

1. Create a new empty Notion page called **"Plugin test"** and point setup there.
2. Run **"set up the thrift assistant"** and check that it:
   - [ ] lists what is connected and what is missing, correctly
   - [ ] asks all 5 essentials in one round (country, currency, email, schedule, Notion location)
   - [ ] detects the seller countries on vinted.hu (PL / RO / GR expected)
   - [ ] creates every database and page, and fills in a complete **Settings** page
   - [ ] creates the two scheduled tasks. **Delete them right after this round** so they don't run alongside your live tasks.
3. Add 2 wishlist items: one easy (lots of results) and one strict (rare material, tight budget).
4. Run **"find my thrift picks"** and check:
   - [ ] at most 3 finds per item on the first run
   - [ ] every find has a photo in Notion
   - [ ] the prices include fees and are converted to EUR
   - [ ] no fake leather or fake suede slips through
   - [ ] you get one plain email with a working link
5. Run it again right away:
   - [ ] the duplicate guard stops it (no second page)
6. Run it again after 30+ minutes:
   - [ ] nothing that was already shown appears again
7. Tick **Not for me** on 2 finds and write a reason. Run **"learn my taste"**:
   - [ ] the favourites sync works (count matches Vinted)
   - [ ] the outcomes are set correctly
   - [ ] the Learnings page is written with evidence, not guesses

**Exit check:** the whole flow works end to end with no fixes by hand.

---

## Round 2 — Break it on purpose (30 min, Fanni)

The plugin promises to "say when it fails". Test that promise:

| Test | How | Expected |
|---|---|---|
| Wishlist row without a price | Leave Max price empty | Skipped, and mentioned in the email |
| Computer not reachable | Close the Claude desktop app, then run from a phone | Reported as **FAILED**, never as "no new finds" |
| Chrome logged out of Vinted | Log out, run "learn my taste" | Nothing changed, failure email sent |
| Favourites load only partly | Stop scrolling early, or test with a large list | The 70% guard prevents mass "Removed" |
| No Settings page | Rename the Settings page | Offers to run setup instead of guessing |
| Another country | Rerun setup on a test page with vinted.fr or vinted.de | Correct currency, languages and search terms |

**Exit check:** every failure is reported as a failure, and no data gets damaged.

---

## Round 3 — Stranger test (3 people, ~45 min each)

Pick testers who differ from you:
- **A:** buys on a different Vinted country (DE, FR or PL)
- **B:** uses Claude but has never connected Notion
- **C:** a heavy Vinted user who is not technical

**How to run it:** send them the repo link and the README only. Watch on a call with screen share, **without helping**. When they get stuck, note where and how long, and only then unblock them.

Ask afterwards:
1. Where did you hesitate or feel lost?
2. Would you look at the finds page without being reminded?
3. Did anything shown feel wrong for you? Why?
4. Would you keep it running next week?

**Exit check:** at least 2 of 3 testers finish setup with no more than one unblock each.

---

## Round 4 — One real week (testers from Round 3)

Let the scheduled runs go for 7 days (3 searches and 1 learning run).

Measure:
- [ ] **Runs that actually happened:** how many fired with the computer awake?
- [ ] **Hit rate:** finds hearted or bought ÷ finds shown, per tester
- [ ] **Silent failures:** any run that broke without a message is a blocker
- [ ] **Did they open the finds page?** Ask them; a page nobody opens is a failed product

**Exit check:** no silent failures, and at least one tester says they'd keep it running.

---

## Ready to submit when

- [ ] Rounds 0–4 pass
- [ ] All blockers in the log are fixed and retested
- [ ] README updated with what testers got stuck on
- [ ] Repo set to public at listing time

---

## Findings log

| # | Round | Tester | What happened | Severity (blocker / annoying / cosmetic) | Fix | Retested |
|---|---|---|---|---|---|---|
| 1 | | | | | | |
