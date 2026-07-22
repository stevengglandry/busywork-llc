# BUSYWORK Distractions and Progression Expansion

## Implementation-Ready Plan

Status: proposed implementation plan; no gameplay code is changed by this document.

Target: the dependency-free, single-file `index.html` browser game.

This expansion adds a mail-triage pressure loop, makes the Manager the source of employee support, expands daily policy variety, exposes deeper worker stats, and turns the existing run charts into a lightweight progression dashboard. The work should preserve the current drag/stack/process/review loop and the five-minute day.

---

## 1. Outcomes

The expansion should make the player:

1. Scan every arrival instead of recognizing cards only by silhouette.
2. Decide quickly whether an item is work, a resource, junk, or a disguised reward.
3. Use the trash can deliberately, including the option to destroy legitimate cards or fire employees.
4. Learn a changing set of policies at the start of each day.
5. Read employees as distinct characters with strengths, flaws, and stress tolerances.
6. See cash momentum change during the day and understand the nightly result.
7. Accumulate visible progress between days and, eventually, between quarters.

The new systems must not turn the game into a modal-heavy spreadsheet. The board remains the primary play surface.

---

## 2. Decisions and Recommended Defaults

These values resolve the current TBDs and should be exposed as named constants for tuning.

| Setting | Recommended starting value | Reason |
|---|---:|---|
| Inbox capacity | 9 stacks | Junk needs room to create pressure without immediately invalidating real work. |
| Arrival interval by day | 20s, 18s, 17s, 16s, 15s | Creates immediate triage pressure and is substantially faster than the current 35–26s schedule. |
| Junk share of ordinary arrivals | 30% | Produces enough junk for the daily phishing-test cadence without crowding out real work. |
| Phishing-test trigger | First 3 correctly deleted junk items each day | Guarantees one reachable reward email on Day 1 and Day 2 when the player catches the scheduled junk. |
| Placeholder phishing reward | $125 | Noticeable against current payroll and hiring costs. |
| Active policies | Day 1: 3; Days 2–4: 4; Day 5: 5 | Builds memory pressure gradually. |
| Cash chart update trigger | Recognized task payout | The graph changes only when completed work produces cash, eliminating meaningless timer samples. |
| Worker stat range | 1–6 pips | Readable at card scale and supports meaningful extremes. |
| Manager check-in | Target stress −20; Manager stress +10; cash cost $20 | Healing remains useful but consumes the reluctant Manager. |

All values belong in a `BALANCE` object rather than being distributed as literals.

---

## 3. Current Architecture Fit

The current build already has most required seams:

- `Content.cards` stores employee, task, resource, and document templates.
- `createCard()` creates seeded card instances.
- `drawCard()` and `dailyDrawInterval()` own arrivals.
- `Drag.onUp()` resolves board drop targets.
- `removeCardFromLocation()` and `assertGameState()` support safe deletion.
- `supportEmployee()` already contains the check-in benefit, but currently acts as a supervisor-only command.
- `Content.policies`, `evaluateDocument()`, and `game.activePolicyIds` support policy sampling.
- `game.history`, `recordSnapshot()`, and `lineChartSvg()` provide a telemetry foundation.
- `cardNode()`, `crewReadoutHtml()`, and the Inspector can expose stats and abilities.
- `renderDayEnd()` and `renderRunEnd()` already own the nightly and quarterly summaries.

The expansion should remain data-driven. New behavior should be added as small commands and selectors, not embedded directly into rendering functions.

---

## 4. Distraction Item Class

### 4.1 Data model

Add a fourth inbox-facing class:

```js
{
  id: 'card_123',
  templateId: 'junk_receipt_typo',
  kind: 'distraction',
  distractionType: 'junk', // 'junk' | 'phishingReward'
  disguiseKind: 'resource', // task or resource presentation
  displayName: 'Receipt (Reçeipt)',
  displayDescription: 'Physic& pruuf of an expense. Consumed during verification.',
  clueIds: ['foreign-character', 'misspelling'],
  deadlineRemainingMs: null,
  stackId: null,
  meta: {}
}
```

`kind === 'distraction'` is authoritative for game logic, but must never be printed on the card. The card should display `TASK` or `RESOURCE` according to `disguiseKind`.

### 4.2 Visual contract

Junk uses the exact normal card shell shown in the supplied reference:

- same border, shadow, code chip, typography, spacing, and metadata row;
- same task/resource accent color as the imitated item;
- no warning color, junk badge, suspicious animation, or Inspector confession;
- anomaly is present in the title, description, source line, code, deadline, or punctuation;
- one obvious anomaly or two subtle anomalies per card—never zero;
- anomalies are text content, not CSS pseudo-content, so saved games and tests remain deterministic.

The accessible name should use the same glitched display copy. Do not secretly announce “junk mail” to screen readers before the player identifies it.

### 4.3 Learnable anomaly library

Use a curated library rather than arbitrary Unicode corruption. Each pattern must be visible in the default font and distinguishable at 100% zoom.

Initial families:

- transposition: `Reciept`, `Invocie`, `Spreadhseet`;
- duplicated word: `Invoice Request (Request)`;
- suspicious parenthetical: `Receipt (Receipt)`;
- foreign/invalid glyph: `Reçeipt`, `Spreadshee†`, `Physic& proof`;
- wrong corporate phrasing: `100% Guaranteed`, `ACT NOW`, `Dear Employée`;
- wrong code chip: a resource styled as `WK` or task styled as `RS`;
- invalid timer formatting: `1:7`, `00;42`, or an hourglass glyph in the wrong position;
- inconsistent capitalization: `invoice Request`, `SPREADsheet`;
- malformed client identifier: `C–882`, `C_321`, `C-32l`;
- near-match sender: `BUSYW0RK-IT`, `BUSYWORK-lT`, or `BUSYWORK IT`.

Avoid clues that depend only on color, animation, or tiny one-pixel differences.

### 4.4 Content volume

Ship at least 18 junk templates:

- 6 task disguises;
- 6 resource disguises;
- 3 fake internal notices;
- 3 fake IT/security emails.

Each template should declare `minDay`, `weight`, and `clueIds`. Day 1 uses the clearest patterns. Later days add glyph substitutions and corporate near-matches.

---

## 5. Arrival and Inbox Pressure

### 5.1 Replace the finite draw queue

The current opening deck is exhausted too quickly for the proposed arrival rate. Replace it with a seeded weighted arrival bag that refills deterministically.

```js
const ARRIVAL_WEIGHTS = {
  task: 38,
  resource: 32,
  junk: 30
};
```

Within each class, select only templates eligible for the current day. Keep a short recent-arrival exclusion list so identical cards do not arrive back-to-back unless the bag has no alternative.

### 5.2 Pacing rules

- Keep the guaranteed Day 1 opening workflow.
- Start normal arrivals 8 seconds after the workday begins.
- Use the recommended 20–15 second interval curve.
- Apply a small seeded jitter of ±8% per draw.
- Constrain the seeded Day 1 and Day 2 bags so at least three junk cards appear within the first ten ordinary arrivals.
- The guarantee controls availability, not success: the reward still requires the player to identify and delete all three correctly.
- Do not draw during briefing, paused, day-end, or run-end states.
- A triggered phishing reward is generated separately, occupies the slot freed by the triggering deletion, and does not consume the next ordinary arrival.
- If Inbox is full, retain the existing oldest-item displacement rule.
- A displaced junk item gives no deletion credit.

Track `statistics.arrivalsByKind`, `statistics.junkExpired`, and `statistics.inboxOverflows` for tuning.

---

## 6. Inbox Trash Can and Deletion Command

### 6.1 UI

Add a persistent drop zone inside the Inbox column, below the cards:

```text
      ┌───────────┐
      │ trash icon│
      │  DELETE   │
      └───────────┘
```

Use an inline SVG or CSS-drawn icon; do not add an external asset. The label `DELETE` is always visible underneath.

States:

- idle: neutral gray, compact;
- card dragging: expands and shows the predicted consequence;
- valid hover: red border and lid-open treatment;
- drop: short paper-shred animation unless reduced motion is enabled;
- keyboard focus: standard visible focus ring.

The drop target may be physically located in Inbox but accepts any unlocked card from Inbox, Backlog, or an inactive In Progress stack. Review/Done documents and cards locked by an active job are not directly deletable.

### 6.2 Command boundary

Implement one transactional command:

```js
deleteCard(cardId, source = 'trash')
```

It must:

1. Validate card existence and lock state.
2. Resolve the deletion consequence before mutating state.
3. Remove the card from its stack or column.
4. Remove empty stacks.
5. Apply rewards or penalties.
6. Clear selection if needed.
7. Log the action, record telemetry, save, and run invariants.

### 6.3 Deleting legitimate cards is allowed

The game should not block mistaken or intentional deletion. The trash hover preview communicates the consequence, but dropping commits immediately.

| Deleted card | Result |
|---|---|
| Ordinary junk | Remove it; increment correct-junk counter. |
| Phishing reward email | Remove it; grant the reward. |
| Task | Treat as abandoned/expired; apply its existing expiration effect and confidence −1. |
| Resource | Remove it; cash −$5 for waste. |
| Employee | Fire the employee; pay one day of salary as severance; morale −8; remove future payroll. |
| Manager | Same firing rules; approvals and check-ins remain unavailable until a Manager is rehired. |
| Unlocked, incomplete active stack card | Remove only that card; re-evaluate the remaining stack. |
| Locked workflow card | Reject the drop and instruct the player to cancel the workflow first. |

Add a conditional overnight `Hire Acting Manager` option if no Manager exists. Escalation remains the expensive fallback while the position is vacant.

### 6.4 Accessible fallback

Every deletable selected card gets an Inspector action: `Delete / fire…`. The button shows the same consequence preview as the drag target. This is required for keyboard-only play.

---

## 7. Phishing-Test Reward Loop

Track:

```js
game.phishing = {
  junkDeletedToday: 0,
  rewardDeliveredDay: 0,
  testsDelivered: 0,
  testsClaimed: 0
};
```

Rules:

1. Only correctly deleting `distractionType === 'junk'` increments `junkDeletedToday`.
2. The first three correct junk deletions each day trigger exactly one phishing reward email for that day.
3. Deliver the email immediately into the Inbox slot freed by deleting the third junk card, so a full Inbox cannot hide or delay the reward.
4. The visible sender is `BUSYWORK-IT`.
5. Body copy: `Congratulations for passing the company phishing test.`
6. It behaves as inert junk until placed in the trash.
7. Trashing it awards $125 in the first implementation.
8. Deleting a legitimate item never advances the phishing counter.
9. Allowing a later Inbox overflow to displace the reward email forfeits the reward.
10. Reset `junkDeletedToday` at the next morning briefing; do not carry partial daily progress forward.
11. Additional junk deletions after the daily reward still clear space but do not produce a second reward that day in the initial balance pass.

The reward toast should be satisfyingly explicit: `PHISHING TEST PASSED · Compliance award +$125`.

Long-term, replace or supplement cash with one `Compliance Token`, the first persistent progression currency described in Section 12.

---

## 8. Manager-Mediated Private Check-Ins

Replace the current supervisor-only `supportEmployee()` command with `managerCheckIn(managerId, employeeId)`.

Eligibility:

- target is a non-Manager employee;
- Manager exists and is available;
- both cards are in separate Backlog stacks;
- neither card is locked;
- target has not received a check-in today;
- company has at least $20.

Effect:

- cash −$20;
- target stress −20;
- target rhythm +5;
- Manager stress +10;
- Manager worked time +8 seconds for workload-balance purposes;
- one check-in per target per day;
- Manager remains in Backlog so the interaction does not create a disappearing-card state.

Interaction options:

1. Select a worker in Backlog and press `Private check-in with Manager` in the Inspector.
2. Drag the Manager onto a worker in Backlog.

Both paths call the same command. The button must explain why it is unavailable when either card is outside Backlog.

Manager approval and check-in availability share the same stress/burnout state. A burned-out or fired Manager cannot perform either role.

---

## 9. Daily Policy Pool

### 9.1 Policy schema additions

```js
{
  id: 'invoice_terms_net_30',
  category: 'billing',
  weight: 10,
  minDay: 1,
  exclusionGroup: 'invoice-terms',
  title: 'Receivables Standard 3.4',
  text: 'Invoices must use Net 30 terms.',
  applies(document) {},
  test(document) {},
  reason: 'Invoice terms are not Net 30.'
}
```

### 9.2 Initial pool

Expand from 3 policies to at least 20:

- 6 reimbursement/expense policies;
- 5 billing/invoice policies;
- 5 routine-data policies;
- 4 cross-document policies.

Candidate rules using existing or modestly extended document fields:

- manager signature thresholds at different daily values;
- receipt required, receipt date window, and receipt/amount match;
- suspended client changes by day;
- invoice terms must be Net 15, Net 30, or Net 45;
- high-value invoices require authorization;
- invoice amounts must not exceed a sampled daily cap;
- data source must be approved;
- sample variance requires correction;
- record count must fall within a daily range;
- control ID parity or filing-day requirements;
- documents produced under excessive stress require secondary review;
- documents from a worker covering outside their role require manager review.

Do not activate contradictory policies together. Use `exclusionGroup` for mutually exclusive threshold, terms, and client rules.

### 9.3 Deterministic daily sampling

At `prepareNextDay()`:

1. Build the eligible pool using `minDay` and unlock state.
2. Seed the selection from `runSeed + day`.
3. Select the day’s required policy count without replacement.
4. Reject exclusion-group conflicts.
5. Guarantee at least one applicable policy for each active document family when enough slots exist.
6. Store only policy IDs in the save.
7. Present additions and removals in the morning briefing.

Policies remain fixed for the whole day. No mid-day reroll.

---

## 10. Worker Stats and Extreme Traits

### 10.1 Instance stats

Move visible capability from template-only `speed`/`accuracy` values to three 1–6 instance stats:

```js
employee.stats = {
  accuracy: 4,
  speed: 3,
  resilience: 2
};
employee.abilityIds = ['thin_skinned'];
```

Templates provide role baselines. New hires receive seeded variation of −1, 0, or +1 per stat, clamped to 1–6. Salary and hiring cost scale with total pips and specialization.

### 10.2 Pip display

Cards show a compact three-row readout on hover, focus, or Inspector selection:

```text
Accuracy    ●●●●○○  4/6
Speed       ●●●○○○  3/6
Resilience  ●●○○○○  2/6
```

- Hover popover appears after 250 ms and stays within the viewport.
- Keyboard focus reveals the same popover.
- Inspector always shows the full readout and exact ability text.
- Compact mode may show icons plus pip counts.
- Do not make hover the only way to access this information.

### 10.3 Extreme-stat abilities

Abilities are derived from the stat value and do not occupy random trait slots.

| Stat | Value | Ability | Mechanical effect |
|---|---:|---|---|
| Accuracy | 6 | **Perfectionist — Never Misses** | Specialist work generates policy-compliant fields, including required signatures; forecast accuracy is 100%. |
| Accuracy | 2 | **Careless — Needs Checking** | Accuracy −8; 20% greater chance of a generated policy violation. |
| Accuracy | 1 | **Compliance Hazard — A Walking Finding** | Accuracy −16; at least one applicable field is generated incorrectly unless corrected by another effect. |
| Speed | 6 | **Inbox Zero — Frighteningly Efficient** | Processing speed +35%; first specialist task each day gains 15% starting progress. |
| Speed | 2 | **Methodical — Thoroughly Eventually** | Processing speed −20%; interventions add one extra stress. |
| Speed | 1 | **Glacial — Schedules Meetings About Starting** | Processing speed −40%; deadlines continue at full rate. |
| Resilience | 6 | **Unflappable — Seen Worse** | Work stress gain −50%; ignores the first stress-threshold condition each day. |
| Resilience | 2 | **Thin-Skinned — Takes Notes Personally** | Work and intervention stress +35%; Backlog recovery −15%. |
| Resilience | 1 | **Brittle — One Email From Collapse** | Stress gain +75%; burnout threshold is 90 instead of 100. |

The Accuracy 6 guarantee prevents errors created by the worker. It does not override external facts introduced before assignment unless the output generator can validly avoid them. For example, it may select a valid client but must not magically legalize a suspended client already fixed on the request.

### 10.4 Formula migration

Centralize stat conversion:

```js
accuracyChance(employee, recipe, context)
speedMultiplier(employee, recipe, context)
stressMultiplier(employee, recipe, context)
```

Coverage penalties, coping traits, stress conditions, morale, rhythm, development bonuses, and extreme abilities all compose through these selectors. Rendering must never recreate these formulas.

---

## 11. Live Cash Charts

### 11.1 Header microchart

Add a compact two-series sparkline above the board near the existing projected-close copy:

- **Prior close**: yesterday’s closing cash, drawn as a stable reference line.
- **Projected close**: today’s work-based projection, with a new point added only when a completed task payout is recognized through a ruling.

Define:

```js
projectedClose = game.cashTelemetry.priorClose
  + game.cashTelemetry.recognizedTaskRevenue
  - payroll()
  - operatingUpkeep();
```

This is a task-performance projection, not a mirror of every cash transaction. Hiring, check-ins, phishing rewards, waste, and other non-task changes remain visible in the normal Cash total and in the nightly cash bridge, but do not create graph points. The label must say `Task-revenue projection` so it is not mistaken for guaranteed closing cash.

### 11.2 Telemetry state

```js
game.cashTelemetry = {
  priorClose: 450,
  recognizedTaskRevenue: 0,
  currentDayPoints: [
    { sequence: 0, elapsedMs: 0, payout: 0, projected: 306, documentId: null }
  ],
  dailySeries: []
};
```

Call `recordTaskRevenuePoint(documentId, payout)` only when `ruleDocument()` recognizes a task payout. Append one point, update `recognizedTaskRevenue`, and patch only the SVG path, endpoint, and value labels. Do not sample from the simulation timer and do not call full `View.render()` for a chart update.

Cap the live series at 64 task-revenue events per day. Persist it directly in `dailySeries` at close and keep no more than five days in an active run. The typical day will contain far fewer points, so downsampling is unnecessary.

### 11.3 Nightly popup

Add:

- the day’s prior-close and projected-close lines;
- an actual closing-cash endpoint;
- a compact bridge: opening cash, recognized task revenue, discretionary spending, payroll, upkeep, closing cash;
- hover/focus values for task-payout events.

### 11.4 Quarterly popup

Add a five-day chart with:

- opening cash;
- final projection before close;
- actual close;
- phishing rewards as annotated positive events;
- hiring, firing, and operating-action costs as annotated negative events.

Reuse `lineChartSvg()` after extracting it into a renderer that accepts arbitrary series and labels.

### 11.5 Performance acceptance

- one chart update per recognized task payout, never per animation frame or clock tick;
- no board rerender caused only by a chart event;
- no more than 64 live points per day and 320 persisted points per quarter;
- no external chart library;
- no chart-specific timers and no unbounded arrays, DOM nodes, or localStorage growth.

---

## 12. Incremental Progression Direction

The best thematic fit is **Process Maturity**: the player converts successful quarters, phishing tests, and operational efficiency into a slowly improving—but increasingly bureaucratic—company.

### 12.1 Three progression horizons

1. **During a day:** cash projection, junk-deletion streak, employee rhythm, and active work.
2. **Between days:** one overnight operating action, hiring, policy preparation, and temporary company developments.
3. **Between quarters:** persistent Process XP, Compliance Tokens, unlocked roles, office systems, and new policy/junk families.

### 12.2 Recommended currencies

- `Cash`: run currency; pays costs and hiring.
- `Process XP`: earned at quarterly review; unlocks nodes permanently.
- `Compliance Tokens`: rare reward from phishing tests; buys specialized security/automation nodes.

Avoid more than these three currencies in the first progression implementation.

### 12.3 Upgrade branches

**Mailroom**

- Inbox shelf: +1 Inbox capacity.
- Known sender registry: previously discovered junk patterns receive a faint source-line clue.
- Quarantine rule: first obvious junk item each day is automatically held for manual confirmation.
- Security liaison: phishing threshold drops from 3 to 2.

**People Operations**

- Training budget: one stat pip upgrade per quarter.
- Better benefits: Backlog stress recovery +10%.
- Employee assistance: first Manager check-in each day costs $0.
- Succession plan: acting Manager recruitment is cheaper.

**Process Engineering**

- Reusable templates: retained resources can serve one additional workflow.
- Parallel review: Review capacity +1.
- Quality gate: preview the most likely policy risk before work starts.
- Workflow automation: one discovered specialist recipe begins with 10% progress.

**Finance Theater**

- Forecasting suite: projection includes confidence range.
- Procurement controls: accidental resource deletion costs $0 once per day.
- Reserve account: begin each quarter with +$50.
- Executive dashboard: one additional overnight action slot at high maturity.

### 12.4 Progression guardrails

- Upgrades should alter decisions, not remove the board interaction.
- Do not auto-delete all junk; recognizing it is the new core skill.
- Do not make early runs feel deliberately crippled.
- Unlock complexity as well as power: stronger upgrades can introduce harder junk or policy families.
- Preserve seeded run determinism after applying the same meta unlocks.
- Meta progression needs a separate persisted object from the active run.

### 12.5 First vertical slice

Implement only:

1. Process XP at quarterly review.
2. Compliance Tokens from phishing rewards.
3. A three-node Mailroom branch.
4. A three-node People Operations branch.
5. A between-quarter upgrade screen.

Defer automation-heavy branches until the junk, policy, and worker systems are balanced.

---

## 13. Save Schema and Migration

Bump the save to schema version 2.

New active-run fields:

```js
{
  activePolicyIds: [],
  phishing: {},
  cashTelemetry: {},
  recentArrivalTemplateIds: [],
  statistics: {
    junkDeleted: 0,
    legitimateItemsDeleted: 0,
    employeesFired: 0,
    phishingRewardsClaimed: 0,
    arrivalsByKind: {},
    inboxOverflows: 0
  }
}
```

New meta-save fields:

```js
{
  processXp: 0,
  complianceTokens: 0,
  unlockedUpgradeIds: [],
  discoveredJunkClueIds: []
}
```

Migration from version 1:

1. Preserve the active board exactly.
2. Convert template speed/accuracy into baseline 1–6 instance stats.
3. Assign resilience from role baseline with deterministic variation derived from existing card ID and run seed.
4. Initialize phishing and telemetry counters.
5. Preserve the current day’s active policies; daily sampling begins next morning.
6. Initialize meta currencies to zero.
7. Run `assertGameState()` and store unreadable payloads under the existing recovery key.

---

## 14. Implementation Sequence

### Phase 0 — Data and command foundation

- Add `BALANCE`, distraction schemas, deletion outcomes, stat selectors, and save migration.
- Extend invariants for distraction cards and employee deletion.
- Keep new content out of the draw pool until tests pass.

Exit criteria: version 1 saves load, new games create version 2 state, and no card becomes unowned.

### Phase 1 — Trash and legitimate deletion

- Render the Inbox trash zone.
- Add drag hover feedback and Inspector fallback.
- Implement `deleteCard()` for every allowed card kind.
- Add acting-Manager recruitment when needed.

Exit criteria: junk is not required yet; every unlocked card type can be deleted with the documented consequence.

### Phase 2 — Junk arrivals and phishing reward

- Add distraction templates and weighted arrival bag.
- Increase arrival pacing and Inbox capacity.
- Track correct junk deletions.
- Queue, deliver, and redeem the BUSYWORK-IT reward email.

Exit criteria: a seeded Day 1 and Day 2 each expose at least three junk cards within the first ten arrivals; deleting the daily third junk item produces exactly one reward email, and only trashing that email grants $125.

### Phase 3 — Manager check-ins

- Route employee support through the Manager.
- Add drag and Inspector interactions.
- Integrate Manager stress, burnout, firing, and availability messages.

Exit criteria: check-ins work only while both employees are in Backlog and never duplicate or hide a card.

### Phase 4 — Policy expansion

- Add the policy metadata and 20-policy pool.
- Add deterministic constrained sampling.
- Expand document fields/generation only where policies require it.
- Show policy changes in each briefing.

Exit criteria: no contradictory group appears together and every day is reproducible from its seed.

### Phase 5 — Worker stats and abilities

- Migrate stats to 1–6 instance pips.
- Centralize accuracy, speed, and resilience calculations.
- Add extreme abilities and negative traits.
- Add hover/focus popovers and Inspector details.

Exit criteria: forecasts and actual outcomes use the same selectors; 6 Accuracy produces compliant specialist work.

### Phase 6 — Cash telemetry

- Add bounded task-revenue event recording.
- Add the header microchart.
- Add nightly and quarterly charts and cash bridges.

Exit criteria: telemetry does not trigger board rerenders and remains bounded after repeated runs.

### Phase 7 — Progression vertical slice

- Add Process XP and Compliance Tokens.
- Add six initial upgrade nodes.
- Add between-quarter selection and persistence.
- Tune new-run difficulty around unlocked power.

Exit criteria: a completed quarter can purchase an upgrade that materially changes the next quarter without bypassing mail triage.

---

## 15. Required Automated Checks

Add embedded `BusyworkTests` coverage for:

1. Seeded weighted arrivals repeat exactly.
2. Every junk template contains at least one registered clue.
3. Junk cards render with a task/resource disguise and no visible junk label.
4. Correct junk deletion increments only the junk counter.
5. Legitimate deletion never increments phishing progress.
6. The third correct junk deletion of a day delivers exactly one reward email into the newly freed Inbox slot.
7. Day 1 and Day 2 each schedule at least three junk cards within their first ten ordinary arrivals.
8. Reward is granted only when the reward email is trashed.
9. A full Inbox can still receive the reward because the triggering deletion frees its slot.
10. Task deletion applies expiration effects.
11. Employee deletion updates payroll and morale and preserves invariants.
12. Manager deletion disables approval/check-ins and enables acting-Manager hiring.
13. Locked cards cannot be deleted.
14. Check-in requires both cards in Backlog.
15. Check-in heals the target, stresses the Manager, and is limited per target/day.
16. Daily policy selection is deterministic and conflict-free.
17. Every sampled policy ID resolves after save/load.
18. Stat pips remain within 1–6 after migration and hiring.
19. Accuracy 6 guarantees worker-caused compliance.
20. Low-stat modifiers affect forecasts and outcomes.
21. Cash telemetry appends only on recognized task payouts and caps at 64 daily points.
22. Non-task cash changes do not create projection points.
23. Chart events do not mark the board dirty.
24. Version 1 save migration retains card ownership.

---

## 16. Manual Playtest Script

1. Start a seeded Day 1 and confirm the guaranteed workflow still exists.
2. Observe at least three ordinary arrivals and one junk arrival.
3. Compare the junk card with its legitimate lookalike at normal zoom.
4. Trash the junk and verify the shred feedback and counter.
5. Trash a legitimate resource and verify the waste cost.
6. Fire a worker through the trash target and verify payroll changes.
7. Delete the third junk item of Day 1 and confirm the BUSYWORK-IT email immediately occupies the freed Inbox slot; repeat on Day 2.
8. Trash the email and verify the $125 reward.
9. Place Manager and a stressed worker in Backlog and perform a check-in.
10. Move either card out of Backlog and verify the check-in becomes unavailable with a reason.
11. Start the next day and verify the policy set changes in the briefing.
12. Hover and keyboard-focus every employee to inspect pips and ability text.
13. Approve completed work and verify the header cash projection adds one point; wait without completing work and verify no new points appear.
14. End the day and compare the chart’s final point with the nightly cash bridge.
15. Finish the quarter and verify daily actual/projection lines and progression awards.
16. Reload the saved run and confirm arrivals, counters, policies, stats, and telemetry persist.

Playtesters should be asked two direct questions:

- `Which visual clue made you decide this card was junk?`
- `Did you understand the consequence before dropping a legitimate card or employee into DELETE?`

---

## 17. Acceptance Criteria by Requested Feature

1. **Faster arrivals:** every day draws all item types more frequently than the current schedule without unbounded deck exhaustion.
2. **Junk class:** junk is internally distinct, visually disguised, seeded, and identifiable through curated subtle errors.
3. **Trash can:** Inbox displays a labeled `DELETE` drop target with drag and keyboard support.
4. **Phishing reward:** the first three correct junk deletions produce one BUSYWORK-IT email on both Day 1 and Day 2, and deleting that email grants a noteworthy reward.
5. **Delete anything:** unlocked legitimate cards and employees can be removed with explicit, deterministic consequences.
6. **Manager healer:** private check-ins require the Manager and target worker to be in Backlog and affect both stress states.
7. **Policy variety:** at least 20 policies exist and a conflict-free seeded subset is sampled each day.
8. **Incremental direction:** Process XP, Compliance Tokens, and a small upgrade tree have a defined vertical slice and guardrails.
9. **Cash charts:** bounded task-revenue projection events appear in the header, nightly close, and quarterly review; idle time and non-task cash changes add no graph points.
10. **Worker stats:** Accuracy, Speed, and Resilience display as 1–6 pips with complete high- and low-extreme abilities.

---

## 18. Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Junk feels arbitrary or inaccessible | Curated clues, escalating difficulty, text-based anomalies, playtest clue recall. |
| Faster arrivals overwhelm Day 1 | Preserve the guaranteed opening workflow, delay draws for 8s, increase Inbox capacity to 9, and tune from deletion/overflow telemetry. |
| Trash causes accidental catastrophic deletion | Large consequence preview during drag, distinct hover state, clear log/toast, locked-work protection. |
| Firing Manager creates a dead end | Keep escalation available and conditionally offer Acting Manager recruitment. |
| Policy pool generates impossible/contradictory rules | Eligibility tags, exclusion groups, constrained seeded sampling, automated coverage. |
| Accuracy 6 erases policy gameplay | Guarantee only worker-caused correctness; external request facts and player rulings still matter. |
| Charts hurt performance | Record only task-payout events, patch SVG attributes directly, and cap each day at 64 points. |
| Progression automates away the core loop | Upgrades assist recognition and capacity but never auto-resolve all junk or rulings. |
| Save size grows | Keep bounded arrays and compact daily summaries; schema migration tests. |

---

## 19. Recommended First Shipping Cut

Do not ship all ten systems simultaneously. The first playtestable cut should include:

1. faster weighted arrivals;
2. 12 of the 18 planned junk templates;
3. the Inbox trash target and legitimate deletion outcomes;
4. the once-per-day, three-junk phishing reward loop with $125 payout and guaranteed Day 1/Day 2 availability;
5. Manager-mediated check-ins;
6. telemetry counters needed to tune false-positive deletion and Inbox pressure.

Then observe whether players can reliably learn the anomaly language before layering daily policy churn, deeper worker stats, charts, and permanent progression on top.
