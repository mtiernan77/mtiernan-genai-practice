# AI-Use Disclosure (Draft): Things Merchant Onboarding

ESTM 60244 · Michael Tiernan · Prototype Revision, November 2026

This builds on the AI-use record in my Project Problem Framing document.

## Tools used

| Tool | Used for |
|---|---|
| **Claude (Opus 5.5), claude.ai** | Problem framing, research aid (DoorDash pricing), document structure, and, for this revision, reviewing the prototype code, proposing revisions, editing `App.tsx`, running the test cases in a browser, and drafting this documentation. |
| **Replit Agent** | Built the original First Working Slice app (React + Vite) and published it. |

Using AI to write code is separate from the product's own AI feature (menu reading), which is described at the end.

## Development assistance

- **Code review:** I gave Claude my GitHub repo, README and the peer feedback. It read `App.tsx` and pointed out two issues I hadn't spotted. The "Your original menu" panel was drawn from the AI's extracted items rather than the source text, so the side-by-side check could not catch a misread. A blank, flagged price could be ticked as checked and published as "Price to be confirmed".
- **Code changes:** Claude wrote the edits to `App.tsx`: the demo-limit messages, the plain "what Things is" line, the source-text panel, the price/name checks, one-click sample menus, and the rule-based reader for pasted text. I pasted the file into Replit.
- **Testing:** Claude ran the test cases against both the old and new versions in an automated browser and took the screenshots in `docs/screenshots/`. At my request, it then re-ran all seven cases on the live Replit site in my Chrome browser, and the results matched `revision.md`.
- **Deployment help:** My first paste of the new code into Replit hit a package-install error, which Replit Agent fixed, but the project was left on the older version of `App.tsx`. Claude found this by checking the live site and the Replit preview, put the new file into the Replit editor (checked to be identical: same 352 lines, 0 errors), and confirmed the paste feature worked in the preview. I republished the site myself.
- **Writing:** Claude drafted `revision.md`, this disclosure, the README update and my demo speaking notes. I edited them to match what I actually decided and saw.

## Representative prompts

1. *"This assignment is a carry on from the previous notion project prototype assignments I've completed."* (with the assignment brief, framing document, repo links and peer feedback attached)
   → Claude summarised what the milestone needs, read the code, and listed four possible priorities.
   **What I changed:** I chose three of them and dropped the fourth (regenerating AI descriptions) to keep scope down.
2. Choosing between keeping the AI simulated with clearer messaging, adding a basic rule-based text reader, or wiring up a live AI model.
   → Claude recommended staying simulated for this milestone.
   **What I decided:** I agreed. The live model is my next priority, not something to rush in two days.
3. *"Cool, it all works, I still find it tricky to move to the next page. Is there a way of getting the two functions working, easily, with code from here?"*
   → Claude laid out three options: a live model via Replit Agent, a rule-based reader for pasted text, or only a one-click fix. It explained the trade-offs, including that photos can't be read without a model.
   **What I decided:** pasted text only, with no AI and clearly labelled. Claude's own testing caught a capitalisation bug ("Nonna'S Deli"), which it fixed before handing over the code.
4. My peer review notes, pasted in.
   → Claude mapped each recommendation to a change and drafted the feedback-response table.
   **What I decided:** which recommendations to act on, adapt or defer. The table reflects my choice to keep the AI simulated for now.

## Human judgment

- **Changed my mind on a rule-based reader for pasted menus.** I first said no: it could make it look like the AI was reading menus when it wasn't. After testing the revised flow myself, owners still hit a dead end with their own menu, so I added it for pasted text only. My conditions were that it is labelled "not AI" on every screen it touches, that its drafts don't carry "AI draft" labels, and that it is tested and reported as a rule-based baseline, not as AI evidence.
- **Rejected: wiring up a live model before Thursday.** It's the biggest open question, but rushing it in two days would risk breaking a flow that now works. It is my next priority.
- **Rejected (from Problem Framing): customer-side natural-language search across shops.** I think normal filters and search cover that need.
- **Kept the risk test result as Partial.** The test showed that an owner can still tick through a wrong price. I decided to report that honestly and make it a next priority rather than claim the safeguard is complete.
- **Declined a rebrand.** Claude offered to rename the prototype from "Things" to my startup's newer name. I said no, because I don't want to reveal the new name in a public repository before my startup competition.

## Verification

- **Checked:** All seven test cases were re-run on the live Replit build after the new `App.tsx` was published, and the results were compared with the table in `revision.md`. I checked the before/after screenshots against the old and new versions. I confirmed the code contains no API keys or real shop data.
- **Still needs checking:** Everything about real AI accuracy. Extraction, flags and price normalisation are prepared examples. The clarity improvements were judged from the screens, not by having a new person try them. The pain points from my owner interviews still need follow-up.

## AI inside the product

| | |
|---|---|
| **Task** | Read a shop's menu and draft a structured listing for the owner to review. |
| **Inputs** | A menu photo or pasted menu text. |
| **Outputs** | Items with name, price, category, an optional "AI draft" description, and flags on uncertain fields. |
| **Status** | **Simulated (Wizard-of-Oz).** Three sample menus return prepared results hard-coded in `App.tsx`. No model or API is called. One sample includes a deliberate simulated misread for risk testing. **Pasted menus** are read by a simple rule-based reader (fixed rules, not AI), labelled as such in the app. Photos are not read, and the app says so. **Planned:** a vision-capable model, tested on real South Bend menus and compared with the rule-based reader. |
