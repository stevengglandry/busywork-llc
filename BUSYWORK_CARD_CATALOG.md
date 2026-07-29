# BUSYWORK Card and Task Catalog

Documentation ground truth: this catalog is the detailed contract paired with the quick reference in `README.md`. It is reconciled against the current `Content.cards`, `Content.recipes`, stat/trait pools, balance and progression constants, opening allocation, and runtime rules in `index.html`. Historical planning files do not override this catalog.

This file catalogs card **templates** and the systems that modify their runtime instances. Runtime cards receive unique IDs such as `card_17`, and the game may create multiple instances from the same template through arrivals, hiring, rework, and completed workflows.

## Catalog Summary

| Card type | Templates |
|---|---:|
| Employees | 4 |
| Tasks | 9 |
| Resources | 3 |
| Review documents | 3 |
| Junk distractions | 12 |
| Phishing reward distractions | 1 |
| **Total** | **32** |

The game defines 52 task workflows: 27 standard-scope recipes and 25 Juiced counterparts for eligible ordinary/regulatory work. Compliance Check adds an Intern specialist recipe and emergency Manager coverage, but never rolls Juiced scope.

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
| Specialist tasks | Expense Report, Approve Purchase Request, Governance Recalibration |
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

The Manager begins as a deliberately poor emergency task worker, but Cash investment at an Employee Development Workshop can turn him into a risky workhorse and stress healer. Each Accuracy pip above baseline adds 12 coverage-chance points on top of the ordinary stat gain; each Speed pip above baseline adds 5% multiplicative processing speed; Resilience raises check-in healing from 20 to as much as 32. Depending on his seeded Resilience, a compliant Manager output relieves every employee by 8–26 stress (10 at the baseline stat), while a noncompliant or junk-tainted output stresses everyone by 18–8 (16 at baseline). The Workshop roster exposes the live values and next Cash prices before purchase.

---

## Task Cards

Task cards are work requests. A valid In Progress stack combines one task, a compatible employee, and its required resource.

Every natural task or document deadline miss applies Confidence −6 in addition to any template-specific expiration effect listed below. Deliberately deleting a Review document applies the same global penalty. Ordinary valid task deletion rolls 75% no consequence, 12.5% Confidence −2, and 12.5% +1 Liability; Stakeholder Alignment Memo uses 50% none / 50% Confidence −2, while Governance Recalibration uses 50% none / 50% +1 Liability. These penalties leave Audit Chance unchanged.

Each positive-revenue task instance receives a contract rate when created. Windfall cards (5%) pay exactly 5× the task type's base reward and use a gold treatment; Premium cards (8%) pay 2×; Low Fee cards (25%) pay 20% and use a muted treatment; the remaining 62% pay 0.75×, 0.9×, or 1×. This keeps the long-run expected multiplier near 1× while making individual requests much more consequential. The card and Inspector show the quote before assignment. Confidence scales the quoted value only when the completed document is approved, and correction preserves the original quote. Task-disguised junk receives the same convincing visual/value roll but still pays nothing when exposed.

Eligible task arrivals have an 8% chance to be **Juiced**. Juiced scope never combines with LOW FEE; it multiplies a standard, premium, or windfall quote by 5×, takes 35% longer, requires a second resource, consumes both resources, and survives production and correction. Older 1.75× Juiced rewards and saved JUICED/LOW contracts migrate to the current 5× quote on load. All resources in every completed workflow are consumed; canceled workflows consume nothing, and correction requires fresh inputs. The guaranteed opening tutorial remains standard scope; audit-generated Regulatory Response arrivals use the same rare roll as ordinary tasks.

| Task type | Base | Low Fee (20%) | Premium (2×) | Windfall (5×) |
|---|---:|---:|---:|---:|
| Data Entry Request | $35 | $7 | $70 | $175 |
| Expense Report | $70 | $14 | $140 | $350 |
| Invoice Request | $75 | $15 | $150 | $375 |
| Approve Purchase Request | $80 | $16 | $160 | $400 |
| Stakeholder Alignment Memo | $45 | $9 | $90 | $225 |
| Revenue Enablement Packet | $85 | $17 | $170 | $425 |
| Governance Recalibration | $65 | $13 | $130 | $325 |
| Regulatory Response | $0 fixed | $0 | $0 | $0 |
| Compliance Check | $0 fixed | Not eligible | Not eligible | Not eligible |

### Juiced Scope Requirements

| Task type | Standard resource | Added juiced resource | Standard-rate juiced quote |
|---|---|---|---:|
| Data Entry Request | Spreadsheet | Client Data | $175 |
| Expense Report | Receipt | Spreadsheet | $350 |
| Invoice Request | Spreadsheet | Client Data | $375 |
| Approve Purchase Request | Receipt | Client Data | $400 |
| Stakeholder Alignment Memo | Spreadsheet | Client Data | $225 |
| Revenue Enablement Packet | Client Data | Spreadsheet | $425 |
| Governance Recalibration | Receipt | Spreadsheet | $325 |
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

### Approve Purchase Request

| Field | Value |
|---|---|
| Template ID | `approve_purchase_request` |
| Card code | `PO` |
| Tags | `task`, `finance`, `procurement` |
| Description | Review a purchase request and its Receipt for device fit, business purpose, origin, and compliance filing. |
| Starting deadline | 2:00 |
| Base reward | $80 |
| Expiration effect | Executive Confidence −3 |
| Required resource | Receipt |
| Output | Verified Expense with Item Description, Reason, Country of Origin, and Compliance Paperwork Filed fields |
| Specialist | Accountant |
| Coverage worker | Manager emergency coverage |
| Always-applicable controls | Apple device ecosystem; valid client-presentation purpose; non-embargoed origin; compliance paperwork filed |

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
| Arrival | Legacy conditional work retained for save/rework compatibility; current audit bands use Compliance Check instead |

### Compliance Check

| Field | Value |
|---|---|
| Template ID | `compliance_check` |
| Card code | `CC` |
| Tags | `task`, `admin`, `mandatory`, `regulatory` |
| Description | Complete the mandatory Compliance Check in the approved Spreadsheet. |
| Starting deadline | 1:15 |
| Base reward | $0 |
| Missed/deleted consequence | +1 Liability |
| Required resource | Spreadsheet |
| Output | Completed Data Entry |
| Specialist / coverage | Intern / emergency Manager coverage |
| Arrival | Four are inserted at seeded positions throughout the day after a level-3 audit |

### Rework Task Instances

`Request correction` converts a Review document back into the originating task template recorded by its completed workflow. Old saves without that source metadata use this fallback map:

For current workflows, Stakeholder Alignment Memo, Revenue Enablement Packet, and Governance Recalibration all return as themselves with their original quoted payout. Regulatory Response and Compliance Check likewise keep their unpaid identities.

| Review document | Rework task |
|---|---|
| Completed Data Entry | Data Entry Request |
| Verified Expense | Expense Report |
| Invoice Document | Invoice Request |
| Regulatory Completed Data Entry | Regulatory Response (`$0` reward retained) |
| Compliance Check Completed Data Entry | Compliance Check (`$0` reward retained) |

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
| Used by | Data Entry Request, Invoice Request, Stakeholder Alignment Memo, Regulatory Response, Compliance Check |
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

Deleting any legitimate resource through the board trash target or Inspector costs `$8` and creates one Liability with the source `legitimate resource destroyed`. Audit Chance is unchanged. Deleting junk has no waste charge and instead advances the daily phishing-test counter.

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
| Produced by | Data Entry Request, Stakeholder Alignment Memo, Regulatory Response, and Compliance Check workflows |
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

Task-disguised junk is also operationally dangerous. It inherits the imitated task's deadline, worker qualifications, resource requirement, duration forecast, and apparent payout. Both task-disguised junk and legitimate tasks supplied with a resource-disguised decoy run all the way to completion. They create a document in Review with a guaranteed **Source Integrity Failure**, add `10` worker stress, and leave the worker waiting in In Progress. Audit Chance and Liability remain unchanged until the player rules on the document. A fake task carries `$0` collectible value; a legitimate task contaminated by a junk resource retains its quoted contract value, making an incorrect approval tempting but liable. Every task and resource input, legitimate or junk, is consumed when the contaminated workflow completes.

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
| Reward | $125 and 1 persistent Compliance Token. Every held token reduces effective nightly Audit Chance by 5 points and is not consumed. |

---

## Task Workflow Matrix

The matrix below lists the 27 standard-scope recipes. The 25 Juiced-eligible rows also have a counterpart using the task-specific added resource above, 1.35× the listed duration, the same worker-fit penalties, and the task card's 5× juiced quote. Compliance Check and its Manager coverage remain standard-only. Durations are base recipe durations before employee Speed, Stress, Morale, sweet spots, coping traits, conditions, or company-development modifiers.

| Task | Worker | Fit | Resource | Base duration | Accuracy penalty | Work-stress multiplier | Completion stress | Output | Typical payout |
|---|---|---|---|---:|---:|---:|---:|---|---:|
| Data Entry Request | Intern | Specialist | Spreadsheet | 18s | 0 | 1.00× | +4 default | Completed Data Entry | $35 |
| Data Entry Request | Junior Analyst | Coverage | Spreadsheet | 25s | −5 | 1.35× | +8 | Completed Data Entry | $35 |
| Data Entry Request | Accountant | Coverage | Spreadsheet | 26s | −4 | 1.30× | +8 | Completed Data Entry | $35 |
| Data Entry Request | Manager | Emergency cover | Spreadsheet | 41s | −70 | 3.20× | +30 | Completed Data Entry | $35 |
| Regulatory Response | Intern | Specialist | Spreadsheet | 24s | 0 | 1.00× | +8 | Completed Data Entry | $0 |
| Regulatory Response | Manager | Emergency cover | Spreadsheet | 54s | −70 | 3.20× | +30 | Completed Data Entry | $0 |
| Compliance Check | Intern | Specialist | Spreadsheet | 22s | 0 | 1.00× | +7 | Completed Data Entry | $0 |
| Compliance Check | Manager | Emergency cover | Spreadsheet | 50s | −70 | 3.20× | +30 | Completed Data Entry | $0 |
| Expense Report | Accountant | Specialist | Receipt | 22s | 0 | 1.00× | +4 default | Verified Expense | $70 |
| Expense Report | Junior Analyst | Coverage | Receipt | 32s | −8 | 1.60× | +12 | Verified Expense | $70 |
| Expense Report | Intern | Coverage | Receipt | 40s | −14 | 2.00× | +18 | Verified Expense | $70 |
| Expense Report | Manager | Emergency cover | Receipt | 50s | −70 | 3.20× | +30 | Verified Expense | $70 |
| Invoice Request | Junior Analyst | Specialist | Spreadsheet | 20s | 0 | 1.00× | +4 default | Invoice Document | $75 |
| Invoice Request | Accountant | Coverage | Spreadsheet | 29s | −6 | 1.45× | +10 | Invoice Document | $75 |
| Invoice Request | Intern | Coverage | Spreadsheet | 36s | −12 | 1.80× | +15 | Invoice Document | $75 |
| Invoice Request | Manager | Emergency cover | Spreadsheet | 45s | −70 | 3.20× | +30 | Invoice Document | $75 |
| Approve Purchase Request | Accountant | Specialist | Receipt | 23s | 0 | 1.00× | +4 default | Verified Expense | $80 |
| Approve Purchase Request | Manager | Emergency cover | Receipt | 52s | −70 | 3.20× | +30 | Verified Expense | $80 |
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
| Purchase approval | Approve Purchase Request, Receipt | Employee |
| Invoice preparation | Invoice Request | Employee, Spreadsheet |
| Stakeholder alignment | Stakeholder Alignment Memo | Employee, Spreadsheet |
| Revenue enablement | Revenue Enablement Packet, Client Data | Employee |
| Governance recalibration | Governance Recalibration, Receipt | Employee |

After completion, the document enters Review and the worker remains in In Progress until the player moves or reassigns them. A taskless worker receives five seconds of grace, then gains stress at a continuously escalating, Resilience-scaled rate. The standalone card and Inspector show both the idle duration and current stress-per-minute pressure; assigning a task or moving the worker out of In Progress resets the timer.

---

## Systems

This section consolidates the pools, modifiers, rarity, progression, and card-state rules that can change a template after it becomes a runtime card. Exact percentages are authoritative. Rarity labels are descriptive: **Common** is at least 20%, **Uncommon** is 8–19.99%, **Rare** is 2–7.99%, and **Very rare** is below 2%. **Conditional** means the effect does not come from an ordinary random roll, and **Guaranteed** means it always applies when its stated trigger occurs.

Systems quick index:

- [Company Resources and Meta Stats](#company-resources-and-meta-stats)
- [Employee Stat Pool](#employee-stat-pool)
- [Extreme-Stat Ability Pool](#extreme-stat-ability-pool)
- [Coping Trait Pool](#coping-trait-pool)
- [Stress-Condition Pool](#stress-condition-pool)
- [Workload, Support, and Burnout](#workload-support-and-burnout)
- [Task Bonus and Rarity Pool](#task-bonus-and-rarity-pool)
- [Distraction, Clue, and Phishing Systems](#distraction-clue-and-phishing-systems)
- [Policy Pool and Review Interaction](#policy-pool-and-review-interaction)
- [Review Decision Pool](#review-decision-pool)
- [Audit Severity Consequences](#audit-severity-consequences)
- [Progression Currencies and Persistent Bonuses](#progression-currencies-and-persistent-bonuses)
- [Corporate Roadmap](#corporate-roadmap)
- [Arrival and Opening Pools](#arrival-and-opening-pools)
- [Player Card Interaction Reference](#player-card-interaction-reference)

### Company Resources and Meta Stats

| Meter or resource | Starting value | Current calculation and use | Failure or persistence |
|---|---:|---|---|
| Cash | $450 | Cash on hand: opening funds + recognized task payouts and other income − payroll, upkeep, route costs, support costs, fines, and other expenses. Recognized task payout is `quoted payout × (0.8 + 0.4 × Confidence / 100)`, rounded. | Cash at or below $0 after operating close ends the run. Cash is run-only. |
| Staff Morale | 70 displayed at opening | Recalculated as `clamp(100 − average employee stress + average sweet-spot contribution + event modifier, 0, 100)`. It multiplies processing speed by 1.10 at 80–100, 1.05 at 65–79, 1.00 at 40–64, 0.85 at 20–39, and 0.70 below 20. It also applies Accuracy −10 at 20–39 and −20 below 20; Accuracy 6 specialist output remains guaranteed except during Bad Vibes. | Does not directly end the run. |
| Employee Stress | 0 per newly created employee | Individual 0–100 meter. Average workflow Stress multiplies task duration by 1.50× at 50–79 and 2.00× at 80+. Resilience scales positive Stress gain. Ordinary employee sweet spots are 10–20%; the Manager's are 0–5% and 50–75%. Any valid band grants speed +15%, Accuracy +8 points, and a +10 contribution to the averaged Morale formula. | Burnout occurs at 100, or 90 for Resilience 1, and rolls a burnout outcome. Stress is run-only. |
| Audit Chance | 50% | Rolled nightly after subtracting 5 points per held Compliance Token and 5 points per run-only Audit Dampening pip. Accurate approval removes 1 point. If an audit occurs, finding chance is `effective Liability ÷ elapsed days`, capped at 100%, where `effective Liability = max(0, open Liability − held Compliance Tokens)`. A finding uses effective Liability to select Severity 1–5. | Severity 5 ends the run. Tokens persist and are not consumed; Audit Dampening expires with the run. |
| Board Confidence | 75 | Trust of the Board. It scales recognized payouts from 80% at 0 to 120% at 100. Accurate approvals and clean audits raise it; failed audits, mistakes, staffing decisions, and route tradeoffs can reduce it. Sweet-spot time affects Morale rather than Confidence directly. | Confidence at or below 0 ends the run. Confidence is run-only. |
| XP | Persistent wallet | +1 at every survived day close. It buys permanent upgrades only after reaching the successful Day 5 Quarterly Review. | Persists across runs in browser `localStorage` until Settings reset. |
| Talent Points | 0 at run start | +1 at every survived day close. The guaranteed phase-1 Talent Tree spends them on Run Process Upgrades. | Points and purchased ranks are run-only and expire at run end. |
| Workshop access | Location reward | Completing an Employee Development Workshop workday inserts a separate Cash-funded employee stat shop after that night's Talent Tree. | Available only during that completed location's overnight sequence. Employee stat gains last for the run. |
| Night Planning access | Location reward | Completing Margin Review inserts a choice among Pizza Party, Compliance Training, and Quiet Recovery after that night's Talent Tree. | Choose exactly one; available only for the completed Margin Review workday. |
| Juiced Recruitment access | Location reward | Completing Talent Pipeline inserts a paid choice among one Intern, Junior Analyst, or Accountant after that night's Talent Tree. | Choose at most one; the hired employee and bonuses last for the run. |
| Compliance Tokens | Persistent wallet | Earned from claimed five-junk phishing-test rewards, Report Defect/Policy Sweep, and completed 10-minute days. Each held token subtracts 5 Audit Chance points and offsets one open Liability before finding chance and Severity are calculated. | Persists in browser `localStorage`; audits never consume tokens. |

Stress sweet spots and preferred workload are separate systems. Preferred workload compares working time with idle time and affects Stress accumulation and Backlog recovery.

### Employee Stat Pool

Every employee card has three independently rolled stats from 1–6. An opening employee rolls baseline −1, baseline, or baseline +1 with equal one-third chances, clamped to the 1–6 range.

| Role | Accuracy pool | Speed pool | Resilience pool |
|---|---|---|---|
| Intern | 2 / 3 / 4 | 2 / 3 / 4 | 3 / 4 / 5 |
| Junior Analyst | 3 / 4 / 5 | 3 / 4 / 5 | 2 / 3 / 4 |
| Accountant | 4 / 5 / 6 | 2 / 3 / 4 | 3 / 4 / 5 |
| Manager | 3 / 4 / 5 | 1 / 2 / 3 | 1 / 2 / 3 |

Each listed value is **Common within that role** at 33.33%. Across the four-card opening roster, role-exclusive extremes such as Accountant Accuracy 6 or Manager Speed 1 occur on 8.33% of employee cards, but still have a one-third chance on their eligible role.

| Stat | What it controls | Current conversion |
|---|---|---|
| Accuracy | Forecast and generated-document compliance | Starts from `74 + 4 × Accuracy`, then applies coverage, Manager investment, coping, sweet-spot, Stress, low-Morale, and Bad Vibes modifiers. Accuracy 2 takes another −8 and Accuracy 1 another −16. Accuracy 6 guarantees worker-caused compliance outside Manager coverage except during Bad Vibes; Accuracy 1 forces a worker-caused violation. |
| Speed | Workflow processing rate | Pip multipliers are 0.60×, 0.80×, 1.00×, 1.10×, 1.20×, and 1.35×, multiplied by the role scalar and current state modifiers. Manager Speed above its two-pip baseline adds another multiplicative 5% per pip. |
| Resilience | Positive stress gain and Manager support/team stakes | Resilience 1 takes 175% standard positive stress, 2 takes 135%, 3–5 take 100%, and 6 takes 50%. Manager Resilience also scales check-in healing and team-wide success/failure stress. |

Completing an Employee Development Workshop workday inserts a stat shop after the guaranteed Talent Tree and before roadmap routing. Each purchase spends Cash and raises one current employee's selected stat by one pip for the rest of the run, up to six. The price is `stat base + current stat × $6 + total prior Workshop pips on that employee × $4`; Accuracy, Speed, and Resilience use bases of $18, $16, and $14 respectively. The shop displays the live effect and next price before purchase.

### Extreme-Stat Ability Pool

Extreme abilities are derived automatically from stat values; they do not replace the employee's coping trait. The opening rarity below is calculated over the complete four-employee opening roster. Values unavailable at opening can still be reached through Workshop training.

| Stat value | Displayed ability | Opening rarity | Current applied effect |
|---|---|---|---|
| Accuracy 6 | Perfectionist — Never Misses | Uncommon · 8.33% of opening employees; Accountant only | Forces worker-caused fields compliant on non-Manager-coverage output except during Bad Vibes, when its −10 Accuracy modifier reduces the result to 90%. |
| Accuracy 2 | Careless — Needs Checking | Uncommon · 8.33%; Intern only | Applies the normal lower pip value plus an additional −8 accuracy points. |
| Accuracy 1 | Compliance Hazard — A Walking Finding | Conditional · not naturally rolled at opening | Forces at least one worker-caused field noncompliant. |
| Speed 6 | Inbox Zero — Frighteningly Efficient | Conditional · not naturally rolled at opening | Applies the 1.35× stat-speed multiplier. |
| Speed 2 | Methodical — Thoroughly Eventually | Common · 25% across the opening roster | Applies the 0.80× stat-speed multiplier. |
| Speed 1 | Glacial — Schedules Meetings About Starting | Uncommon · 8.33%; Manager only | Applies the 0.60× stat-speed multiplier while deadlines retain their normal rate. |
| Resilience 6 | Unflappable — Seen Worse | Conditional · not naturally rolled at opening | Halves positive stress gain. |
| Resilience 2 | Thin-Skinned — Takes Notes Personally | Uncommon · 16.67%; Junior Analyst or Manager | Multiplies positive stress gain by 1.35×. |
| Resilience 1 | Brittle — One Email From Collapse | Uncommon · 8.33%; Manager only | Multiplies positive stress gain by 1.75× and lowers burnout from 100 to 90 stress. |

### Coping Trait Pool

Every non-Manager employee independently receives one of four coping traits at a **Common 25%** chance. The Manager is **Guaranteed** to receive Boundary Setter. Traits persist on the card.

| Trait | Rarity | Current applied effect |
|---|---|---|
| Boundary Setter | 25% on non-Managers; guaranteed on Manager | Backlog stress recovery ×1.35. |
| Pressure Performer | 25% on non-Managers | Processing speed ×1.10 while stress is 50–79; no bonus at 80+. |
| Perfectionist | 25% on non-Managers | +3 forecast/output accuracy. |
| People Pleaser | 25% on non-Managers | Correct approval relieves 8 stress instead of 4; incorrectly approved work adds 12 instead of 8. |

### Juiced Hire Bonus

Juiced Hire is a **location-exclusive special bonus**, not a random opening attribute. It appears only in the New Hire reward after completing Talent Pipeline. The menu always offers the three eligible roles when Cash, the eight-employee roster limit, and Backlog capacity permit them; the player may hire exactly one or skip.

| Candidate | Hiring cost | Daily payroll | Guaranteed bonus |
|---|---:|---:|---|
| Administrative Intern | $60 | $15 | At least +2 total stat pips versus this run's starting Intern, with no stat below the Intern baseline; processing speed ×1.10; Backlog recovery ×1.20. |
| Junior Analyst | $110 | $24 | At least +2 total stat pips versus this run's starting Junior Analyst, with no stat below the role baseline; processing speed ×1.10; Backlog recovery ×1.20. |
| Accountant | $145 | $28 | At least +2 total stat pips versus this run's starting Accountant, with no stat below the role baseline; processing speed ×1.10; Backlog recovery ×1.20. |

The employee's rolled coping trait and any extreme-stat abilities still apply. Juiced Hire lasts only for the current run and is visually identified by a heavy purple border and lightning badge.

### Stress-Condition Pool

Crossing into 80+ stress without an existing condition rolls one of four conditions. Clutch Focus has a 25% roll; otherwise one of the three adverse conditions is selected uniformly, making every condition **Common at 25% conditional on the stress break**. A condition clears after recovery below 50 stress.

| Condition | Current applied effect |
|---|---|
| Clutch Focus | Processing speed ×1.10. |
| Tunnel Vision | Processing speed ×0.85. |
| Reckless Urgency | Processing speed ×1.20. |
| Withdrawal | Processing speed ×0.70 and Backlog recovery ×2. |

### Workload, Support, and Burnout

Each employee rolls a preferred-work target within eight percentage points of the role template: Intern 37–53%, Junior Analyst 54–70%, Accountant 70–86%, and Manager 5–18%. Work share is time spent working divided by total tracked working and idle time.

- The Intern, Junior Analyst, and Accountant enter their sweet spot at 10–20% stress. The Manager has two separate sweet spots, 0–5% and 50–75%. Being inside any valid band grants +15% processing speed, +8 accuracy, and +10 morale contribution.
- Stress 0–49 is Steady, 50–79 is Strained, and 80+ is Fractured and rolls a stress condition. Reaching the employee's burnout threshold cancels active work, applies morale −6 and Confidence −8, and resolves the conditional outcome pool below.
- A taskless employee left in In Progress gets five seconds of grace, then accumulates continuously accelerating Resilience-scaled stress until assigned or moved.
- A private Manager check-in requires both cards in Backlog, costs $20, and heals `20 + 3 × Manager Resilience pips above 2`. The Manager takes a base +10 Stress modified by their own Resilience. Each target can receive one check-in per day.

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

Because scope and payout tier are separate rolls, an eligible arrival is both Juiced and Windfall 0.40% of the time (**Very rare**), Juiced and Premium 0.64% (**Very rare**), and Juiced with a standard tier 6.96% (**Rare**). The guaranteed opening tutorial is standard scope. Regulatory Response is retained for legacy/save compatibility; the current audit system generates non-Juiced Compliance Checks instead.

### Distraction, Clue, and Phishing Systems

- Each deterministic ten-card arrival bag normally contains three distinct junk templates: three draws from the 12-card junk pool, or 30% of ordinary pulls. Spam Intake Filter permanently changes the mix to two junk cards, four tasks, and four resources.
- Junk is internally marked as a distraction but visually uses its task/resource disguise. Each template has registered textual clue IDs and one of two deterministic print-glitch families.
- Correctly deleting junk records its clue IDs permanently and advances the daily phishing threshold. Deleting legitimate cards never advances it.
- The threshold is five correct deletions each day.
- Reaching the threshold produces exactly one BUSYWORK-IT reward notice for that day in the slot freed by the triggering deletion. Deleting the notice awards $125 and one persistent Compliance Token.
- Every held Compliance Token subtracts 5 points from the nightly Audit Chance roll and offsets one open Liability before finding chance and Severity are calculated, without being consumed.
- A reward notice displaced by Inbox overflow is forfeited. Partial junk progress resets the next morning.

### Policy Pool and Review Interaction

The active policy pool contains 34 controls. A seeded, conflict-aware sampler chooses 3 / 4 / 4 / 4 / 5 base policies on Days 1–5, plus the consequence-band penalty after an audit. It attempts to include reimbursement, billing, and routine families, respects minimum-day eligibility, and avoids mutually exclusive groups. The four purchase controls always apply to Approve Purchase Request output in addition to that day's sampled policies.

The in-game **Policies** panel shows both today’s active controls and this complete library. “Day” is the first day the control may enter the seeded pool. Policies sharing a conflict group cannot be active together. Purchase controls are always applied to purchase-request output and do not consume a daily sampled slot.

| Policy | Family | Day | Severity | Compliance requirement | Conflict group |
|---|---|---:|---:|---|---|
| Financial Authorization 4.2 | Reimbursement | 1 | 8 | Expenses over $300 require a Manager signature. | — |
| Evidence Standard 2.1 | Reimbursement | 1 | 6 | A receipt must be attached. | — |
| Client Suspension Notice | Client | 1 | 9 | Client C-882 may not be released. | — |
| Billing Cap A | Billing | 1 | 6 | Invoice amount must be $2,500 or less. | `invoice-cap` |
| Billing Cap B | Billing | 2 | 5 | Invoice amount must be $3,500 or less. | `invoice-cap` |
| Standard Terms | Billing | 1 | 5 | Invoice terms must be Net 30. | `terms` |
| Accelerated Terms | Billing | 3 | 5 | Invoice terms must be Net 15. | `terms` |
| Release Authority | Billing | 1 | 7 | Invoice must be authorized. | — |
| Source Registry | Routine | 1 | 6 | Source must be Operations Intake. | `data-source` |
| Variance Control | Routine | 1 | 6 | Variance must be “No material variance.” | — |
| Minimum Batch | Routine | 1 | 4 | Record count must be at least 60. | `records` |
| Maximum Batch | Routine | 2 | 4 | Record count must be 110 or less. | `records` |
| Control Parity | All documents | 3 | 3 | Control ID must end in an even digit. | — |
| Expense Ceiling | Reimbursement | 1 | 5 | Reimbursement must be $450 or less. | `expense-cap` |
| Tight Expense Ceiling | Reimbursement | 4 | 6 | Reimbursement must be $350 or less. | `expense-cap` |
| Preferred Client Day | Client | 4 | 7 | Only client C-321 may be released. | `client-rule` |
| Client Hold | Client | 2 | 7 | Client C-555 may not be released. | `client-rule` |
| Fatigue Review | All documents | 3 | 5 | Producer Stress must be 75 or less. | `fatigue-limit` |
| Role Boundary Review | All documents | 3 | 5 | Out-of-role coverage requires a Manager signature. | — |
| Whole-Dollar Filing | Financial | 2 | 3 | Amount must be a whole-dollar value. | — |
| Materiality Threshold | Reimbursement | 2 | 4 | Reimbursement must be at least $125. | — |
| Spend Harmonization Grid | Reimbursement | 3 | 4 | Reimbursement must use $25 increments. | — |
| Revenue Materiality Floor | Billing | 2 | 5 | Invoice must be at least $1,000. | — |
| Commercial Packaging Standard | Billing | 3 | 4 | Invoice must use $100 increments. | — |
| Strategic Liquidity Window | Billing | 4 | 6 | Invoice terms must be Net 45. | `terms` |
| Enterprise Batch Floor | Routine | 3 | 5 | Record count must be at least 75. | `records` |
| Five-Point Normalization | Routine | 2 | 4 | Record count must be divisible by five. | — |
| Revenue Operations Mandate | Routine | 4 | 6 | Source must be Revenue Operations. | `data-source` |
| Cognitive Load Ceiling | All documents | 4 | 6 | Producer Stress must be 60 or less. | `fatigue-limit` |
| Core-Competency Utilization | All documents | 3 | 6 | Work completed through role coverage is noncompliant. | — |
| Device Ecosystem Standard | Purchase | Always | 7 | Item description must identify an Apple device. | — |
| Business Purpose Test | Purchase | Always | 8 | Reason must be “Client presentation replacement.” | — |
| Embargoed Origin Control | Purchase | Always | 10 | Country of origin may not be North Korea or Iran. | — |
| Procurement Filing Rule | Purchase | Always | 7 | Compliance paperwork must be filed. | — |

### Review Decision Pool

All task and resource inputs are consumed when a workflow finishes, before its document reaches Review.

| Decision | Policy-aligned use | Mistake or tradeoff |
|---|---|---|
| Approve | Accurate output moves to Done, pays Confidence-scaled Cash, grants Confidence +2, reduces Audit Chance by 1, and relieves producer stress by 4 (8 for People Pleaser). | Inaccurate output still pays but creates +1 Liability, adds producer stress 8 (12 for People Pleaser), and applies Morale modifier −1. Audit Chance is unchanged. |
| Request correction | Inaccurate, salvageable output returns to Backlog as its originating task with a 40% shorter deadline, requires fresh resources, and gives the producer a flat +10 Stress. | Correcting accurate output still returns it as rework and gives the producer the same flat +10 Stress. Audit Chance and Liability are unchanged. Some unsalvageable violations block correction. |
| Reject | Inaccurate or unsalvageable output is finalized with no payout and gives the producer a flat +10 Stress. | Rejecting accurate output also creates +1 Liability and Board Confidence −2. Consumed inputs are not returned. |
| Escalate | Always finalizes the document in Done without task revenue. It is treated as a deliberate handoff rather than a hit/miss ruling. | Board Confidence −5 and Audit Chance −5 points. It does not return consumed inputs. |

### Audit Severity Consequences

The nightly audit resolves in three steps:

1. Roll effective Audit Chance: base Audit Chance minus 5 points per held Compliance Token.
2. Calculate `effective Liability = max(0, open Liability − held Compliance Tokens)`.
3. If an audit occurs, roll finding chance as `effective Liability ÷ elapsed days`, capped at 100%.

If oversight finds something, effective Liability directly selects the consequence. For example, three open Liabilities minus one held token is effective Liability 2 and therefore Severity 2:

| Effective Liability | Consequence | Result |
|---:|---|---|
| 1 | Consent Decree | +2 Active Policies the next day |
| 2 | Cash Fine | Cash −$50, Audit Chance +10 percentage points, Board Confidence −5 |
| 3 | Compliance Check | Four unpaid Compliance Check cards are distributed through the next day; each missed or deleted card adds +1 Liability |
| 4 | Bad Vibes | Next-day Accuracy −10 percentage points and speed ×0.9; Staff Morale −20; Board Confidence −5 |
| 5+ | Termination | The run ends immediately |

Audit Chance and Severity are related but independently mutable. Accurate approval, Escalate, Safety Seminar, and Cash Fine change Audit Chance without directly changing Liability. Policy violations, Grease the Wheels, and Whistleblower change Liability/Severity without directly changing Audit Chance. A Compliance Token changes both by subtracting 5 Audit Chance points and offsetting one Liability.

### Task-Revenue Telemetry

The header projection is a task-performance view rather than a complete Cash ledger. It records one point only when a Review ruling recognizes task revenue and calculates `prior close + recognized task revenue − payroll − operating upkeep`. Hiring, check-ins, phishing rewards, waste, penalties, and other non-task Cash changes affect the displayed Cash total and nightly bridge but do not create graph points. The series is capped at 64 recognized payouts per day and five days, or 320 persisted points per quarter.

### Progression Currencies and Persistent Bonuses

| Currency | Earned from | Card-system use |
|---|---|---|
| Cash | Starting funds, approved task payouts, phishing rewards | Check-ins, roadmap costs, payroll, penalties, and employee stat pips during a completed Development Workshop's overnight shop. |
| XP | +1 banked immediately at every survived day close | Buy permanent upgrades only at a successful Day 5 Quarterly Review; otherwise remains banked across runs. |
| Talent Points | +1 at every survived day close | Buy one pip in a run-only process branch during the guaranteed nightly Talent Tree. Unspent points and ranks expire at run end. |
| Compliance Tokens | Claimed five-junk BUSYWORK-IT rewards, Report Defect/Policy Sweep route rewards, and completed 10-minute days | Each held token subtracts 5 Audit Chance points and offsets one Liability for finding chance and Severity. Tokens persist and are not consumed by audits. |

The guaranteed nightly **Run Process Talent Tree** contains five three-pip branches. Every pip costs one Talent Point:

| Branch | Pip 1 | Pip 2 | Pip 3 |
|---|---|---|---|
| Mailroom — Elastic Intake | Inbox capacity +1 | Inbox capacity +2 | Inbox capacity +3 |
| Workflow Design — Parallel Processing | In Progress capacity +1 | In Progress capacity +2 | In Progress capacity +3 |
| Finance — Revenue Assurance | Approved payouts +5% | Approved payouts +10% | Approved payouts +15% |
| People Operations — Restorative Controls | Overnight Stress recovery +3 | Overnight Stress recovery +6 | Overnight Stress recovery +9 |
| Compliance — Audit Dampening | Nightly Audit Chance −5 points | Nightly Audit Chance −10 points | Nightly Audit Chance −15 points |

The **permanent upgrade office** is unlocked only at a successful Day 5 Quarterly Review. Costs rise in 5-XP increments; a fresh save that survives all five days arrives with 5 XP and can afford only Spam Scanner:

| Upgrade | Cost and prerequisite | Persistent card-system bonus |
|---|---|---|
| Spam Scanner | 5 XP | Every junk card receives a full-card glitch, including its covered folder token. |
| Process Lane Annex | 10 XP | +1 In Progress slot. |
| Inbox Expansion | 15 XP; requires Spam Scanner | +2 Inbox slots. |
| Spam Intake Filter | 20 XP; requires Spam Scanner | Junk falls from 30% to 20% of each ten-card intake bag. |
| Review Annex | 25 XP; requires Process Lane Annex | +3 Review slots. |
| Six-Tab Folders | 30 XP | Matching task/resource folders hold six cards instead of five. |
| Manager Triage Protocol | 35 XP; requires Review Annex | Each morning, the Manager automatically relieves 8 stress from the most stressed available worker. |

The persistent wallet is visible in the header and at the top of the Progress panel. Progress distinguishes persistent XP and Compliance Tokens from run-only Talent Points, the Cash-funded Workshop shop, and the three location reward gates. It shows the exact token and Audit Dampening reductions and previews permanent upgrade costs and prerequisites without allowing purchases before Quarterly Review. The run-only Talent Tree is guaranteed as phase 1 of every survived night's rewards, including the final close before Quarterly Review.

Run-end accounting separates the XP earned by that run from the permanent wallet and separately held Compliance Tokens. Failure postmortems show retained unlocks but do not permit new purchases. Browser storage persists the wallet and upgrade IDs on that device until **Reset all data** is confirmed in Settings.

Persistent unlocks never auto-play cards: they add capacity, spam recognition/filtering, folder space, or one bounded Manager stress intervention while preserving mail triage and Review decisions.

### Corporate Roadmap

The corporate org-chart/Gantt roadmap is the only normal between-day decision surface. A new quarter starts by choosing Margin Review, Team Calibration, or Policy Sweep. Later choices are limited to the same or an adjacent workstream row, and all routes converge on the Day 5 Quarterly Board Review.

- Short, medium, and long nodes create real 3-, 5-, and 10-minute workdays. Completing a long day grants one Compliance Token.
- The rendered route uses department swimlanes, dependency connectors, filing stamps, and variable-width initiative bars modeled on the corporate-roadmap prototype. Every location carries a named program and concise corporate flavor memo that explains its mechanical tradeoff.
- Route inspectors separate setup applied when a route is filed from a single stacked column of payouts or reward menus awarded after that workday. They cover targeted stress reset, Night Planning, Report Defect, Grease the Wheels, Stage a Demo, Juiced Recruitment, Whistleblower, Casual Friday, a one-day Temp, and Safety Seminar.
- Vendor Exception creates an Accounting-focused day and a random non-Manager callout. Accounting intake emphasizes Approve Purchase Request, Expense Report, Invoice Request, and Governance Recalibration.
- Completing Employee Development Workshop is the only way to insert the Cash-funded employee stat shop into an overnight sequence.
- Night Planning and New Hire menus appear only after their matching completed locations; they do not form a general overnight catalog.
- `Rich Kid` / +1 extra life remains an unimplemented plan idea, not a cataloged current reward. The proposed generic single-resource day is likewise not implemented; Vendor Exception's Accounting-focused intake is the shipped strategic task-mix event.

| Day | Location / program | Duration | Current setup or settled reward |
|---:|---|---|---|
| 1 | Margin Review / Night Planning | Short · 3 min | Insert one choice after the Talent Tree: Pizza Party (Cash −$35, Morale +8, all employee Stress −5), Compliance Training (Cash −$25, Audit Chance −8), or Quiet Recovery (all employee Stress −8, Confidence −1). |
| 1 | Team Calibration / Targeted Stress Reset | Medium · 5 min | Reset the most stressed available worker to 0 Stress at close. |
| 1 | Policy Sweep / Report Defect | Long · 10 min | +2 Compliance Tokens; Board Confidence −5; completed-long-day bonus +1 token. |
| 2 | Vendor Exception / Accounting Surge | Medium · 5 min | Accounting work dominates intake; one random non-Manager calls out sick. |
| 2 | Development Workshop / Employee Development | Short · 3 min | Insert a Cash-funded current-employee stat shop after that night's Talent Tree. |
| 2 | Audit Readiness / Grease the Wheels | Long · 10 min | Remove 1 open Liability; Board Confidence −5; completed-long-day bonus +1 token. |
| 3 | Budget Lock / Stage a Demo | Long · 10 min | Staff Morale +10; Board Confidence +15; +1 Liability; completed-long-day bonus +1 token. |
| 3 | Talent Pipeline / Recruit Overnight | Medium · 5 min | Insert one paid Juiced candidate choice after the Talent Tree: Intern $60, Junior Analyst $110, or Accountant $145. |
| 3 | Whistleblower / Clean Disclosure | Short · 3 min | Clear all open Liability; Board Confidence −10. |
| 4 | Casual Friday / Mandatory Relaxation | Short · 3 min | Staff Morale +10; Board Confidence −1. |
| 4 | Temp Desk / One-Day Specialist | Medium · 5 min | Add a random Intern, Junior Analyst, or Accountant for one workday. |
| 4 | Safety Seminar / Mandatory Caution | Long · 10 min | Audit Chance −10 points; Staff Morale −5; completed-long-day bonus +1 token. |
| 5 | Quarterly Board Review / Final Boss | Boss · 10 min | Final workday; surviving unlocks permanent-upgrade spending and earns the completed-long-day +1 token. |

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

Pulls use repeating deterministic ten-card bags. Normally every bag contains three distinct legitimate tasks selected from the seven-card ordinary task pool, four resources (one of each plus one seeded duplicate), and three distinct junk templates. Spam Intake Filter changes that to four tasks, four resources, and two junk cards. Vendor Exception uses an Accounting task pool: Approve Purchase Request, Expense Report, Invoice Request, and Governance Recalibration. A level-3 audit inserts four Compliance Checks at seeded random positions in the following day's first bag.

Regulatory Response, Compliance Check, and the BUSYWORK-IT reward notice are conditional deliveries and therefore have 0% ordinary random-pull probability.

---

### Player Card Interaction Reference

| Player action | Runtime result | Constraint or feedback |
|---|---|---|
| Wait for an arrival | The next seeded card enters Inbox when the visible countdown reaches zero. | The automatic interval scales by day. If Inbox is full, the automatic arrival displaces the oldest stack and applies the documented overflow consequences. |
| Select **Pull next item** | The next seeded card enters immediately and the automatic-arrival clock resets. | Disabled while Inbox is full. Each ten-card bag preserves the 30% task, 40% resource, and 30% junk composition above. |
| Click a visible card | Opens that card or stack in the Inspector. | Review output arriving later does not replace this selection. |
| Drop a card or stack into a visible gap in its current lane | Reorders that entire lane item at the chosen position. | Folder contents, active job state, employee Stress state, timers, and unrelated runtime workflows are unchanged. |
| Drag a visible top card to empty lane space | Extracts that card into a new stack in the destination lane. | Lane capacity and movement restrictions still apply. |
| Drag one stack onto another | Merges the complete source stack atomically. | The result must contain no more than five cards, or six for a homogeneous matching-task/resource folder after Six-Tab Folders is unlocked. Otherwise a workflow stack accepts at most one employee, task-like card, document, and resource. The exact two-resource Juiced recipe is the only mixed-resource exception. Active and locked workflows reject merges. |
| Click a folder token or covered-card token | Opens that represented card in the Inspector. | Homogeneous folders show only the covered cards, nearest to the top first, in a four-slot rail; the full-size folder face already represents the top card. Mixed stacks also show only genuinely covered cards. |
| Drag a folder token or covered-card token | Pulls only that represented card into its own stack. | A covered card's paused deadline resumes only when it becomes a physical top card. Rectangular token shape, abbreviation, color, and decoration identify the referent and relevant timer, Juiced, low-value, glitch, or employee state. A two-card folder dissolves into one ordinary card when either card is removed. |
| Drag an In Progress resource chip | Removes only that staged resource from the composite workflow. | In Progress does not show covered pips because the employee, task, and resources are represented directly. |
| Drag an assigned employee header to Backlog | Cancels active work, dismantles the composite workflow, and releases every card into its own stack. | The stack under the pointer is ignored, so release can never combine the employee or task with it. Backlog must have room for the employee; each remaining card fills another Backlog slot, then an Inbox slot, or is deleted with its normal consequences when both lanes are full. |
| Complete a workflow | Consumes every staged resource, sends the document to Review, and leaves the employee in In Progress. | No resource is retained. A matching disguised junk input is accepted as supplied but guarantees Source Integrity Failure. |

Card faces pair a stable type color and shape with a specific abbreviation such as `SP` for Spreadsheet or `RE` for Receipt; generic `RS` and `WK` labels are not used. Employees use blue/avatar circles with executive brown reserved for the Manager, tasks amber/target circles, resources purple/diamonds, and documents green/folded pages. Secondary payout and scope attributes remain compact colored pips. Juiced tasks add a heavy double edge, layered surface, deeper shadow, and lightning pip while retaining the base card-type identity. Hovering or keyboard-focusing a Juiced, Windfall, Premium, or Low Fee token opens a compact explanation of its benefits and tradeoffs. Selecting a card does not visually accent other cards that could interact with it; drag targets still provide direct valid/invalid feedback during the drag itself. Task flavor text names the required resource in natural language without a separate “consumed” footer. Standalone employees show their Accuracy, Speed, and Resilience values and compact meters at all times.

---

### Runtime Instance Notes

- Every instance has a unique `card_*` ID, location, creation day, and optional deadline.
- Employee instances add Stress, workload preference, coping trait, condition, daily work/idle time, 1–6 stats, derived abilities, per-stat purchased-pip counts, and total Cash invested.
- Employee instances also persist their continuous taskless In Progress wait. This timer drives the escalating idle-pressure rate and resets when work is assigned or the employee leaves In Progress.
- Task instances may become rework tasks and retain revision metadata.
- Positive-revenue task instances retain their payout tier, multiplier, quoted contract amount, and standard/juiced scope through production and rework.
- Document instances store generated fields, producer ID, recipe ID, originating task template, producer stress, coverage status, reward, and final ruling.
- Distraction instances store their internal distraction type, visual disguise type, imitated template, and deterministic glitch variant.
- Multiple employees may share one template but have independently seeded stats, traits, workload preferences, and labels. Temp Desk specialists use the ordinary role pool. Talent Pipeline candidates receive the documented Juiced Hire stat floor, speed, recovery, border, and modifier token.
- Reaching each survived day close immediately awards `+1 XP` to the persistent on-device wallet and `+1 Talent Point` to the current run. The guaranteed phase-1 Talent Tree spends Talent Points on Run Process Upgrades; permanent XP spending remains locked until a successful Day 5 Quarterly Review. The receipt also shows any long-shift Compliance Token.
- The corporate roadmap replaces the old overnight choice catalog. The player selects one adjacent workstream for the next day; each node exposes its exact effect and real 3-, 5-, or 10-minute duration. A completed 10-minute shift awards one persistent Compliance Token.
- Completing Development Workshop, Margin Review, or Talent Pipeline inserts only its matching Cash stat shop, Night Planning choice, or Juiced Recruitment choice after the Talent Tree for that night. Permanent upgrades and XP remain on the device through browser storage until reset in Settings; Talent Points, Run Process ranks, employee training, and hired employees expire with the run.
- Standalone employee cards always display their current Accuracy, Speed, and Resilience values in a compact three-column strip; no hover is required.
- Matching resource cards and matching task cards can share homogeneous storage piles. Other stacks allow at most one employee, task-like card, document, and resource card; resource-disguised junk occupies the resource slot, and a Juiced task may carry its exact two-resource recipe pair. Compatible stacks can be combined atomically up to five total cards, or six for a homogeneous folder after Six-Tab Folders is unlocked. Active or locked stacks cannot be merged.
- Only the physical top card advances its deadline. Homogeneous folders use a compact filing-folder face plus four rectangular slots for covered cards, nearest to the top first; the visible folder face represents the active top card and is never repeated as a mini token. Unused slots remain empty, and the folder appearance disappears when only one card remains. Mixed stacks use compact identity/status tokens only for genuinely covered cards while their timers pause. Every populated token can be clicked for inspection or dragged to pull that individual card out. Composite In Progress workflows omit the tokens because all participating cards are already represented directly.
- The opening Data Entry Request and every legitimate card/action that can advance it are eligible for the first-workflow sparkle guide.
- Work products entering Review do not replace the current card or panel selection.
- Policy, progression, and operating systems are summarized here only where they change card generation, card state, workflow outcomes, or card-facing decisions; they are not counted as card templates.
