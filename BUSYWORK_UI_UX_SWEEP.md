# BUSYWORK UI/UX Sweep

Status: design audit, approved visual direction, and partial implementation checkpoint

Scope: current `index.html`, five supplied captures, and live renders at 1920×1080 and 1280×720

Intent: preserve the fast, tactile card game while making the interface easier to read, scan, and operate

## Implemented checkpoint — 2026-07-28

This document began as a full redesign brief. The current source now implements the first approved portion while retaining the remaining sections as the next-stage design contract.

Implemented in `index.html`:

- Compact task, resource, employee, folder, and active-workflow cards with stronger semantic type color, shorter landscape proportions, and clearer status tokens.
- Filing-folder treatment for homogeneous task/resource piles. The four-slot rectangular rail represents covered cards only, nearest to the top first; the full-size folder face represents the active top card, unused capacity stays visibly empty, and the folder becomes a normal card when only one item remains.
- Drag insertion targets between lane items. Reordering moves a whole card/stack without recreating it or disturbing active jobs, employee rhythms, card deadlines, or unrelated runtime timers.
- Cleaner Inspector copy and navigation, including the compact `Log` tab, assignment choices, review ruling heading, and active-workflow employee/stress band.
- Updated phishing reward baseline of four correctly deleted junk cards; Security Liaison still lowers the threshold to two.
- A sixth Run System, **Grease the Wheels**, which restores five Board Confidence per purchased rank.
- The approved Process Award composition: `End of Shift` crescent stamp, budget rail, directly actionable Run Systems, `&` divider, and Employee Development shop in two equal panels. It fits the 1280×720 baseline without modal scrolling.
- Operating Close retains its existing company, cash, action, and recruitment content in three equal panels; a circular `OR` divider now makes the action-versus-hire choice explicit.
- The third page uses the fixed `Morning Brief` title and three equal read-only panels while retaining the existing day-specific briefing sentence and data. Its first panel is `Changes overnight`, showing signed Cash, Staff Morale, Audit Chance, and Board Confidence deltas from the prior close.
- Capability-based mobile detection now uses coarse-pointer support and the short viewport dimension rather than a user-agent string. Portrait phones receive a landscape prompt that safely pauses and restores a running workday, with an explicit continue-in-portrait escape hatch.
- In mobile landscape the complete five-lane board remains horizontally scrollable while the complete Inspector stays pinned on the right. Lane widths compress to a 210 / 172 / 198 / 174 / 128 px decision hierarchy, lane bodies retain independent vertical scrolling, and no cards, controls, reference tabs, policies, or status metrics are removed.
- Touch input distinguishes intent: tap selects, vertical card-body movement remains available to scroll a lane, horizontal movement can drag a card, and card headers plus folder/workflow handles provide full-direction grab surfaces. Drop-between slots and controls expand for coarse pointers.

Still design guidance, not yet implemented:

- The revised Operating Close with full-moon treatment, action/effect tables, and exactly one randomized recruit.
- The delta-focused **Morning BRIEF** screen.
- The revised successful quarterly-review/end-of-run layout. The compact terminal-failure report is implemented; the successful-run counterpart remains future work.

The canonical visual references are in `design/ui-concepts/`; `design/BUSYWORK_UI_STYLE_GUIDE.css` is the implementation guide for future UI work.

## Executive diagnosis

BUSYWORK already has a strong visual premise: a restrained navy-and-paper palette, filing-card borders, terse institutional language, and dense dashboards that feel like an internal operations tool. The problem is not a lack of style. The problem is that the style and the information density compete with the game.

The clearest opportunities are:

1. **Raise the legibility floor.** The live workday contains visible text from 6–16 px; night pages still reach 6–8 px. Labels this small work as texture in a mockup but not as game information.
2. **Give the workday a stronger focal hierarchy.** The header, notice bar, five lanes, cards, navigation, save status, and policy list all ask for attention at once. The player’s immediate decision should be the loudest thing on screen.
3. **Separate “reference” from “action.”** The Inspector mixes selected-item actions, four reference tabs, save state, end-day control, and default policies. It behaves like a side drawer, dashboard, and command console simultaneously.
4. **Allocate lane width by decision complexity.** Inbox, In Progress, and Review need more width than Done. At 1280 px, the five visible lanes compress to roughly 190 / 200 / 220 / 200 / 132 px while the Inspector remains 300 px.
5. **Make night screens sequential, not merely dense.** Their three-stage rhythm is good, but each modal resembles a spreadsheet of equally weighted panels. One obvious primary decision per screen would reduce comparison cost.
6. **Use the corporate fiction more purposefully.** The interface is convincingly “corporate,” but can become more distinctive through memo headers, control numbers, approval stamps, stamped status language, and restrained paper texture—without adding visual noise.

The recommended direction is **“regional operations console”: readable 12–14 px body copy, compact but generous cards, one blue action color, semantic status strips, and playful corporate artifacts used as hierarchy rather than decoration.**

## Audit method and evidence

- Reviewed the current single-file app structure and every major render path.
- Inspected the five supplied captures at original size.
- Rendered the live app at 1920×1080 and 1280×720.
- Checked the workday, Process award, Operating close, Morning briefing, Quarterly review, and terminal run states.
- Measured the 1280×720 workday:
  - 980 px board plus a fixed 300 px Inspector.
  - lane widths of approximately 190 / 200 / 220 / 200 / 132 px.
  - the board has horizontal overflow at that size.
- Measured visible workday typography at 1920×1080:
  - 6 px: 28 visible elements.
  - 7 px: 13 visible elements.
  - 8 px: 17 visible elements.
  - 9 px: 24 visible elements.
- Measured night screens at 1280×720:
  - Process award: minimum 8 px.
  - Operating close: minimum 6 px.
  - Morning briefing: minimum 8 px.
  - Quarterly review: minimum 8 px.
- Confirmed the existing runtime correctly distinguishes read-only and action panels and avoids modal overflow at 1280×720. The redesign should preserve these functional strengths.

## Screen and state inventory

| Area | Screen or state | Primary purpose | Main elements |
|---|---|---|---|
| Entry | Welcome / resume | Explain the first workflow and enter a run | appointment memo, objective, resume, begin |
| Workday | Default board | Route arrivals and watch company health | header, notice bar, five lanes, Inspector |
| Workday | Inbox | Triage new work, resources, and junk | cards, stacks, deadlines, delete target |
| Workday | Backlog | Hold employees and staged work | employees, spare cards/stacks, recovery |
| Workday | In Progress | Assemble and operate workflows | incomplete stacks, active jobs, idle employees |
| Workday | Review | Evaluate completed documents | document fields, policies, manager approval, ruling actions |
| Workday | Done | Record resolved work | compact completion log |
| Workday | Selected task/resource | Explain requirements and offer moves | details, compatible workers/inputs, destinations, delete |
| Workday | Selected employee | Explain fit, stress, and availability | stats, sweet spot, assignment/check-in actions |
| Workday | Selected workflow | Show readiness, outcome, progress, and intervention | worker, task, resources, ETA, accuracy, payout, progress |
| Workday | Selected document | Make a ruling | fields, applicable policies, manager requirement, four outcomes |
| Workday | Progress panel | Show run telemetry and carryover | balances, audit, quarter, health, cash, outcomes |
| Workday | Policies panel | Reference current constraints | policy list |
| Workday | Recipes panel | Reference valid workflows | recipe network, durations, reward ranges |
| Workday | Company Log | Review chronological events | timestamped event list |
| Workday | Paused | Stop runtime while preserving context | board veil, pause card |
| Workday | Settings | Adjust display and storage behavior | compact, motion, contrast, reset |
| Workday | Toasts | Report feedback and incidents | ordinary, harmful, and security notices |
| Night 1 | Process award | Spend or bank a Run Process Point | process upgrades, employee development |
| Night 2 | Operating close | Understand the day and choose one overnight action | company position, cash movement, planning, recruitment |
| Night 2 alt | Strategic planning | Make the mandatory Day 3 choice | strategic options instead of ordinary overnight choices |
| Night 3 | Morning briefing | Understand the opening state | company position, operating brief, active policies |
| Run end | Quarterly review | Summarize successful run and spend persistent rewards | company position, charts, XP/token upgrades |
| Run end | Terminated run | Explain failure and retained progress | terminal cause, factors, incidents, carryover |

## Cross-system findings

### 1. Readability and legibility

What works:

- The palette has high structural contrast: dark global header, pale work surface, colored semantic edges.
- Numeric readouts use tabular figures.
- Headings and labels are consistent in case and letter spacing.
- Dangerous outcomes use red and are not presented as ordinary notices.

What needs work:

- Six- to nine-pixel labels are below a comfortable reading floor on normal-density desktop screens. The smallest labels include actual gameplay facts: employee stats, stress state, reward modifiers, chart axes, and recruitment details.
- Uppercase plus wide letter spacing makes tiny text even harder to parse.
- Muted gray copy often carries essential rules rather than optional explanation.
- The same card may contain kind, modifier, code, title, trait, description, stress, three stats, timing, and state messaging. Shrinking every part is being used as the solution to density.

Recommended standard:

| Use | Minimum size | Notes |
|---|---:|---|
| Primary body and actionable explanation | 13 px | 1.4–1.5 line height |
| Card body | 12 px | May clamp to two lines |
| Secondary data and metadata | 11 px | Never use for critical consequences |
| Eyebrow / compact label | 10 px | Uppercase allowed; reduce letter spacing |
| Page title | 22–28 px | Modal and run-end screens |
| Lane title / card title | 13–15 px | Semibold, not extra-bold |
| Buttons | 12–13 px | 32 px minimum height; 36 px for primary actions |

The current compact mode can preserve the denser 9–11 px presentation as an explicit player choice. It should not define the default.

### 2. Information hierarchy

The workday has four competing command layers:

1. global status and run controls,
2. contextual instruction and arrival control,
3. the board,
4. Inspector navigation and actions.

The recommended hierarchy is:

1. **Current operational risk:** time, urgent deadlines, Review queue, Audit.
2. **Current action:** the selected card/workflow and its next valid move.
3. **Throughput:** the five lanes and their capacities.
4. **Reference:** policies, recipes, log, progress.
5. **Meta controls:** carryover, settings, new run.

This does not remove information. It changes when and where it competes for attention.

### 3. Organization and use of space

- The top bar uses a lot of width for five separately boxed metrics and four meta controls. It becomes two rows below 1120 px, but at 1280 it is still dense enough to slow scanning.
- Empty lanes consume the same vertical area as busy lanes, which is correct for drag targets, but their empty-state boxes add another layer of chrome.
- The Inspector keeps 300 px even when nothing is selected. On the 1280 px workday, that is nearly one quarter of the viewport.
- Done receives permanent lane width even though it is a log, not a staging area.
- Night screens correctly fit without scrolling, but accomplish this by shrinking dense content. “No scroll” should be a preference, not a reason for 6 px text.

Recommended space model:

- Workday desktop:
  - collapsible utility rail: 320–360 px open, 44–52 px closed.
  - board uses all recovered width.
  - weighted lanes: Inbox 1.05, Backlog 1.0, In Progress 1.3, Review 1.15, Done 0.55.
- Workday 900–1279 px:
  - utility rail becomes an overlay drawer.
  - Done becomes a compact “Completed 4” ledger button or a narrow lane.
  - board remains horizontally scrollable, with clear edge affordances.
- Night screens:
  - allow modest vertical scrolling below the fixed title/action frame.
  - present the choice first, with evidence alongside or behind expandable “details.”

### 4. Action clarity

- The notice bar is useful onboarding, but reads like another notification rather than the current command.
- Drag-and-drop and Inspector buttons duplicate some operations without a consistent vocabulary.
- “Move to?” is spread across contextual buttons. The game would benefit from one standard action group: `Assign`, `Add input`, `Move`, `Review`, `Delete`.
- The always-visible Inbox delete zone is discoverable but visually dominates an otherwise empty lower lane.
- “End day early” sits beside save state and reference navigation, even though it changes the entire run state.

Recommended action rules:

- Every selection exposes one **Recommended next action**.
- Secondary actions sit under an `Other actions` disclosure.
- Invalid destinations are hidden from the primary list, but can remain visible with a specific reason in a full `Move` menu.
- Destructive actions are never adjacent to routine primary actions.
- `End day` belongs with the clock and pause control; require a concise consequence summary before confirmation.
- Dragging remains first-class, but Inspector actions are equal, reliable alternatives—not a separate interaction model.

### 5. Mock corporate aesthetics

Keep:

- navy global shell,
- warm white documents,
- pale blue operations surfaces,
- semantic green, amber, red, and purple,
- institutional copy,
- stamps, control IDs, dotted rules, filing cues.

Reduce:

- a border around every piece of information,
- pill labels for ordinary static metadata,
- multiple different blue outlines in the same component,
- tiny all-caps used everywhere.

Add selectively:

- memo routing strips: `FROM`, `TO`, `SUBJECT`, `CONTROL`.
- approval stamps for Review outcomes.
- binder tabs for reference sections.
- subtle alternating ledger rows.
- “carbon copy” shadows only on draggable cards/stacks.
- one visual metaphor per state:
  - Inbox = intake slip,
  - Backlog = filing tray,
  - In Progress = work order,
  - Review = approval packet,
  - Done = ledger entry.

## Main Workday redesign

### Current strengths

- The five-stage process is visible without navigation.
- Cards remain the main object of play.
- The fixed Inspector makes selection details stable.
- Capacity counts and lane subtitles communicate mechanical differences.
- The top status bar makes the run feel like a control room.

### Current problems

- At 1280 px, the lanes are readable only because card content is compressed aggressively.
- The 300 px Inspector is mostly empty when nothing is selected.
- Top-level reference tabs have the same visual weight as current-card actions.
- Critical state is fragmented: the task deadline is on its card; the workflow progress is elsewhere; Review urgency is in another lane; company risk is above.
- Done behaves like a full working lane even though it contains historical receipts.
- The visual order says “read everything”; the gameplay order is “deal with the next bottleneck.”

### Recommended alternative

Use a three-part shell:

1. a thinner two-tier operations header,
2. a weighted five-lane board,
3. a collapsible selection drawer.

```text
┌ BUSYWORK / Regional Ops ─ Day 2 · 03:41 ───────────── Pause  End day ▾ ┐
│ $391 ↘59   Morale 72 ↑2   Audit 45% / Exposed   Confidence 77 ↑2       │
├─────────────────────────────────────────────────────────────────────────┤
│ NEXT ACTION  Expense Report needs a Receipt     [Find Receipt] [×]      │
├────────────┬───────────┬────────────────┬──────────────┬───────┬────────┤
│ INBOX 5/9  │ BACKLOG   │ IN PROGRESS    │ REVIEW 2/5   │ DONE 4│ DETAIL │
│ Urgent 2   │ People 4  │ 2 jobs · 1 gap │ Action req.  │ ▸ log │        │
│            │           │                │              │       │ Card   │
│ cards      │ workers   │ work orders    │ documents    │ rows  │ facts  │
│ & stacks   │ & staging │ with progress  │ with due cue │       │        │
│            │           │                │              │       │ NEXT   │
│            │           │                │              │       │ ACTION │
├────────────┴───────────┴────────────────┴──────────────┴───────┴────────┤
│ Progress  Policies  Recipes  Log                         Saved just now │
└─────────────────────────────────────────────────────────────────────────┘
```

### Workday implementation rules

- Keep the entire board visible at 1440 px and above.
- Use the right drawer only when selected. When closed, leave a 48 px tab with selection count and keyboard shortcut.
- Put reference tabs in a slim bottom utility bar or a drawer sub-navigation, not above selected-item actions.
- Replace the large empty-state cards with a single centered line and a faint lane-specific icon.
- Turn Done into a ledger: one-line entries with document, ruling, payout, and time.
- Let lane headers surface actionable summaries:
  - Inbox: `2 urgent`.
  - Backlog: `4 people · 2 stored`.
  - In Progress: `2 running · 1 incomplete`.
  - Review: `2 waiting · oldest 0:31`.
  - Done: `$418 recognized`.
- Make the next-action banner contextual and dismissible after onboarding. It should link directly to the needed card or action.

## Lane-by-lane redesign

### Inbox

Problems:

- Cards are visually complete records, so five items already feel like a full page.
- Deadline urgency relies mainly on small timing text and border color.
- The large Delete drop zone consumes useful space and competes with valid routing.

Alternative:

- Default to a compact intake-card anatomy: code, title, reward, deadline, and one-line requirement.
- Reveal description and modifiers on selection/hover/focus.
- Group homogeneous piles with a clear count tab and show the top deadline.
- Replace the full-width Delete zone with a persistent shredder icon in the lane footer. Expand it into a target only during drag or when an item is selected.
- Add sort-free visual grouping through a left status stripe:
  - red = urgent,
  - amber = task,
  - purple = resource,
  - hatched gray = suspicious/junk clue.

### Backlog

Problems:

- Employees use the same footprint as cards even though their key information is different.
- Employee flavor text competes with availability, stress, and fit.
- Stored work and employees are visually interleaved.

Alternative:

- Split the lane internally into `People` and `Staged files`.
- Employee rows show avatar, name, status, stress band, and the strongest stat.
- Full stats, traits, and flavor text live in the selection drawer.
- Stored stacks use filing-folder tabs and a visible count.
- Make recovery explicit in the lane header: `Recovery −1.2 stress/min`.

### In Progress

Problems:

- Active workflows are information-rich and visually successful, but too much is set at 7–9 px.
- An idle employee stranded in this lane looks like a healthy green employee card until the red explanation is read.
- Dashed selection and solid card outlines compete.

Alternative:

- Use a work-order layout with four priority rows:
  1. worker + state,
  2. task + missing/attached inputs,
  3. ETA / accuracy / payout,
  4. progress and intervention.
- Make “idle in active lane” a distinct amber `Awaiting reassignment` work-order state, not an employee card with an error appended.
- Replace the dashed selection rectangle with a 3 px blue edge and a checkmark tab.
- Show current stress and sweet zone as a single labeled band with legible endpoints.
- Use `Intervene` as a real button beside the progress bar; do not make the entire bar an unexplained click target.

### Review

Problems:

- The lane accurately signals attention, but the real decision happens entirely in the Inspector.
- Policies and document fields are separated vertically from the ruling buttons.
- Four outcomes are presented with similar visual weight despite different risk.

Alternative:

- Review cards should show: document, originating task, payout at stake, number of applicable policies, and waiting time.
- Selecting a document opens a comparison drawer:

```text
REVIEW PACKET  EX-1042                         Waiting 00:31
Expense Report                         Potential payout $280
────────────────────────────────────────────────────────────
FIELD                 DOCUMENT          POLICY / RESULT
Amount                $412              > $300 → Manager required
Receipt               Attached          Evidence Standard → Pass
Manager signed        No                Authorization 4.2 → Blocked
────────────────────────────────────────────────────────────
Recommended: Call Manager into Review
[Call Manager · 15s]   [Request correction]   Other outcomes ▾
```

- Once requirements are satisfied, promote `Approve · Collect $X` to the primary action.
- Keep risky alternatives under `Other outcomes` with consequence copy.
- Stamp the packet after resolution: `APPROVED`, `RETURNED`, `REJECTED`, or `ESCALATED`.

### Done

Problems:

- A full lane implies continued manipulation, but Done is primarily history.
- A small lane gives completed cards too little room to be useful.

Alternative:

- Use compact ledger rows rather than cards.
- Show the daily payout total in the header.
- Allow expansion into the Company Log or a day summary.
- Retain the lane as a drop/animation destination, then collapse completed work into the ledger after the arrival animation.

## Card and stack system

### Base card anatomy

```text
┌ TASK · ER                          DUE 01:44 ┐
│ Expense Report                     $280     │
│ Receipt required                             │
│ [5× PAYOUT]                                  │
└───────────────────────────────────────────────┘
```

Default cards should answer only four questions:

1. What is it?
2. What does it need?
3. How urgent is it?
4. What is it worth or what state is it in?

Flavor text, control IDs, full rules, and secondary stats belong in the drawer.

### Stack anatomy

- Use a folder tab with `×3` rather than several tiny buried-card pips competing with the top card.
- On focus/hover, fan a compact list of buried items with paused deadlines.
- Keep individual buried cards draggable.
- Show a stack-level state label: `Stored`, `Needs input`, `Processing`, `Review ready`.
- Never encode the stack state only in border style.

### Employee anatomy

```text
AC  Accountant                  AVAILABLE
Stress 14%  ● sweet zone
Accuracy 5   Speed 3   Resilience 3
Best fit: Finance workflows
```

- Use full stat labels in the drawer and abbreviations only on the board.
- Preserve the Manager’s dual sweet zones, but render them as two highlighted ranges on one meter.
- Show `Below`, `Sweet`, `Strained`, or `On leave` as text next to the meter.

## Selection drawer / right action bar

The drawer should use a stable information architecture:

1. **Identity:** title, code, kind, current lane.
2. **State:** deadline, stress, workflow readiness, or review status.
3. **Recommended next action:** one prominent button and consequence.
4. **Requirements / details:** short comparison or checklist.
5. **Other actions:** Move, check in, release, delete.
6. **Reference links:** relevant recipes and policies only.

The current global tabs should become a separate `Reference` mode:

```text
DETAIL                                      [collapse]
Expense Report · Task
Due 01:44 · $280 contract

NEXT ACTION
Assign a qualified employee
[Choose employee]

REQUIREMENTS
✓ Receipt available in Inbox
○ Employee required

OTHER ACTIONS
[Move ▾]  [Delete…]

RELATED
Expense workflow · Financial Authorization 4.2
```

Manager check-in should appear only on an eligible selected employee and should state the exact stress change and availability rule before activation.

## Header and global controls

Recommended two-tier arrangement:

- Tier 1: brand, day/time, Pause/Resume, End day.
- Tier 2: Cash, Staff Morale, Audit, Board Confidence, each with value, direction, and state word.
- Carryover, Settings, and New run move into a compact overflow menu. `New run` remains protected by confirmation.

Metric treatment:

- Cash: `$391`, change `−$59`, projection optional on expand.
- Staff Morale: `72 · Healthy`, change `+2`.
- Audit: `45% · Exposed`, severity dots remain but enlarge.
- Board Confidence: `77 · Strong`, change `+2`.
- Time: show day and countdown together as the dominant metric.

The tiny cash sparkline can remain, but should be at least 88×28 px and gain a visible `Trend` tooltip/focus label.

## Night phase information architecture

Keep the established sequence:

```text
Days 1–4: Process award → Night/Strategic planning → Morning briefing
Day 5:    Process award → Quarterly review
```

Improve it by making each step answer one question:

1. **Process award:** What permanent-for-this-run improvement do I want?
2. **Operating close:** What happened, and what single overnight response should I choose?
3. **Morning briefing:** What changed, and what matters today?

The current progress pips should remain. Add short future labels on desktop and keep the compact `2 of 3` treatment on smaller layouts.

## Night 1: Process award

### Current strengths

- Run Process Points and cash are clearly distinguished.
- Upgrade pips communicate a three-rank system.
- Employee development shows price escalation and caps.
- The screen offers a clear “keep point” exit.

### Current problems

- Two dense catalogs are placed side by side, requiring broad scanning.
- Every upgrade row repeats `Next pip` and `Invest 1`.
- Employee cards repeat three full purchase buttons, producing twelve equal blue calls to action.
- The new sixth process option increases left-column density.
- Benefits are legible only at very small sizes.

### Approved and implemented alternative

Use a simultaneous decision workspace with a persistent budget rail:

```text
PROCESS AWARD  1 of 3                         END OF SHIFT ☾
Invest today’s reward                     1 point · $450 cash

RUN SYSTEMS                               &     EMPLOYEE DEVELOPMENT
Elastic Intake          Inbox +1 [Invest]       Intern
Parallel Processing     Active +1 [Invest]      Junior Analyst
Revenue Assurance       Payout +5% [Invest]     Accountant
Restorative Controls    Recovery +3 [Invest]    Manager
Audit Dampening         Audit −5 [Invest]
Grease the Wheels       Confidence +5 [Invest]

                    [Keep point and continue]
```

Run Systems and Employee Development stay visible together in equal panels because they use separate currencies; the `&` marker communicates that both shops are available during the same reward step. Each system row contains the current rank, next effect, and purchase action, so a separate detail panel would only repeat information. At narrower widths the columns reflow while retaining the same hierarchy.

## Night 2: Operating close and planning

### Current strengths

- The screen already distinguishes read-only and choice panels.
- Company score summarizes four metrics.
- Cash movement is more understandable than a raw ledger.
- Recruitment visually resembles employee cards.

### Current problems

- The decision panel is the third column even though it is the only required action.
- The cash chart and ledger repeat opening/closing values.
- Recruitment cards become extremely small to fit three across.
- Ordinary night actions and hiring are visually presented as adjacent, but the fact that either consumes the single choice is buried in prose.

### Recommended alternative

Lead with the decision, then supporting evidence:

```text
OPERATING CLOSE  2 of 3                           Day 2 closed at $391

CHOOSE ONE OVERNIGHT ACTION
[Pizza Party]       [Compliance Training]       [Quiet Recovery]
 +8 morale           −5 audit risk               free · −8 stress
 −$35                −$25                        −1 confidence

or RECRUIT ONE EMPLOYEE
[Administrative Intern] [Junior Analyst] [Accountant]

────────────────────────────────────────────────────────────
WHY THIS MATTERS
Company 77 / Strong        Cash $391 (−$59)
Morale 72 (+2)             Audit 45% (−5)
Confidence 77 (+2)         Payroll tomorrow $109
[View full cash ledger] [View policy changes]
```

- State `Choose exactly one: action or recruit` immediately above the choices.
- Make the selected option expand with a precise before/after preview.
- Collapse the detailed ledger by default.
- On Day 3, change the heading and visual stamp to `MANDATORY STRATEGIC REVIEW`; do not show ordinary overnight options.

Current implementation checkpoint: the existing content remains intact in three equal-width panels. Within Night Planning, the three operating actions and the overnight recruitment section are separated by a circular `OR` divider. Recruit cards retain their boosted mechanics but display a `NEW HIRE` token.

## Night 3: Morning briefing

### Current strengths

- Read-only labeling avoids false affordances.
- Opening position, operating brief, and policies are logically grouped.
- The final `Begin Day` action is clear.

### Current problems

- It repeats much of the previous screen’s company dashboard.
- The operating brief is a list of numbers without prioritization.
- All active policies receive the same weight, even if unchanged.
- A large modal interrupts the transition for information that could be summarized in a shorter memo.

### Recommended alternative

Use a concise “what changed / what matters” briefing:

```text
MORNING BRIEF  3 of 3                              DAY 2
Build a cadence

CHANGED OVERNIGHT
Morale +8 · Audit −0 · Confidence −0 · Cash −$35

TODAY’S OPERATING PLAN
5 inbox slots open · 3 active slots · Payroll $109
Primary risk: Financial Authorization 4.2 affects Expense Reports

POLICY CHANGES
NEW  Minimum Batch — Data batches require at least 60 records.
UNCHANGED 2 other policies                                  [View all]

[Begin Day 2]
```

- Show deltas, not a complete duplicate dashboard.
- Put new/removed policies before unchanged policies.
- Link to the full Progress and Policies views for detail.

Current implementation checkpoint: the duplicate opening-position dashboard has been replaced by `Changes overnight`, calculated from the prior closing snapshot. Operating-brief and policy content remains intact in the other two equal-width read-only panels. The main title is `Morning Brief`, with the day-specific briefing sentence beneath it.

## Today’s active policies

Current policies are clear prose blocks, but their presentation does not help the player connect them to cards.

Recommended policy row:

```text
FA-4.2  Financial Authorization                APPLIES TO: Expense
Amounts over $300 require Manager signature.   2 current items
```

Rules:

- Add a short code.
- Show the affected task/document type.
- Highlight `NEW`, `CHANGED`, and policy-origin penalties.
- On selection, highlight affected cards on the board.
- In a Review packet, show only applicable policies first; keep `All active policies` behind a link.
- Avoid using policy blue merely as decoration; reserve it for cross-linking affected work.

## Workflow stack states

### Awaiting assignment

- Header: `NEEDS EMPLOYEE`.
- Show task, required inputs, and best available employee shortcut.
- Primary action: `Choose employee`.

### Awaiting resource

- Header: `NEEDS RECEIPT` or the exact missing input.
- Show available matching resource and location.
- Primary action: `Add Receipt from Inbox`.

### Ready to start

- Header: `READY`.
- Show worker, task, inputs, expected time, accuracy, payout.
- If starting is automatic, say `Starts when assembled`; do not present a fake action.

### Processing

- Header: `WORKING`.
- Show ETA first, then accuracy and payout.
- Use a labeled `Intervene +X stress` button.
- Keep stress rhythm visible without overwhelming the progress bar.

### Completed, employee awaiting release

- Header: `AWAITING REASSIGNMENT`.
- Use amber, not healthy green.
- Primary action: `Return to Backlog`.
- State the idle stress rate in one line.

### Awaiting review

- Header: `REVIEW REQUIRED`.
- Show payout at stake, waiting time, and affected policies.
- Primary action: `Open review packet`.

## End-run screens

### Successful quarterly review

Current three-column structure is complete, but the player must scan dashboard, chart, prose, and upgrade grid to discover the outcome.

Recommended order:

1. verdict and run grade,
2. rewards earned,
3. three decisive trends,
4. permanent upgrade decision,
5. full detailed report.

```text
QUARTERLY REVIEW                                  RATING: STRONG
$528 close · 14 completed · 12 correct rulings

EARNED   +34 XP   +1 Compliance Token

Cash       ▁▃▅▆█  +$78
Morale     ▅▆▆▇▇  +9
Audit      ▆▅▄▃▂  −25

RECOMMENDED PERMANENT UPGRADE
Document Retention · 25 XP                         [Unlock]

[Review full report]                     [Start next quarter]
```

### Terminated run

The implemented terminated screen uses the compact incident-report format rather than the earlier wide red dashboard. Its navy report strip, off-white paper, binder hardware, red serif headline, translucent `TERMINATED` stamp, and shield-shaped cause emblem keep the mock-corporate document voice without resembling a game-over panel.

Current behavior:

- The exact terminal system and threshold lead the report.
- `What you keep` immediately follows with Compliance Tokens, awarded Process XP and its award-time wallet balance, and retained permanent upgrades.
- `What drove the failure` ranks only the three highest-impact negative drivers; zero-impact and positive ledger entries cannot displace them.
- A cause-specific `Key lesson` summarizes the terminal pattern.
- `Review incidents` and `Review upgrades` expand their complete functional sections in place. Both remain collapsed and visually absent until requested.
- `Start a new run` remains the primary next step.
- A deterministic run-log reference and quiet footer close the report.

Recommended hierarchy:

```text
RUN TERMINATED
RUN TERMINATED · DAY 2
Board Confidence Lost

BOARD CONFIDENCE
0
THE RUN ENDS AT 0 CONFIDENCE.

WHAT YOU KEEP
47 Compliance Tokens | +0 XP | 2 upgrades retained

WHAT DROVE THE FAILURE (by impact)
1 Expired tasks ........ 10
2 Payroll paid ......... −$233
3 Other cash ........... −$66

KEY LESSON
Confidence reached zero after accumulated rulings and losses.

[Review incidents] [Review upgrades] [Start a new run]
```

## Secondary screens

### Welcome / resume

- Keep the appointment memo voice.
- Replace the paragraph-heavy first objective with a three-step mini-diagram: `Task → Employee → Required input`.
- Show `Continue autosave` as the primary action when a save exists; `Start new appointment` is secondary and destructive.

### Settings

- Group settings into `Display`, `Motion`, and `Data`.
- Add one-line consequences beneath each toggle.
- Separate `Reset all data` behind a danger section and confirmation phrase.
- Consider a `UI scale` choice instead of relying only on Compact mode.

### Paused

- Keep the veil and explicit clock suspension.
- Add `Resume` as a real button in the center card.
- Leave the selected card faintly visible so the player retains context.

### Toasts and incidents

- Ordinary confirmation: short neutral toast.
- Recoverable warning: amber toast with one action.
- Harmful event: red incident notice with cause and exact consequence.
- Security reward: visually distinct but not the same red vocabulary as punishment.
- Never place transient feedback over the selection drawer’s primary action.

## Proposed responsive behavior

### 1440 px and wider

- Full five-lane board.
- Selection drawer open at 340 px.
- Two-tier header.
- Night screens may use two or three columns when body text remains 11 px or larger.

### 900–1439 px

- Selection drawer overlays the right side and closes after a successful move.
- Done collapses to a narrow ledger rail.
- Board has visible horizontal edge fades and trackpad/mouse-wheel support.
- Night decisions use tabs or two columns.

### 560–899 px

- Board stays a horizontal lane carousel with one and a half lanes visible.
- Sticky lane switcher: `Inbox 5`, `Backlog 4`, `Active 2`, `Review 1`, `Done 3`.
- Inspector becomes a bottom sheet.
- Top metrics become a horizontally scrollable status strip.

### Below 560 px

- One lane at a time.
- No card text below 11 px.
- Tap card → bottom-sheet detail → choose action.
- Night screens become a vertical sequence with sticky budget and Continue controls.

## Accessibility requirements

- Maintain full keyboard alternatives for every drag operation.
- Ensure the visible focus ring is not confused with stack selection or workflow state.
- Minimum 32×32 px target for secondary controls; 40×40 px preferred on touch.
- Never rely on red/green alone; pair every state with text or icon.
- Use `aria-live` sparingly for arrivals and major state changes; avoid announcing every timer tick.
- Tooltip-only rules must also be reachable by focus and available in the Inspector.
- Respect reduced motion for card transfers, stamps, pulsing sweet spots, and modal transitions.
- Add a UI scale setting: `Comfortable`, `Compact`, `Large`.
- Preserve logical heading order inside modals and move focus to the modal title.

## Recommended design tokens

```text
Type
  Body                13 / 1.45
  Card body           12 / 1.4
  Metadata            11 / 1.3
  Eyebrow              10 / 1.2, tracking .06em
  Card/lane title      13–15
  Page title           24–28

Space
  4, 8, 12, 16, 24, 32
  Card padding         12
  Lane gap              8
  Major panel gap      16

Controls
  Secondary height    32
  Primary height      36–40
  Touch height        40

Surfaces
  Global shell        deep navy
  Work surface        cool office gray
  Cards               warm white
  Selected            blue edge + check tab
  Warning             amber edge + label
  Danger              red edge + label
  Success             green stamp/edge
  Resource/reference  restrained purple
```

## Prioritized redesign roadmap

### P0 — Legibility and action safety

- Raise all critical text to 11 px minimum and body/action copy to 12–13 px.
- Make button targets at least 32 px tall.
- Move `End day early` out of the Inspector tool row.
- Give idle workers in In Progress a distinct amber state.
- Make Intervene an explicit button with its stress cost.
- Ensure all destructive actions have separation and consequence copy.

### P1 — Workday shell

- Build the collapsible selection drawer.
- Weight lane widths and collapse Done into a ledger.
- Split Backlog into People and Staged files.
- Introduce the simplified card anatomy.
- Change the notice bar into a focused Next Action strip.

### P2 — Review and night decisions

- Build the field-versus-policy Review packet.
- Convert Process award to catalog + selected-detail or tabs.
- Lead Operating close with the single overnight decision.
- Turn Morning briefing into a delta-focused memo.

### P3 — Corporate polish and responsive modes

- Add restrained memo/stamp/ledger motifs.
- Add UI scaling.
- Improve tablet lane navigation and bottom-sheet Inspector.
- Refine successful and failed run-end storytelling.

## Acceptance criteria for an implementation pass

- No meaningful gameplay fact is rendered below 10 px; default action/body content is at least 12 px.
- At 1280×720, the selected action and at least four full working lanes are usable without compressed 6–8 px text.
- At 900 px and below, the Inspector does not permanently consume board width.
- Each selected object has one visually dominant next action.
- In Progress clearly distinguishes incomplete, processing, and awaiting-reassignment states.
- Review shows document facts against applicable policy in one visual comparison.
- Night screens preserve the three-stage logic and make the single required decision unmistakable.
- The visual language still reads as mock stock corporate, but status and interaction outrank decoration.
- Drag, keyboard actions, timers, employee rhythms, autosave, and modal sequencing remain mechanically unchanged unless a redesign explicitly calls for behavior changes.

## Recommended first implementation slice

Start with the workday shell and active workflow card:

1. establish 12–13 px default card/action typography,
2. make the Inspector collapsible,
3. weight the lane widths,
4. simplify base task/resource cards,
5. redesign active and awaiting-reassignment work orders,
6. move `End day` beside time/pause,
7. validate at 1920×1080, 1440×900, 1280×720, 900×700, and 390×844.

This slice produces the largest usability gain while touching the fewest game systems. The night screens can then reuse the same typography, action hierarchy, and corporate document language.
