# BUSYWORK Mechanics and Payout Retune

Status: implemented in `index.html` on 2026-07-22. This document is the balance contract for the current browser build.

## 2026-07-29 Audit, Roadmap, and Permanent Progression Amendment

This amendment is authoritative where older audit, overnight, Process Point, XP-award, Compliance Token, and permanent-upgrade sections below conflict.

- A corporate org-chart/Gantt roadmap now controls the quarter. The player chooses one of three starting workstreams, then selects only a nearby connected destination after each daily progression receipt. Every route converges on the Day 5 Quarterly Board Review.
- Roadmap bars set real day length: short is 3 minutes, medium is 5 minutes, and long/boss is 10 minutes. Completing a 10-minute day grants one persistent Compliance Token. Vendor Exception creates an Accounting-focused intake day and one non-Manager callout.
- The old general overnight activity/development/hire catalog is not in the normal playable flow. Each route inspector separates immediate day setup from its end-of-day payout or gated reward menu. Margin Review inserts Night Planning, Development Workshop inserts the Cash stat shop, and Talent Pipeline inserts one paid Juiced recruit choice. Temp Desk still adds one random specialist immediately and removes that worker after the workday.
- Every survived day close immediately banks +1 persistent XP and awards +1 run-only Talent Point. The guaranteed Talent Tree is overnight phase 1 and spends Talent Points on five three-rank Run Process Upgrade branches. Persistent XP cannot be spent nightly; successful Day 5 Quarterly Review unlocks the permanent upgrade office.
- Permanent upgrades are Spam Scanner, Process Lane Annex, Inbox Expansion, Spam Intake Filter, Review Annex, Six-Tab Folders, and Manager Triage Protocol. Their visible effects cover full-card/covered-token spam glitches, +1 In Progress, +2 Inbox, junk intake reduced from 30% to 20%, +3 Review, folder capacity 5→6, and automatic morning stress relief for the most stressed worker.
- Run Process Upgrades are Elastic Intake (+1/+2/+3 Inbox), Parallel Processing (+1/+2/+3 In Progress), Revenue Assurance (+5/+10/+15% approved payouts), Restorative Controls (+3/+6/+9 overnight recovery), and Audit Dampening (−5/−10/−15 Audit Chance points). Ranks and unspent Talent Points expire at run end.
- Compliance Tokens are never consumed by audits. Each held token subtracts 5 percentage points from effective nightly Audit Chance and offsets one open Liability before finding chance and Severity are calculated. The header and Progress panel show both reductions and the held balance.
- Liability is the number of open policy violations. `effective Liability = max(0, open Liability − held Compliance Tokens)` directly selects Audit Severity, capped at 5. The current consequences are Consent Decree, Cash Fine, Compliance Check, Bad Vibes, and Termination.
- `Approve Purchase Request` is an Accounting task using Accountant + Receipt. Its Review document shows Item Description, Reason, Country of Origin, and Compliance Paperwork Filed; Apple ecosystem, valid business purpose, embargo, and filing controls always apply.
- XP, Compliance Tokens, and purchased permanent upgrades persist in device-local browser `localStorage` until the player confirms **Reset all data** in Settings. Talent Points, Run Process ranks, trained employee stats, and Juiced hires are run-only. This is cookie-like on-device persistence, but the implementation does not use HTTP cookies.

| Liability / Severity | Name | Failed-audit consequence |
|---:|---|---|
| 1 | Consent Decree | Add 2 Active Policies the next day. |
| 2 | Cash Fine | Cash −$50, Audit Chance +10 percentage points, and Board Confidence −5. |
| 3 | Compliance Check | Distribute four mandatory, unpaid Compliance Check task cards at seeded positions throughout the next day. Each one that expires or is deleted adds +1 Liability. |
| 4 | Bad Vibes | The next day has Accuracy −10 percentage points and speed ×0.9; Staff Morale −20 and Board Confidence −5 apply immediately. |
| 5+ | Termination | End the run immediately. |

## Goals

- Make Cash, Morale, Audit Chance, and Confidence interact instead of behaving as isolated meters.
- Make every Review ruling legible as a risk/reward choice.
- Reward keeping workers near their preferred workload without removing stress management.
- Turn liabilities into delayed, escalating audit danger.
- Surface harmful events immediately with flavor text and exact consequences.
- Make worker status and card type readable without opening multiple panels.

## Resource model

### Cash

Cash is cash on hand. During the day it changes through recognized task income and discretionary actions. At close, payroll and operating expenses are deducted. Cash costs attached to the selected roadmap destination are also applied; the retired overnight activity/hire catalog is not part of normal play.

- Starting Cash: `$450`.
- Each positive-revenue task instance rolls a visible contract rate: 5% Windfall at `5×` its task-type base reward, 8% Premium at `2×`, 25% Low Fee at `0.2×`, and 62% ordinary work split across `0.75×`, `0.9×`, and `1×`. The expected multiplier remains approximately `1×`, so variance changes priorities without broadly inflating the economy.
- Eligible arrivals have an independent 8% chance to receive Juiced scope. Juiced cards never roll the LOW FEE tier; they multiply a standard, premium, or windfall quote by `5×`, require a task-specific second resource, consume both inputs, and use a recipe lasting `1.35×` the standard duration. Existing 1.75× Juiced rewards and saved JUICED/LOW contracts migrate to the current 5× quote. Every completed workflow consumes every supplied resource; correction requires fresh inputs. The opening tutorial remains standard; audit-generated Regulatory Response arrivals use the same rare roll.
- Approving work recognizes `quoted contract payout × (0.8 + 0.4 × Confidence / 100)`, rounded to a whole dollar. The quoted payout follows the task through processing, Review, and any requested rework.
- Regulatory Response work pays `$0`.
- Death condition: Cash at or below `$0` after the operating close.

### Morale

Morale is derived from the workforce rather than being a detached health bar:

`Morale = clamp(100 - average worker stress + average sweet-spot contribution + event modifier, 0, 100)`

Each worker contributes either `+10` or `+0` to the averaged sweet-spot term depending on whether their current stress is inside a role-specific sweet-spot band. Pizza, firing, burnout, death, and roadmap events change the persistent event modifier.

Morale modifies processing speed:

| Morale | Productivity |
| --- | --- |
| 80–100 | +10% |
| 65–79 | +5% |
| 40–64 | no modifier |
| 20–39 | -15% |
| 0–19 | -30% |

Morale is not a direct run-ending condition. Its danger is slower throughput, more deadline misses, and the resulting Cash/Confidence pressure.

### Stress and productivity

- Stress is an employee-specific `0–100` meter. New employee cards currently start at `0`, not 15.
- Working, covering outside a role, interventions, inaccurate approvals, correction/rejection mistakes, and waiting taskless in In Progress add stress. Backlog time, correct approvals, Manager check-ins, and some roadmap rewards relieve it.
- Average workflow stress slows processing by 10% at 50–79 and 30% at 80+. Resilience changes how quickly positive stress is gained.
- Stress is separate from preferred workload. Work/idle share changes Rhythm, while current stress determines whether the employee is in a sweet spot.
- The Intern, Junior Analyst, and Accountant use a 10–20% stress sweet spot. The Manager uses two bands, 0–5% and 50–75%.
- Any valid sweet spot grants processing speed +15%, Accuracy +8 percentage points, and a +10 contribution to the averaged Morale formula. The Manager does not receive a separate flat +10 Morale rule beyond this same contribution.

### Audit Chance

- A run starts at `50%` Audit Chance.
- The audit roll happens every night.
- Each held Compliance Token subtracts 5 percentage points from the nightly roll and is not consumed.
- Operational penalties normally do not directly raise Audit Chance. They create Liability or alter a future consequence; the Severity-2 Cash Fine is the explicit exception and adds 10 points.
- Every automatic Inbox overflow removes `6` Confidence, leaves Audit Chance and Liability unchanged, and also applies any normal expiration consequence on the displaced card.
- Accurate approvals remove `1` point; Compliance Training removes `8`.
- If an audit occurs, finding chance is `open Liability ÷ elapsed days`, capped at 100%.
- When a violation is found, the open Liability count directly selects Audit Severity 1–5.

### Confidence

Confidence is **Trust of the Board**:

- It scales recognized Cash payouts from 80% at 0 Confidence to 120% at 100.
- Accurate approvals and clean audits usually raise it. Failed audits, incorrect rulings, missed work, staffing events, and roadmap tradeoffs can lower it.
- Worker sweet-spot time affects Morale, not Confidence directly.
- Confidence no longer changes the audit discovery roll or the five-band Severity result.

Death condition: Confidence at or below `0`.

## Worker sweet spot

A worker enters the sweet spot when their current stress is inside a role-specific band. The Intern, Junior Analyst, and Accountant use 10–20% stress. The Manager has two distinct valid bands: 0–5% and 50–75%.

While active:

- Processing speed: `+15%`.
- Accuracy forecast: `+8` percentage points.
- Morale contribution: `+10` while inside either valid band.
- Visual feedback: green border, glow, pulse, and `SWEET SPOT` badge.

Preferred workload remains a separate daily working-time target that affects Stress accumulation and Backlog recovery. A company-learning burnout outcome now improves overnight Stress recovery by two points, up to six additional points for the run.

## Review choices

| Choice | Hit | Miss |
| --- | --- | --- |
| Approve | Accurate work pays Confidence-scaled revenue, Confidence +2, Audit Chance -1 | Revenue still pays, but +1 Liability raises the direct Severity band; producer stress and Morale modifier -1 |
| Request correction | Inaccurate work returns to Backlog as rework with a 40% shorter deadline; producer Stress +10 | Accurate work is unnecessarily reworked; producer Stress +10 and a red consequence popup |
| Reject | Inaccurate/unsalvageable work is destroyed with no reward; producer Stress +10 | Accurate work is destroyed; +1 Liability, Confidence −2, and producer Stress +10 |
| Escalate | Work moves to Done regardless of accuracy | Confidence −5 and Audit Chance −5 points, with no task revenue |

Rulings remain final except Request correction, which creates a new assignable rework task.

Workflow resources are consumed when production finishes, before the output reaches Review. Rejecting or escalating a document therefore does not return its task or resource cards. Approving inaccurate work does not add the proposed +5 Audit Chance penalty; Audit Chance is unchanged. Escalate costs 5 Confidence and directly subtracts 5 points from Audit Chance.

Regulatory Response corrections remain Regulatory Response tasks and retain their `$0` reward. They never convert into paid Data Entry work.

## Active policy pool

Each day samples a deterministic, conflict-free subset from 34 policies: three on Day 1, four on Days 2–4, and five on Day 5 before failed-audit additions. Four procurement controls always apply to Approve Purchase Request output in addition to the sampled daily set.

| Policy | Requirement | First day | Conflict group |
| --- | --- | ---: | --- |
| Financial Authorization 4.2 | Expenses over $300 require a Manager signature. | 1 | — |
| Evidence Standard 2.1 | Reimbursements require an attached Receipt. | 1 | — |
| Client Suspension Notice | Client C-882 may not be released. | 1 | — |
| Billing Cap A | Invoices may not exceed $2,500. | 1 | Invoice cap |
| Billing Cap B | Invoices may not exceed $3,500. | 2 | Invoice cap |
| Standard Terms | Invoices require Net 30 terms. | 1 | Terms |
| Accelerated Terms | Invoices require Net 15 terms. | 3 | Terms |
| Release Authority | Invoices must be authorized. | 1 | — |
| Source Registry | Routine data must originate from Operations Intake. | 1 | Data source |
| Variance Control | Routine data must show no material variance. | 1 | — |
| Minimum Batch | Routine output requires at least 60 records. | 1 | Record count |
| Maximum Batch | Routine output may not exceed 110 records. | 2 | Record count |
| Control Parity | Document control IDs must end in an even digit. | 3 | — |
| Expense Ceiling | Reimbursements may not exceed $450. | 1 | Expense cap |
| Tight Expense Ceiling | Reimbursements may not exceed $350. | 4 | Expense cap |
| Preferred Client Day | Only client C-321 may be released. | 4 | Client rule |
| Client Hold | Client C-555 may not be released. | 2 | Client rule |
| Fatigue Review | Output produced above 75 stress requires correction. | 3 | Fatigue limit |
| Role Boundary Review | Coverage work requires a Manager signature. | 3 | — |
| Whole-Dollar Filing | Financial amounts must use whole dollars. | 2 | — |
| Materiality Threshold | Reimbursements must be at least $125. | 2 | — |
| Spend Harmonization Grid | Reimbursements must use $25 increments. | 3 | — |
| Revenue Materiality Floor | Invoices must be at least $1,000. | 2 | — |
| Commercial Packaging Standard | Invoices must use $100 increments. | 3 | — |
| Strategic Liquidity Window | Invoices require Net 45 terms. | 4 | Terms |
| Enterprise Batch Floor | Routine outputs require at least 75 records. | 3 | Record count |
| Five-Point Normalization | Routine record totals must use increments of five. | 2 | — |
| Revenue Operations Mandate | Routine data must originate from Revenue Operations. | 4 | Data source |
| Cognitive Load Ceiling | Output produced above 60 stress requires correction. | 4 | Fatigue limit |
| Core-Competency Utilization | Coverage-produced output is noncompliant. | 3 | — |
| Approved Device Ecosystem | Purchase requests must specify an Apple item. | Always for purchase requests | — |
| Business Purpose Validation | Purchase requests require a valid business reason rather than suspected personal use or redundant multitasking. | Always for purchase requests | — |
| Embargoed-Origin Prohibition | Purchase requests may not originate in an embargoed country. | Always for purchase requests | — |
| Procurement Filing Standard | Purchase requests require filed compliance paperwork. | Always for purchase requests | — |

Mutually exclusive caps, terms, sources, record ranges, client rules, and fatigue limits share exclusion groups, so no daily sample demands contradictory values.

## Expanded work request pool

Ordinary arrivals also include Stakeholder Alignment Memo (Spreadsheet), Revenue Enablement Packet (Client Data), Governance Recalibration (Receipt; internal template ID remains `spend_governance_calibration` for save compatibility), and Approve Purchase Request (Receipt). Purchase requests belong to the Accounting intake pool and use Accountant specialist work plus Manager emergency coverage. Their Verified Expense output records Item Description, Reason, Country of Origin, and whether Compliance Paperwork was Filed. The game currently defines 52 standard and Juiced workflows; completed documents record the originating request and scope so correction returns the same task type and standard/Juiced requirement rather than generic legacy work.

Compliance Check is conditional mandatory work rather than an ordinary random arrival. It pays $0, uses Intern + Spreadsheet for its specialist recipe, and allows emergency Manager coverage. Severity 3 inserts four checks at seeded positions throughout the following day's first arrival bag. Missing the 75-second deadline or deleting the card adds one Liability.

### Employee stat shop and Manager strategy

Employee development appears only after completing a Development Workshop roadmap location. Accuracy, Speed, and Resilience can each be raised one run-persistent base-stat pip at a time to the six-pip cap in a one-night shop after the Talent Tree. Each purchase spends Cash using `stat base + current stat × $6 + total prior Workshop pips on that employee × $4`, with bases of $18, $16, and $14 respectively. Accuracy adds four ordinary chance points per pip and guarantees worker-caused compliance at six outside Manager coverage except during Bad Vibes; Speed uses `0.60× / 0.80× / 1.00× / 1.10× / 1.20× / 1.35×`; Resilience uses `1.75× / 1.35× / 1.00× / 1.00× / 1.00× / 0.50×` work-stress multipliers. Manager Accuracy above baseline adds another 12 emergency-coverage points per pip, Manager Speed above baseline adds another 5% multiplicative speed per pip, and Manager Resilience scales check-in healing and team-wide task stakes.

- Accuracy is used when generating the actual work product, not only in the forecast. Six Accuracy guarantees compliant specialist work. Manager emergency coverage retains its severe −70 base penalty, but each Accuracy pip above the Manager’s four-pip baseline adds another 12 points to his real and displayed coverage chance.
- Speed continues to use the normal stat curve. Each Manager Speed pip above his two-pip baseline also adds 5% multiplicative speed, allowing a fully developed Manager to work through the 2.25× emergency-coverage duration.
- Resilience reduces work stress for every employee. Manager Resilience additionally scales private check-ins from 20 stress relief at baseline to 32 at six pips.
- When a Manager-produced document reaches Review, its active-policy compliance is resolved immediately. A compliant result relieves every current employee by `6 + 2 × Resilience + 2 × pips above baseline` stress, spanning 8–26 depending on his seeded stat (10 at baseline). A noncompliant or junk-tainted result stresses every current employee by `max(6, 20 − 2 × Resilience)`, spanning 18–8 (16 at baseline).

The Staff shop shows current Cash, each worker’s purchased investment total, live stat effects, escalating next prices, and the Manager’s current whole-team win/failure stakes. These upgrades persist in the run save.

## Liabilities and audits

Liability is the number of open policy-violation records. It is created by incorrect Review rulings and other explicitly documented violations, including approving inaccurate work, rejecting accurate work, deleting a legitimate resource, deleting or missing a Compliance Check, and route effects such as Stage a Demo.

Every night first rolls the effective Audit Chance:

`effective Audit Chance = max(0, base Audit Chance − 5 × held Compliance Tokens)`

`effective Liability = max(0, open Liability − held Compliance Tokens)`

Compliance Tokens are earned by claiming a BUSYWORK-IT reward after deleting five JUNK SPAM cards in one day, visiting Report Defect/Policy Sweep, and completing a 10-minute day. They persist between runs, reduce both Audit Chance and effective Liability, and are not spent or consumed by an audit.

If the audit occurs and at least one Liability is open, its count directly selects the consequence:

| Severity | Consequence | Applied result |
|---:|---|---|
| 1 | Consent Decree | Queue +2 Active Policies for the next day. |
| 2 | Cash Fine | Cash −$50, base Audit Chance +10 points, Board Confidence −5. |
| 3 | Compliance Check | Queue four $0 mandatory checks, seeded throughout the next day. Every check that expires or is deleted creates +1 Liability. |
| 4 | Bad Vibes | Apply Staff Morale −20 and Board Confidence −5, then apply −10 percentage points Accuracy and a 0.9× speed multiplier for the next workday. |
| 5+ | Termination | Set Critical Audit Failure and end the run. |

Grease the Wheels removes one open Liability and costs 5 Board Confidence. Escalate costs 5 Confidence and removes 5 points of Audit Chance. Regulatory Response remains a conditional task card, while the five-tier audit system generates Compliance Checks at Severity 3.

## Burnout outcomes

Crossing a worker's burnout threshold always applies Confidence -8 and Morale modifier -6. The follow-up is randomly selected:

- Leave plus a permanent +1 Accuracy, Speed, or Resilience increase.
- Leave plus a company-wide +2 overnight stress-recovery improvement.
- Employee quits permanently.
- Employee dies, permanently removing the worker and applying an additional Morale modifier -12.

The deterministic embedded test mode forces the leave/stat-growth branch so logic tests remain stable.

## Pacing and player agency

Automatic Inbox arrivals remain on the day-scaled schedule, but the current countdown is always visible in the action bar. While the clock is running and the Inbox has capacity, the player may use **Pull next item** to deliver the next seeded arrival immediately. Pulling an item resets the automatic-arrival clock; a full Inbox disables the action instead of silently displacing existing work. Every deterministic ten-card bag contains exactly three distinct legitimate tasks, four resources (one of each plus one seeded duplicate), and three distinct junk cards. This yields 30% tasks, 40% resources, and 30% junk and applies to every refill, not only the first ten pulls.

Card faces use a permanent semantic visual language: blue avatar circles for employees (with executive brown reserved for the Manager), amber target circles for tasks, purple diamonds for resources, and green folded-page marks for documents. The name-specific code square repeats that type color instead of falling back to neutral grey. Small colored pips communicate secondary attributes such as Premium, Windfall, Low Fee, and Juiced status. Juiced tasks use a heavier double edge, layered surface, stronger depth, and lightning pip without replacing their underlying type color. Matching task/resource piles use a filing-folder face, raised tab, `Folder` label, and a four-slot rectangular covered-card rail rather than layered hidden-card silhouettes. The folder face itself represents the active top card. Selecting a card does not add compatibility accents to other cards; only the one-time first-workflow guide may sparkle valid next actions. Task flavor text naturally names the resource needed to begin work and does not add generic consumption boilerplate. Standalone employee cards expose compact Accuracy, Speed, and Resilience values and meters without requiring hover. Invalid-drop feedback remains available during a drag, and lane-gap targets allow cards or whole stacks to be reordered without restarting unrelated timers or employee Stress state.

Task-disguised junk can be assigned using the same worker and resource flow as the legitimate task it imitates. Matching resource-disguised junk is deliberately prioritized by the Inspector shortcut and also starts work. Both forms run for the normal workflow duration, accept interventions, and produce a document in Review with a guaranteed Source Integrity Failure. Completion adds `10` employee Stress and leaves the employee in In Progress. Audit Chance and Liability remain unchanged until the player rules on the document. Fake tasks carry no collectible value; legitimate tasks contaminated by a junk resource retain their quote, so approving the invalid output creates the same immediate-revenue-versus-liability trap as other bad work.

## End-of-day progression and corporate roadmap

Reaching each survived daily close immediately banks +1 persistent XP and awards +1 run-only Talent Point. The guaranteed phase-1 Talent Tree spends Talent Points on Run Process Upgrades. A completed 10-minute shift also banks +1 Compliance Token. The receipt reports these awards and balances before any completed-location reward and the roadmap.

The roadmap replaces the old overnight activity, employee-development, and new-hire menus. A new quarter starts at one of three visible workstreams. At later daily closes, the player may choose only the same or an adjacent workstream, and every path converges on the Day 5 Quarterly Board Review. The selected node determines the next day's real duration—3, 5, or 10 minutes—and any special intake, staffing event, cost, or reward.

- Completing an Employee Development Workshop node inserts a separate Cash-funded employee stat shop after that night's Talent Tree.
- Margin Review inserts one Night Planning choice; Talent Pipeline inserts one paid Juiced recruit choice; Temp Desk provides one random specialist for one day.
- Other route rewards include targeted stress reset, Pizza Party, Report Defect, Grease the Wheels, Stage a Demo, Whistleblower, Casual Friday, and Safety Seminar.
- Permanent XP purchases are available only after reaching a successful Day 5 Quarterly Review. Early failure keeps earned XP, Compliance Tokens, and existing permanent upgrades, but loses Talent Points and Run Process ranks.
- The permanent office uses 5-XP cost increments: Spam Scanner (5 XP), Process Lane Annex (10), Inbox Expansion (15; requires Spam Scanner), Spam Intake Filter (20; requires Spam Scanner), Review Annex (25; requires Process Lane Annex), Six-Tab Folders (30), and Manager Triage Protocol (35; requires Review Annex). A fresh five-day survivor reaches its first Quarterly Review with exactly 5 XP, making Spam Scanner the only affordable purchase.
- Persistent progress is stored in browser `localStorage` on that device until **Settings → Reset all data** is confirmed.

### Card stacking and deletion

- A normal stack may contain at most one employee, one task-like card, one document, and one resource card. Resource-disguised junk occupies that resource slot. The only two-resource exception is a Juiced task whose stack contains its exact required resource pair.
- Dragging a stack onto another stack combines the complete source stack only when the resulting stack respects those compatibility slots and the five-card limit. A compatible three-card stack can therefore still land on a compatible two- or one-card stack.
- Active jobs and locked workflow stacks cannot merge. Saved homogeneous stacks from earlier builds are split into compatible stacks when loaded.
- Only the physical top card in a stack advances its deadline. Every covered timer pauses at its exact remaining value and resumes when that card becomes the top card.
- A homogeneous pile renders as a filing folder only while it contains two or more cards. Its four-slot rail represents only covered cards, nearest to the top first; the active top card is already represented by the full-size folder face and never receives a duplicate token. Covered timers are labeled paused, and unused capacity appears as empty slots. Removing cards until one remains dissolves the folder into a normal single-card face. Mixed stacks continue to show hoverable/focusable rectangular tokens only for genuinely covered cards. Tokens use name-specific abbreviations, expose concise identity and state details, and can be clicked for inspection or dragged to pull that individual card out. Composite In Progress workflows suppress these tokens because their employee, task, and resources are already visible. Paused countdown, Juiced/low-value/glitch state, and employee status remain visually encoded.
- Staged resource chips can be dragged back out of an assignment. The board always rerenders from state after a drop, preventing a rejected resource from remaining visually over an employee card.
- Dragging a visible top card to empty lane space extracts that top card into a new stack; dragging a compatible stack onto another stack merges the complete source stack atomically. Dropping between lane items reorders the entire source stack in that lane, while dragging a specific populated token extracts only its represented card. Reordering does not recreate cards, cancel jobs, reset employee Stress state, or disturb unrelated countdowns.
- Completing a workflow consumes every staged resource, leaves its worker in In Progress for reassignment, and queues the work product in Review without changing the player's current card or panel selection.
- An employee left in In Progress without a task receives a five-second grace period, then gains stress at `0.04 + 0.001 × exposed seconds` per second, capped at `0.12` before the employee's Resilience multiplier. The continuous wait timer persists in saves, the card and Inspector show elapsed idle time plus current stress per minute, and the rate rises until capped. Assigning a task or moving the employee out of In Progress resets this pressure; Backlog then applies its ordinary recovery.
- The board trash target includes a trash-can icon and has an equivalent Inspector action.
- Deleting ordinary junk safely increments phishing-test progress. Deleting a valid resource costs `$8` and creates one severity-3 liability. Valid tasks use the seeded mild deletion outcomes above; Review documents retain the full expiration consequence. Firing an employee costs `$25` plus Morale and Confidence.

### First-workflow guidance

A newly created run records the opening Data Entry Request as its guided workflow. The task, every legitimate available employee that can perform or cover it, every legitimate Spreadsheet, the resulting partial workflow, and all matching Inspector actions receive the yellow sparkle treatment. Junk decoys never receive the guide even when they can contaminate the shortcut. The opening **Begin workday** button uses the same cue. The guide permanently retires when any first legitimate workflow starts, or when its recorded task is deleted or expires. Migrated runs without guide state do not gain the tutorial retroactively.

### Charts and labels

- Daily opening and closing Cash is presented as two bars connected by a line, making the size and direction of the daily change explicit.
- Quarterly Opening, Projection, and Actual series use a legend above the chart, right-aligned with the plot.
- Timeline endpoints use full labels such as `Day 1 00:00`, never the abbreviated `D1` form.
- Task-revenue telemetry changes only for recognized task payouts. It remains capped at 64 points per day and 320 persisted quarterly points.
- The small persistent task-revenue sparkline lives inside the Cash header tile and opens the detailed Progress panel.
- The company-status header uses five equal-height proportional columns. Day/Time, Morale, and Confidence center their short readouts; Cash and Audit remain left-aligned for secondary details. Cash separates its visible sparkline from the value block, while responsive layouts distribute every tile across the available row before allowing horizontal scrolling below 560 pixels. Header compaction changes spacing and alignment without reducing text size.
- The Recipes panel is a compact input-to-output network for the seven standard specialist routes: specialist, task, primary resource, duration, output document, and each task's possible contract range. Coverage and Juiced branches remain runtime rules rather than separate panel rows.
- Done uses a compact narrow lane so Inbox, Backlog, In Progress, and Review receive more horizontal space.
- At a 1920-pixel desktop viewport, all five lanes and the roughly 300-pixel Inspector remain visible simultaneously at compact card density. This large-monitor view is the baseline; Done alone is intentionally narrower than the reference layout.
- Completed work entering Review never changes the current card or panel selection. Review attention styling and the completion toast announce the arrival.

## Feedback and visual language

- Harmful rulings, deadline misses, burnout, termination, and audit failures use a red popup with an event title, flavor description, and explicit consequence line.
- A terminal Cash, Confidence, or Critical Audit outcome opens a compact incident-report postmortem instead of the ordinary Quarterly Review. A navy report strip, paper texture, red serif death reason, terminal stamp, binder hardware, and shield-shaped cause mark distinguish it from both the workday and the red incident popups. The exact terminal threshold leads the page, followed by three retained assets (Compliance Tokens, awarded XP with its award-time wallet balance, and permanent upgrades), the three highest-impact failure drivers, and a cause-specific key lesson. `Review incidents` and `Review upgrades` expand the complete chronological ledger and read-only permanent-upgrade record in place; `Start a new run` remains the primary action. Migrated saves without structured events recover relevant incidents from the company log.
- The top header carries an always-visible persistent wallet for XP and Compliance Tokens. The Progress panel distinguishes that wallet from run-only Talent Points, Cash-funded Workshop development, and location-gated planning/recruitment; it displays the exact Audit Chance, Audit Dampening, and Liability offsets. Tokens are not consumed. The permanent-upgrade preview is read-only until successful Day 5 Quarterly Review.
- Reaching the phishing-test threshold uses the same high-attention security notice and explains how to claim the reward.
- Every end-of-day decision flow starts with a compact stage tracker for progression receipt, roadmap selection, and morning briefing. Premature run-failure summaries do not show a continuation tracker.
- The progression receipt shows guaranteed +1 XP, +1 Talent Point, and any long-day Compliance Token. The run-only Talent Tree is phase 1; a completed Development Workshop, Margin Review, or Talent Pipeline then adds its Cash stat shop, Night Planning choice, or Juiced Recruitment choice before the roadmap decision. Morning Brief remains a read-only summary of the resulting state.
- Every standalone employee card permanently shows a compact ACC / SPD / RES strip with each base stat as a number out of six and a tiny fill meter. The same data remains available in the Inspector and Staff upgrade shop.
- Standalone employee cards keep their coping/status tags beside the worker name and compress workload state, stress percentage, target band, and current marker into a mini gauge beside the EMPLOYEE header. Full gauges remain available in workflows, the Inspector, and Staff upgrade shop.
- Smaller coarse-pointer devices receive a capability-based mobile board rather than a user-agent-specific fork. Portrait orientation opens a dismissible rotate-to-landscape prompt and pauses only an actively running workday until rotation or dismissal. Landscape retains all five horizontally scrollable lanes at compact widths and pins the full Inspector to the right; touch taps select, vertical card-body swipes scroll lanes, horizontal card movement drags, and card headers/folder/workflow handles support unrestricted drag-and-drop with enlarged insertion targets.
- Every visible card has a text type label plus a type-specific shape: employee square, task circle, resource diamond, document square. Distractions retain their disguise type so the mechanic is not spoiled.
- The currently selected card or workflow uses a three-pixel black dashed frame, offset isolation halo, and slight lift so Inspector context remains obvious over every card type and status treatment.
- Selecting a worker, task, or resource does not restyle other cards as compatible. The one-time opening guide may sparkle legitimate first-workflow options, while valid and invalid destination feedback appears only during an active drag.
- Ordinary junk cards use one of two deterministic glitch signatures—chromatic registration/scanline tearing or offset-code/clipped-edge printing—without displaying a junk label. Legitimate cards and the phishing reward notice do not receive these effects.
- Only a new run's valid opening workflow options receive the gold-and-blue sparkle aura; it follows the relevant Inspector buttons and disappears after the first legitimate workflow begins.
- The Audit header shows effective nightly Audit Chance, raw Liability, token offset, effective Liability, finding chance, and the five-level consequence rail. If an audit occurs, finding chance is effective Liability divided by elapsed days, capped at 100%.

## Implementation order

1. Completed: migrate run state and centralize the balance constants.
2. Completed: retune resource formulas, sweet spots, payouts, Review outcomes, and deaths.
3. Completed: replace the audit settlement and add Regulatory Response work.
4. Completed: add high-attention event feedback and card/worker visual identifiers.
5. Completed: update documentation and embedded logic tests.
6. Verification gate: all embedded tests pass; direct-file browser load has no console errors; sweet-spot, card labels, hover/focus, and popup layout remain readable at desktop and narrow widths.

## Current implementation architecture

The single-file build keeps the runtime dependency-free but separates high-risk transitions into named helpers:

- `createInitialRunState`, `migrateRunState`, `freshPhishingState`, and `freshCashTelemetry` own state creation and save compatibility.
- Workforce selectors calculate work share, stress sweet-spot bands, payout multiplier, stress, and derived Morale without duplicating formulas in UI code.
- Review uses separate approve, reject, correction, escalation, and finalization helpers. Unknown actions are ignored without mutating state.
- Audit calculation rolls effective Audit Chance, then rolls finding chance as effective Liability divided by elapsed days. A finding maps effective Liability directly to the five-band consequence.
- New-day preparation separates daily resets, employee recovery, and scheduled/regulatory arrivals.
- Legacy unused deck, discard, generic data state, and shuffle helpers were removed from the active runtime. Seeded arrival bags remain the authoritative draw model.

The embedded browser suite covers save migration, state invariants, ruling outcomes, task-contract payout tiers and persistence, Confidence modifiers, derived Morale, audit penalties, and the zero-revenue regulatory correction loop.

## Deployment

GitHub Pages publishes the repository root from `main` at `https://stevengglandry.github.io/busywork-llc/`. Publishing requires a clean verification pass, a direct commit to `main`, a push to `origin/main`, a successful Pages build, and a live HTTP 200 check for `index.html` and the documentation files.

## Acceptance criteria

- A new run displays 50% Audit Chance and cannot survive an operating close at 0 Cash.
- Confidence changes the displayed approval payout.
- Ordinary employee sweet spots are 10–20% stress; the Manager's two sweet spots are 0–5% and 50–75%.
- Task cards visibly distinguish 5× Windfall, 2× Premium, and 20% Low Fee contracts; the quoted amount survives processing and correction.
- Juiced task cards are visibly distinct, require exactly two task-specific resources, pay 5× their rolled quote, last 35% longer, spawn rarely, and retain their scope through correction.
- The three new ordinary task types use existing resources and document schemas, preserve their identity through correction, and expose both specialist and coverage routes.
- Completing a Development Workshop roadmap node inserts a one-night shop where Cash buys an Accuracy, Speed, or Resilience pip for a particular employee, never beyond six.
- A heavily upgraded Manager has materially better coverage accuracy, throughput, check-in healing, and whole-team success relief; every Manager task also applies its displayed team-wide success or failure stress outcome.
- Similar cards cannot share a stack; compatible multi-card stacks merge atomically up to five cards while locked work remains immovable.
- Deleting a valid resource deducts $8 and creates exactly one liability.
- A worker held in the sweet spot receives both speed and accuracy bonuses and visible flair.
- Incorrect approval and incorrect rejection each add exactly one liability.
- Correction never pays or charges Cash and always returns a shortened-deadline task.
- Escalation pays no task revenue, costs 5 Confidence, and removes 5 points of Audit Chance.
- Every night rolls effective Audit Chance after subtracting 5 points per held Compliance Token; each token also offsets one open Liability. An audit that occurs then rolls finding chance as effective Liability divided by elapsed days, and a finding uses effective Liability as Severity.
- An automatic Inbox overflow displays its consequences, leaves Audit Chance and Liability unchanged, and removes 6 Confidence; the displaced card can still apply its normal expiration consequence.
- Task- or resource-disguised junk can complete a workflow; it adds 10 worker Stress without changing Audit Chance or Liability, and creates an unapprovable Source Integrity Failure document in Review.
- An employee waiting taskless in In Progress accrues no stress during the five-second grace period, then gains stress at an increasing visible rate; assigning a task or moving to Backlog resets the wait timer.
- Every survived day close grants one non-duplicating persistent XP plus one run-only Talent Point and opens the Talent Tree for Run Process purchases; a completed Employee Development Workshop inserts the separate Cash-funded employee stat shop.
- The roadmap is the only normal between-day route choice surface, limits movement to connected workstreams, uses real 3/5/10-minute days, and converges on a Day 5 Board Review after the final nightly Talent Tree, where permanent XP spending unlocks.
- Quarterly chart legends are right-aligned above the plot and use full `Day x` endpoint labels.
- Audit Severity 1–5 applies Consent Decree, Cash Fine, Compliance Check, Bad Vibes, or Termination exactly as specified above.
- Severity 3 distributes four mandatory $0 Compliance Checks through the next day; each deleted or expired check creates +1 Liability.
- Cash at 0, Confidence at 0, or Severity-5 Termination ends the run. Compliance Tokens reduce Audit Chance, offset one Liability each, and are never consumed as extra lives.
- The header wallet and Progress panel expose persistent XP and token balances plus run-only Talent Points during play; the permanent upgrade preview shows costs and prerequisites but remains noninteractive until Day 5, while Audit Dampening and the tokens' automatic Audit Chance reductions remain explicit.
- Bad events and the phishing threshold display a high-attention popup containing both description and consequence.
