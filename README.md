# Things — Merchant Onboarding (Prototype)

**Things is a delivery platform for locally owned shops only, and this prototype shows how a shop owner turns their existing menu into a ready-to-publish storefront in minutes.**

ESTM 60244 · Project Prototype: First Working Slice · Michael Tiernan

---

## What the first working slice lets a user do

A single-location shop owner (café, bakery, deli, ice cream shop) can:

1. **Bring their menu** — upload a photo, paste menu text, or try one of three sample menus.
2. **Review an AI-drafted listing** — see their original menu beside an editable table of items (name, price, category, description). Fields the AI is unsure about are highlighted in yellow and must be edited or confirmed.
3. **Approve and publish** — the "Approve & publish" button stays locked until every flagged field is checked. Approving shows a simple customer-facing storefront page with a "Locally owned" badge.

Nothing goes live until the owner approves. The AI drafts; the owner decides.

## How to open the prototype

- **Prototype link:** https://things-merchant-onboarding.replit.app
- **Replit project:** https://replit.com/@mtiernan7/Things-Merchant-Onboarding

To try it: click **Try sample: Clean café menu**, then **Extract items**, review the table, and click **Approve & publish**.

## AI: what is working vs. simulated

| | Status |
|---|---|
| **Intended AI role** | A vision-capable language model reads a photo of a real menu (printed, handwritten, chalkboard, or pasted text) and returns structured items — name, price, category, an optional short description — with a confidence flag on any field it is unsure about. |
| **Menu reading / extraction** | **Simulated (Wizard-of-Oz).** The three sample menus return prepared, hard-coded results. No AI model or external API is called, and no API keys are used. |
| **Uploading your own photo** | **Simulated.** The app does not read real photos yet; it shows a notice and points to the sample menus rather than inventing a result. |
| **Low-confidence flags** | **Simulated.** Which fields are flagged is pre-set in each sample result. |
| **Item descriptions** | **Simulated.** Pre-written and labelled "AI draft". No dietary or allergen claims are made. |
| **Review table, editing, add/delete, approval lock, storefront page** | **Working.** These run for real in the browser. |
| **Empty / no-price input check** | **Working.** |

A prototype banner on every screen tells users that AI menu reading is simulated.

## Test results

| Test | Input or action | What happened | Result |
|---|---|---|---|
| **Typical case** | "Try sample: Clean café menu" (Maple Street Café, 8 items), then Extract items | Draft showed all 8 items beside the original menu, grouped into Coffee, Pastries and Breakfast, each with an "AI draft" description and nothing flagged. Approve & publish led to a storefront with all items and prices matching the menu. | **Pass** |
| **Challenge case** | "Try sample: Handwritten chalkboard" (Corner Scoop Ice Cream, 6 items), then Extract items | Draft showed 6 items with 3 fields flagged "Please check" (2 prices, 1 item name). Approve & publish stayed disabled until all 3 flagged fields were confirmed, then unlocked. | **Pass** |
| **Invalid or empty case** | Clicked Extract items with no photo, no text and no sample | Stayed on the upload screen and showed: "We couldn't find any items with prices. Try a clearer photo, paste your menu, or use a sample." No broken table or dead end. | **Pass** |

## Known limitations

1. **Menu reading is simulated.** Only the three sample menus produce a draft; real photos and free-form menus are not read yet. Next step: connect a vision-capable model and test extraction accuracy on real South Bend menus, especially handwritten ones.
2. **No saved data.** Approved storefronts exist only in the current browser session and disappear afterwards. Next step: add simple storage so an owner can return and edit their listing.
3. **Confidence flags are pre-set, not measured.** Highlighting shows how uncertainty would be communicated, but nothing is calculated yet, and price normalisation ("8.5", "$8.50", "8 dollars") is only demonstrated on one prepared sample. Next step: generate confidence from the model's output and test on messy real-world text.

## Out of scope for this slice

Customer ordering, checkout, payments, delivery, drivers, accounts, and verification that a business is locally owned are deliberately excluded. This slice tests merchant onboarding only.

## Built with

Replit Agent (AI-assisted development). Using Replit to write code is not the product's AI feature; the product's AI role is the menu extraction described above.
