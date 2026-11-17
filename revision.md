# Project Prototype Revision: Things Merchant Onboarding

ESTM 60244 · Michael Tiernan · November 2026

**Product:** Things is a delivery app for locally owned shops only. The prototype covers merchant onboarding: a single-location shop owner turns the menu they already have into a storefront by reviewing and approving an AI-drafted listing.

**Intended user:** The owner-operator of a small café, deli, bakery or ice cream shop, with no tech or marketing staff.

---

## 1. Priority improvements

I looked at the three First Working Slice tests, the known limitations and the two peer reviews. Both reviewers hit the same dead end: they brought their own menu (one pasted text, one uploaded a photo) and either found out too late that it wouldn't be read, or couldn't tell why nothing happened. When I tried the flow again myself, moving to the review page still felt awkward: pick a sample, scroll, then click Extract items. Looking at the code, I also found two places where the review step didn't do the job my framing document said it would. I chose three improvements, all on the central task (menu → review → storefront).

| # | Improvement | Why | Source |
|---|---|---|---|
| 1 | **Make the first step work and say what's real.** (a) **Pasted menus now produce a draft**, using a simple rule-based reader that is labelled "not AI" everywhere it appears. (b) **Sample menus open the review page in one click.** (c) A plain line under the heading says what Things is. A "Demo limit" note says, before any action, what is AI-simulated, what is rule-based, and that photos can't be read yet. Choosing a photo shows a message right under the upload box, naming the file and pointing to the paste box. | The first thing a real owner does is use their own menu. Before, pasted text and photos were dead ends, and the owner only found out after trying. | Both peer reviews + my own re-test |
| 2 | **The "Your original menu" panel now shows the real source.** It is drawn from the menu text as written. Before, it was rebuilt from the AI's extracted items. | The side-by-side check is the main safeguard against misreads, but if the AI read $5.50 as $5.00, the old "original" would have shown $5.00 too. The check could never catch an error. | Code review against my framing doc's risk table |
| 3 | **A listing can't be published with a missing name or an invalid or blank price.** Those fields turn red with a fix-it message, and "Approve & publish" stays locked. A blank flagged price can no longer just be ticked as checked. Prices are shown as $0.00 on the storefront. | Before, Nonna's Deli could be published with the Lasagna Special listed as "Price to be confirmed", and a manually added row could go live as "Untitled item" with no price. Customers would see a misleading listing. | Risk response (Section 4) |

What I deliberately did **not** change: the AI is still simulated (see below), photos still can't be read, and there is still no saved data. These are larger than this milestone allows, and I'd rather make the existing flow honest and safe first.

**About the pasted-text reader.** This is not the product's AI. It is a short set of fixed rules in `App.tsx` (`readPastedMenu`). A line ending in a number becomes an item and that number its price. A short line with no price becomes a category heading, or the shop name if it comes first. A line mentioning "price" or "?" becomes an item with a blank price. Anything else is listed on the review page as "not used". Prices it had to guess at (no decimals, a "$4", "10 dollars") are flagged yellow. It exists so owners can try their own menu now, and it is a useful baseline: it shows exactly where fixed rules break, which is why the product needs a model (see the Challenge test).

### The AI role and how it is produced right now

| | |
|---|---|
| **Task** | Read a real shop menu (photo, chalkboard, or pasted text) and turn it into structured listing data. |
| **Input** | A menu photo or pasted menu text. |
| **Output** | A list of items: name, price, category, a short optional description marked "AI draft", and a flag on any field the model is unsure about. |
| **Value** | Typing every item and price by hand is the barrier that keeps small shops off delivery platforms. The AI does the typing; the owner only checks. |
| **Current implementation** | **Simulated (Wizard-of-Oz).** The three sample menus return prepared results hard-coded in `App.tsx`, after a 1.5-second "Reading your menu…" pause. No model or external API is called and no API keys are used. Low-confidence flags and descriptions are pre-set. |
| **New for this revision** | The Corner Scoop chalkboard sample now includes one **deliberate simulated misread**: the board says Salted Caramel is $5.50, and the draft says $5.00 (flagged). This lets me test whether the review step catches a wrong price. It is a prepared example, not a real model error. |

| Part of the product | Status |
|---|---|
| Menu reading from the three samples | Simulated |
| Reading your own pasted text | **Working, but rule-based, not AI.** Labelled on the upload page, the banner and the review page. |
| Reading your own photo | Not available (planned); the app says so up front and at the point of use |
| Low-confidence flags, AI-draft descriptions | Simulated (pre-set) |
| Original-menu panel, review table, edit/add/delete, flag confirmation, price/name checks, approval lock, storefront | Working |
| Empty-input check | Working |

---

## 2. Response to peer feedback

I received two peer reviews. They're summarised here without names.

| Feedback or observed issue | Decision | What changed, or why it was deferred |
|---|---|---|
| Reviewer A pasted their own menu and only then learned the demo accepts only the three samples. Their top recommendation: say so **before** the owner pastes or uploads. | **Acted on, and went further** | Added a "Demo limit" note at the top of the menu box, visible before any action. Pasted menus now also produce a draft (rule-based, labelled not AI), so this dead end is gone. A hint under the paste box shows the expected format ("Meatball sub 8.50"). |
| Reviewer B uploaded their own menu photo, saw nothing appear, and couldn't tell why. | **Acted on** | The message now appears directly under the upload box, names the file ("We can't read 'menu.jpg' yet…"), says nothing from the photo will appear, and points to pasting the menu as text instead. The upload box itself changes to "Not read in this demo". |
| Reviewer B wasn't sure at first what Things is for. | **Acted on** | Added a bold plain-language line under the heading: "Things is a delivery app for locally owned shops only, with no chains." |
| Reviewer A couldn't judge how accurate the AI would be on a real menu, because results are pre-written. | **Deferred** | This is fair and is the biggest open question. Connecting a vision model, then testing it on real South Bend menus, is my next priority (Section 6). I chose not to do it in this milestone so I could fix the flow and the safeguards first. Until then I don't claim any accuracy. |
| Both reviewers implicitly wanted their own menu to produce a draft. | **Adapted** | At first I decided against a rule-based reader, because it could look like the AI was working when it wasn't. After re-testing, the flow still felt like a dead end, so I added one for **pasted text only**, on the condition that it is labelled "not AI" on every screen it touches and its drafts don't show "AI draft" labels. Photos still need a real model, so that part is deferred. |
| Reviewer A liked that an edit (renaming an item) carried through to the storefront. Reviewer B liked that nothing goes live until approval. | **Kept and extended** | Kept owner control as the core of the flow. Approval is now also blocked when a name or price is missing or invalid (Improvement 3). |

---

## 3. Evaluation

**How I tested:** On November 17, 2026, each case was run on the live prototype (https://things-merchant-onboarding.replit.app) in Chrome, using only the built-in sample menus and made-up input (no real shop data). I ran every case myself, then asked Claude to re-run them in my browser as a double check. Both runs matched this table (see the AI-use disclosure). The success criterion for each case was written before the run. "Before" results come from the First Working Slice version of the same code.

> AI results here are **simulated**, and pasted text is read by fixed rules, not AI. These tests evaluate whether the flow and the outputs are understandable and safe. They do **not** show how accurate a real model would be.

### Test cases

All inputs are sample or made-up menus. "Main Street Pizza" is invented.

| Case | Input or action | Expected result / success criterion | Actual result | Pass, partial, or fail | Implication or next step |
|---|---|---|---|---|---|
| **Typical 1** | Click **Try sample: Clean café menu** (Maple Street Café, 8 items) → Approve & publish | One click opens the review page. All 8 items appear beside the original menu, every price matches the source, and nothing is flagged. The storefront shows all 8 items with correct prices. | The review page opened straight from the sample click. 8 items found; the original-menu panel shows the café menu text as written. No flags; publish enabled. The storefront listed all 8 items under Coffee, Pastries and Breakfast, with prices matching the source ($3.25 … $11.00). | **Pass** | Baseline flow works end to end, one step shorter than before. |
| **Typical 2** | Paste my own menu ("Main Street Pizza": Slices / Sides headings, prices written as "3.50", "- $4", "..... 4.25", "$9.25", "10 dollars", plus a "Call ahead for large orders" line) → Extract items → check flags → publish | A draft appears for every priced line, with the right names, prices and categories. The owner is told it's rule-based, not AI. Guessed prices are flagged, and the non-menu line isn't silently dropped. | 5 items found with the correct names, prices and categories (Slices / Sides); the shop name was read from the first line. "$4", "4.25" (after dots) and "10 dollars" were flagged yellow; "3.50" and "$9.25" were not. The review page said "Read by a simple rule-based reader, not AI" and listed "1 line was not used: 'Call ahead for large orders please thanks'". After ticking the three flags, the storefront showed $3.50, $4.00, $4.25, $9.25, $10.00. | **Pass** | Owners can now use their own menu. This is fixed rules on tidy text, not evidence about AI. |
| **Typical 3** | Clean café sample → rename "House Drip" to "Mate", change Cold Brew to "$5", delete Blueberry Scone, add "Hot Chocolate" 3.75 in Coffee → publish | Every owner edit reaches the storefront. A new blank row can't be published until it has a name and a price. | After "Add item", publish locked with "2 red fields are missing a name or a valid price." Once the name and price were filled in, publish unlocked. The storefront showed Mate $3.25, Cold Brew $5.00, Hot Chocolate $3.75, and no Blueberry Scone. | **Pass** | The owner keeps full control. |
| **Challenge** | Paste messy, lower-case deli text: "meatball hero 8.5", "chicken parm sandwich $8.50", "italian sub 8 dollars", "lasagna special price?", "all made fresh". Separately paste "Coffee / Combo #2 / Latte 4.50". | Prices are normalised to 8.50 / 8.50 / 8.00 and the guessed ones flagged. The lasagna appears with a blank price that blocks publishing. Names are tidied. "Combo #2" is not read as a $2 item. | **Deli:** prices normalised correctly, with "8.5", "8 dollars", "6.5" and "9" flagged yellow and "$8.50" not. "Lasagna Special" appeared with a red blank price, and publish stayed locked. Names were capitalised ("Meatball Hero", "Nonna's Deli"), though "Soup Of The Day" came out awkwardly. "all made fresh" was quietly treated as a category heading rather than listed as unused. **Combo:** "Combo #2" became an item called "Combo #" at **$2.00** (flagged yellow), and "Coffee" became the shop name. | **Partial** | Fixed rules break exactly where real menus get messy: numbers in item names, taglines, missing shop names. This is the case for a real model. Next: run the same text through a model and compare. |
| **Invalid / unsupported** | Upload my own menu photo (a JPG), then click Extract items | Before uploading, the owner can see that photos aren't read. After uploading, a clear message next to the upload box explains why and what to do instead. No fake draft. | The "Demo limit" note was visible before upload. After choosing the photo, the upload box read "Not read in this demo", and a message directly underneath named the file and suggested pasting the menu as text. Extract items stayed on the upload screen with no fake draft. Pasting text after choosing a photo still worked. | **Pass** | Fixes the peer-review confusion. Real photo reading is still missing. |
| **Empty** | Click Extract items with nothing entered. Separately paste text with no prices ("just some words / no numbers here"). | A clear error; no broken table. | Both showed "We couldn't find any items with prices. Try a clearer photo, paste your menu, or use a sample." and stayed on the upload screen. | **Pass** | Regression check. |
| **Risk** | Handwritten chalkboard sample (Corner Scoop). The chalkboard says **Salted Caramel $5.50**; the simulated AI draft says **5.00** (flagged as uncertain). Owner ticks every yellow "Matches my menu" box without editing. | The original-menu panel shows the true price ($5.50), so the owner can spot the misread. Ideally the wrong price should not go live. | The panel correctly showed "Salted Caramel — $5.50" next to the yellow 5.00 field, and publish was locked until all 3 flags were handled. But once the owner ticked all three boxes without comparing, publish unlocked and the storefront showed **Salted Caramel $5.00**. | **Partial** | The safeguard makes the error visible but can't stop an owner who clicks through. Next: require a flagged price to be edited, or re-typed if it really matches, instead of just ticked (Section 6). |

### Before and after: the same input on both versions

**Issue (from peer review):** an owner pastes their own menu, which was Reviewer A's exact experience. Same input on both versions: the "Main Street Pizza" text from Typical 2.

| | Before (First Working Slice) | After (this revision) |
|---|---|---|
| **Before clicking anything** | No warning that pasted text wouldn't be read. | The "Demo limit" note explains what's AI-simulated, what's rule-based and what isn't available. A hint under the paste box shows the expected format. |
| **After clicking Extract items** | Stayed on the upload page with "This demo only provides prepared sample results, so it won't create a draft from pasted text." Dead end. | Opens the review page with 5 items, guessed prices flagged, a clear "rule-based, not AI" label and a list of unused lines. The owner can publish after checking the flags. |
| **Screenshots** | `docs/screenshots/before-pasted-own.png` | `after-pasted-menu-review.png`, `after-pasted-menu-storefront.png` |

**Did it help?** Yes. The main dead end both reviewers hit is gone for pasted text, and the owner always knows which parts are AI, which are rules, and which are missing. It doesn't help with photos (only the message is better: compare `before-photo-uploaded.png` and `after-photo-uploaded.png`), and the Challenge case shows the reader is fragile on messy text. I judged the clarity from the screens; I haven't re-tested with a new person yet.

**Second before/after: blank price (risk-related).** Same input: Nonna's Deli sample, tick the flag on the blank Lasagna price, then publish.
- **Before:** Publish was allowed, and the storefront showed "Lasagna Special — Price to be confirmed" (`before-deli-storefront.png`).
- **After:** There's no tick box for a blank price; the field is red and publish is locked until a price is entered or the item is deleted (`after-deli-blank-price.png`).

### What the evidence tells me

The core flow now works reasonably well. An owner understands what Things is, sees the demo limits before acting, and can get from a sample or their own pasted menu to a review page in one step. They can check the draft against the real source, fix or add items, and publish only a listing where every item has a name and a valid price. Two things remain weak. The misread risk test was only a partial pass, because an owner who ticks every yellow box can still publish a wrong price, so the review depends on the owner actually comparing. And the pasted-text reader is brittle (the Challenge case turned "Combo #2" into a $2 item), which is a useful baseline but not the product. Everything about the AI itself is still untested: accuracy, flagging and description quality are prepared examples. So these results support the claim that the *review experience* is clear and keeps the owner in control, not the claim that AI saves owners time on real menus. Next I would put the same messy text and a few real South Bend menus through a model and compare with the rule-based reader, and make flagged prices harder to wave through.

## 4. Risk response

**Risk: a misleading listing goes live because the owner trusts the AI draft (over-reliance).**

The people affected are the shop owner and their customers. A misread or missing price ($5.50 read as $5.00, or a lasagna with no price) could go live, and customers would be charged differently from what they saw. The owner then takes the blame and loses trust in a platform that exists to help them. My framing document's main safeguard was a side-by-side check against the original menu, but the code was drawing the "original" from the AI's own output, so it would have repeated any misread. I changed the panel to show the real source text, and made the approval lock refuse missing names and blank or non-numeric prices instead of letting them through as "Price to be confirmed". The risk test shows what remains: the misread is now visible, but an owner in a hurry can still tick "Matches my menu" on a wrong price and publish. That's the over-reliance problem from class, where a confirm button can become a rubber stamp. The safeguard is real code, but the misread it was tested against is simulated.

---

## 5. Remaining limitations (ranked by effect on the central task)

1. **The AI is simulated.** Owners can paste a menu, but it is read by fixed rules that break on messy text (Challenge: Partial), and photos, the main promise, can't be read at all. No accuracy claim is possible until a real model is connected.
2. **Flagged prices can still be waved through.** Ticking "Matches my menu" unlocks publishing without the owner changing anything (risk test, Partial).
3. **Nothing is saved.** The storefront exists only in the current browser session.
4. **Only flagged fields get extra attention.** If a real model misreads a price *without* flagging it, only the owner's own comparison would catch it.

## 6. Next priorities

1. **Connect a vision-capable model to pasted text first, then photos.** Run the Challenge text and 3–5 real South Bend menus (with permission) through both the model and the rule-based reader. Compare how many prices each gets right, and whether the model flags the ones it gets wrong. Repeat each input three times to check consistency.
2. **Make flagged prices harder to rubber-stamp.** For example, require the owner to re-type a flagged price, or highlight the matching line in the original menu when a field is selected.
3. **Save the approved storefront** so an owner can come back and edit it.
