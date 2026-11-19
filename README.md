# Things: Merchant Onboarding (Prototype)

**Things is a delivery app for locally owned shops only, with no chains. This prototype shows how a shop owner turns the menu they already have into a storefront by reviewing and approving an AI-drafted listing.**

ESTM 60244 · Project Prototype Revision · Michael Tiernan

**Intended user:** the owner-operator of a single-location café, deli, bakery or ice cream shop, with no tech or marketing staff. Typing every item and price into a delivery platform by hand is slow and easy to put off, and that keeps small shops offline.

---

## Demo Video

The demo is in two parts. My recording software would not save a single video with both my narration and my screen, so I've provided both recordings:

- **Narrated walkthrough:** [Watch on Google Drive](https://drive.google.com/file/d/1WZGEW3vQUvYrKCUZOULMwGTPTCjLEyUH/view?usp=drive_link). My spoken explanation of the product, the revision and the AI role. The screen did not record in this version.
- **Screen recording:** [Watch on Google Drive](https://drive.google.com/file/d/1OMjNO_93K5cjxTE0RHwjrIaljmTPIgw2/view?usp=drive_link). The same walkthrough on screen, showing the central task from menu input to published storefront.

Access to both is granted to Emelia Hughes and Samuel Huang.

## How to open the prototype

- **Prototype link:** https://things-merchant-onboarding.replit.app
- **Replit project:** https://replit.com/@mtiernan7/Things-Merchant-Onboarding
- **Source code (GitHub):** https://github.com/mtiernan77/Things-Merchant-Onboarding. The app is in `artifacts/things-onboarding/src/App.tsx`.

No login is needed. To try it: click **Try sample: Handwritten chalkboard**, which opens the review page in one click. Compare the draft with the original menu on the left, fix or confirm the yellow fields, and click **Approve & publish**. To try your own menu, paste it as text (one item per line with its price) and click **Extract items**.

## What a user can do

1. **Bring their menu.** Click a sample menu (one click to the review page), or paste your own menu as text. The page says up front what is AI-simulated, what is rule-based, and that photos can't be read yet.
2. **Review an AI-drafted listing.** The draft appears in an editable table beside the original menu. Yellow fields are ones the AI was unsure about. Red fields (missing name, or a blank or invalid price) must be fixed.
3. **Approve and publish.** Publishing stays locked until every yellow field is checked and every red field is fixed. The result is a customer-facing storefront with a "Locally owned" badge.

Nothing goes live until the owner approves. The AI drafts; the owner decides.

## AI: what is working vs. simulated

| | Status |
|---|---|
| **Intended AI role** | A vision-capable model reads a real menu (printed, handwritten, chalkboard or pasted) and returns items with name, price, category, an optional short description, and a flag on any uncertain field. |
| **Menu reading (three samples)** | **Simulated (Wizard-of-Oz).** Prepared results are hard-coded in `App.tsx`. No model or API is called and no API keys are used. |
| **Your own pasted text** | **Working, but rule-based, not AI.** A short set of fixed rules (`readPastedMenu` in `App.tsx`) turns each line ending in a price into an item and flags guessed prices. It is labelled "not AI" on the upload page, the banner and the review page. |
| **Your own photo** | **Not available yet (planned).** The app says so before and at the point of upload. |
| **Low-confidence flags and "AI draft" descriptions** | **Simulated** (pre-set). The chalkboard sample includes one deliberate simulated misread (Salted Caramel: $5.50 on the board, $5.00 in the draft) for risk testing. |
| **Original-menu panel, review table, edit/add/delete, price and name checks, approval lock, storefront** | **Working.** |

## Revision documentation

All revision work for this milestone is in **[revision.md](revision.md)**:

- [Priority improvements and the AI role](revision.md#1-priority-improvements)
- [Response to peer feedback](revision.md#2-response-to-peer-feedback)
- [Test cases, before/after comparison, and interpretation](revision.md#3-evaluation)
- [Risk response](revision.md#4-risk-response)
- [Remaining limitations](revision.md#5-remaining-limitations-ranked-by-effect-on-the-central-task) and [next priorities](revision.md#6-next-priorities)
- Screenshots: [docs/screenshots/](docs/screenshots/)

## AI-use disclosure

See **[ai-use-disclosure.md](ai-use-disclosure.md)**.

## Out of scope

Customer ordering, checkout, payments, delivery, drivers, accounts, saved data, and verification that a business is locally owned are deliberately excluded. The prototype tests merchant onboarding only.

## Earlier milestones

The First Working Slice tests (typical, challenge, empty) were re-run as part of the revision tests in `revision.md`.
