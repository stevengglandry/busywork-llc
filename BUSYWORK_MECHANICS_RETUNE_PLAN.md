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
- Approving work recognizes `base reward × (0.8 + 0.4 × Confidence / 100)`, rounded to a whole dollar.
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

- A run starts at `25%` Audit Chance.
- The audit roll happens every night.
- An inaccurate approval adds `6` points.
- Accurate approvals remove `1` point; Compliance Training removes `8`.
- When an audit occurs, its chance to find something is:

`min(1, liabilities / elapsed days) × (1 - Confidence / 200)`

This makes liabilities the error rate while Confidence provides partial board/compliance protection.

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
| Approve | Accurate work pays Confidence-scaled revenue, Confidence +2, Audit Chance -1 | Revenue still pays, but +1 liability, Audit Chance +6, producer stress, Morale modifier -1 |
| Request correction | Inaccurate work returns to Backlog as rework with a 40% shorter deadline; producer stress +5 | Accurate work is unnecessarily reworked; producer stress +9 and a red consequence popup |
| Reject | Inaccurate/unsalvageable work is destroyed with no reward or additional consequence | Accurate work is destroyed; +1 liability, Confidence -2, producer stress +10 |
| Escalate | Work moves to Done regardless of accuracy | Confidence -4 and future audit severity -15%, with no task revenue |

Rulings remain final except Request correction, which creates a new assignable rework task.

Regulatory Response corrections remain Regulatory Response tasks and retain their `$0` reward. They never convert into paid Data Entry work.

## Liabilities and audits

Liabilities are created by:

- Approving inaccurate work.
- Rejecting accurate work.

Deadline misses do not create a liability, but apply Confidence -3 and add 15% to future audit severity.

When an audit finds liabilities:

1. All currently discoverable liabilities are counted as findings.
2. Severity is calculated from their policy severity.
3. The severity multiplier increases by `0.5` for each prior audit failure, plus deadline penalties, minus escalation relief and Confidence protection. It cannot fall below `0.5`.
4. A minimum `$15` fine is assessed.
5. Confidence falls by `6 + 2 × audit fail count`.
6. The next day gains two additional active policies.
7. Two unpaid Regulatory Response tasks are added to the Inbox.
8. Confirmed liabilities are cleared; the audit fail count remains.

A third audit failure or a single fine of at least `$225` is a Critical Audit Failure and ends the run.

## Burnout outcomes

Crossing a worker's burnout threshold always applies Confidence -8 and Morale modifier -6. The follow-up is randomly selected:

- Leave plus a permanent +1 Accuracy, Speed, or Resilience increase.
- Leave plus a company-wide +2 sweet-spot width improvement.
- Employee quits permanently.
- Employee dies, permanently removing the worker and applying an additional Morale modifier -12.

The deterministic embedded test mode forces the leave/stat-growth branch so logic tests remain stable.

## Feedback and visual language

- Harmful rulings, deadline misses, burnout, termination, and audit failures use a red popup with an event title, flavor description, and explicit consequence line.
- Reaching the phishing-test threshold uses the same high-attention security notice and explains how to claim the reward.
- Employee hover/focus consistently exposes the stat popover; selected employees also show the same data in the Inspector and Staff roster.
- Every visible card has a text type label plus a type-specific shape: employee square, task circle, resource diamond, document square. Distractions retain their disguise type so the mechanic is not spoiled.

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
- Audit calculation separates roll chance, finding chance, severity multiplier, and failed-audit consequences.
- New-day preparation separates daily resets, employee recovery, and scheduled/regulatory arrivals.
- Legacy unused deck, discard, generic data state, and shuffle helpers were removed from the active runtime. Seeded arrival bags remain the authoritative draw model.

The embedded browser suite covers save migration, state invariants, ruling outcomes, Confidence modifiers, derived Morale, audit penalties, and the zero-revenue regulatory correction loop.

## Deployment

GitHub Pages publishes the repository root from `main` at `https://stevengglandry.github.io/busywork-llc/`. Publishing requires a clean verification pass, a direct commit to `main`, a push to `origin/main`, a successful Pages build, and a live HTTP 200 check for `index.html` and the documentation files.

## Acceptance criteria

- A new run displays 25% Audit Chance and cannot survive an operating close at 0 Cash.
- Confidence changes the displayed approval payout and sweet-spot width.
- A worker held in the sweet spot receives both speed and accuracy bonuses and visible flair.
- Incorrect approval and incorrect rejection each add exactly one liability.
- Correction never pays or charges Cash and always returns a shortened-deadline task.
- Escalation pays no task revenue, costs 4 Confidence, and reduces audit severity by 15%.
- Every night rolls the current Audit Chance; findings use liabilities per elapsed day and Confidence protection.
- A failed audit adds two active policies and two zero-revenue Regulatory Response tasks the next day.
- Cash at 0, Confidence at 0, or Critical Audit Failure ends the run.
- Bad events and the phishing threshold display a high-attention popup containing both description and consequence.
