# BUSYWORK LLC

BUSYWORK is a single-player management game disguised as a corporate kanban board. Assign work to employees, assemble the required resources, inspect completed documents, and decide which policies the company can afford to follow.

The current build contains 30 card templates, 46 standard and juiced production recipes, and a deterministic pool of 30 active policies. Work requests roll visible contract values from 20% Low Fee cards through rare 5× Windfalls, while rare juiced variants require two resources for a larger payout. Disguised junk can start a normal-looking workflow and produce a guaranteed-invalid Source Integrity Failure in Review. Inbox overflow, legitimate deletion, missed deadlines, and Review rulings all feed the Audit and Confidence economy.

New runs visually guide only the first valid Data Entry workflow. After that, the player can pull arrivals early, merge matching multi-card stacks, triage junk through the trash target, and manage the five-day quarter without further tutorial highlighting. Every successful daily close grants one bankable Process Point before overnight planning; a randomized specialization tree lets the player fill three-pip rows that improve lane capacity, security, throughput, payouts, recovery, or audit control for the rest of the run.

## Play

Open `index.html` directly in a modern browser. The game is self-contained and makes no network requests.

The public playtest build is available at [stevengglandry.github.io/busywork-llc](https://stevengglandry.github.io/busywork-llc/). GitHub Pages publishes the repository root from `main`, so a successful push updates the live build without a separate generated branch.

## Development

- Product and technical brief: `BUSYWORK_IMPLEMENTATION_PLAN.md`
- Distractions and progression implementation record: `BUSYWORK_DISTRACTIONS_AND_PROGRESSION_PLAN.md`
- Current mechanics and payout contract: `BUSYWORK_MECHANICS_RETUNE_PLAN.md`
- Card/content catalog: `BUSYWORK_CARD_CATALOG.md`
- Current browser build: `index.html`
- Logic checks: open the browser console and run `BusyworkTests.runAll()`

Before publishing, run the embedded suite and playtest the first-workflow guide, stack merging, junk-contaminated Review output, the Process Maturity reward tree, Review rulings, audit rollover, regulatory rework, saved-run migration, chart labels, keyboard focus, and desktop/narrow layouts through a local HTTP server.

No package installation or build command is required.
