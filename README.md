# BUSYWORK LLC

BUSYWORK is a single-player management game disguised as a corporate kanban board. Assign work to employees, assemble the required resources, inspect completed documents, and decide which policies the company can afford to follow.

## Documentation ground truth

This README is the quick mechanics contract included on `main`. [`BUSYWORK_CARD_CATALOG.md`](BUSYWORK_CARD_CATALOG.md) is the detailed card-and-systems contract: exact templates, recipes, stat pools, rarity, Review outcomes, audit consequences, progression, and runtime card rules. Planning documents are historical implementation records when they conflict with these two files or the current `index.html`.

The current build contains 32 card templates, 52 standard and Juiced production recipes, and 34 policy controls. Work requests roll visible contract values from 20% Low Fee through rare 5× Windfall; Juiced scope never combines with Low Fee, requires a second resource, and multiplies the remaining quote by 5×.

### Company resources and operating stats

| Resource or stat | Ground-truth behavior |
|---|---|
| Cash | Starts at $450. Cash on hand equals opening funds plus recognized task income and other awards, minus payroll, upkeep, route/support costs, fines, and other expenses. Cash at or below $0 after operating close ends the run. |
| Staff Morale | Scales from 0–100 and reflects average worker Stress, time spent in worker sweet spots, and company events. Lower Staff Morale applies global task-speed and Accuracy debuffs. It does not directly end the run. Exact bands are in the Card Catalog. |
| Employee Stress | New employees start at 0 on a 0–100 meter. Stress affects task duration and Accuracy: strained workers take longer, fractured workers take substantially longer, and extreme Stress can cause burnout. Burnout may produce employee growth, company learning, resignation, or death. Exact bands, modifiers, and outcome probabilities are in the Card Catalog. |
| Stress sweet spot | Any valid sweet-spot band grants task-speed and Accuracy bonuses and contributes to Staff Morale. |
| Preferred workload | Separate from the sweet-spot system. Work share versus the employee's preferred target affects Stress accumulation and Backlog recovery. |
| Audit Chance | Starts at 50%. The effective chance is rolled nightly after subtracting 5 points per held Compliance Token. Accurate approval removes 1 point. |
| Liability and audit finding | Liability is the number of open policy violations. If a nightly audit occurs, finding chance is `open Liability ÷ elapsed days`, capped at 100%. If oversight finds something, Liability count directly selects Audit Severity. |
| Board Confidence | Starts at 75 and represents Trust of the Board. It scales recognized task payouts from 80% at 0 Confidence to 120% at 100. Accurate approvals and clean audits raise it; mistakes, failed audits, staffing decisions, and route tradeoffs can lower it. Confidence at or below 0 ends the run. Sweet-spot time affects Morale, not Confidence directly. |

Recognized task income is:

`round(quoted payout × (0.8 + 0.4 × Confidence / 100))`

### Review decisions

All task and resource inputs are consumed when production finishes, before the completed document enters Review.

| Choice | Correct use | Mistake or tradeoff |
|---|---|---|
| Approve | Accurate output moves to Done, pays Confidence-scaled Cash, grants Confidence +2, reduces Audit Chance by 1, and relieves producer Stress by 4 or 8 for People Pleaser. | Inaccurate output still pays, but creates +1 Liability, adds producer Stress 8 or 12 for People Pleaser, and applies Morale modifier −1. Audit Chance does not increase. |
| Request correction | Inaccurate, salvageable output returns to Backlog as its original task with a 40% shorter deadline, requires fresh resources, and gives the producer a flat +10 Stress. | Correcting accurate output still returns it as rework and gives the producer the same flat +10 Stress. Some unsalvageable violations block correction. |
| Reject | Inaccurate or unsalvageable output is finalized with no payout and gives the producer a flat +10 Stress. | Rejecting accurate output also creates +1 Liability and Board Confidence −2. Consumed inputs are not returned. |
| Escalate | Always moves the document to Done without task revenue, avoiding deletion and further handling. | Board Confidence −5 and Audit Chance −5 points. It does not return consumed inputs. |

### Audit Severity

Open Liability directly selects the consequence when the nightly audit occurs:

| Liability | Level | Consequence |
|---:|---|---|
| 0 | Clear | No reportable issue; Board Confidence +2 and base Audit Chance resets to 50%. |
| 1 | Consent Decree | Add 2 Active Policies the next day. |
| 2 | Cash Fine | Cash −$50, Audit Chance +10 points, and Board Confidence −5. |
| 3 | Compliance Check | Insert four mandatory, unpaid Compliance Check cards at seeded positions throughout the next day. Each one that expires or is deleted creates +1 Liability. |
| 4 | Bad Vibes | Staff Morale −20 and Board Confidence −5; the next workday has Accuracy −10 points and speed ×0.9. |
| 5+ | Termination | End the run immediately. |

Compliance Checks pay $0, use Intern + Spreadsheet for the specialist workflow, allow emergency Manager coverage, and have a 75-second deadline.

### Run and meta progression

New runs begin on the corporate roadmap: choose one of three starting workstreams, then move only to the same or an adjacent destination at each daily close. Short, medium, and long nodes create real 3-, 5-, and 10-minute shifts; completing a long day awards one Compliance Token. The route replaces the old overnight activity, employee-development, and multi-card hire menus.

| Resource | Earning and use | Persistence |
|---|---|---|
| Talent Points | +1 talent point awarded  at every end of day reached. Spend only at Employee Development Workshop nodes on a current employee's Accuracy, Speed, or Resilience. | Run-only; unspent points expire. |
| Process XP | +1 at every end of day reached. Spend on permanent upgrades only after completing the Day 5 Board Review. | Persists on this browser/device. |
| Compliance Tokens | Earned from claimed BUSYWORK-IT phishing rewards, Report Defect/Policy Sweep, and completed 10-minute days. Each held token automatically subtracts 5 points from Audit Chance and is never consumed by audits. | Persists on this browser/device. |

Permanent upgrades are Spam Scanner, Process Lane Annex, Inbox Expansion, Spam Intake Filter, Review Annex, Six-Tab Folders, and Manager Triage Protocol. They provide stronger full-card spam glitches, +1 In Progress, +2 Inbox, junk intake reduced from 30% to 20%, +3 Review, matching folders increased from five to six cards, and automatic morning relief of 8 Stress for the most stressed available worker.

Process XP, Compliance Tokens, and purchased permanent upgrades use browser `localStorage` until **Settings → Reset all data** is confirmed. They are not HTTP cookies. `Rich Kid` / +1 extra life and a generic single-resource day remain unimplemented plan ideas, not current rewards.

## Current card controls

- **Read:** Employees use blue/avatar circles (with executive brown reserved for the Manager), tasks amber/target circles, resources purple/diamonds, and documents green/folded pages. Cards also use a name-specific abbreviation such as `SP` for Spreadsheet or `RE` for Receipt. Colored pips carry bonus attributes; Juiced cards add a heavier double edge, layered surface, and lightning mark while retaining their base type identity. Selecting a card does not restyle other cards as possible partners. Task descriptions name the resource they need without repeating consumption boilerplate. Standalone employees show Accuracy, Speed, and Resilience as compact always-visible values and meters.
- **Pull:** The action bar counts down to the next automatic Inbox arrival. **Pull next item** delivers the next seeded card immediately and resets that clock; it is disabled when Inbox is full. Normal ten-card bags contain three distinct legitimate tasks, four resources, and three junk cards; a level-3 audit inserts four mandatory Compliance Checks at seeded positions throughout the next day.
- **Move and stack:** Click a card to inspect it. Drag a visible top card into empty lane space to pull that card into a new stack. Drag one stack onto another to merge the complete source stack when the combination is legal. A normal stack permits at most one employee, one task-like card, one document, and one resource; the sole two-resource exception is the exact pair requested by a Juiced task. Active or locked workflows cannot merge. Matching-card folders hold five cards by default or six after the permanent Six-Tab Folders upgrade.
- **Use touch:** On a coarse-pointer phone or small tablet, portrait mode prompts for landscape and safely pauses a running shift. In landscape, all five lanes remain horizontally browsable while the full Inspector stays pinned on the right. Tap cards to select them, swipe card bodies vertically to browse a lane, or grab the persistent 22 px blue tracks with 48–64 px thumbs for precise two-axis scrolling. Drag a card header/folder/workflow handle for an unambiguous move.
- **Reach covered cards:** Only the physical top card's timer advances. Covered cards appear as small card-shaped pips with their own abbreviation and status treatment. Click a pip to inspect that card or drag the pip to pull only that card out. The visible top card does not receive a redundant pip.
- **Run work:** In Progress replaces the physical pile with a composite workflow card that directly shows its employee, task, resources, forecasts, coverage effects, and progress. Its resource chips can be dragged back out before completion. A matching disguised junk resource can begin work, but it taints the result with a Source Integrity Failure.
- **Finish and review:** Completion consumes every supplied resource and leaves the employee in In Progress for reassignment. After a five-second grace period, an employee left there without a task gains stress at an escalating visible rate; assign work or move them to Backlog to stop it. The finished work product enters Review without changing the player's current selection.

The exact card list, descriptions, pull probabilities, stat and bonus pools, economy, Review outcomes, audit rules, progression, and runtime fields are in [`BUSYWORK_CARD_CATALOG.md`](BUSYWORK_CARD_CATALOG.md).

## Play

Open `index.html` directly in a modern browser. The game is self-contained and makes no network requests.

The public playtest build is available at [stevengglandry.github.io/busywork-llc](https://stevengglandry.github.io/busywork-llc/). GitHub Pages publishes the repository root from `main`, so a successful push updates the live build without a separate generated branch.

## Development

- Quick ground-truth mechanics contract: `README.md`
- Detailed ground-truth card and systems contract: `BUSYWORK_CARD_CATALOG.md`
- Historical product and technical brief: `BUSYWORK_IMPLEMENTATION_PLAN.md`
- Historical distractions/progression implementation record: `BUSYWORK_DISTRACTIONS_AND_PROGRESSION_PLAN.md`
- Historical mechanics retune record: `BUSYWORK_MECHANICS_RETUNE_PLAN.md`
- Current browser build: `index.html`
- Interactive org-chart/Gantt route concept: `design/corporate-roadmap-concept.html`
- Logic checks: open the browser console and run `BusyworkTests.runAll()`

Before publishing, run the embedded suite and playtest starting-location selection, adjacent roadmap routing, Workshop spending, the Approve Purchase Request workflow, permanent upgrades, stack merging, junk-contaminated Review output, audit rollover, regulatory rework, saved-run migration, keyboard focus, and desktop/narrow layouts through a local HTTP server.

No package installation or build command is required.
