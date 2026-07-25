# BUSYWORK LLC

BUSYWORK is a single-player management game disguised as a corporate kanban board. Assign work to employees, assemble the required resources, inspect completed documents, and decide which policies the company can afford to follow.

The current build contains 30 card templates, 46 standard and juiced production recipes, and a deterministic pool of 30 active policies. Work requests roll visible contract values from 20% Low Fee cards through rare 5× Windfalls; rare juiced variants never roll LOW FEE, require two resources, and multiply their standard, premium, or windfall quote by 1.75×. Disguised junk can start a normal-looking workflow and produce a guaranteed-invalid Source Integrity Failure in Review. Audit Chance controls whether the nightly audit occurs; operational mistakes primarily increase Exposure—the chance that an audit discovers a liability—or raise the eventual punishment severity.

New runs visually guide only the first valid Data Entry workflow. After that, the player can pull arrivals early, merge matching multi-card stacks, triage junk through the trash target, and manage the five-day quarter without further tutorial highlighting. Every successful daily close grants one bankable Process Point before overnight planning; a randomized specialization tree lets the player fill three-pip rows that improve lane capacity, security, throughput, payouts, recovery, or audit control for the rest of the run.

## Current card controls

- **Read:** Employees use blue/avatar circles (with executive brown reserved for the Manager), tasks amber/target circles, resources purple/diamonds, and documents green/folded pages. Cards also use a name-specific abbreviation such as `SP` for Spreadsheet or `RE` for Receipt. Colored pips carry bonus attributes; Juiced cards add a heavier double edge, layered surface, and lightning mark while retaining their base type identity. Selecting a card does not restyle other cards as possible partners. Task descriptions name the resource they need without repeating consumption boilerplate. Standalone employees show Accuracy, Speed, and Resilience as compact always-visible values and meters.
- **Pull:** The action bar counts down to the next automatic Inbox arrival. **Pull next item** delivers the next seeded card immediately and resets that clock; it is disabled when Inbox is full. Each repeating ten-card bag contains three distinct legitimate tasks, four resources, and three junk cards.
- **Move and stack:** Click a card to inspect it. Drag a visible top card into empty lane space to pull that card into a new stack. Drag one stack onto another to merge the complete source stack when the combination is legal. A normal stack permits at most one employee, one task-like card, one document, and one resource; the sole two-resource exception is the exact pair requested by a Juiced task. Active or locked workflows cannot merge, and every stack is capped at five cards.
- **Reach covered cards:** Only the physical top card's timer advances. Covered cards appear as small card-shaped pips with their own abbreviation and status treatment. Click a pip to inspect that card or drag the pip to pull only that card out. The visible top card does not receive a redundant pip.
- **Run work:** In Progress replaces the physical pile with a composite workflow card that directly shows its employee, task, resources, forecasts, coverage effects, and progress. Its resource chips can be dragged back out before completion. A matching disguised junk resource can begin work, but it taints the result with a Source Integrity Failure.
- **Finish and review:** Completion consumes every supplied resource and leaves the employee in In Progress for reassignment. After a five-second grace period, an employee left there without a task gains stress at an escalating visible rate; assign work or move them to Backlog to stop it. The finished work product enters Review without changing the player's current selection.

The exact card list, descriptions, pull probabilities, and runtime fields are in `BUSYWORK_CARD_CATALOG.md`. The authoritative economy, stacking, deletion, Review, audit, and progression rules are in `BUSYWORK_MECHANICS_RETUNE_PLAN.md`.

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
