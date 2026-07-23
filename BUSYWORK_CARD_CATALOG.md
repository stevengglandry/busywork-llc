# BUSYWORK Card and Task Catalog

Source of truth: the current `Content.cards`, `Content.recipes`, opening allocation, and daily opening rules in `index.html`.

This file catalogs card **templates**. Runtime card instances receive unique IDs such as `card_17`, and the game may create multiple instances from the same template through arrivals, hiring, rework, and completed workflows.

## Catalog Summary

| Card type | Templates |
|---|---:|
| Employees | 4 |
| Tasks | 7 |
| Resources | 3 |
| Review documents | 3 |
| Junk distractions | 12 |
| Phishing reward distractions | 1 |
| **Total** | **30** |

The game also defines 16 task workflows: seven specialist recipes and nine cross-role coverage recipes.

---

## Employee Cards

Employee instances receive seeded Accuracy, Speed, and Resilience stats around the listed 1–6 pip baselines. Hired duplicates use labels such as `Intern 2`; a replacement Manager is labeled `Acting Manager`.

### Intern

| Field | Value |
|---|---|
| Template ID | `intern` |
| Card code | `IN` |
| Tags | `employee`, `admin` |
| Description | Eager generalist. First intervention each day adds no stress. |
| Baseline pips | Accuracy 3 / Speed 3 / Resilience 4 |
| Processing scalar | 0.8× |
| Salary | $15/day |
| Preferred workload | 45% before seeded variation |
| Hiring cost | $60 |
| Specialist tasks | Data Entry Request, Stakeholder Alignment Memo, Regulatory Response |
| Coverage tasks | Expense Report, Invoice Request, Spend Governance Calibration |

### Junior Analyst

| Field | Value |
|---|---|
| Template ID | `junior_analyst` |
| Card code | `JA` |
| Tags | `employee`, `analysis`, `sales` |
| Description | Meticulous when assigned alone. |
| Baseline pips | Accuracy 4 / Speed 4 / Resilience 3 |
| Processing scalar | 1.1× |
| Salary | $24/day |
| Preferred workload | 62% before seeded variation |
| Hiring cost | $110 |
| Specialist tasks | Invoice Request, Revenue Enablement Packet |
| Coverage tasks | Expense Report, Data Entry Request, Stakeholder Alignment Memo |

### Accountant

| Field | Value |
|---|---|
| Template ID | `accountant` |
| Card code | `AC` |
| Tags | `employee`, `finance` |
| Description | Exacting with financial work. |
| Baseline pips | Accuracy 5 / Speed 3 / Resilience 4 |
| Processing scalar | 1.0× |
| Salary | $28/day |
| Preferred workload | 78% before seeded variation |
| Hiring cost | $145 |
| Specialist tasks | Expense Report, Spend Governance Calibration |
| Coverage tasks | Invoice Request, Data Entry Request, Revenue Enablement Packet |

### Manager

| Field | Value |
|---|---|
| Template ID | `manager` |
| Card code | `MG` |
| Tags | `employee`, `approval` |
| Description | Authorized to sign exceptional expenses. Deeply resents being asked to work. |
| Baseline pips | Accuracy 4 / Speed 2 / Resilience 2 |
| Processing scalar | 0.45× |
| Salary | $42/day |
| Preferred workload | 10%, with a 5% minimum |
| Replacement hiring cost | $90 |
| Specialist task | None |
| Special functions | Signs qualifying Review documents; conducts private employee check-ins while both cards are in Backlog |

The Manager is not a normal task worker and appears in no production recipe.

---

## Task Cards

Task cards are work requests. A valid In Progress stack combines one task, a compatible employee, and its required resource.

Every task or document deadline miss now also applies Confidence −3 and +15% future audit severity, in addition to any template-specific expiration effect listed below.

Each positive-revenue task instance receives a contract rate when created. Windfall cards (5%) pay exactly 5× the task type's base reward and use a gold treatment; Premium cards (8%) pay 2×; Low Fee cards (25%) pay 20% and use a muted treatment; the remaining 62% pay 0.75×, 0.9×, or 1×. This keeps the long-run expected multiplier near 1× while making individual requests much more consequential. The card and Inspector show the quote before assignment. Confidence scales the quoted value only when the completed document is approved, and correction preserves the original quote. Task-disguised junk receives the same convincing visual/value roll but still pays nothing when exposed.

| Task type | Base | Low Fee (20%) | Premium (2×) | Windfall (5×) |
|---|---:|---:|---:|---:|
| Data Entry Request | $35 | $7 | $70 | $175 |
| Expense Report | $70 | $14 | $140 | $350 |
| Invoice Request | $75 | $15 | $150 | $375 |
| Stakeholder Alignment Memo | $45 | $9 | $90 | $225 |
| Revenue Enablement Packet | $85 | $17 | $170 | $425 |
| Spend Governance Calibration | $65 | $13 | $130 | $325 |
| Regulatory Response | $0 fixed | $0 | $0 | $0 |

### Data Entry Request

| Field | Value |
|---|---|
| Template ID | `data_entry_request` |
| Card code | `WK` |
| Tags | `task`, `admin` |
| Description | Transcribe a routine operational dataset. |
| Starting deadline | 1:40 |
| Base reward | $35 |
| Expiration effect | Executive Confidence −2 |
| Required resource | Spreadsheet |
| Output | Completed Data Entry |
| Specialist | Intern |
| Coverage workers | Junior Analyst, Accountant |

### Expense Report

| Field | Value |
|---|---|
| Template ID | `expense_report` |
| Card code | `WK` |
| Tags | `task`, `finance`, `reimbursement` |
| Description | Validate an employee reimbursement claim. |
| Starting deadline | 2:00 |
| Base reward | $70 |
| Expiration effect | Morale −2 |
| Required resource | Receipt |
| Output | Verified Expense |
| Specialist | Accountant |
| Coverage workers | Junior Analyst, Intern |

### Invoice Request

| Field | Value |
|---|---|
| Template ID | `invoice_request` |
| Card code | `WK` |
| Tags | `task`, `analysis` |
| Description | Prepare client billing for completed services. |
| Starting deadline | 1:55 |
| Base reward | $75 |
| Expiration effect | Cash −$15 |
| Required resource | Spreadsheet |
| Output | Invoice Document |
| Specialist | Junior Analyst |
| Coverage workers | Accountant, Intern |

### Stakeholder Alignment Memo

| Field | Value |
|---|---|
| Template ID | `stakeholder_alignment_memo` |
| Card code | `WK` |
| Tags | `task`, `admin`, `routine` |
| Description | Operationalize cross-functional consensus into an actionable alignment artifact. |
| Starting deadline | 1:45 |
| Base reward | $45 |
| Expiration effect | Executive Confidence −2 |
| Required resource | Spreadsheet (retained) |
| Output | Completed Data Entry |
| Specialist / coverage | Intern / Junior Analyst |

### Revenue Enablement Packet

| Field | Value |
|---|---|
| Template ID | `revenue_enablement_packet` |
| Card code | `WK` |
| Tags | `task`, `analysis`, `billing` |
| Description | Synthesize client intelligence into a monetization-ready enablement packet. |
| Starting deadline | 1:50 |
| Base reward | $85 |
| Expiration effect | Cash −$18 |
| Required resource | Client Data (consumed) |
| Output | Invoice Document |
| Specialist / coverage | Junior Analyst / Accountant |

### Spend Governance Calibration

| Field | Value |
|---|---|
| Template ID | `spend_governance_calibration` |
| Card code | `WK` |
| Tags | `task`, `finance`, `reimbursement` |
| Description | Right-size reimbursement controls against the enterprise spend-governance posture. |
| Starting deadline | 1:55 |
| Base reward | $65 |
| Expiration effect | Morale −2 |
| Required resource | Receipt (consumed) |
| Output | Verified Expense |
| Specialist / coverage | Accountant / Intern |

### Regulatory Response

| Field | Value |
|---|---|
| Template ID | `regulatory_response` |
| Card code | `RG` |
| Tags | `task`, `admin`, `regulatory` |
| Description | Mandatory audit remediation. It consumes capacity and pays no revenue. |
| Starting deadline | 1:10 |
| Base reward | $0 |
| Expiration effect | Confidence −4, plus the universal deadline penalty |
| Required resource | Spreadsheet |
| Output | Completed Data Entry |
| Specialist | Intern |
| Arrival | Two are added the morning after a failed audit |

### Rework Task Instances

`Request correction` converts a Review document back into the originating task template recorded by its completed workflow. Old saves without that source metadata use this fallback map:

For current workflows, Stakeholder Alignment Memo, Revenue Enablement Packet, and Spend Governance Calibration all return as themselves with their original quoted payout. Regulatory work likewise keeps its unpaid identity.

| Review document | Rework task |
|---|---|
| Completed Data Entry | Data Entry Request |
| Verified Expense | Expense Report |
| Invoice Document | Invoice Request |
| Regulatory Completed Data Entry | Regulatory Response (`$0` reward retained) |

The same card instance is transformed, marked as rework, and returned to Backlog with 60% of its remaining document deadline, never less than 25 seconds.

---

## Resource Cards

### Spreadsheet

| Field | Value |
|---|---|
| Template ID | `spreadsheet` |
| Card code | `RS` |
| Tags | `resource`, `spreadsheet` |
| Description | Approved shared workbook. Retained after routine use. |
| Used by | Data Entry Request, Invoice Request, Stakeholder Alignment Memo, Regulatory Response |
| Consumption | Retained after every supported workflow |

### Receipt

| Field | Value |
|---|---|
| Template ID | `receipt` |
| Card code | `RS` |
| Tags | `resource`, `receipt` |
| Description | Physical proof of an expense. Consumed during verification. |
| Used by | Expense Report, Spend Governance Calibration |
| Consumption | Consumed when the workflow completes |

### Client Data

| Field | Value |
|---|---|
| Template ID | `client_data` |
| Card code | `RS` |
| Tags | `resource`, `client-data` |
| Description | Restricted client information packet. |
| Used by | Revenue Enablement Packet |
| Consumption | Consumed when the workflow completes |

Deleting any legitimate resource through the board trash target or Inspector costs `$8` and creates one severity-3 liability with the source `legitimate resource destroyed`. Deleting junk has no waste charge and instead advances the daily phishing-test counter.

---

## Review Document Cards

Documents are generated by completed workflows and enter Review with a 1:30 ruling deadline. The player must Approve, Reject, Request correction, or Escalate.

### Completed Data Entry

| Field | Value |
|---|---|
| Template ID | `completed_data_entry` |
| Card code | `DC` |
| Tags | `document`, `routine` |
| Description | A completed transcription awaiting review. |
| Produced by | Data Entry Request, Stakeholder Alignment Memo, and Regulatory Response workflows |
| Generated fields | `records`, `variance`, `source` |
| Possible base anomaly | `variance` has a 28% chance to be `Review sample mismatch` before worker-ability adjustments |

### Verified Expense

| Field | Value |
|---|---|
| Template ID | `verified_expense` |
| Card code | `DC` |
| Tags | `document`, `reimbursement` |
| Description | A reimbursement recommendation awaiting authorization. |
| Produced by | Expense Report and Spend Governance Calibration workflows |
| Generated fields | `amount`, `receiptAttached`, `managerSigned`, `clientCode` |
| Possible base anomalies | Missing Manager signature; suspended client; policy-sensitive amount/cents |

### Invoice Document

| Field | Value |
|---|---|
| Template ID | `invoice_document` |
| Card code | `DC` |
| Tags | `document`, `billing` |
| Description | A prepared invoice awaiting release. |
| Produced by | Invoice Request and Revenue Enablement Packet workflows |
| Generated fields | `amount`, `clientCode`, `terms`, `authorized` |
| Possible base anomalies | Suspended client; policy-sensitive amount, terms, or authorization |

Document correctness is determined by the active daily policies, not by the template alone.

---

## Junk Distraction Cards

Junk uses `kind: distraction` internally but imitates a normal task or resource card. Deleting ordinary junk advances the daily phishing-test counter. Every junk template consistently receives one of two subtle visual defects: chromatic text misregistration with a scanline tear, or an offset code block with a clipped edge artifact. These defects are clues rather than explicit labels; legitimate cards and the BUSYWORK-IT reward notice remain clean.

The Inspector's **Add [resource] and begin** shortcut is intentionally unsafe: when a resource-disguised junk card imitates the requested input, the shortcut selects that decoy before legitimate stock. The decoy enters the assignment but cannot start its workflow, forcing the player to inspect and remove the staged input. Manual dragging remains an explicit safe choice.

Task-disguised junk is also operationally dangerous. It inherits the imitated task's deadline, worker qualifications, resource requirement, duration forecast, and apparent payout. The player can assign it and start a normal-looking progress job. On completion it reveals itself as junk, produces no Review document or revenue, adds `6` worker stress, and leaves the worker waiting in In Progress. Resources committed to the bogus request return to Backlog.

### Task Disguises

| Template ID | Mimics | Displayed card | Code | Description | Source | Clue IDs |
|---|---|---|---|---|---|---|
| `junk_invoice_repeat` | Invoice Request | Invoice Request (Request) | `WK` | Prepare client billing for completed services. | Client Services | `duplicated-word` |
| `junk_data_urgent` | Data Entry Request | URGENT Data Entry | `WK` | Transcribe immediately to prevent account closure. | External Admin | `wrong-phrasing` |
| `junk_expense_guarantee` | Expense Report | Expense Report | `WK` | 100% Guaranteed reimbursement approval. | Benefits Reward Center | `wrong-phrasing` |
| `junk_invoice_domain` | Invoice Request | Invoice Request | `WK` | Prepare client billing for completed services. | Billing via busyw0rk-it.co | `sender-domain` |
| `junk_expense_currency` | Expense Report | Expense Report | `WК` | Validate an employee reimbursement claim. | Expense Operations | `invalid-code` |
| `junk_data_secret` | Data Entry Request | Data Entry Request | `WK` | Transcribe a routine operational dataset. Do not tell Finance. | Operations Intake | `wrong-phrasing` |

The `К` in the `WК` code is Cyrillic, not the normal Latin `K`.

### Resource Disguises

| Template ID | Mimics | Displayed card | Code | Description | Source | Clue IDs |
|---|---|---|---|---|---|---|
| `junk_receipt_typo` | Receipt | Reçeipt | `RS` | Physic& proof of an expense. Consumed during verification. | Expenses Desk | `foreign-character` |
| `junk_sheet_glyph` | Spreadsheet | Spreadshee† | `RS` | Approved shared workbook. Retained after routine use. | Operations | `foreign-character` |
| `junk_receipt_reply` | Receipt | Receipt (Receipt) | `RS` | Physical proof of an expense. Kindly reply with credentials. | Expenses Desk | `suspicious-parenthetical`, `wrong-phrasing` |
| `junk_sheet_spacing` | Spreadsheet | Spread sheet | `RS` | Approved shared workbook. Retained after routine use. | Operations | `misspelling` |
| `junk_client_gift` | Client Data | Client Data | `RS` | Restricted client gift card packet. | Client Appreciation | `wrong-phrasing` |
| `junk_receipt_code` | Receipt | Reciept | `R5` | Physical proof of an expense. | Expenses Desk | `transposition`, `invalid-code` |

---

## Phishing Reward Card

### BUSYWORK-IT Security Test Result

| Field | Value |
|---|---|
| Template ID | `phishing_reward` |
| Internal type | `distraction` / `phishingReward` |
| Disguise | Task |
| Card code | `WK` |
| Display name | BUSYWORK-IT Security Test Result |
| Description | Delete this notice to claim the approved security-awareness stipend. |
| Source | BUSYWORK-IT |
| Clue ID | `reward-instruction` |
| Trigger | The daily junk-deletion threshold |
| Effect on arrival | Occupies the Inbox slot freed by the triggering deletion |
| Reward condition | Reward is granted only when this card is deleted |
| Reward | $125 and 1 persistent Compliance Token |

---

## Task Workflow Matrix

Durations are base recipe durations before employee Speed, stress, morale, rhythm, coping traits, conditions, or company-development modifiers.

| Task | Worker | Fit | Resource | Base duration | Accuracy penalty | Work-stress multiplier | Completion stress | Output | Typical payout |
|---|---|---|---|---:|---:|---:|---:|---|---:|
| Data Entry Request | Intern | Specialist | Spreadsheet | 18s | 0 | 1.00× | +4 default | Completed Data Entry | $35 |
| Data Entry Request | Junior Analyst | Coverage | Spreadsheet | 25s | −5 | 1.35× | +8 | Completed Data Entry | $35 |
| Data Entry Request | Accountant | Coverage | Spreadsheet | 26s | −4 | 1.30× | +8 | Completed Data Entry | $35 |
| Regulatory Response | Intern | Specialist | Spreadsheet | 24s | 0 | 1.00× | +8 | Completed Data Entry | $0 |
| Expense Report | Accountant | Specialist | Receipt | 22s | 0 | 1.00× | +4 default | Verified Expense | $70 |
| Expense Report | Junior Analyst | Coverage | Receipt | 32s | −8 | 1.60× | +12 | Verified Expense | $70 |
| Expense Report | Intern | Coverage | Receipt | 40s | −14 | 2.00× | +18 | Verified Expense | $70 |
| Invoice Request | Junior Analyst | Specialist | Spreadsheet | 20s | 0 | 1.00× | +4 default | Invoice Document | $75 |
| Invoice Request | Accountant | Coverage | Spreadsheet | 29s | −6 | 1.45× | +10 | Invoice Document | $75 |
| Invoice Request | Intern | Coverage | Spreadsheet | 36s | −12 | 1.80× | +15 | Invoice Document | $75 |
| Stakeholder Alignment Memo | Intern | Specialist | Spreadsheet | 22s | 0 | 1.00× | +4 default | Completed Data Entry | $45 |
| Stakeholder Alignment Memo | Junior Analyst | Coverage | Spreadsheet | 30s | −6 | 1.40× | +9 | Completed Data Entry | $45 |
| Revenue Enablement Packet | Junior Analyst | Specialist | Client Data | 24s | 0 | 1.00× | +4 default | Invoice Document | $85 |
| Revenue Enablement Packet | Accountant | Coverage | Client Data | 33s | −8 | 1.50× | +11 | Invoice Document | $85 |
| Spend Governance Calibration | Accountant | Specialist | Receipt | 25s | 0 | 1.00× | +4 default | Verified Expense | $65 |
| Spend Governance Calibration | Intern | Coverage | Receipt | 38s | −12 | 1.80× | +15 | Verified Expense | $65 |

### Workflow Consumption

| Workflow family | Consumed | Retained |
|---|---|---|
| Data Entry | Data Entry Request | Employee, Spreadsheet |
| Expense verification | Expense Report, Receipt | Employee |
| Invoice preparation | Invoice Request | Employee, Spreadsheet |
| Stakeholder alignment | Stakeholder Alignment Memo | Employee, Spreadsheet |
| Revenue enablement | Revenue Enablement Packet, Client Data | Employee |
| Spend governance | Spend Governance Calibration, Receipt | Employee |

After completion, the document enters Review and the worker remains in In Progress until the player moves or reassigns them.

---

## Opening and Scheduled Card Instances

### New Run Opening Allocation

Backlog begins with one instance of each employee:

- Intern
- Junior Analyst
- Accountant
- Manager

Inbox begins with:

- Data Entry Request
- Spreadsheet
- Expense Report
- Receipt
- Invoice Request

The specific opening Data Entry Request, Spreadsheet, and Intern instances are linked by the first-workflow guide. Their aura and the corresponding Inspector action cues disappear permanently as soon as the first legitimate workflow starts; later instances of the same templates are never highlighted.

### Guaranteed Daily Openings

| Day | Added at briefing |
|---:|---|
| 1 | Uses the new-run opening allocation above |
| 2 | Expense Report + Receipt |
| 3 | Invoice Request + Spreadsheet |
| 4 | Data Entry Request + Spreadsheet |
| 5 | Expense Report + Receipt |

Ordinary arrivals can additionally create Spreadsheet, Client Data, Data Entry Request, Receipt, Expense Report, Invoice Request, Stakeholder Alignment Memo, Revenue Enablement Packet, and Spend Governance Calibration instances. The seeded daily arrival bag also includes three junk templates before switching to the weighted refill system.

---

## Runtime Instance Notes

- Every instance has a unique `card_*` ID, location, creation day, and optional deadline.
- Employee instances add stress, workload preference, coping trait, condition, rhythm, daily work/idle time, 1–6 stats, and derived abilities.
- Task instances may become rework tasks and retain revision metadata.
- Positive-revenue task instances retain their payout tier, multiplier, and quoted contract amount through production and rework.
- Document instances store generated fields, producer ID, recipe ID, originating task template, producer stress, coverage status, reward, and final ruling.
- Distraction instances store their internal distraction type, visual disguise type, imitated template, and deterministic glitch variant.
- Multiple hired employees may share one template but have different seeded stats, traits, workload preferences, and labels.
- Matching multi-card stacks can be dragged onto each other to merge the complete source stack. Resource stacks have no merge-size cap; other homogeneous stacks retain the five-card cap. Active or locked stacks cannot be merged.
- The opening Data Entry Request, Spreadsheet, and Intern are the only instances eligible for the first-workflow sparkle guide.
- The catalog excludes policies, upgrades, overnight actions, and company developments because they are not cards.
