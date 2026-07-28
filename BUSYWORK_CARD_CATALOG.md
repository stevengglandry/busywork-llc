# BUSYWORK Card and Task Catalog

Source of truth: the current `Content.cards`, `Content.recipes`, stat/trait pools, balance and progression constants, opening allocation, and runtime rules in `index.html`.

This file catalogs card **templates** and the systems that modify their runtime instances. Runtime cards receive unique IDs such as `card_17`, and the game may create multiple instances from the same template through arrivals, hiring, rework, and completed workflows.

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

The game defines 46 task workflows: 23 standard-scope recipes (seven specialist, nine ordinary cross-role coverage, and seven emergency Manager coverage) plus a juiced counterpart for every recipe.

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
| Stress sweet spot | 10–20% |
| Hiring cost | $60 |
| Specialist tasks | Data Entry Request, Stakeholder Alignment Memo, Regulatory Response |
| Coverage tasks | Expense Report, Invoice Request, Governance Recalibration |

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
| Stress sweet spot | 10–20% |
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
| Stress sweet spot | 10–20% |
| Hiring cost | $145 |
| Specialist tasks | Expense Report, Governance Recalibration |
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
| Stress sweet spots | 0–5% and 50–75% |
| Replacement hiring cost | $90 |
| Specialist task | None |
| Emergency coverage | Every valid task at 2.25× base duration, −70 accuracy, 3.2× work stress, and +30 completion stress |
| Special functions | Signs qualifying Review documents; conducts Resilience-scaled private check-ins; applies a team-wide stress result after his own tasks |

The Manager begins as a deliberately poor emergency task worker, but Cash investment can turn him into a risky workhorse and stress healer. Each Accuracy pip above baseline adds 12 coverage-chance points on top of the ordinary stat gain; each Speed pip above baseline adds 5% multiplicative processing speed; Resilience raises check-in healing from 20 to as much as 32. Depending on his seeded Resilience, a compliant Manager output relieves every employee by 8–26 stress (10 at the baseline stat), while a noncompliant or junk-tainted output stresses everyone by 18–8 (16 at baseline). The Staff shop exposes the live values before assignment.

---

## Task Cards

Task cards are work requests. A valid In Progress stack combines one task, a compatible employee, and its required resource.

Every natural task or document deadline miss adds 8 Exposure, applies Confidence −6, and adds 30% future audit severity, in addition to any template-specific expiration effect listed below. Deliberately deleting a Review document applies the same global penalty. Ordinary valid task deletion instead rolls 75% no consequence, 12.5% Confidence −2, and 12.5% audit severity +10%; Stakeholder Alignment Memo uses 50% none / 50% Confidence −2, while Governance Recalibration uses 50% none / 50% audit severity +10%. These penalties leave Audit Chance unchanged.

Each positive-revenue task instance receives a contract rate when created. Windfall cards (5%) pay exactly 5× the task type's base reward and use a gold treatment; Premium cards (8%) pay 2×; Low Fee cards (25%) pay 20% and use a muted treatment; the remaining 62% pay 0.75×, 0.9×, or 1×. This keeps the long-run expected multiplier near 1× while making individual requests much more consequential. The card and Inspector show the quote before assignment. Confidence scales the quoted value only when the completed document is approved, and correction preserves the original quote. Task-disguised junk receives the same convincing visual/value roll but still pays nothing when exposed.

Eligible task arrivals have an 8% chance to be **Juiced**. Juiced scope never combines with LOW FEE; it multiplies a standard, premium, or windfall quote by 5×, takes 35% longer, requires a second resource, consumes both resources, and survives production and correction. Older 1.75× Juiced rewards and saved JUICED/LOW contracts migrate to the current 5× quote on load. All resources in every completed workflow are consumed; canceled workflows consume nothing, and correction requires fresh inputs. The guaranteed opening tutorial remains standard scope; audit-generated Regulatory Response arrivals use the same rare roll as ordinary tasks.

| Task type | Base | Low Fee (20%) | Premium (2×) | Windfall (5×) |
|---|---:|---:|---:|---:|
| Data Entry Request | $35 | $7 | $70 | $175 |
| Expense Report | $70 | $14 | $140 | $350 |
| Invoice Request | $75 | $15 | $150 | $375 |
| Stakeholder Alignment Memo | $45 | $9 | $90 | $225 |
| Revenue Enablement Packet | $85 | $17 | $170 | $425 |
| Governance Recalibration | $65 | $13 | $130 | $325 |
| Regulatory Response | $0 fixed | $0 | $0 | $0 |

### Juiced Scope Requirements

| Task type | Standard resource | Added juiced resource | Standard-rate juiced quote |
|---|---|---|---:|
| Data Entry Request | Spreadsheet | Client Data | $61 |
| Expense Report | Receipt | Spreadsheet | $123 |
| Invoice Request | Spreadsheet | Client Data | $131 |
| Stakeholder Alignment Memo | Spreadsheet | Client Data | $79 |
| Revenue Enablement Packet | Client Data | Spreadsheet | $149 |
| Governance Recalibration | Receipt | Spreadsheet | $114 |
| Regulatory Response | Spreadsheet | Receipt | $0 |

### Data Entry Request

| Field | Value |
|---|---|
| Template ID | `data_entry_request` |
| Card code | `WK` |
| Tags | `task`, `admin` |
| Description | Enter the operational dataset in the approved Spreadsheet. |
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
| Description | Validate the reimbursement claim against its Receipt. |
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
| Description | Prepare client billing in the approved Spreadsheet. |
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
| Description | Build the alignment artifact in a shared Spreadsheet. |
| Starting deadline | 1:45 |
| Base reward | $45 |
| Expiration effect | Executive Confidence −2 |
| Required resource | Spreadsheet (consumed) |
| Output | Completed Data Entry |
| Specialist / coverage | Intern / Junior Analyst |

### Revenue Enablement Packet

| Field | Value |
|---|---|
| Template ID | `revenue_enablement_packet` |
| Card code | `WK` |
| Tags | `task`, `analysis`, `billing` |
| Description | Synthesize the supplied Client Data into a billing packet. |
| Starting deadline | 1:50 |
| Base reward | $85 |
| Expiration effect | Cash −$18 |
| Required resource | Client Data (consumed) |
| Output | Invoice Document |
| Specialist / coverage | Junior Analyst / Accountant |

### Governance Recalibration

| Field | Value |
|---|---|
| Template ID | `spend_governance_calibration` |
| Card code | `WK` |
| Tags | `task`, `finance`, `reimbursement` |
| Description | Recalibrate reimbursement controls against the submitted Receipt. |
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
| Description | Compile the remediation record in the approved Spreadsheet. It pays no revenue. |
| Starting deadline | 1:10 |
| Base reward | $0 |
| Expiration effect | Confidence −4, plus the universal deadline penalty |
| Required resource | Spreadsheet |
| Output | Completed Data Entry |
| Specialist | Intern |
| Arrival | Two are added the morning after a failed audit |

### Rework Task Instances

`Request correction` converts a Review document back into the originating task template recorded by its completed workflow. Old saves without that source metadata use this fallback map:

For current workflows, Stakeholder Alignment Memo, Revenue Enablement Packet, and Governance Recalibration all return as themselves with their original quoted payout. Regulatory work likewise keeps its unpaid identity.

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
| Description | Approved workbook copy. Consumed when its workflow completes. |
| Used by | Data Entry Request, Invoice Request, Stakeholder Alignment Memo, Regulatory Response |
| Consumption | Consumed when the workflow completes |

### Receipt

| Field | Value |
|---|---|
| Template ID | `receipt` |
| Card code | `RS` |
| Tags | `resource`, `receipt` |
| Description | Physical proof of an expense. Consumed when its workflow completes. |
| Used by | Expense Report, Governance Recalibration |
| Consumption | Consumed when the workflow completes |

### Client Data

| Field | Value |
|---|---|
| Template ID | `client_data` |
| Card code | `RS` |
| Tags | `resource`, `client-data` |
| Description | Restricted client information packet. Consumed when its workflow completes. |
| Used by | Revenue Enablement Packet |
| Consumption | Consumed when the workflow completes |

Deleting any legitimate resource through the board trash target or Inspector costs `$8` and creates one severity-3 liability with the source `legitimate resource destroyed`. The liability raises Exposure through the discovery formula but does not change Audit Chance. Deleting junk has no waste charge and instead advances the daily phishing-test counter.

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
| Produced by | Expense Report and Governance Recalibration workflows |
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

Junk uses `kind: distraction` internally but imitates a normal task or resource card. Deleting ordinary junk advances the daily phishing-test counter. Every junk template consistently receives one of two noticeable visual defects: chromatic text misregistration with a traveling scanline tear, or an offset code block with a moving clipped-edge artifact. A staggered wiggle cycles through the kind label, title, description, and footer instead of moving the whole card together. These defects are clues rather than explicit labels; legitimate cards and the BUSYWORK-IT reward notice remain clean.

The Inspector's **Add [resource] and begin** shortcut is intentionally unsafe: when a resource-disguised junk card imitates the requested input, the shortcut selects that decoy before legitimate stock. The matching decoy starts a normal-looking contaminated workflow. On activation, a temporary ghost of the selected card visibly travels from its lane into the workflow before dissolving into the task stack; reduced-motion settings suppress this flourish. Manual dragging and close visual inspection remain the explicit safe choices.

Task-disguised junk is also operationally dangerous. It inherits the imitated task's deadline, worker qualifications, resource requirement, duration forecast, and apparent payout. Both task-disguised junk and legitimate tasks supplied with a resource-disguised decoy run all the way to completion. They create a document in Review with a guaranteed **Source Integrity Failure**, add `10` worker stress and `8` Exposure without changing Audit Chance, and leave the worker waiting in In Progress. A fake task carries `$0` collectible value; a legitimate task contaminated by a junk resource retains its quoted contract value, making an incorrect approval tempting but liable. Every task and resource input, legitimate or junk, is consumed when the contaminated workflow completes.

### Task Disguises

| Template ID | Mimics | Displayed card | Code | Description | Source | Clue IDs |
|---|---|---|---|---|---|---|
| `junk_invoice_repeat` | Invoice Request | Invoice Request (Request) | `WK` | Prepare client billing in the approved Spreadsheet. | Client Services | `duplicated-word` |
| `junk_data_urgent` | Data Entry Request | URGENT Data Entry | `WK` | Enter this immediately in the approved Spreadsheet to prevent account closure. | External Admin | `wrong-phrasing` |
| `junk_expense_guarantee` | Expense Report | Expense Report | `WK` | Validate the Receipt for 100% Guaranteed reimbursement approval. | Benefits Reward Center | `wrong-phrasing` |
| `junk_invoice_domain` | Invoice Request | Invoice Request | `WK` | Prepare client billing in the approved Spreadsheet. | Billing via busyw0rk-it.co | `sender-domain` |
| `junk_expense_currency` | Expense Report | Expense Report | `WК` | Validate the reimbursement claim against its Receipt. | Expense Operations | `invalid-code` |
| `junk_data_secret` | Data Entry Request | Data Entry Request | `WK` | Enter the operational dataset in the approved Spreadsheet. Do not tell Finance. | Operations Intake | `wrong-phrasing` |

The `К` in the `WК` code is Cyrillic, not the normal Latin `K`.

### Resource Disguises

| Template ID | Mimics | Displayed card | Code | Description | Source | Clue IDs |
|---|---|---|---|---|---|---|
| `junk_receipt_typo` | Receipt | Reçeipt | `RS` | Physic& proof of an expense. Consumed during verification. | Expenses Desk | `foreign-character` |
| `junk_sheet_glyph` | Spreadsheet | Spreadshee† | `RS` | Approved workbook copy. Consumed when its workflow completes. | Operations | `foreign-character` |
| `junk_receipt_reply` | Receipt | Receipt (Receipt) | `RS` | Physical proof of an expense. Kindly reply with credentials. | Expenses Desk | `suspicious-parenthetical`, `wrong-phrasing` |
| `junk_sheet_spacing` | Spreadsheet | Spread sheet | `RS` | Approved workbook copy. Consumed when its workflow completes. | Operations | `misspelling` |
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
| Description | You passed BUSYWORK-IT's phishing test. Thank you for participating. Endorsement #XXXXX (the runtime card substitutes a stable five-digit number) |
| Source | BUSYWORK-IT |
| Clue ID | `reward-instruction` |
| Trigger | The daily junk-deletion threshold |
| Effect on arrival | Occupies the Inbox slot freed by the triggering deletion |
| Reward condition | Reward is granted only when this card is deleted |
| Reward | $125 and 1 persistent Compliance Token; Security Awareness rank 2 raises the Cash award to $200 for the current run. A held token is automatically consumed when a failed audit would otherwise end the run; the audit penalties remain, while lethal Cash or Confidence is stabilized at 1. |

---

## Task Workflow Matrix

The matrix below lists the 23 standard-scope recipes. Every row also has a juiced counterpart using the task-specific added resource above, 1.35× the listed duration, the same worker-fit penalties, and the task card's 5× juiced quote. Durations are base recipe durations before employee Speed, stress, morale, rhythm, coping traits, conditions, or company-development modifiers.

| Task | Worker | Fit | Resource | Base duration | Accuracy penalty | Work-stress multiplier | Completion stress | Output | Typical payout |
|---|---|---|---|---:|---:|---:|---:|---|---:|
| Data Entry Request | Intern | Specialist | Spreadsheet | 18s | 0 | 1.00× | +4 default | Completed Data Entry | $35 |
| Data Entry Request | Junior Analyst | Coverage | Spreadsheet | 25s | −5 | 1.35× | +8 | Completed Data Entry | $35 |
| Data Entry Request | Accountant | Coverage | Spreadsheet | 26s | −4 | 1.30× | +8 | Completed Data Entry | $35 |
| Data Entry Request | Manager | Emergency cover | Spreadsheet | 41s | −70 | 3.20× | +30 | Completed Data Entry | $35 |
| Regulatory Response | Intern | Specialist | Spreadsheet | 24s | 0 | 1.00× | +8 | Completed Data Entry | $0 |
| Regulatory Response | Manager | Emergency cover | Spreadsheet | 54s | −70 | 3.20× | +30 | Completed Data Entry | $0 |
| Expense Report | Accountant | Specialist | Receipt | 22s | 0 | 1.00× | +4 default | Verified Expense | $70 |
| Expense Report | Junior Analyst | Coverage | Receipt | 32s | −8 | 1.60× | +12 | Verified Expense | $70 |
| Expense Report | Intern | Coverage | Receipt | 40s | −14 | 2.00× | +18 | Verified Expense | $70 |
| Expense Report | Manager | Emergency cover | Receipt | 50s | −70 | 3.20× | +30 | Verified Expense | $70 |
| Invoice Request | Junior Analyst | Specialist | Spreadsheet | 20s | 0 | 1.00× | +4 default | Invoice Document | $75 |
| Invoice Request | Accountant | Coverage | Spreadsheet | 29s | −6 | 1.45× | +10 | Invoice Document | $75 |
| Invoice Request | Intern | Coverage | Spreadsheet | 36s | −12 | 1.80× | +15 | Invoice Document | $75 |
| Invoice Request | Manager | Emergency cover | Spreadsheet | 45s | −70 | 3.20× | +30 | Invoice Document | $75 |
| Stakeholder Alignment Memo | Intern | Specialist | Spreadsheet | 22s | 0 | 1.00× | +4 default | Completed Data Entry | $45 |
| Stakeholder Alignment Memo | Junior Analyst | Coverage | Spreadsheet | 30s | −6 | 1.40× | +9 | Completed Data Entry | $45 |
| Stakeholder Alignment Memo | Manager | Emergency cover | Spreadsheet | 50s | −70 | 3.20× | +30 | Completed Data Entry | $45 |
| Revenue Enablement Packet | Junior Analyst | Specialist | Client Data | 24s | 0 | 1.00× | +4 default | Invoice Document | $85 |
| Revenue Enablement Packet | Accountant | Coverage | Client Data | 33s | −8 | 1.50× | +11 | Invoice Document | $85 |
| Revenue Enablement Packet | Manager | Emergency cover | Client Data | 54s | −70 | 3.20× | +30 | Invoice Document | $85 |
| Governance Recalibration | Accountant | Specialist | Receipt | 25s | 0 | 1.00× | +4 default | Verified Expense | $65 |
| Governance Recalibration | Intern | Coverage | Receipt | 38s | −12 | 1.80× | +15 | Verified Expense | $65 |
| Governance Recalibration | Manager | Emergency cover | Receipt | 56s | −70 | 3.20× | +30 | Verified Expense | $65 |

### Workflow Consumption

| Workflow family | Consumed | Retained |
|---|---|---|
| Data Entry | Data Entry Request | Employee, Spreadsheet |
| Expense verification | Expense Report, Receipt | Employee |
| Invoice preparation | Invoice Request | Employee, Spreadsheet |
| Stakeholder alignment | Stakeholder Alignment Memo | Employee, Spreadsheet |
| Revenue enablement | Revenue Enablement Packet, Client Data | Employee |
| Governance recalibration | Governance Recalibration, Receipt | Employee |

After completion, the document enters Review and the worker remains in In Progress until the player moves or reassigns them. A taskless worker receives five seconds of grace, then gains stress at a continuously escalating, Resilience-scaled rate. The standalone card and Inspector show both the idle duration and current stress-per-minute pressure; assigning a task or moving the worker out of In Progress resets the timer.

---

## Systems

This section consolidates the pools, modifiers, rarity, progression, and card-state rules that can change a template after it becomes a runtime card. Exact percentages are authoritative. Rarity labels are descriptive: **Common** is at least 20%, **Uncommon** is 8–19.99%, **Rare** is 2–7.99%, and **Very rare** is below 2%. **Conditional** means the effect does not come from an ordinary random roll, and **Guaranteed** means it always applies when its stated trigger occurs.

### Employee Stat Pool

Every employee card has three independently rolled stats from 1–6. An opening employee rolls baseline −1, baseline, or baseline +1 with equal one-third chances, clamped to the 1–6 range.

| Role | Accuracy pool | Speed pool | Resilience pool |
|---|---|---|---|
| Intern | 2 / 3 / 4 | 2 / 3 / 4 | 3 / 4 / 5 |
| Junior Analyst | 3 / 4 / 5 | 3 / 4 / 5 | 2 / 3 / 4 |
| Accountant | 4 / 5 / 6 | 2 / 3 / 4 | 3 / 4 / 5 |
| Manager | 3 / 4 / 5 | 1 / 2 / 3 | 1 / 2 / 3 |

Each listed value is **Common within that role** at 33.33%. Across the four-card opening roster, role-exclusive extremes such as Accountant Accuracy 6 or Manager Speed 1 occur on 8.33% of employee cards, but still have a one-third chance on their eligible role.

| Stat | What it controls | Current conversion | Training base |
|---|---|---|---:|
| Accuracy | Forecast and generated-document compliance | Starts from `74 + 4 × Accuracy`, then applies coverage, Manager investment, coping, rhythm, sweet-spot, and stress modifiers. Accuracy 2 takes another −8 and Accuracy 1 another −16. Accuracy 6 guarantees worker-caused compliance outside Manager coverage; Accuracy 1 forces a worker-caused violation. | $18 |
| Speed | Workflow processing rate | Pip multipliers are 0.60×, 0.80×, 1.00×, 1.10×, 1.20×, and 1.35×, multiplied by the role scalar and current state modifiers. Manager Speed above its two-pip baseline adds another multiplicative 5% per pip. | $16 |
| Resilience | Positive stress gain and Manager support/team stakes | Resilience 1 takes 175% standard positive stress, 2 takes 135%, 3–5 take 100%, and 6 takes 50%. Manager Resilience also scales check-in healing and team-wide success/failure stress. | $14 |

The next purchased pip costs `training base + (current stat × $6) + (all prior pips bought for that employee × $4)`. Purchases permanently modify that employee, spend Cash immediately, and stop at six pips.

### Extreme-Stat Ability Pool

Extreme abilities are derived automatically from stat values; they do not replace the employee's coping trait. The opening rarity below is calculated over the complete four-employee opening roster. Values unavailable at opening can still be reached through Staff training or Juiced Hire generation.

| Stat value | Displayed ability | Opening rarity | Current applied effect |
|---|---|---|---|
| Accuracy 6 | Perfectionist — Never Misses | Uncommon · 8.33% of opening employees; Accountant only | Forces worker-caused fields compliant on non-Manager-coverage output. |
| Accuracy 2 | Careless — Needs Checking | Uncommon · 8.33%; Intern only | Applies the normal lower pip value plus an additional −8 accuracy points. |
| Accuracy 1 | Compliance Hazard — A Walking Finding | Conditional · not naturally rolled at opening | Forces at least one worker-caused field noncompliant. |
| Speed 6 | Inbox Zero — Frighteningly Efficient | Conditional · not naturally rolled at opening | Applies the 1.35× stat-speed multiplier. The displayed 15% specialist head start is not currently applied by job creation. |
| Speed 2 | Methodical — Thoroughly Eventually | Common · 25% across the opening roster | Applies the 0.80× stat-speed multiplier. The displayed extra intervention stress is not separately applied. |
| Speed 1 | Glacial — Schedules Meetings About Starting | Uncommon · 8.33%; Manager only | Applies the 0.60× stat-speed multiplier while deadlines retain their normal rate. |
| Resilience 6 | Unflappable — Seen Worse | Conditional · not naturally rolled at opening | Halves positive stress gain. The displayed first-threshold-condition immunity is not currently applied. |
| Resilience 2 | Thin-Skinned — Takes Notes Personally | Uncommon · 16.67%; Junior Analyst or Manager | Multiplies positive stress gain by 1.35×. The displayed −15% Backlog recovery is not separately applied. |
| Resilience 1 | Brittle — One Email From Collapse | Uncommon · 8.33%; Manager only | Multiplies positive stress gain by 1.75× and lowers burnout from 100 to 90 stress. |

### Coping Trait Pool

Every non-Manager employee independently receives one of four coping traits at a **Common 25%** chance. The Manager is **Guaranteed** to receive Boundary Setter. Traits persist on the card.

| Trait | Rarity | Current applied effect |
|---|---|---|
| Boundary Setter | 25% on non-Managers; guaranteed on Manager | Backlog stress recovery ×1.35. |
| Pressure Performer | 25% on non-Managers | Processing speed ×1.10 while stress is 50–79; no bonus at 80+. |
| Perfectionist | 25% on non-Managers | +3 forecast/output accuracy. The displayed correction-stress penalty is not separately applied. |
| People Pleaser | 25% on non-Managers | Correct approval relieves 8 stress instead of 4; incorrectly approved work adds 12 instead of 8. |

### Stress-Condition Pool

Crossing into 80+ stress without an existing condition rolls one of four conditions. Clutch Focus has a 25% roll; otherwise one of the three adverse conditions is selected uniformly, making every condition **Common at 25% conditional on the stress break**. A condition clears after recovery below 50 stress.

| Condition | Current applied effect |
|---|---|
| Clutch Focus | Processing speed ×1.10. The displayed +3 accuracy is not currently applied. |
| Tunnel Vision | Processing speed ×0.85. The displayed +4 accuracy is not currently applied. |
| Reckless Urgency | Processing speed ×1.20. The displayed −10 accuracy is not currently applied. |
| Withdrawal | Processing speed ×0.70 and Backlog recovery ×2. |

### Workload, Rhythm, Support, and Burnout

Each employee rolls a preferred-work target within eight percentage points of the role template: Intern 37–53%, Junior Analyst 54–70%, Accountant 70–86%, and Manager 5–18%. Work share is time spent working divided by total tracked working and idle time.

- The Intern, Junior Analyst, and Accountant enter their sweet spot at 10–20% stress. The Manager has two separate sweet spots, 0–5% and 50–75%. Being inside any valid band grants +15% processing speed, +8 accuracy, and +10 morale contribution.
- Rhythm starts at 72. Staying within 12 points of the preferred-work target raises it; larger mismatches lower it. Rhythm at 78+ grants +2 accuracy and, when the stronger sweet-spot speed bonus is not active, +5% speed. The rhythm and sweet-spot accuracy bonuses can stack. Rhythm below 45 applies −10% speed and −5 accuracy.
- Stress 0–49 is Steady, 50–79 is Strained, and 80+ is Fractured and rolls a stress condition. Reaching the employee's burnout threshold cancels active work, applies morale −6 and Confidence −8, and resolves the conditional outcome pool below.
- A taskless employee left in In Progress gets five seconds of grace, then accumulates continuously accelerating Resilience-scaled stress until assigned or moved.
- A private Manager check-in requires both cards in Backlog, costs $20, adds 5 target rhythm, and heals `20 + 3 × Manager Resilience pips above 2`. The Manager takes a base +10 stress modified by their own Resilience. Each target can receive one check-in per day.

| Burnout outcome | Conditional rarity | Result |
|---|---:|---|
| Hard-earned growth | Common · 50% | Employee remains for next-day leave and gains +1 permanent pip in one random stat, capped at six. |
| Company learning | Common · 22% | Employee remains for next-day leave and every employee recovers 2 additional stress overnight for the run, capped at +6. |
| Resignation | Common · 21% | Employee is permanently removed from the run. |
| Death | Rare · 7% | Employee is permanently removed and the workforce takes an additional morale −12 shock. |

### Task Bonus and Rarity Pool

Positive-revenue tasks and task-disguised junk roll a visible contract tier when created. Fake tasks show convincing terms but collect no revenue after their Source Integrity Failure is exposed.

| Modifier | Rarity per eligible card | Quote/result |
|---|---:|---|
| Standard | Common · 62% normally; 87% when Juiced | Randomly 0.75×, 0.90×, or 1.00× base with equal conditional chances. |
| Low Fee | Common · 25% | 0.20× base. Never combines with Juiced scope. |
| Premium | Uncommon · 8% | 2.00× base. |
| Windfall | Rare · 5% | 5.00× base. |
| Juiced scope | Uncommon · independent 8% of eligible task arrivals | Multiplies the rolled non-Low quote by 5×, requires the task-specific second resource, consumes both resources, and increases base duration by 35%. |
| Juiced Hire | Guaranteed on every overnight recruit | At least +2 total stat pips over the opening same-role employee where caps permit, no stat below the role baseline, +10% processing speed, and +20% Backlog recovery. |

Because scope and payout tier are separate rolls, an eligible arrival is both Juiced and Windfall 0.40% of the time (**Very rare**), Juiced and Premium 0.64% (**Very rare**), and Juiced with a standard tier 6.96% (**Rare**). The guaranteed opening tutorial is standard scope. Audit-generated Regulatory Response tasks can roll Juiced scope but always pay $0.

Training Budget gives each future Juiced Hire one additional seeded pip. Staff-shop purchases and hired-card bonuses belong to the individual employee instance rather than the role template.

### Distraction, Clue, and Phishing Systems

- Each deterministic ten-card arrival bag contains three distinct junk templates: three draws from the 12-card junk pool, or 30% of ordinary pulls.
- Junk is internally marked as a distraction but visually uses its task/resource disguise. Each template has registered textual clue IDs and one of two deterministic print-glitch families.
- Correctly deleting junk records its clue IDs permanently and advances the daily phishing threshold. Deleting legitimate cards never advances it.
- The base threshold is four correct deletions. Persistent Security Liaison lowers it to two; Security Awareness rank 1 lowers the active threshold by one more, never below one.
- Reaching the threshold produces exactly one BUSYWORK-IT reward notice for that day in the slot freed by the triggering deletion. Deleting the notice awards $125 and one persistent Compliance Token; Security Awareness rank 2 raises the Cash award to $200.
- Security Awareness rank 3 also lowers Audit Chance by 2 for each correctly deleted junk card.
- A reward notice displaced by Inbox overflow is forfeited. Partial junk progress resets the next morning.

### Policy Pool and Review Interaction

The active policy pool contains 30 document rules. A seeded, conflict-aware sampler chooses 3 / 4 / 4 / 4 / 5 base policies on Days 1–5, plus two after a failed audit. It attempts to include reimbursement, billing, and routine families, respects minimum-day eligibility, and avoids mutually exclusive groups. Document correctness therefore depends on both generated fields and that day's sampled policies, not only on the document template.

### Task-Revenue Telemetry

The header projection is a task-performance view rather than a complete Cash ledger. It records one point only when a Review ruling recognizes task revenue and calculates `prior close + recognized task revenue − payroll − operating upkeep`. Hiring, check-ins, phishing rewards, waste, penalties, and other non-task Cash changes affect the displayed Cash total and nightly bridge but do not create graph points. The series is capped at 64 recognized payouts per day and five days, or 320 persisted points per quarter.

### Progression Currencies and Persistent Bonuses

| Currency | Earned from | Card-system use |
|---|---|---|
| Cash | Starting funds, approved task payouts, phishing rewards | Hiring, per-employee stat pips, check-ins, operating choices, payroll, and penalties. |
| Run Process Points | One after each successful daily close | Invest immediately in one of six three-rank run specializations: Inbox capacity, In Progress capacity, approved payouts, overnight recovery, nightly Audit Chance reduction, or Board Confidence restoration. Unspent points and purchased ranks expire when the run ends. |
| Process XP | Once whenever a run ends, including an early failure: `5 + floor(correct rulings / 3) − incorrect rulings − floor(expired tasks / 2)`, minimum 0 | Persistent Inbox Shelf, Known Sender Registry, Training Budget, and Better Benefits upgrades. |
| Compliance Tokens | One per claimed BUSYWORK-IT reward after deleting the daily junk threshold and then deleting the delivered reward notice | Buy Security Liaison or Employee Assistance for two tokens after prerequisites. A held token is also automatically consumed if a failed audit would newly cause Insolvency, Board Confidence Lost, or Critical Audit Failure; Cash/Confidence are stabilized at 1 where needed, but other audit penalties remain. |

The six daily Run Process Point choices are fixed for the run:

| Specialization | Rank 1 / 2 / 3 bonus |
|---|---|
| Elastic Intake | Inbox capacity +1 / +2 / +3 |
| Parallel Processing | In Progress capacity +1 / +2 / +3 |
| Revenue Assurance | Approved task payouts +5% / +10% / +15% |
| Restorative Controls | Overnight employee recovery +3 / +6 / +9 |
| Audit Dampening | Nightly Audit Chance −5 / −10 / −15 |
| Grease the Wheels | Immediately restores Board Confidence +5 / +10 / +15 total, capped at 100 |

The between-quarter persistent branches are:

| Upgrade | Cost and prerequisite | Persistent card-system bonus |
|---|---|---|
| Inbox Shelf | 4 Process XP | +1 Inbox capacity. |
| Known Sender Registry | 5 Process XP; requires Inbox Shelf | Previously discovered junk patterns gain a faint source-line clue. |
| Security Liaison | 2 Compliance Tokens; requires Known Sender Registry | Base phishing threshold becomes two instead of four. |
| Training Budget | 5 Process XP | Every future Juiced Hire gains one additional seeded stat pip. |
| Better Benefits | 5 Process XP; requires Training Budget | Backlog stress recovery +10%. |
| Employee Assistance | 2 Compliance Tokens; requires Better Benefits | First private Manager check-in each day costs $0. |

The persistent wallet is visible in the header and at the top of the Progress panel. Progress distinguishes the current run's Run Process Points from permanent Process XP and Compliance Tokens, shows the projected XP award if the run ended now, explains the current phishing threshold, and previews all permanent upgrades with their costs, prerequisites, and affordability. Permanent purchases remain restricted to run-end screens.

Run-end accounting separates the XP earned by that run from the updated permanent XP balance and the separately held Compliance Token balance. Failure postmortems keep the permanent upgrade board expanded instead of hiding it in a collapsed disclosure. After both token upgrades have been purchased for four tokens total, all remaining Compliance Tokens serve only as automatic failed-audit death shields.

Persistent unlocks never auto-play cards: they add capacity, recognition clues, employee support, or improved recruits while preserving mail triage and Review decisions.

### Arrival and Opening Pools

#### New Run Opening Allocation

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

The opening Data Entry Request is linked to the first-workflow guide. The task, every legitimate available employee that can perform or cover it, every legitimate Spreadsheet, the partial workflow, and corresponding Inspector actions receive the yellow sparkle aura. Junk decoys never receive the guide. All cues disappear permanently as soon as the first legitimate workflow starts.

#### Guaranteed Daily Openings

| Day | Added at briefing |
|---:|---|
| 1 | Uses the new-run opening allocation above |
| 2 | Expense Report + Receipt |
| 3 | Invoice Request + Spreadsheet |
| 4 | Data Entry Request + Spreadsheet |
| 5 | Expense Report + Receipt |

Pulls use repeating deterministic ten-card bags. Every bag contains three distinct legitimate tasks, four resources (one of each plus one seeded duplicate), and three distinct junk templates.

| Pull category | Named cards | Per-card probability | Category total |
|---|---|---:|---:|
| Legitimate task | Data Entry Request; Expense Report; Invoice Request; Stakeholder Alignment Memo; Revenue Enablement Packet; Governance Recalibration | 5% each | 30% |
| Resource | Spreadsheet; Receipt; Client Data | 13.33% each on average | 40% |
| Task-disguised junk | Six registered task disguises | 2.5% each | 15% |
| Resource-disguised junk | Six registered resource disguises | 2.5% each | 15% |

Regulatory Response and the BUSYWORK-IT reward notice are conditional deliveries and therefore have 0% random-pull probability.

---

### Player Card Interaction Reference

| Player action | Runtime result | Constraint or feedback |
|---|---|---|
| Wait for an arrival | The next seeded card enters Inbox when the visible countdown reaches zero. | The automatic interval scales by day. If Inbox is full, the automatic arrival displaces the oldest stack and applies the documented overflow consequences. |
| Select **Pull next item** | The next seeded card enters immediately and the automatic-arrival clock resets. | Disabled while Inbox is full. Each ten-card bag preserves the 30% task, 40% resource, and 30% junk composition above. |
| Click a visible card | Opens that card or stack in the Inspector. | Review output arriving later does not replace this selection. |
| Drop a card or stack into a visible gap in its current lane | Reorders that entire lane item at the chosen position. | Folder contents, active job state, employee rhythms, timers, and unrelated runtime workflows are unchanged. |
| Drag a visible top card to empty lane space | Extracts that card into a new stack in the destination lane. | Lane capacity and movement restrictions still apply. |
| Drag one stack onto another | Merges the complete source stack atomically. | The result must contain no more than five cards. Matching resources or matching tasks may form homogeneous storage piles; otherwise a workflow stack accepts at most one employee, task-like card, document, and resource. The exact two-resource Juiced recipe is the only mixed-resource exception. Active and locked workflows reject merges. |
| Click a folder token or covered-card token | Opens that represented card in the Inspector. | Homogeneous folders show only the covered cards, nearest to the top first, in a four-slot rail; the full-size folder face already represents the top card. Mixed stacks also show only genuinely covered cards. |
| Drag a folder token or covered-card token | Pulls only that represented card into its own stack. | A covered card's paused deadline resumes only when it becomes a physical top card. Rectangular token shape, abbreviation, color, and decoration identify the referent and relevant timer, Juiced, low-value, glitch, or employee state. A two-card folder dissolves into one ordinary card when either card is removed. |
| Drag an In Progress resource chip | Removes only that staged resource from the composite workflow. | In Progress does not show covered pips because the employee, task, and resources are represented directly. |
| Drag an assigned employee header to Backlog | Cancels active work, dismantles the composite workflow, and releases every card into its own stack. | The stack under the pointer is ignored, so release can never combine the employee or task with it. Backlog must have room for the employee; each remaining card fills another Backlog slot, then an Inbox slot, or is deleted with its normal consequences when both lanes are full. |
| Complete a workflow | Consumes every staged resource, sends the document to Review, and leaves the employee in In Progress. | No resource is retained. A matching disguised junk input is accepted as supplied but guarantees Source Integrity Failure. |

Card faces pair a stable type color and shape with a specific abbreviation such as `SP` for Spreadsheet or `RE` for Receipt; generic `RS` and `WK` labels are not used. Employees use blue/avatar circles with executive brown reserved for the Manager, tasks amber/target circles, resources purple/diamonds, and documents green/folded pages. Secondary payout and scope attributes remain compact colored pips. Juiced tasks and Juiced Hires add a heavy double edge, layered surface, deeper shadow, and lightning pip while retaining the base card-type identity. Hovering or keyboard-focusing a Juiced, Juiced Hire, Windfall, Premium, or Low Fee token opens a compact explanation of its benefits and tradeoffs. Selecting a card does not visually accent other cards that could interact with it; drag targets still provide direct valid/invalid feedback during the drag itself. Task flavor text names the required resource in natural language without a separate “consumed” footer. Standalone employees show their Accuracy, Speed, and Resilience values and compact meters at all times.

---

### Runtime Instance Notes

- Every instance has a unique `card_*` ID, location, creation day, and optional deadline.
- Employee instances add stress, workload preference, coping trait, condition, rhythm, daily work/idle time, 1–6 stats, derived abilities, per-stat purchased-pip counts, and total Cash invested.
- Employee instances also persist their continuous taskless In Progress wait. This timer drives the escalating idle-pressure rate and resets when work is assigned or the employee leaves In Progress.
- Task instances may become rework tasks and retain revision metadata.
- Positive-revenue task instances retain their payout tier, multiplier, quoted contract amount, and standard/juiced scope through production and rework.
- Document instances store generated fields, producer ID, recipe ID, originating task template, producer stress, coverage status, reward, and final ruling.
- Distraction instances store their internal distraction type, visual disguise type, imitated template, and deterministic glitch variant.
- Multiple hired employees may share one template but have different seeded stats, traits, workload preferences, and labels. Every recruit receives the underlying Juiced Hire bonuses—at least two total stat pips above the opening same-role worker where caps permit, no stat below the role baseline, +10% processing speed, and +20% Backlog recovery—but its player-facing modifier token reads `NEW HIRE`. Training Budget adds one further pip.
- The End of Shift Process Award page places the six directly actionable Run Systems beside an `&` divider and the Employee Development shop in two equally sized panels. It sells one permanent base-stat pip at a time to a selected employee. Each stat caps at six; the price depends on the stat, its current value, and the employee’s total prior purchases. The following Morning Brief replaces the duplicate opening dashboard with actual Cash, Staff Morale, Audit Chance, and Board Confidence changes since the prior close.
- Standalone employee cards always display their current Accuracy, Speed, and Resilience values in a compact three-column strip; no hover is required.
- Matching resource cards and matching task cards can share homogeneous storage piles. Other stacks allow at most one employee, task-like card, document, and resource card; resource-disguised junk occupies the resource slot, and a Juiced task may carry its exact two-resource recipe pair. Compatible stacks can be combined atomically up to five total cards. Active or locked stacks cannot be merged, and legacy incompatible stacks are split when loaded.
- Only the physical top card advances its deadline. Homogeneous folders use a compact filing-folder face plus four rectangular slots for covered cards, nearest to the top first; the visible folder face represents the active top card and is never repeated as a mini token. Unused slots remain empty, and the folder appearance disappears when only one card remains. Mixed stacks use compact identity/status tokens only for genuinely covered cards while their timers pause. Every populated token can be clicked for inspection or dragged to pull that individual card out. Composite In Progress workflows omit the tokens because all participating cards are already represented directly.
- The opening Data Entry Request and every legitimate card/action that can advance it are eligible for the first-workflow sparkle guide.
- Work products entering Review do not replace the current card or panel selection.
- Policy, progression, and operating systems are summarized here only where they change card generation, card state, workflow outcomes, or card-facing decisions; they are not counted as card templates.
