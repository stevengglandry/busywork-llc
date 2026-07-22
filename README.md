# BUSYWORK LLC

BUSYWORK is a single-player management game disguised as a corporate kanban board. Assign work to employees, assemble the required resources, inspect completed documents, and decide which policies the company can afford to follow.

## Play

Open `index.html` directly in a modern browser. The game is self-contained and makes no network requests.

The public playtest build is available at [stevengglandry.github.io/busywork-llc](https://stevengglandry.github.io/busywork-llc/). GitHub Pages publishes the repository root from `main`, so a successful push updates the live build without a separate generated branch.

## Development

- Product and technical brief: `BUSYWORK_IMPLEMENTATION_PLAN.md`
- Current mechanics and payout contract: `BUSYWORK_MECHANICS_RETUNE_PLAN.md`
- Card/content catalog: `BUSYWORK_CARD_CATALOG.md`
- Current browser build: `index.html`
- Logic checks: open the browser console and run `BusyworkTests.runAll()`

Before publishing, run the embedded suite and playtest the Review rulings, audit rollover, regulatory rework, saved-run migration, keyboard focus, and desktop/narrow layouts through a local HTTP server.

No package installation or build command is required.
