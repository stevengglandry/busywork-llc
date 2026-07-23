# BUSYWORK LLC

BUSYWORK is a single-player management game disguised as a corporate kanban board. Assign work to employees, assemble the required resources, inspect completed documents, and decide which policies the company can afford to follow.

The current build contains 30 card templates, 16 production recipes, and a deterministic pool of 30 active policies. Work requests roll visible contract values from 20% Low Fee cards through rare 5× Windfalls. Disguised junk can contaminate resource shortcuts or consume an employee's time as bogus work, while Inbox overflow, legitimate deletion, missed deadlines, and Review rulings all feed the Audit and Confidence economy.

New runs visually guide only the first valid Data Entry workflow. After that, the player can pull arrivals early, merge matching multi-card stacks, triage junk through the trash target, and manage the five-day quarter without further tutorial highlighting.

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

Before publishing, run the embedded suite and playtest the first-workflow guide, stack merging, junk routing, Review rulings, audit rollover, regulatory rework, saved-run migration, chart labels, keyboard focus, and desktop/narrow layouts through a local HTTP server.

No package installation or build command is required.
