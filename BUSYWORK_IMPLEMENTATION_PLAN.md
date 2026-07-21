# BUSYWORK

## Implementation-Ready Product and Technical Plan

**Working title:** BUSYWORK  
**Format:** Single-player, single-file browser game (`index.html`)  
**Genre:** Roguelite card/deck builder, idle/clicker management game, document-inspection game  
**Presentation:** Work-friendly corporate kanban board  
**Primary inspiration:** The spatial card-combination loop of *Stacklands*, the escalating automation of an idle game, and the rule-checking pressure of *Papers, Please*  
**Implementation target:** A polished vertical slice that runs locally by opening one HTML file, with no build step, server, framework, external assets, or network dependency  
**Audience for this document:** The coding agent responsible for implementation

---

## 1. Executive Summary

BUSYWORK is a roguelite deck-builder disguised as ordinary project-management software. The player is a newly promoted middle manager at Synergy Solutions LLC. They drag employee, work, and resource cards across a five-column kanban board; combine cards into productive stacks; intervene in timed work; inspect completed documents against changing policies; and survive payroll, audits, incidents, and quarterly performance reviews.

The interface should look credible enough to resemble a muted internal business tool at a glance. The game reveals itself through escalating mechanics and writing: believable office friction gradually gives way to contradictory policies, corrupt choices, impossible departments, and corporate horror.

The first implementation must prove one complete, satisfying loop:

> Receive work → assemble a valid stack → process it over time → inspect the resulting document → make a consequential ruling → pay operating costs → choose a development → survive a quarterly review.

The vertical slice should favor systemic depth and legibility over content volume. Do not implement hundreds of cards, a sprawling tech tree, or multiple companies before the core loop feels good.

---

## 2. Product Vision

### 2.1 Player fantasy

The player begins as a hands-on manager manually sorting requests, assigning staff, and correcting forms. As the run progresses, they become an operations architect who builds workflows, delegates reviews, and manages exceptions. In the late game, they act as an executive choosing which rules, people, and truths are expendable.

The fantasy is not merely “numbers go up.” It is the transformation from worker to manager to institution.

### 2.2 Core promise

Every meaningful decision should force a tradeoff between at least two of the following:

- Speed and accuracy
- Profit and legality
- Efficiency and employee welfare
- Transparency and survival
- Immediate output and long-term stability
- Manual control and automation risk

### 2.3 Desired emotional arc

1. **Competence:** “I understand this board and can organize it.”
2. **Pressure:** “There is more work than I can safely process.”
3. **Optimization:** “I built a clever workflow.”
4. **Compromise:** “I know this approval is wrong, but I need the cash.”
5. **Suspicion:** “The rules and the company no longer agree with reality.”
6. **Reckoning:** “The quarterly review is evaluating the kind of company I created.”

### 2.4 Target session

- First tutorialized run: 20–30 minutes
- Standard run after familiarity: 30–45 minutes
- Individual workday: 4–6 minutes in the vertical slice
- Quarter: five workdays followed by one review
- The player must be able to pause at any time without penalty

---

## 3. Design Pillars

### Pillar 1: The board is the game

The kanban board is not a decorative menu. Card position, stack composition, column capacity, and deadlines are the central strategic state. The player should understand most problems by looking at the board.

### Pillar 2: Simple actions create difficult choices

Primary actions remain approachable: drag, drop, click, inspect, approve, reject, and buy. Difficulty comes from interacting constraints rather than complicated controls.

### Pillar 3: Automation changes responsibility

Automation should remove repetitive input while creating oversight problems. It may process tasks quickly, but it can miss fraud, amplify a bad rule, increase audit exposure, or overwhelm Review.

### Pillar 4: Rules are gameplay content

Policies must materially alter document decisions and board strategy. A policy is not flavor text or a passive stat bonus. Each active policy should create a condition the player can inspect and act upon.

### Pillar 5: Serious interface, escalating absurdity

The visual language remains restrained and professional. Humor comes from corporate phrasing, mechanical consequences, and increasingly impossible events—not from a cartoon UI.

### Pillar 6: Failure teaches and unlocks options

A failed company should produce a clear post-run explanation, record useful discoveries, and award modest permanent progression. Permanent upgrades should mostly unlock decisions, recipes, or starting options rather than flat power.

---

## 4. Scope and Non-Goals

### 4.1 Vertical-slice scope

The first shippable version includes:

- One company archetype: **Regional Services Contractor**
- One starting manager: **Newly Promoted Supervisor**
- Five kanban columns: Inbox, Backlog, In Progress, Review, Done
- Pointer-based card dragging with mouse and touch support
- Stack creation and card removal
- Timed recipe processing
- Managerial Intervention clicking
- Six employee templates
- Twelve work-task templates
- Ten resource templates
- Fifteen recipes
- Eight active policies, with three selected per run/day as specified below
- Eight incidents
- Cash, morale, audit risk, executive confidence, data, and employee stress
- End-of-day payroll and upkeep
- Document inspection with approve, reject, correction, and escalate actions
- One five-day quarter and one boss review
- Three weekly/company developments shown as a choice after Day 3
- Run failure and one successful ending
- Lightweight permanent progression using Experience
- Local save, autosave, reset, and save-version migration handling
- Onboarding prompts, pause, settings, reduced-motion support, and keyboard fallback controls

### 4.2 Explicit non-goals for the first implementation

Do not add these until the vertical-slice acceptance criteria pass:

- Multiple company archetypes
- Multiple quarters or endless mode
- Full Contacts and Corporate Secrets meta-currencies
- Branching narrative campaigns
- Hundreds of card templates
- Freeform card physics
- Multiplayer or online services
- Accounts, cloud saves, telemetry, or a backend
- Audio files or externally hosted media
- A framework, bundler, package manager, or build system
- Splitting source into separate JavaScript or CSS files
- Procedural natural-language generation

### 4.3 Repository boundary

This is a standalone game prototype. If placed inside an existing repository, add it in a clearly isolated location and do not modify or import unrelated application code. The completed artifact must still work when its `index.html` is copied to an empty folder and opened directly.

---

## 5. Core Gameplay Loop

### 5.1 Moment-to-moment loop

1. New work and resource cards arrive in Inbox.
2. The player moves cards to Backlog for safe storage or directly assembles a stack in In Progress.
3. A valid stack begins the highest-priority matching recipe.
4. Progress advances with real time while the game is unpaused.
5. The player may click the active task to perform Managerial Intervention.
6. Completing a production recipe creates a reviewable document in Review.
7. The player compares document fields with visible policies and chooses a ruling.
8. The ruling changes cash, morale, audit risk, confidence, data, or future deck contents.
9. The board receives additional work on a schedule, increasing pressure.

### 5.2 Daily loop

1. Start-of-day briefing introduces the day’s target and any new policy.
2. The workday timer begins when the player dismisses the briefing.
3. Scheduled draws add requests and resources to Inbox.
4. Expired tasks apply their stated penalty.
5. When the timer ends, active progress pauses and End of Day opens.
6. Payroll and department upkeep are charged.
7. Employee stress recovery is calculated.
8. The player chooses one overnight action.
9. The next day begins, or the run ends if a failure condition was reached.

### 5.3 Run loop

1. Begin with a fixed starter deck and staff roster.
2. Survive five increasingly demanding days.
3. Choose one company development after Day 3.
4. Face a quarterly review after Day 5.
5. Win by meeting the minimum performance conditions; otherwise receive a specific failure result.
6. Earn Experience based on performance, unlock an option, and return to the title/run-summary screen.

---

## 6. Board and Interaction Design

### 6.1 Main layout

Desktop layout:

```text
┌──────────────────────────────────────────────────────────────────────────┐
│ BUSYWORK | Day 2  02:41 | $840 | Morale 63 | Audit 18% | Confidence 74 │
├─────────────┬─────────────┬─────────────┬─────────────┬──────────────────┤
│ INBOX       │ BACKLOG     │ IN PROGRESS │ REVIEW      │ DONE             │
│ 3 / 7       │ 5 / 8       │ 2 / 3       │ 1 / 5       │ 4 today          │
│             │             │             │             │                  │
│ [cards]     │ [cards]     │ [stacks]    │ [documents] │ [receipts/log]   │
│             │             │             │             │                  │
├─────────────┴─────────────┴─────────────┴─────────────┴──────────────────┤
│ Staff | Resources | Policies | Recipes | Company Log | Pause / Settings │
└──────────────────────────────────────────────────────────────────────────┘
```

At widths below 900 px, columns become horizontally scrollable with sticky headers. Do not collapse columns into an unrelated list; spatial movement must remain understandable.

### 6.2 Column behavior

| Column | Purpose | Capacity | Overflow consequence |
|---|---|---:|---|
| Inbox | New arrivals and ticking deadlines | 7 | Oldest card expires immediately when a new draw cannot fit |
| Backlog | Safer storage and staging | 8 | Each card above 6 applies a global 3% work-speed penalty |
| In Progress | Valid processing stacks | 3 stacks | Invalid stacks remain idle and show a reason |
| Review | Completed documents awaiting a ruling | 5 | Production pauses when Review is full |
| Done | Today’s resolved work and receipts | No hard cap | Clears to a daily summary at day end |

### 6.3 Card anatomy

Every card visibly contains:

- Name
- Type icon or two-letter category marker
- Rarity/status color strip
- Deadline or salary when relevant
- One-line role or requirement summary
- Stress chip for employees
- Progress bar when processing
- Stack count when other cards are underneath
- Warning/error badge when blocked, expiring, or invalid

Detailed fields appear in a side inspector on selection. Avoid putting every stat on the card face.

### 6.4 Drag and stacking rules

- Use Pointer Events, `setPointerCapture`, and a movement threshold so clicks do not become accidental drags.
- The dragged card follows the pointer using a lightweight ghost; the original position remains visible as a placeholder.
- Valid column and card targets receive a clear highlight.
- Dropping on empty column space appends the card as a new stack.
- Dropping on a card adds it to that card’s stack if the target column permits stacking.
- Dragging the visible top card of a stack removes only that card.
- Selecting a stack opens its ordered contents and recipe status in the inspector.
- Pressing Escape during a drag cancels and restores the original state.
- A failed drop restores the card without changing state.
- Card order inside a stack must not affect recipe matching.
- Review and Done do not permit arbitrary stacking.
- Cards being consumed or transformed cannot be dragged until the recipe is cancelled.

### 6.5 Keyboard and accessible fallback

A selected card must expose **Move to…**, **Add to stack…**, and **Remove from stack** actions in the inspector. This ensures the full core loop does not depend exclusively on precise dragging. Interactive controls require visible focus, accessible names, and usable contrast.

---

## 7. Time, Pressure, and Managerial Intervention

### 7.1 Clock

- One workday lasts 300 real-time seconds by default.
- The game pauses automatically when a modal is open, the browser tab is hidden, or the player presses Pause.
- No offline progress occurs in the vertical slice.
- Use `performance.now()` deltas for simulation; never assume one timer callback equals a fixed amount of game time.
- Clamp any frame delta to 250 ms to prevent tab restoration from completing or expiring the entire board.

### 7.2 Task deadlines

- Deadlines count down only during active work time.
- Inbox deadlines run at 100% speed.
- Backlog deadlines run at 50% speed.
- In Progress deadlines run at 100% speed.
- Review documents have a separate review deadline.
- Expiration consequences are defined per task template and shown before expiry.

### 7.3 Managerial Intervention

Clicking the progress area of an active stack:

- Adds 4% base progress.
- Adds 2 stress to each employee in the stack.
- Applies a 1.5-second diminishing-return window.
- Successive clicks within the window add 4%, 3%, 2%, then 1%; further clicks add 1%.
- Each click after the third has a 4% chance to add a latent processing error to the output document.
- Display the diminishing return and risk; do not hide this mechanic.

This action should feel useful in emergencies but harmful as a default production strategy.

---

## 8. Employees, Stress, and Morale

### 8.1 Employee stats

- **Speed:** Multiplier applied to compatible recipes
- **Accuracy:** Reduces generated document defects
- **Salary:** Paid at end of day
- **Stress:** Individual value from 0–100
- **Skill tags:** Determine recipe eligibility
- **Trait:** One readable special rule

### 8.2 Stress thresholds

| Stress | State | Effect |
|---:|---|---|
| 0–49 | Stable | No modifier |
| 50–79 | Strained | 10% slower; accuracy reduced by 5% |
| 80–99 | Overloaded | 30% slower; cannot satisfy approval-authority requirements |
| 100 | Burnout | Employee becomes unavailable for the next day; confidence −8 and morale −6 |

Stress rises from active work, intervention, incidents, overcrowding, and certain overnight choices. Idle employees recover 1 stress every 20 active seconds. All available employees recover 10 stress overnight unless an action states otherwise.

### 8.3 Morale

Morale is company-wide and clamped from 0–100:

- 70–100: +5% employee speed
- 40–69: no modifier
- 20–39: −10% employee speed
- 1–19: −25% employee speed and increased incident chance
- 0: immediate run failure through Employee Walkout

---

## 9. Stack and Recipe System

### 9.1 Matching algorithm

When a stack in In Progress changes:

1. Collect its cards’ `templateId` values and tags.
2. Filter recipes by column and minimum/maximum input count.
3. Confirm all required exact inputs and tag requirements.
4. Confirm employee availability and any policy restrictions.
5. Sort valid matches by `priority` descending, then stable recipe ID.
6. Start the first match.
7. If none match, show the most useful unmet requirement rather than “invalid stack.”

A recipe locks its participating cards. Moving any participant requires the player to cancel the recipe, preserving its cards but losing accumulated progress.

### 9.2 Processing speed

```text
effectiveDuration = baseDuration
  / employeeSpeedTotal
  / moraleMultiplier
  / upgradeMultiplier
  * stressMultiplier
  * backlogPenalty
```

Clamp effective speed to a reasonable range: never below 35% or above 300% of base speed in the vertical slice.

### 9.3 Completion

Recipe completion should be transactional:

1. Calculate output and defects.
2. Verify Review has capacity if the output requires review.
3. Consume only listed inputs.
4. Unlock and return non-consumed participants to Backlog, unless the recipe specifies another destination.
5. Create output card(s).
6. Record recipe discovery and log entry.
7. Autosave.

If Review is full, freeze the completed stack at 100% with **Awaiting Review Capacity**; do not consume inputs twice.

### 9.4 Fifteen MVP recipes

| # | Inputs | Duration | Output | Consumption / notes |
|---:|---|---:|---|---|
| 1 | Intern + Data Entry Request + Spreadsheet | 18s | Completed Data Entry | Spreadsheet retained |
| 2 | Junior Analyst + Client Proposal + Client Data | 28s | Proposal Document | Client Data consumed |
| 3 | Accountant + Expense Report + Receipt | 22s | Verified Expense | Receipt consumed |
| 4 | Compliance Officer + Background Check + Customer Data | 30s | Background Report | Customer Data consumed |
| 5 | IT Specialist + Software Bug + Server Access | 34s | Patch Report | Server Access retained |
| 6 | HR Representative + Employee Complaint + Interview Notes | 30s | HR Finding | Interview Notes consumed |
| 7 | Junior Analyst + Invoice Request + Spreadsheet | 20s | Invoice Document | Spreadsheet retained |
| 8 | Accountant + Payroll Dispute + Spreadsheet | 26s | Payroll Resolution | Spreadsheet retained |
| 9 | Compliance Officer + Building Permit + Rubber Stamp | 25s | Permit Recommendation | Stamp retained |
| 10 | Junior Analyst + Shipment Manifest + Client Data | 24s | Checked Manifest | Client Data consumed |
| 11 | IT Specialist + Data Cleanup + Server Access | 35s | Clean Dataset | Generates +4 Data on approval |
| 12 | Any employee + Coffee | 5s | Caffeinated employee | Coffee consumed; +20% speed for 60s, +8 delayed stress |
| 13 | Any employee + Training Manual | 20s | Trained employee | Manual consumed; +1 accuracy for run, max once per employee |
| 14 | Compliance Officer + Questionable Reimbursement + Legal Template | 32s | Compliance Memorandum | Template retained; reveals one hidden flag |
| 15 | IT Specialist + Repetitive Task + Script Template | 40s | Invoice Automation | Both task/template consumed; unlocks simple invoice auto-routing |

“Any employee” is a tag requirement, not a literal template ID. If more than one valid recipe matches, the more specific recipe receives higher priority.

---

## 10. Document Inspection and Policy Engine

### 10.1 Review panel

Selecting a document in Review opens a full, readable inspection view containing:

- Document title and ID
- All relevant fields
- Attached resources/stamps
- Visible anomalies such as inconsistent dates or altered signatures
- Active policy list beside the document
- Deadline and projected effects of each action
- Actions: Approve, Reject, Request Correction, Escalate

The game must never require the player to remember a policy that is not visible or quickly accessible during review.

### 10.2 Rulings

**Approve**

- Pays the task’s revenue.
- Correct approval: confidence +2, audit risk −1.
- Incorrect approval: immediate cash still pays, audit risk increases by task severity, and an audit liability is recorded.

**Reject**

- Pays no revenue.
- Correct rejection: compliance reward, confidence +1.
- Incorrect rejection: morale −2, reputation proxy/confidence −2, and possible client complaint incident.

**Request Correction**

- Returns a corrected variant to Backlog with 60% of the original deadline.
- Costs $10 administration and adds 3 stress to the employee who produced it.
- Removes one correctable violation; cannot repair prohibited-client or fraudulent-identity flags.

**Escalate**

- Removes the document and delays the ruling until end of day.
- Costs 2 Influence in future versions; in the vertical slice it costs $25.
- Produces a safe but low-value result: 25% revenue, no decision accuracy reward, confidence −1.

### 10.3 Determining correctness

Each active policy evaluates the normalized document. The review engine returns a list of policy findings:

```js
{
  policyId: 'expense_manager_signature',
  passed: false,
  severity: 8,
  publicReason: 'Expenses over $300 require manager approval.'
}
```

A document is approvable only when all applicable active policies pass. Hidden flags influence fields or tests but must have a discoverable clue. Never fail a player for information that could not be inferred from the document, an attached card, or visible policy.

### 10.4 Eight MVP policies

| ID | Policy text | Rule |
|---|---|---|
| `expense_manager_signature` | Expenses over $300 require a manager signature. | `amount <= 300 || managerSigned` |
| `receipt_required` | Reimbursements require an attached receipt. | Non-reimbursement or `receiptAttached` |
| `international_compliance` | International work requires a compliance seal. | Domestic or `complianceSeal` |
| `sanctioned_client` | Client C-882 is suspended pending review. | `clientCode !== 'C-882'` |
| `stressed_approver` | Employees at 80% stress or above lack approval authority. | Producer stress `< 80` when authority required |
| `friday_red_forms` | Red forms are invalid on Friday. | Day is not Friday or `formColor !== 'red'` |
| `contractor_access` | Contractors may not use internal server access. | Not contractor or no internal access |
| `division_c_escalation` | Division C requests must be escalated. | Division is not C; UI marks Escalate as the only correct action |

At run start, use three simple policies. Add one policy on Days 2, 3, and 5. The policy panel should distinguish newly added rules.

### 10.5 Document generation

Use seeded random selection from valid field variants. Every document stores its generated fields and intended findings; it must not change when reopened or after save/load. Approximately 35% of early documents and 50% of late documents should contain at least one violation. Do not allow more than two simultaneous violations before Day 4.

---

## 11. Economy and Company Meters

### 11.1 Starting state

| Meter | Start | Range / failure |
|---|---:|---|
| Cash | $450 | Failure if end-of-day obligations leave cash below −$100 |
| Morale | 70 | Failure at 0 |
| Audit Risk | 5% | Audit check begins at 50%; forced audit at 100% |
| Executive Confidence | 75 | Failure at 0 |
| Data | 0 | Used for the automation development and score |

### 11.2 Payroll and upkeep

Starter employees cost $135 total per day. The prototype has no separate department cards; board capacity represents the basic Operations department. Fixed daily operating upkeep is $35.

At end of day:

```text
netCash = cash - totalAvailableEmployeeSalary - operatingUpkeep - incidentCosts
```

Show the projected end-of-day balance continuously in the header or Cash tooltip.

### 11.3 Audit behavior

When audit risk is at least 50%, make one seeded audit roll at end of day:

```text
auditChance = (auditRisk - 40) / 100
```

On audit:

- Each recorded incorrect approval adds a fine based on severity.
- Confidence loses 2 per confirmed violation.
- Audit risk resets to 20 plus 3 per unresolved liability.
- If there are no liabilities, confidence +3 and audit risk resets to 10.

At 100 audit risk, trigger the audit immediately after the current atomic action completes.

### 11.4 Failure conditions

- Executive Confidence reaches 0: **Employment Terminated**
- Morale reaches 0: **Company-Wide Walkout**
- Cash is below −$100 after end-of-day settlement: **Insolvency**
- Audit fine cannot be paid and liabilities exceed three: **Regulatory Intervention**

Pause simulation before showing a failure summary.

---

## 12. Deck, Draws, and Daily Pacing

### 12.1 Deck behavior

- The run deck is an ordered array of card specs.
- Draw from the front; discard resolved non-persistent work cards.
- When empty, shuffle the discard using the run’s seeded PRNG.
- Employees are persistent board entities and do not return to the draw deck.
- Resources may be retained or consumed according to recipes.
- Incidents resolve immediately and go to the log rather than occupying a permanent slot.

### 12.2 Starter deck

- 2 Data Entry Requests
- 1 Expense Report
- 1 Invoice Request
- 1 Client Proposal
- 1 Software Bug
- 2 Spreadsheets
- 2 Receipts
- 2 Client Data
- 1 Server Access
- 2 Coffee
- 1 Training Manual

### 12.3 Draw schedule

- Begin Day 1 with five cards drawn.
- Draw one card every 35 seconds while the clock runs.
- Days 3–5 add one incident card to the deck at day start.
- Day 4 reduces draw interval to 30 seconds; Day 5 reduces it to 26 seconds.
- If Inbox is full, apply its overflow rule and clearly log what expired.

### 12.4 Seeded randomness

Store a `runSeed` and PRNG state. All gameplay randomness—deck order, fields, violations, incidents, audit rolls, and development choices—must use this generator. Cosmetic animation may use `Math.random()` but must not affect outcomes.

---

## 13. Content Catalog

### 13.1 Six employees

| Template | Skills | Speed | Accuracy | Salary | Trait |
|---|---|---:|---:|---:|---|
| Intern | admin | 0.8 | 1 | $15 | **Eager:** first intervention each day adds no stress |
| Junior Analyst | analysis, sales | 1.1 | 2 | $24 | **Meticulous:** +1 accuracy when alone |
| Accountant | finance | 1.0 | 3 | $28 | **Exacting:** finance mistakes add 50% more stress but are less likely |
| Compliance Officer | compliance, authority | 0.9 | 4 | $30 | **By the Book:** reveals one applicable policy clue |
| IT Specialist | technical | 1.2 | 2 | $26 | **Shortcut:** automation recipes are 20% faster |
| HR Representative | hr, authority | 1.0 | 3 | $27 | **Mediator:** correct HR rulings restore 2 morale |

Start each run with Intern, Junior Analyst, and Accountant. Introduce the other three through fixed Day 2/3 hires or development rewards so all can be exercised in one run.

### 13.2 Twelve work tasks

| Task | Skill / recipe route | Base reward | Deadline | Failure |
|---|---|---:|---:|---|
| Data Entry Request | admin | $35 | 100s | Confidence −2 |
| Client Proposal | analysis | $90 | 150s | Confidence −5 |
| Expense Report | finance | $70 | 120s | Morale −2 |
| Background Check | compliance | $80 | 145s | Audit +4 |
| Software Bug | technical | $105 | 160s | Confidence −7 |
| Employee Complaint | hr | $65 | 140s | Morale −6 |
| Invoice Request | analysis/finance | $75 | 115s | Cash −15 |
| Payroll Dispute | finance | $85 | 135s | Morale −8 |
| Building Permit | compliance | $100 | 165s | Confidence −5 |
| Shipment Manifest | analysis | $90 | 125s | Audit +6 |
| Data Cleanup | technical | $60 + Data | 150s | Data −2 minimum 0 |
| Questionable Reimbursement | compliance | $140 | 130s | Audit +8 |

### 13.3 Ten resources

1. Spreadsheet
2. Receipt
3. Client Data
4. Customer Data
5. Server Access
6. Interview Notes
7. Rubber Stamp
8. Coffee
9. Training Manual
10. Legal Template

The Script Template used in Recipe 15 is awarded by the Automation development and does not need to appear in the base resource pool.

### 13.4 Eight incidents

| Incident | Immediate choice/effect |
|---|---|
| Broken Printer | Pay $25 or all document recipes are 15% slower today |
| Coffee Shortage | Lose one Coffee or morale −3 |
| Angry Client | Pay $20, or confidence −4 and add a Complaint task |
| Server Outage | IT employee can spend 20s fixing it; otherwise technical work pauses for 45s |
| Surprise Audit | Resolve audit immediately |
| Fire Drill | Pause all progress for 15 active seconds; employees recover 2 stress |
| Executive Visit | Complete one task in 60s for confidence +5; failure −6 |
| Supply Closet Movement | Take a sealed box for $50 and audit +8, or ignore it and morale −2 |

### 13.5 Overnight actions

- **Standard Close:** all employees recover 10 stress.
- **Unpaid Overtime:** complete 15% of each active stack; morale −6 and active employees +8 stress.
- **Pizza Party:** pay $35; morale +8 and all employees recover 5 extra stress.
- **Compliance Training:** pay $25; audit risk −8, no extra stress recovery.
- **Destroy Questionable Records:** remove one audit liability; audit risk +12 and confidence +1.

### 13.6 Company developments after Day 3

Show three and allow one choice:

1. **Document Templates:** choose one completed routine task; add a prepared version to each future day’s opening hand.
2. **Mandatory Standups:** +10% work speed; all employees gain 4 stress at day start.
3. **Lean Operations:** In Progress capacity −1; work speed +25%.
4. **Automation Pilot:** gain Script Template and Repetitive Task; unlock Recipe 15.
5. **Wellness Initiative:** morale cannot fall below 15 from incidents; daily upkeep +$15.
6. **Creative Accounting:** once per day, waive one expense-related violation; creates a hidden audit liability.

Use a fixed, seeded set of three. Always include at least one low-risk option.

---

## 14. Quarterly Review and Progression

### 14.1 Review scorecard

After Day 5, calculate:

- Cash on hand
- Total correct rulings
- Total incorrect rulings
- Confirmed violations
- Tasks completed and expired
- Ending morale
- Ending confidence
- Automation created

### 14.2 Vertical-slice victory

The player earns **Quarterly Promotion** if all are true:

- Executive Confidence is at least 40
- Cash is at least $300
- At least 12 tasks were correctly resolved
- No more than three confirmed violations remain

Otherwise show a thematically specific failure or probation ending based on the weakest metric.

### 14.3 Experience award

```text
experienceEarned =
  5
  + floor(correctRulings / 3)
  + (victory ? 10 : 0)
  + (zeroConfirmedViolations ? 5 : 0)
```

Persist lifetime Experience. At 15 Experience, unlock **Compliance Veteran** as a visible “coming next run” starting modifier: begin with all Day 2 policies revealed, but rewards are 10% lower. It is acceptable for the vertical slice to implement this single unlock and show additional branches as locked previews.

### 14.4 Run summary

The summary must explain:

- Why the run ended
- Three strongest positive performance signals
- Up to three costly mistakes, with concrete consequences
- Experience earned and next unlock progress
- Buttons for New Run, Continue From Autosave when applicable, and Reset All Data

---

## 15. Narrative and Tone Guide

### 15.1 Writing progression

- Days 1–2: mundane and plausible workplace problems
- Day 3: bureaucratic contradictions and suspicious accounting
- Day 4: impossible organizational facts treated as routine
- Day 5: existential corporate horror in calm professional language

### 15.2 Voice

- Dry, concise, politely coercive
- Avoid meme phrasing, fourth-wall jokes, and fantasy vocabulary in the UI
- The company never acknowledges that anything is supernatural
- Error messages should resemble enterprise software notices

### 15.3 Example copy

- “This request has exceeded its recommended visibility window.”
- “Division C does not exist. Requests from Division C must still be escalated.”
- “The printer has submitted a self-evaluation. Management response is overdue.”
- “Your continued employment has been categorized as a discretionary expense.”

---

## 16. Visual and UX Specification

### 16.1 Visual direction

- Off-white page background
- Blue-gray panels and column surfaces
- Muted semantic colors: blue/information, amber/warning, red/risk, green/approved
- Compact system sans-serif type stack
- One-pixel borders, restrained radius, subtle shadows
- Tiny sparklines/progress marks only where they convey state
- No fantasy frames, glossy game buttons, particle explosions, or saturated neon

### 16.2 CSS tokens

Define all styling through custom properties at `:root`:

```css
:root {
  --bg: #eef1f4;
  --surface: #ffffff;
  --surface-subtle: #f7f8fa;
  --border: #d7dde4;
  --text: #24303d;
  --muted: #657383;
  --accent: #356a9a;
  --success: #3f7d5a;
  --warning: #a66d1f;
  --danger: #a94747;
  --shadow: 0 2px 8px rgb(25 39 52 / 8%);
  --radius: 8px;
}
```

These are starting values, not a prohibition on refinement.

### 16.3 Feedback

- Every accepted drop produces a brief positional transition.
- Blocked stacks show a textual reason.
- Meter changes briefly show signed deltas.
- Policy violations are not revealed until the player rules unless an employee trait explicitly reveals them.
- Use a restrained toast region and persistent Company Log; do not rely on transient toasts for critical information.
- Respect `prefers-reduced-motion` and provide a settings toggle.

### 16.4 Work-friendly mode

The default visual treatment is already discreet. Include one **Compact Mode** toggle that reduces card color and hides nonessential flavor snippets without changing mechanics.

---

## 17. Technical Architecture

### 17.1 Constraints

- One `index.html` containing semantic HTML, CSS, and JavaScript
- Vanilla JavaScript targeting current evergreen browsers
- No dependencies, imports, CDNs, fetched JSON, fonts, images, or network requests
- Must run under both `file://` and a simple static server
- All icons should be text, CSS, or inline SVG defined inside the file
- The game remains usable if audio is unavailable; audio is optional and may be omitted

### 17.2 File organization

Keep the single file navigable with large, searchable banners in this order:

```html
<!doctype html>
<html>
<head>
  <style>
    /* 01 TOKENS AND RESET */
    /* 02 LAYOUT */
    /* 03 COMPONENTS */
    /* 04 STATES AND ANIMATION */
    /* 05 RESPONSIVE AND ACCESSIBILITY */
  </style>
</head>
<body>
  <!-- 01 APP SHELL -->
  <!-- 02 BOARD -->
  <!-- 03 INSPECTOR -->
  <!-- 04 MODALS -->
  <!-- 05 TEMPLATES / LIVE REGIONS -->
  <script>
    // 01 CONSTANTS AND CONTENT DATA
    // 02 STATE AND SAVE MIGRATION
    // 03 SEEDED RANDOMNESS
    // 04 SELECTORS AND DERIVED STATE
    // 05 COMMANDS / STATE TRANSITIONS
    // 06 RECIPE AND POLICY ENGINES
    // 07 SIMULATION LOOP
    // 08 DRAG CONTROLLER
    // 09 RENDERING
    // 10 MODALS, ONBOARDING, AND SETTINGS
    // 11 BOOTSTRAP
  </script>
</body>
</html>
```

### 17.3 Architectural rules

1. **Single mutable root state:** Store the run in one `game` object; do not scatter authoritative state across DOM elements.
2. **Command-based mutations:** User actions call named functions such as `moveCard`, `startRecipe`, `ruleOnDocument`, and `endDay`.
3. **Derived selectors:** Compute totals and eligibility through pure functions rather than duplicating cached values unnecessarily.
4. **Data-driven content:** Templates, recipes, policies, incidents, and developments are objects in content registries.
5. **Stable IDs:** Runtime instances receive unique IDs; content templates use stable string identifiers.
6. **Atomic transitions:** Complete, approve, expire, audit, and end-day operations must each finish before render or save.
7. **Deterministic simulation:** Gameplay randomness uses the saved seeded PRNG.
8. **Event delegation:** Use a small number of listeners on stable containers rather than listeners on every rendered card.
9. **Render batching:** Mark views dirty and render once per animation frame; do not rebuild the full board on every timer tick.
10. **Invariant checks:** In development mode, verify that each card exists in exactly one location and locked cards belong to a valid job.

### 17.4 Recommended runtime modules as object namespaces

Because source must remain one file, organize related functions into namespaced objects:

```js
const Content = { cards: {}, recipes: {}, policies: {}, incidents: {} };
const Store = { load, save, migrate, reset };
const Rules = { findRecipe, evaluateDocument, canMoveCard };
const Commands = { moveCard, intervene, rule, endDay };
const Simulation = { start, stop, tick };
const Drag = { onPointerDown, onPointerMove, onPointerUp, cancel };
const View = { renderHeader, renderBoard, renderInspector, renderModal };
```

These are organization aids, not separate ES modules.

---

## 18. Data Models

### 18.1 Root game state

```js
const game = {
  schemaVersion: 1,
  mode: 'title', // title | briefing | playing | paused | dayEnd | review | runEnd
  runId: 'run_...',
  runSeed: 123456789,
  rngState: 123456789,
  day: 1,
  dayElapsedMs: 0,
  drawElapsedMs: 0,
  cash: 450,
  morale: 70,
  auditRisk: 5,
  executiveConfidence: 75,
  data: 0,
  cardsById: {},
  columns: {
    inbox: [],       // stack IDs
    backlog: [],
    active: [],
    review: [],      // card IDs, never stacks
    done: []
  },
  stacksById: {},
  jobsById: {},
  deck: [],
  discard: [],
  activePolicyIds: [],
  liabilities: [],
  activeEffects: [],
  chosenDevelopmentIds: [],
  discoveredRecipeIds: [],
  dailyFlags: {},
  log: [],
  statistics: {
    tasksCompleted: 0,
    tasksExpired: 0,
    correctRulings: 0,
    incorrectRulings: 0,
    interventions: 0,
    confirmedViolations: 0
  }
};
```

### 18.2 Card template

```js
{
  id: 'expense_report',
  kind: 'task', // employee | task | resource | document | automation
  name: 'Expense Report',
  rarity: 'routine',
  tags: ['finance', 'document', 'reimbursement'],
  description: 'Validate an employee reimbursement claim.',
  defaultDeadlineMs: 120000,
  baseReward: 70,
  expiration: { morale: -2 },
  fieldGenerator: 'expenseFields'
}
```

### 18.3 Card instance

```js
{
  id: 'card_102',
  templateId: 'expense_report',
  kind: 'task',
  createdDay: 2,
  deadlineRemainingMs: 84320,
  stackId: 'stack_14',
  lockedByJobId: null,
  ownerEmployeeId: null,
  fields: {
    amount: 418.20,
    receiptAttached: true,
    managerSigned: false,
    clientCode: 'C-882',
    destinationCountry: 'CA'
  },
  hiddenFlags: ['international'],
  modifiers: [],
  meta: {}
}
```

### 18.4 Stack

```js
{
  id: 'stack_14',
  columnId: 'active',
  cardIds: ['card_employee_2', 'card_task_9', 'card_resource_3'],
  activeJobId: 'job_7',
  createdAtSequence: 41
}
```

### 18.5 Recipe

```js
{
  id: 'verify_expense',
  priority: 100,
  exactInputs: ['accountant', 'expense_report', 'receipt'],
  requiredTags: [],
  minInputs: 3,
  maxInputs: 3,
  durationMs: 22000,
  outputTemplateId: 'verified_expense',
  consumeTemplateIds: ['receipt'],
  retainKinds: ['employee'],
  requirements: { columnId: 'active' },
  reviewRequired: true
}
```

### 18.6 Active job

```js
{
  id: 'job_7',
  recipeId: 'verify_expense',
  stackId: 'stack_14',
  progress: 0.42,
  status: 'processing', // processing | blockedReview | cancelled
  interventionChain: 2,
  lastInterventionAtMs: 91230,
  latentErrorCount: 0
}
```

### 18.7 Policy

Functions cannot be serialized, so store policy IDs in state and functions in the content registry:

```js
{
  id: 'expense_manager_signature',
  title: 'Financial Authorization 4.2',
  text: 'Expenses over $300 require a manager signature.',
  severity: 8,
  applies(document, context) {
    return document.tags.includes('reimbursement');
  },
  test(document, context) {
    return document.fields.amount <= 300 || document.fields.managerSigned;
  },
  reason: 'Missing manager signature for a large expense.'
}
```

### 18.8 Persistent meta-save

```js
{
  schemaVersion: 1,
  lifetimeExperience: 0,
  unlockedIds: [],
  discoveredRecipeIds: [],
  statistics: {},
  settings: {
    reducedMotion: false,
    compactMode: false,
    highContrast: false,
    autosave: true
  },
  activeRun: null
}
```

---

## 19. State Invariants

The implementation must preserve these rules:

1. Every card instance appears in exactly one stack or one non-stack column list.
2. Every stack belongs to exactly one stack-capable column.
3. A card’s `stackId` agrees with its containing stack.
4. A locked card references an existing job, and that job references its stack.
5. A completed job cannot complete twice.
6. Review documents are immutable except for ruling metadata.
7. Meter values are clamped after every command.
8. Simulation runs only when `game.mode === 'playing'`.
9. Modals pause all gameplay deadlines and progress.
10. Content registry objects are treated as immutable.
11. Save/load cannot regenerate existing document fields.
12. A ruling’s meter changes and liability record are applied as one transaction.

Implement an optional `assertGameState()` and call it after commands during development. It may be disabled for the final production loop if performance requires it.

---

## 20. Persistence and Recovery

### 20.1 Storage

- Key: `busywork.save.v1`
- Store meta progression, settings, and active run in one JSON document.
- Autosave after consequential actions and at most once every 20 seconds during passive simulation.
- Save on `visibilitychange` when the document becomes hidden.
- Do not save on every animation frame or progress tick.

### 20.2 Loading

1. Read raw string.
2. Parse inside `try/catch`.
3. Validate top-level shape and schema version.
4. Apply migrations sequentially.
5. Reconnect content behavior by IDs; never serialize functions.
6. Run invariants.
7. If invalid, preserve the bad payload under `busywork.save.recovery` and offer New Run rather than crashing.

### 20.3 Reset

Reset All Data requires a confirmation that explicitly mentions active run and permanent progress. It removes only BUSYWORK-owned localStorage keys.

---

## 21. Rendering and Performance

### 21.1 Rendering strategy

- Use semantic HTML and DOM nodes, not canvas, for the board and documents.
- Keep a stable shell and patch only dirty regions.
- Progress bars may update through CSS custom properties without rebuilding cards.
- Re-render a column only when its membership or relevant card state changes.
- Use document fragments for batched card insertion.
- Avoid measuring layout during pointer movement; cache target rectangles at drag start and refresh if auto-scroll occurs.

### 21.2 Performance targets

- Smooth interaction at 60 fps on a typical laptop with 75 visible cards
- Pointer move handler under 4 ms in normal conditions
- No continuously growing timers or detached DOM nodes after starting five new runs
- Saved payload under 500 KB for the vertical slice
- First interactive render under 1 second on a local modern browser

### 21.3 Simulation safety

Use one animation-frame loop for visual updates and accumulated time steps for simulation. Never create a timer per card. Apply deadline/progress changes by iterating active entities from the central state.

---

## 22. Onboarding

Teach through a short, skippable Day 1 sequence:

1. Highlight Inbox and ask the player to select Data Entry Request.
2. Explain its requirement in the inspector.
3. Ask the player to place Intern, Spreadsheet, and request into In Progress as a stack.
4. Let processing run; explain one Managerial Intervention click.
5. On completion, highlight Review.
6. Show policies beside the document and ask for a ruling.
7. Explain projected payroll and the end-of-day target.

Tutorial prompts should validate actual state, not require one exact pointer path. A player using keyboard fallback must be able to complete them.

---

## 23. Testing Strategy

Because the deliverable has no build system, include a hidden development test harness callable from the console:

```js
BusyworkTests.runAll()
```

It should return and log pass/fail results for pure logic. Keep the harness in the single HTML file; it can be compact and inert during normal play.

### 23.1 Required unit-style tests

- Seeded PRNG produces repeatable sequences.
- Recipe matching ignores stack order.
- Specific recipe beats generic recipe.
- Missing input produces the expected reason.
- Cancelling a recipe unlocks every participant.
- Completion consumes only declared inputs.
- A blocked 100% job resumes when Review gains capacity.
- Each policy passes and fails representative documents correctly.
- Approve/reject/correct/escalate apply the correct transaction once.
- Audit chance and liability fines are deterministic under a seed.
- Deadline rates differ correctly between Inbox and Backlog.
- Stress and morale modifiers affect processing speed.
- Save round-trip retains documents, deck order, PRNG state, and active jobs.
- Corrupt save enters recovery flow.
- End-of-day settlement triggers each relevant failure condition.
- State invariants catch duplicate card ownership.

### 23.2 Manual interaction checks

- Drag between every legal pair of columns.
- Drop onto a card to form a stack.
- Pull the top card out of a stack.
- Cancel a drag with Escape.
- Attempt invalid drops and confirm exact restoration.
- Scroll a narrow board while dragging.
- Complete the full loop without dragging, using inspector actions.
- Pause/resume during a job and deadline.
- Hide the browser tab for 30 seconds and confirm no time jump.
- Save mid-job, reload, and finish the same job.
- Fill Review, complete another job, free a slot, and confirm exactly one output.
- Play through Day 5 and both win/fail review paths.
- Verify reduced motion, compact mode, focus order, and zoom at 200%.

### 23.3 Browser target

Manually verify current Chrome, Edge, and Firefox. Safari is a best-effort target for the first prototype; avoid APIs that obviously prevent compatibility.

---

## 24. Milestones and Acceptance Criteria

### Milestone 0: Foundation

**Deliverable:** Static app shell and deterministic state core.

Acceptance criteria:

- Opening `index.html` via `file://` shows a polished board shell without console errors.
- Content registries and `newRun(seed)` create the same initial state for the same seed.
- Five columns, header meters, tabs, inspector, modal layer, and log region exist.
- Pause, settings, and responsive horizontal board layout function.
- Root state is serializable.

### Milestone 1: Board manipulation

**Deliverable:** Cards can be created, selected, moved, stacked, and unstacked.

Acceptance criteria:

- Pointer drag works with mouse and simulated touch.
- Clicking remains distinguishable from dragging.
- Invalid drops restore the exact original order and stack.
- Keyboard/inspector controls can perform the same state changes.
- Capacity and overflow rules are enforced.
- State invariants pass after 100 scripted moves.

### Milestone 2: Production loop

**Deliverable:** Recipe matching, timed jobs, intervention, stress, and outputs.

Acceptance criteria:

- At least five representative recipes work end to end before all content is added.
- Invalid stacks state a missing requirement.
- Progress pauses correctly in every paused/modal/tab-hidden state.
- Intervention shows diminishing returns, stress, and error risk.
- Completion is atomic and cannot duplicate outputs.
- Full Review correctly blocks new outputs without losing a job.

### Milestone 3: Inspection loop

**Deliverable:** Generated documents, visible policies, rulings, and consequences.

Acceptance criteria:

- At least six policy rules can be evaluated independently.
- Every violation has a visible or discoverable clue.
- All four rulings perform their documented effects.
- Correctness is determined from policy functions, not hard-coded document names.
- Reopening or reloading a document does not change its fields.

### Milestone 4: Day and economy loop

**Deliverable:** Draw pacing, deadlines, payroll, incidents, overnight actions, and failure.

Acceptance criteria:

- A complete day can be played from briefing through overnight action.
- Payroll preview equals settlement.
- Expiration, cash, morale, audit, and confidence consequences are logged.
- Each failure condition produces the correct paused run summary.
- Seeded runs reproduce deck order, incident outcomes, and audit rolls.

### Milestone 5: Complete quarter

**Deliverable:** Five-day arc, development choice, quarterly review, Experience, and one unlock.

Acceptance criteria:

- Difficulty visibly escalates across five days.
- Day 3 development changes real mechanics.
- Both victory and non-victory quarterly summaries are reachable.
- Experience persists across New Run.
- The first unlock activates in a later run.

### Milestone 6: Content and polish

**Deliverable:** Full vertical-slice content and release candidate.

Acceptance criteria:

- All 6 employees, 12 tasks, 10 base resources, 15 recipes, 8 policies, 8 incidents, 5 overnight actions, and 6 developments are implemented.
- Onboarding is completable with pointer and keyboard paths.
- Layout remains usable at 1280×720, 1920×1080, and 390×844.
- No critical information is conveyed only by color or transient animation.
- The test harness passes all required logic tests.
- A fresh player can reach the first review decision in under five minutes.
- No network request occurs during play.
- Copying only `index.html` to an empty folder preserves full functionality.

---

## 25. Prioritized Implementation Sequence

The next coding agent should implement in this order and keep the app playable at the end of every numbered step.

### P0 — Prove the core interaction

1. Create the semantic shell, CSS tokens, board columns, modal host, inspector, and status header.
2. Define stable content registries, ID generation, seeded PRNG, and `newRun(seed)`.
3. Implement state selectors, command boundaries, render scheduling, and development invariant checks.
4. Render a minimal set: Intern, Junior Analyst, Data Entry Request, Spreadsheet, and one Expense Report.
5. Implement selection plus pointer dragging, legal drop zones, stack creation, unstacking, cancellation, and keyboard fallback.
6. Implement recipe matching with only Data Entry and Expense Report recipes.
7. Add the central simulation loop, progress, pause rules, atomic completion, and Review capacity blocking.
8. Add one generated expense document, two policies, and approve/reject rulings.

**Core-loop gate:** Stop expanding content until a player can receive, stack, process, inspect, and rule on a document without errors.

### P1 — Make one day strategically complete

9. Add stress, morale, Managerial Intervention, deadline behavior, and visible deltas.
10. Add deck/discard, scheduled draws, Inbox/Backlog capacity, expiration, and log entries.
11. Add cash, projected payroll, end-of-day settlement, overnight actions, and failure modals.
12. Add local save/load/recovery and deterministic resume.
13. Add remaining ruling actions: Request Correction and Escalate.
14. Add audit liabilities, audit resolution, and immediate audit at 100 risk.

**Day-loop gate:** Play one complete day, reload at three different moments, and receive identical outcomes under the same inputs.

### P2 — Build the run arc

15. Add the five-day schedule, policy introductions, increased draw pace, and employee introductions.
16. Add Day 3 development choice and implement three developments first.
17. Add quarterly scorecard, victory thresholds, failure/probation outcomes, and Experience.
18. Add the first persistent unlock and prove it changes a new run.
19. Add onboarding that observes state rather than intercepting controls.

**Run-loop gate:** Complete one winning and one losing five-day run without using developer shortcuts.

### P3 — Fill content and tune

20. Add remaining employee, task, resource, recipe, policy, incident, overnight, and development definitions.
21. Add content-specific document generators and clue presentation.
22. Tune economy, deadline, violation, and stress distributions using at least five fixed seeds.
23. Add responsive, compact, reduced-motion, high-contrast, and focus refinements.
24. Complete the test harness and run the full manual test matrix.
25. Remove debug-only shortcuts, audit console errors, and verify the single-file/no-network constraint.

**Release gate:** All Milestone 6 acceptance criteria pass.

---

## 26. Balance Targets

Use these as initial tuning goals, not immutable constants:

- A new player should resolve 2–4 documents on Day 1.
- A competent player should end Day 1 with $250–$400 after costs.
- Review should become a meaningful bottleneck by Day 3.
- Intervention should be used a few times per day, not continuously.
- At least one employee should approach 70 stress in an average run, but unavoidable Day 1 burnout is a tuning failure.
- A careful first-time player should have roughly a 30–45% chance of passing the quarter.
- A knowledgeable player should pass most runs but still face meaningful tradeoffs from incidents and document variance.
- Incorrect approval should be tempting when cash is tight; the expected delayed audit cost must exceed the immediate profit on average.
- No single development should be the obvious choice across all seeds.

Add a small development-only balance report to `BusyworkTests.simulate(seed)` if practical. It may use simplified automated decisions; it is for detecting extreme economy failures, not proving fun.

---

## 27. Risks and Mitigations

| Risk | Why it matters | Mitigation |
|---|---|---|
| Dragging feels unreliable | The primary action becomes frustrating | Pointer capture, movement threshold, cached targets, cancel/restore, and keyboard fallback |
| Single-file code becomes unmaintainable | Content and logic become tangled | Ordered sections, namespaces, command boundaries, stable IDs, and data registries |
| Full-board rerenders cause jank | Timers and dragging feel poor | Dirty-region rendering, stable shell, CSS progress updates, one simulation loop |
| Recipes match unexpectedly | Players cannot form a mental model | Priority rules, order-independent matching, precise blocked reasons, recipe tests |
| Policies feel arbitrary | Inspection becomes guessing | Always-visible rules and a clue for every violation; seeded immutable documents |
| Clicking dominates optimal play | Idle/management fantasy collapses into spam | Diminishing returns, stress, and defect risk displayed up front |
| Automation removes gameplay | Player becomes passive | Make automation create review load and error exposure; preserve exception decisions |
| Economy snowballs or deadlocks | Runs become trivial or unrecoverable | Projected payroll, early low-risk work, bounded modifiers, fixed-seed balance testing |
| Review fills and destroys outputs | State loss feels unfair | Freeze completed jobs at 100% and resume transactionally when capacity opens |
| Save corruption bricks the game | The only artifact becomes unusable | Schema version, validation, recovery copy, graceful new-run path |
| Background tabs advance time | Players return to unexplained failure | Pause on visibility loss and clamp frame deltas |
| Scope expands before fun is proven | Large content set masks weak core | Enforce P0/P1 gates before content expansion |
| Corporate disguise harms clarity | Important game state becomes too subtle | Consistent semantic colors, badges, tooltips, log, and explicit consequences |
| Inspired mechanics feel derivative | Product lacks identity | Emphasize policy contradictions, corporate transformation, and automation accountability |

---

## 28. Definition of Done

The vertical slice is done only when all of the following are true:

- The deliverable is one self-contained `index.html`.
- It opens directly from disk and makes zero network requests.
- The complete five-day loop, quarterly review, failure, victory, Experience award, and new run all work.
- The board supports reliable mouse, touch-style pointer, and keyboard/inspector workflows.
- Core state transitions are deterministic, transactional, saveable, and covered by the embedded test harness.
- All listed MVP content is present and discoverable in normal play.
- Policies create understandable review decisions with no unknowable violations.
- Automation, clicking, and overtime each create an explicit downside.
- The visual presentation reads first as professional project-management software.
- The writing escalates from mundane bureaucracy to controlled corporate absurdity.
- At least one full fresh run has been manually completed in each target desktop browser, and the primary loop has been verified at a narrow mobile viewport.
- There are no uncaught console errors during a complete run or after save/load.
- A final code comment near the top records the schema version, last validation date, and the commands/manual steps used to validate the release.

---

## 29. Final Guidance to the Implementing Agent

Build the smallest complete game first. The decisive question is not whether the prototype contains enough cards; it is whether dragging, stacking, waiting, inspecting, and compromising form a satisfying pressure loop.

When a feature request competes with clarity, protect the player’s ability to answer these questions at a glance:

1. What needs attention now?
2. What can I combine?
3. Why is this stack blocked?
4. What rule applies to this document?
5. What will this decision cost me today and later?

If those answers remain clear while the company becomes increasingly impossible, BUSYWORK is delivering its core promise.
