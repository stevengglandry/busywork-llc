# BUSYWORK Mechanics and Payout Retune

Status: implemented in `index.html` on 2026-07-22. This document is the balance contract for the current browser build.

## Goals

- Make Cash, Morale, Audit Chance, and Confidence interact instead of behaving as isolated meters.
- Make every Review ruling legible as a risk/reward choice.
- Reward keeping workers near their preferred workload without removing stress management.
- Turn liabilities into delayed, escalating audit danger.
- Surface harmful events immediately with flavor text and exact consequences.
- Make worker status and card type readable without opening multiple panels.

## Resource model

### Cash

Cash is cash on hand. During the day it changes through recognized task income and discretionary actions. At close, payroll and operating expenses are deducted. Night activities and hiring also spend Cash.

- Starting Cash: `$450`.
- Each positive-revenue task instance rolls a visible contract rate: 5% Windfall at `5×` its task-type base reward, 8% Premium at `2×`, 25% Low Fee at `0.2×`, and 62% ordinary work split across `0.75×`, `0.9×`, and `1×`. The expected multiplier remains approximately `1×`, so variance changes priorities without broadly inflating the economy.
- Eligible arrivals have an independent 8% chance to receive Juiced scope. Juiced cards never roll the LOW FEE tier; they multiply a standard, premium, or windfall quote by `1.75×`, require a task-specific second resource, consume both inputs, and use a recipe lasting `1.35×` the standard duration. Existing saved JUICED/LOW contracts migrate to a full standard JUICED quote. Every completed workflow consumes every supplied resource; correction requires fresh inputs. The opening tutorial remains standard; audit-generated Regulatory Response arrivals use the same rare roll.
- Approving work recognizes `quoted contract payout × (0.8 + 0.4 × Confidence / 100) × Process Revenue Assurance`, rounded to a whole dollar. Revenue Assurance ranges from `1.00×` to `1.15×`; the quoted payout follows the task through processing, Review, and any requested rework.
- Regulatory Response work pays `$0`.
- Death condition: Cash at or below `$0` after the operating close.

### Morale

Morale is derived from the workforce rather than being a detached health bar:

`Morale = clamp(100 - average worker stress + average sweet-spot contribution + event modifier, 0, 100)`

Each worker can contribute up to `+10` Morale while close to their preferred workload. Pizza, firing, burnout, death, and operating-model events change the persistent event modifier.

Morale modifies processing speed:

| Morale | Productivity |
| --- | --- |
| 80–100 | +10% |
| 65–79 | +5% |
| 40–64 | no modifier |
| 20–39 | -15% |
| 0–19 | -30% |

Morale is not a direct run-ending condition. Its danger is slower throughput, more deadline misses, and the resulting Cash/Confidence pressure.

### Audit Chance

- A run starts at `50%` Audit Chance.
- The audit roll happens every night.
- Operational penalties do not directly raise Audit Chance. They create liabilities, add Exposure, or increase audit severity instead.
- Every automatic Inbox overflow adds `8` Exposure and removes `6` Confidence, in addition to consequences from the displaced card.
- Accurate approvals remove `1` point; Compliance Training removes `8`.
- When an audit occurs, **Exposure** is its chance to discover a liability:

`min(1, min(1, liabilities / elapsed days) × (1 - Confidence / 200) + exposure penalties / 100)`

Exposure is `0%` when there are no liabilities. This keeps the nightly audit roll stable while liabilities and specific operational mistakes make an audit more dangerous. Confidence provides partial board/compliance protection.

### Confidence

Confidence is Trust of the Board and has three mechanical effects:

- It scales recognized Cash payouts from 80% at 0 Confidence to 120% at 100.
- It lowers an audit's chance to find a liability and reduces the severity multiplier.
- It widens every worker sweet spot from a base width of 8 percentage points up to 12 at 100 Confidence, before company burnout bonuses.

Death condition: Confidence at or below `0`.

## Worker sweet spot

A worker enters the sweet spot after at least ten seconds of observed work/idle time when their worked-time share is within the current comfort width of their personal preferred-work target.

While active:

- Processing speed: `+15%`.
- Accuracy forecast: `+8` percentage points.
- Morale contribution: up to `+10`, strongest at the center of the target.
- Visual feedback: green border, glow, pulse, and `SWEET SPOT` badge.

Confidence widens the comfort zone. A burnout learning outcome can permanently widen it by another two points, up to six additional points.

## Review choices

| Choice | Hit | Miss |
| --- | --- | --- |
| Approve | Accurate work pays Confidence-scaled revenue, Confidence +2, Audit Chance -1 | Revenue still pays, but +1 liability increases Exposure, producer stress, Morale modifier -1 |
| Request correction | Inaccurate work returns to Backlog as rework with a 40% shorter deadline; producer stress +7 | Accurate work is unnecessarily reworked; producer stress +14, future audit severity +10%, and a red consequence popup |
| Reject | Inaccurate/unsalvageable work is destroyed with no reward or additional consequence | Accurate work is destroyed; +1 liability increases Exposure, Confidence -2, producer stress +16 |
| Escalate | Work moves to Done regardless of accuracy | Confidence -4 and future audit severity -15%, with no task revenue |

Rulings remain final except Request correction, which creates a new assignable rework task.

Regulatory Response corrections remain Regulatory Response tasks and retain their `$0` reward. They never convert into paid Data Entry work.

## Active policy pool

Each day samples a deterministic, conflict-free subset from 30 policies: three on Day 1, four on Days 2–4, and five on Day 5 before failed-audit additions.

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

Mutually exclusive caps, terms, sources, record ranges, client rules, and fatigue limits share exclusion groups, so no daily sample demands contradictory values.

## Expanded work request pool

Ordinary arrivals also include Stakeholder Alignment Memo (Spreadsheet), Revenue Enablement Packet (Client Data), and Governance Recalibration (Receipt; internal template ID remains `spend_governance_calibration` for save compatibility). Each has a specialist and one ordinary cross-role coverage recipe. Every valid task also has an emergency Manager recipe at 2.25× base duration, −70 accuracy, 3.2× work stress, and +30 completion stress. Every one of those 23 worker/task combinations has a juiced counterpart requiring two resources and 35% more base time. They reuse the three existing Review document schemas, and completed documents record the originating request and scope so correction returns the same task type and standard/juiced requirement rather than generic legacy work.

Every overnight recruit, including an Acting Manager, is a **Juiced Hire**. Its deterministic stat total is at least two pips above the opening worker of the same role when the six-pip caps permit, no stat may fall below the role baseline, and the primary role stat receives priority. Juiced Hires also process 10% faster and recover stress 20% faster in Backlog. Training Budget adds one further seeded pip rather than one pip to every stat.

### Employee stat shop and Manager strategy

The Staff side panel is a per-employee stat upgrade shop. Accuracy, Speed, and Resilience can each be raised one permanent base-stat pip at a time to the six-pip cap. Each purchase spends Cash on that specific employee. The exact next-pip price is `stat base + current stat × $6 + all prior purchased pips on that employee × $4`, with bases of `$18` Accuracy, `$16` Speed, and `$14` Resilience. Accuracy adds four ordinary chance points per pip and guarantees worker-caused compliance at six outside Manager coverage; Speed uses `0.60× / 0.80× / 1.00× / 1.10× / 1.20× / 1.35×`; Resilience uses `1.75× / 1.35× / 1.00× / 1.00× / 1.00× / 0.50×` work-stress multipliers. Manager Accuracy above baseline adds another 12 emergency-coverage points per pip, Manager Speed above baseline adds another 5% multiplicative speed per pip, and Manager Resilience scales check-in healing and team-wide task stakes. Specializing one worker remains accessible while maximizing an entire worker—especially the Manager—is a major economic commitment.

- Accuracy is used when generating the actual work product, not only in the forecast. Six Accuracy guarantees compliant specialist work. Manager emergency coverage retains its severe −70 base penalty, but each Accuracy pip above the Manager’s four-pip baseline adds another 12 points to his real and displayed coverage chance.
- Speed continues to use the normal stat curve. Each Manager Speed pip above his two-pip baseline also adds 5% multiplicative speed, allowing a fully developed Manager to work through the 2.25× emergency-coverage duration.
- Resilience reduces work stress for every employee. Manager Resilience additionally scales private check-ins from 20 stress relief at baseline to 32 at six pips.
- When a Manager-produced document reaches Review, its active-policy compliance is resolved immediately. A compliant result relieves every current employee by `6 + 2 × Resilience + 2 × pips above baseline` stress, spanning 8–26 depending on his seeded stat (10 at baseline). A noncompliant or junk-tainted result stresses every current employee by `max(6, 20 − 2 × Resilience)`, spanning 18–8 (16 at baseline).

The Staff shop shows current Cash, each worker’s purchased investment total, live stat effects, escalating next prices, and the Manager’s current whole-team win/failure stakes. These upgrades persist in the run save.

## Liabilities and audits

Liabilities are created by:

- Approving inaccurate work.
- Rejecting accurate work.
- Deleting a legitimate resource.

Liabilities no longer add Audit Chance; each one instead raises Exposure through the discovery formula. Deadline misses and deliberate Review-document deletion add 8 Exposure, apply Confidence -6, and add 30% to future audit severity. Deliberately deleting an ordinary valid task instead rolls 75% no consequence, 12.5% Confidence -2, and 12.5% future audit severity +10%. Stakeholder Alignment Memo overrides that roll with 50% no consequence / 50% Confidence -2; Governance Recalibration uses 50% no consequence / 50% future audit severity +10%. Inbox overflow adds 8 Exposure and removes 6 Confidence; a task displaced by overflow also receives the deadline penalties. Firing an employee adds 15% future audit severity, completing junk-disguised work adds 8 Exposure, and requesting an unnecessary correction adds 10% future audit severity. None of these penalties directly raises Audit Chance.

When an audit finds liabilities:

1. Exposure is rolled; if successful, all currently discoverable liabilities are counted as findings.
2. Severity is calculated from their policy severity.
3. The severity multiplier increases by `0.5` for each prior audit failure, plus deadline penalties, minus escalation relief and Confidence protection. It cannot fall below `0.5`.
4. A minimum `$15` fine is assessed.
5. Confidence falls by `6 + 2 × audit fail count`.
6. The next day gains two additional active policies.
7. Two unpaid Regulatory Response tasks are added to the Inbox.
8. Confirmed liabilities are cleared; the audit fail count remains.

A third audit failure or a single fine of at least `$225` is a Critical Audit Failure and ends the run. If that failed audit newly causes any run-ending condition—Critical Audit Failure, Cash at `0` or below, or Confidence at `0` or below—one held Compliance Token is automatically consumed to prevent the loss. The audit's fine, Confidence loss, failure count, added policies, and Regulatory Response tasks still apply; Critical Audit Failure is cleared, and any lethal Cash or Confidence value is stabilized at `1`. Nonlethal audit failures do not consume a token.

## Burnout outcomes

Crossing a worker's burnout threshold always applies Confidence -8 and Morale modifier -6. The follow-up is randomly selected:

- Leave plus a permanent +1 Accuracy, Speed, or Resilience increase.
- Leave plus a company-wide +2 sweet-spot width improvement.
- Employee quits permanently.
- Employee dies, permanently removing the worker and applying an additional Morale modifier -12.

The deterministic embedded test mode forces the leave/stat-growth branch so logic tests remain stable.

## Pacing and player agency

Automatic Inbox arrivals remain on the day-scaled schedule, but the current countdown is always visible in the action bar. While the clock is running and the Inbox has capacity, the player may use **Pull next item** to deliver the next seeded arrival immediately. Pulling an item resets the automatic-arrival clock; a full Inbox disables the action instead of silently displacing existing work. Every deterministic ten-card bag contains exactly three distinct legitimate tasks, four resources (one of each plus one seeded duplicate), and three distinct junk cards. This yields 30% tasks, 40% resources, and 30% junk and applies to every refill, not only the first ten pulls.

Card faces use a permanent semantic visual language: blue avatar circles for employees (with executive brown reserved for the Manager), amber target circles for tasks, purple diamonds for resources, and green folded-page marks for documents. The name-specific code square repeats that type color instead of falling back to neutral grey. Small colored pips communicate secondary attributes such as Premium, Windfall, Low Fee, and Juiced status. Juiced tasks and hires use a heavier double edge, layered surface, stronger depth, and lightning pip without replacing their underlying type color. Selecting a card does not add compatibility accents to other cards; only the one-time first-workflow guide may sparkle valid next actions. Task flavor text naturally names the resource needed to begin work and does not add generic consumption boilerplate. Standalone employee cards expose compact Accuracy, Speed, and Resilience values and meters without requiring hover. Invalid-drop feedback remains available during a drag.

Task-disguised junk can be assigned using the same worker and resource flow as the legitimate task it imitates. Matching resource-disguised junk is deliberately prioritized by the Inspector shortcut and also starts work. Both forms run for the normal workflow duration, accept interventions, and produce a document in Review with a guaranteed Source Integrity Failure. Completion adds `10` employee stress and `8` Exposure and leaves the employee in In Progress. Fake tasks carry no collectible value; legitimate tasks contaminated by a junk resource retain their quote, so approving the invalid output creates the same immediate-revenue-versus-liability trap as other bad work.

## Daily process maturity specialization

Every successful operating close enters a bonus award stage before overnight activity. Day 5 also grants its award before the quarterly review. The stage grants exactly one Process Point per day; save migration initializes the run tree safely, and reopening a saved reward stage cannot duplicate the daily point.

The stage always displays the same five specialization rows: Elastic Intake, Parallel Processing, Revenue Assurance, Restorative Controls, and Audit Dampening. A player may spend available points to fill the next pip from left to right or bank points for a later day. Each row has three ranks, benefits apply immediately, and all ranks last for the remainder of the current five-day run.

| Specialization | Pip 1 | Pip 2 | Pip 3 |
| --- | --- | --- | --- |
| Elastic Intake | Inbox capacity +1 | +2 | +3 |
| Backlog Architecture | Backlog capacity +1 | +2 | +3 |
| Parallel Processing | In Progress capacity +1 | +2 | +3 |
| Review Throughput | Review capacity +1 | +2 | +3 |
| Security Awareness | phishing threshold -1 | security reward +$75 | correct junk deletion lowers Audit Chance by 2 |
| Managed Intake Cadence | automatic arrivals +2 seconds | +4 seconds | +6 seconds |
| Revenue Assurance | approved payouts +5% | +10% | +15% |
| Restorative Controls | overnight recovery +3 | +6 | +9 |
| Audit Dampening | nightly Audit Chance -5 points | -10 points | -15 points |
| Correction Buffer | correction stress -2 | -4 | -6 |

Capacity ranks stack with persistent meta upgrades such as Inbox Shelf. Security Awareness also stacks with Security Liaison but never lowers the phishing threshold below one deletion.

### Card stacking and deletion

- A normal stack may contain at most one employee, one task-like card, one document, and one resource card. Resource-disguised junk occupies that resource slot. The only two-resource exception is a Juiced task whose stack contains its exact required resource pair.
- Dragging a stack onto another stack combines the complete source stack only when the resulting stack respects those compatibility slots and the five-card limit. A compatible three-card stack can therefore still land on a compatible two- or one-card stack.
- Active jobs and locked workflow stacks cannot merge. Saved homogeneous stacks from earlier builds are split into compatible stacks when loaded.
- Only the physical top card in a stack advances its deadline. Every covered timer pauses at its exact remaining value and resumes when that card becomes the top card.
- The top card displays a compact card-shaped, hoverable/focusable pip for every genuinely covered card. Pips use name-specific abbreviations, expose only concise identity and state details, and can be clicked for inspection or dragged to pull that individual card out of the stack. Composite In Progress workflows suppress these pips because their employee, task, and resources are already visible. Paused countdown, Juiced/low-value/glitch state, and employee status remain visually encoded.
- Staged resource chips can be dragged back out of an assignment. The board always rerenders from state after a drop, preventing a rejected resource from remaining visually over an employee card.
- Dragging a visible top card to empty lane space extracts that top card into a new stack; dragging a compatible stack onto another stack merges the complete source stack atomically. The visible top card never receives its own covered-card pip.
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
- The Recipes panel is a compact input-to-output network: specialist, task, consumed resource, optional Juiced resource branch, coverage alternatives, duration, and resulting document are visible together.
- Done uses a compact narrow lane so Inbox, Backlog, In Progress, and Review receive more horizontal space.
- At a 1920-pixel desktop viewport, all five lanes and the roughly 300-pixel Inspector remain visible simultaneously at compact card density. This large-monitor view is the baseline; Done alone is intentionally narrower than the reference layout.
- Completed work entering Review never changes the current card or panel selection. Review attention styling and the completion toast announce the arrival.

## Feedback and visual language

- Harmful rulings, deadline misses, burnout, termination, and audit failures use a red popup with an event title, flavor description, and explicit consequence line.
- A terminal Cash, Confidence, or Critical Audit outcome opens a visually distinct red **GAME OVER** postmortem instead of the ordinary Quarterly Review. It identifies the exact terminal threshold, snapshots all three run-ending systems, itemizes the cash-flow, judgment, staffing, liability, Discovery Potential, and Audit Severity factors that shaped the result, and lists every structured failed decision or harmful incident in chronological order. Migrated saves without structured events recover relevant incidents from the company log. Permanent Process XP and upgrades remain available in a collapsed carryover section.
- Reaching the phishing-test threshold uses the same high-attention security notice and explains how to claim the reward.
- Every end-of-day decision menu starts with a compact stage tracker. Days 1–4 show three pips for Process award, Night planning (Strategic planning on Day 3), and Morning briefing; Day 5 shows two pips for Process award and Quarterly review. Premature run-failure summaries do not show a continuation tracker.
- Every standalone employee card permanently shows a compact ACC / SPD / RES strip with each base stat as a number out of six and a tiny fill meter. The same data remains available in the Inspector and Staff upgrade shop.
- Standalone employee cards keep their coping/status tags beside the worker name and compress workload state, stress percentage, target band, and current marker into a mini gauge beside the EMPLOYEE header. Full gauges remain available in workflows, the Inspector, and Staff upgrade shop.
- Every visible card has a text type label plus a type-specific shape: employee square, task circle, resource diamond, document square. Distractions retain their disguise type so the mechanic is not spoiled.
- The currently selected card or workflow uses a three-pixel black dashed frame, offset isolation halo, and slight lift so Inspector context remains obvious over every card type and status treatment.
- Selecting a worker, task, or resource does not restyle other cards as compatible. The one-time opening guide may sparkle legitimate first-workflow options, while valid and invalid destination feedback appears only during an active drag.
- Ordinary junk cards use one of two deterministic glitch signatures—chromatic registration/scanline tearing or offset-code/clipped-edge printing—without displaying a junk label. Legitimate cards and the phishing reward notice do not receive these effects.
- Only a new run's valid opening workflow options receive the gold-and-blue sparkle aura; it follows the relevant Inspector buttons and disappears after the first legitimate workflow begins.
- The Audit header shows effective nightly Audit Chance, liability count, Exposure, and a red **Likely Reprimand** rail with five Clear/Fine/Escalated/Severe/Critical pips. Exposure is the chance that an audit discovers a liability. The Progress panel additionally shows overall failure chance, liability severity, projected multiplier, and projected fine.

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
- Workforce selectors calculate work share, sweet-spot width, payout multiplier, stress, and derived Morale without duplicating formulas in UI code.
- Review uses separate approve, reject, correction, escalation, and finalization helpers. Unknown actions are ignored without mutating state.
- Audit calculation separates the nightly roll, Exposure, severity multiplier, and failed-audit consequences.
- New-day preparation separates daily resets, employee recovery, and scheduled/regulatory arrivals.
- Legacy unused deck, discard, generic data state, and shuffle helpers were removed from the active runtime. Seeded arrival bags remain the authoritative draw model.

The embedded browser suite covers save migration, state invariants, ruling outcomes, task-contract payout tiers and persistence, Confidence modifiers, derived Morale, audit penalties, and the zero-revenue regulatory correction loop.

## Deployment

GitHub Pages publishes the repository root from `main` at `https://stevengglandry.github.io/busywork-llc/`. Publishing requires a clean verification pass, a direct commit to `main`, a push to `origin/main`, a successful Pages build, and a live HTTP 200 check for `index.html` and the documentation files.

## Acceptance criteria

- A new run displays 50% Audit Chance and cannot survive an operating close at 0 Cash.
- Confidence changes the displayed approval payout and sweet-spot width.
- Task cards visibly distinguish 5× Windfall, 2× Premium, and 20% Low Fee contracts; the quoted amount survives processing and correction.
- Juiced task cards are visibly distinct, require exactly two task-specific resources, pay 1.75× their rolled quote, last 35% longer, spawn rarely, and retain their scope through correction.
- The three new ordinary task types use existing resources and document schemas, preserve their identity through correction, and expose both specialist and coverage routes.
- The Staff side panel can buy permanent Accuracy, Speed, or Resilience pips for a particular employee with escalating Cash prices, never beyond six.
- A heavily upgraded Manager has materially better coverage accuracy, throughput, check-in healing, and whole-team success relief; every Manager task also applies its displayed team-wide success or failure stress outcome.
- Similar cards cannot share a stack; compatible multi-card stacks merge atomically up to five cards while locked work remains immovable.
- Deleting a valid resource deducts $8 and creates exactly one liability.
- A worker held in the sweet spot receives both speed and accuracy bonuses and visible flair.
- Incorrect approval and incorrect rejection each add exactly one liability.
- Correction never pays or charges Cash and always returns a shortened-deadline task.
- Escalation pays no task revenue, costs 4 Confidence, and reduces audit severity by 15%.
- Every night rolls the current Audit Chance; findings use liabilities per elapsed day and Confidence protection.
- An automatic Inbox overflow displays its consequences, leaves Audit Chance unchanged, adds 8 Exposure, and removes 6 Confidence.
- Task- or resource-disguised junk can complete a workflow; it adds 10 worker stress and 8 Exposure without changing Audit Chance, and creates an unapprovable Source Integrity Failure document in Review.
- An employee waiting taskless in In Progress accrues no stress during the five-second grace period, then gains stress at an increasing visible rate; assigning a task or moving to Backlog resets the wait timer.
- Every successful daily close grants one non-duplicating Process Point before overnight planning; the randomized specialization tree fills three ordered pips per row, permits banking, and applies each benefit for the rest of the run.
- Quarterly chart legends are right-aligned above the plot and use full `Day x` endpoint labels.
- A failed audit adds two active policies and two zero-revenue Regulatory Response tasks the next day.
- Cash at 0, Confidence at 0, or Critical Audit Failure ends the run, except that a newly lethal failed audit automatically consumes one held Compliance Token to stabilize the run once.
- Bad events and the phishing threshold display a high-attention popup containing both description and consequence.
