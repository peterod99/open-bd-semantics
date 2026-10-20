# Glossary

Every term defined in [`nynch-bd.ossie.yaml`](nynch-bd.ossie.yaml), in plain English, with the rule that decides it. Definitions version **2026-10-20**.

This page is generated from the YAML; when the two disagree, the YAML wins. Column names are the live database columns. A **derived** term is computed by the product from other columns; its rule is the exact condition, in ANSI SQL where one exists.

## Model

Nynch's relationship-led business development model: people, the accounts they work for, the opportunities in play, and the signals that point at the next one. Definitions are canonical (one meaning per word), raw labels are kept beside them, and nothing here is inferred from a stage name.

**How to read the model:** Use lifecycle_stage, not lead_status, to say where a person is in the relationship; lead_status is the raw label someone typed or synced and only maps to meaning through the lifecycle_stage_map dataset. Use relationship_warmth_tier for how warm a relationship is and treat NULL as "no two-way contact ever recorded", never as cold. An opportunity's status comes from its outcome, never from the name or type of the stage it sits in: won opportunities live in client-delivery stages by design. "Deals" and "opportunities" are the same thing on two rails; signals (revenue_opportunities) are AI-detected openings, not pipeline records.

## Contents

- [people](#people)
- [accounts](#accounts)
- [opportunities](#opportunities)
- [signals](#signals)
- [lifecycle_stage_map](#lifecycle_stage_map)
- [Metrics](#metrics)
- [Relationships](#relationships)

## people

A person Nynch has a relationship with. One row per person per workspace; merged duplicates keep their row with merged_into_id set and soft-deleted people keep theirs with deleted_at set.

**Also called:** contacts, relationships, leads, prospects.

**How to read it:** Filter deleted_at IS NULL and merged_into_id IS NULL for the live book.

**Source:** `public.contacts`, keyed by `id`.

### people.id

Person identifier (UUID).

Type: String.

### people.workspace_id

The workspace (tenant) the person belongs to.

Type: String.

### people.name

Full name as written.

Type: String.

### people.primary_email

The email Nynch treats as the person's main address.

Type: String.

### people.company_id

The account (companies.id) the person currently works for, when known.

Type: String.

### people.owner_id

The teammate who owns the relationship.

Type: String.

### people.lead_status (raw label)

The raw status label exactly as a person, a CSV import, HubSpot or Zapier wrote it. Free text; it is never rewritten by derivation and moves only when a writer changes the status on purpose. Carries no meaning on its own: read lifecycle_stage instead.

Type: String.

**How to read it:** Never test this field for meaning (for example lead_status = 'customer'); use lifecycle_stage.

### people.lifecycle_stage (canonical stage)

Where the person is in the relationship, as one of seven canonical stages: lead (a person we want a relationship with and have not yet qualified), qualified (a fit for what we sell, with a reason to talk), opportunity (an active commercial conversation, usually with a proposal), client (buying from us now or in the past), partner (a referrer, advocate or collaborator rather than a buyer), lost (a conversation that ended without a sale) and dormant (a relationship deliberately parked). Derived on every write from lead_status through the lifecycle_stage_map dataset; a write that sets the stage directly wins for that write. NULL means no stage is recorded (lead_status is null or an unmapped label), never "lead".

Type: String; also called status, contact status, relationship stage.

**Rule details:**

- **Canonical values:** `lead`, `qualified`, `opportunity`, `client`, `partner`, `lost`, `dormant`
- **Null means:** no stage recorded
- **Derived from:** lead_status via lifecycle_stage_map
- **Is client:** lifecycle_stage = 'client'

### people.relationship_warmth_tier

How warm the relationship is, computed every fifteen minutes from connection_strength and the most recent two-way touch (the older of the last inbound and last outbound message, per channel, taking the most recent channel). hot: strength of 50 or more and a two-way touch within 30 days. warm: strength of 25 or more, or a two-way touch within 90 days. cold: a two-way touch exists but neither condition holds. NULL: no two-way contact has ever been recorded; this is not cold, it is unknown.

Type: String.

**How to read it:** Treat NULL as "no two-way contact recorded", never as cold.

**Rule details:**

- **Rule:**
  - `hot`: connection_strength >= 50 AND two_way_touch_within_days <= 30
  - `warm`: connection_strength >= 25 OR two_way_touch_within_days <= 90
  - `cold`: a two-way touch exists and neither holds
  - `null`: no two-way contact ever recorded
- **Thresholds:**
  - `HOT_MIN_STRENGTH`: `50`
  - `HOT_MAX_DAYS`: `30`
  - `WARM_MIN_STRENGTH`: `25`
  - `WARM_MAX_DAYS`: `90`
- **Computed by:** run_relationship_signal_sweep on the connection-strength-sweep cron, 400 contacts per 15 minutes

### people.connection_strength

A 0 to 100 score of how much real contact there has been, recomputed every fifteen minutes; direction-blind.

Type: Integer.

### people.last_activity_at

When the last recorded relationship activity happened. NULL means none recorded; never substitute created_at or updated_at.

Type: DateTimeTz; a time dimension.

### people.created_at

When the record was created in Nynch (an audit timestamp, not an activity).

Type: DateTimeTz.

### people.deleted_at

Soft-delete marker; NULL for live people.

Type: DateTimeTz.

### people.merged_into_id

Set when this row was merged into another person; the surviving row is the one to read.

Type: String.

## accounts

An organisation people work for and opportunities belong to. One row per account per workspace.

**Also called:** companies, organisations, firms, clients when account_type says so.

**Source:** `public.companies`, keyed by `id`.

### accounts.id

Account identifier (UUID).

Type: String.

### accounts.workspace_id

The workspace the account belongs to.

Type: String.

### accounts.name

Account name as written.

Type: String.

### accounts.domain

Primary web domain, used to match people to the account.

Type: String.

### accounts.industry

Industry label as enriched or typed.

Type: String.

### accounts.owner_id

The teammate who owns the account.

Type: String.

### accounts.account_type

What the account is to Nynch (for example prospect or client), as the product records it.

Type: String.

### accounts.market_tier

The commercial tier the workspace assigned (its own vocabulary); NULL when unassessed.

Type: String.

### accounts.last_activity_at

When the last recorded activity with anyone at the account happened; NULL means none recorded.

Type: DateTimeTz; a time dimension.

### accounts.deleted_at

Soft-delete marker; NULL for live accounts.

Type: DateTimeTz.

## opportunities

A commercial conversation that could end in a sale. Nynch has two physical rails with one meaning: deals (the live rail) and opportunities (a retiring rail, frozen; 27 rows). The rail column says which; nothing else differs in meaning. AI-detected openings are not here, they are signals.

**Also called:** deals, pipeline, open conversations, proposals.

**How to read it:** Status comes from outcome, never from stage or stage_type: won opportunities are moved into client-delivery stages whose stage_type is active, by design. "Deal" and "opportunity" mean the same thing.

**Source:** a query over the product database (see the YAML).

### opportunities.id

Opportunity identifier (UUID), unique across both rails.

Type: String.

### opportunities.workspace_id

The workspace the opportunity belongs to.

Type: String.

### opportunities.rail

Which physical table the row comes from, deals or opportunities. Same meaning either way.

Type: String.

### opportunities.title

What the opportunity is called.

Type: String.

### opportunities.company_id

The account the opportunity is with.

Type: String.

### opportunities.primary_contact_id

The main person in the conversation.

Type: String.

### opportunities.owner_id

The teammate running the opportunity.

Type: String.

### opportunities.value

Expected value in the opportunity's currency; NULL when not yet quantified (never zero by default).

Type: Decimal.

### opportunities.currency

ISO 4217 code for value.

Type: String.

### opportunities.stage

The workspace's own stage name. Workspaces name stages freely; read stage_type for the role and outcome for the result.

Type: String.

### opportunities.stage_type

The role of the current stage: active, won or lost. Every custom stage carries one. It is a position on a board, not the result: a won opportunity sits in an active client-delivery stage.

Type: String.

### opportunities.outcome

The recorded result: won, lost, no_decision, or pending (a close awaiting review). NULL means the conversation is still open. Written only through the reviewed close contracts, never by moving a card.

Type: String.

### opportunities.status (derived)

Whether the opportunity is open, won, lost, no_decision or pending, derived from outcome alone. Never derived from stage_type: a Lost stage with no recorded outcome is still open and is queued for a manager to record the outcome.

Type: String.

**Rule (ANSI SQL):**

```sql
CASE outcome WHEN 'won' THEN 'won' WHEN 'lost' THEN 'lost' WHEN 'no_decision' THEN 'no_decision' WHEN 'pending' THEN 'pending' ELSE 'open' END
```

**Rule details:**

- **Derived from:** outcome
- **Never from:** stage_type
- **Open means:** outcome IS NULL
- **Repair:** live Lost-stage rows with no outcome go to deal_outcome_repair_queue for manager review

### opportunities.is_qualified (derived)

True when all four SPEC qualification elements are confirmed on the opportunity: Strategic Commitment (recorded as challenges), Problem Resonance (authority), Economic Fit (money) and Committee Gravity (prioritization). Each element is unknown, confirmed or not_confirmed; anything short of four confirmed is not qualified. Opportunities on the retiring rail carry no qualification and are never qualified.

Type: Boolean.

**Rule (ANSI SQL):**

```sql
JSON_VALUE(champ_qualification, '$.challenges') = 'confirmed' AND JSON_VALUE(champ_qualification, '$.authority') = 'confirmed' AND JSON_VALUE(champ_qualification, '$.money') = 'confirmed' AND JSON_VALUE(champ_qualification, '$.prioritization') = 'confirmed'
```

**Rule details:**

- **Elements:**
  - `challenges`: Strategic Commitment
  - `authority`: Problem Resonance
  - `money`: Economic Fit
  - `prioritization`: Committee Gravity
- **Element values:** `unknown`, `confirmed`, `not_confirmed`
- **Rule:** all four confirmed

### opportunities.close_date

The expected or actual close date as recorded.

Type: Date; a time dimension.

### opportunities.last_activity_at

When the last recorded activity on the opportunity happened; NULL means none recorded.

Type: DateTimeTz; a time dimension.

### opportunities.created_at

When the record was created (audit timestamp).

Type: DateTimeTz.

### opportunities.deleted_at

Soft-delete marker; NULL for live opportunities.

Type: DateTimeTz.

## signals

An AI-detected opening (expansion, upsell, referral, re-engagement and the like) found in conversations and activity. A signal is a suggestion about a person, account or opportunity; it is not an opportunity until someone opens one.

**Also called:** revenue opportunities, openings, triggers.

**How to read it:** Do not count signals as pipeline. They point at where an opportunity might be opened.

**Source:** `public.revenue_opportunities`, keyed by `id`.

### signals.id

Signal identifier (UUID).

Type: String.

### signals.opportunity_type

The kind of opening the AI detected (for example expansion, upsell, referral).

Type: String.

### signals.status

Where the signal is in review (as the product records it); not an opportunity status.

Type: String.

### signals.confidence_score

The model's confidence in the signal, 0 to 1.

Type: Float.

### signals.estimated_value

A rough value if the opening became an opportunity; NULL when unknown.

Type: Decimal.

### signals.contact_id

The person the signal is about, when there is one.

Type: String.

### signals.company_id

The account the signal is about, when there is one.

Type: String.

### signals.deal_id

The opportunity (deals rail) the signal relates to, when there is one.

Type: String.

### signals.source_date

When the evidence behind the signal happened.

Type: DateTimeTz; a time dimension.

### signals.opportunity_summary

The AI's one-paragraph description of the opening.

Type: String.

## lifecycle_stage_map

The global map from raw lead_status labels (lower-cased and trimmed) to canonical lifecycle stages. Every label a writer has used maps to exactly one stage; exactly one label per stage is the default a stage-only writer mirrors into lead_status. A label not in this map derives no stage.

**How to read it:** Read-only reference data; the product derives lifecycle_stage through it on every write.

**Source:** `public.lead_status_lifecycle_map`, keyed by `raw_label`.

**Rule:**

- **Seed:**

  | Key | Value |
  |---|---|
  | `new` | `lead` |
  | `new lead` | `lead` |
  | `lead` | `lead` |
  | `prospect` | `lead` |
  | `contacted` | `lead` |
  | `nurture` | `lead` |
  | `nurturing` | `lead` |
  | `subscriber` | `lead` |
  | `marketingqualifiedlead` | `lead` |
  | `qualified` | `qualified` |
  | `engaged` | `qualified` |
  | `salesqualifiedlead` | `qualified` |
  | `proposal` | `opportunity` |
  | `negotiation` | `opportunity` |
  | `opportunity` | `opportunity` |
  | `customer` | `client` |
  | `client` | `client` |
  | `won` | `client` |
  | `partner` | `partner` |
  | `advocate` | `partner` |
  | `evangelist` | `partner` |
  | `lost` | `lost` |
  | `dormant` | `dormant` |

- **Defaults:**
  - `lead`: new
  - `qualified`: qualified
  - `opportunity`: proposal
  - `client`: customer
  - `partner`: partner
  - `lost`: lost
  - `dormant`: dormant

### lifecycle_stage_map.raw_label

The raw label, lower-cased and trimmed.

Type: String.

### lifecycle_stage_map.lifecycle_stage

The canonical stage the label means.

Type: String.

### lifecycle_stage_map.is_default

True for the one label per stage that a stage-only writer mirrors into lead_status.

Type: Boolean.

## Metrics

### open_pipeline_value

Total value of live, open opportunities (outcome not yet recorded), in each opportunity's own currency; do not sum across currencies.

Type: Decimal.

**Rule (ANSI SQL):**

```sql
SUM(CASE WHEN outcome IS NULL AND deleted_at IS NULL THEN value ELSE 0 END)
```

### open_opportunity_count

Number of live opportunities with no recorded outcome.

Type: Integer.

**Rule (ANSI SQL):**

```sql
COUNT(CASE WHEN outcome IS NULL AND deleted_at IS NULL THEN 1 END)
```

### client_count

Number of live people at the client stage.

Type: Integer.

**Rule (ANSI SQL):**

```sql
COUNT(CASE WHEN lifecycle_stage = 'client' AND deleted_at IS NULL AND merged_into_id IS NULL THEN 1 END)
```

### hot_relationship_count

Number of live people whose computed warmth tier is hot.

Type: Integer.

**Rule (ANSI SQL):**

```sql
COUNT(CASE WHEN relationship_warmth_tier = 'hot' AND deleted_at IS NULL AND merged_into_id IS NULL THEN 1 END)
```

## Relationships

How the datasets join.

| Relationship | From | To | Join |
|---|---|---|---|
| people_work_for_accounts | people | accounts | `people.company_id = accounts.id` |
| opportunities_belong_to_accounts | opportunities | accounts | `opportunities.company_id = accounts.id` |
| opportunities_have_a_primary_person | opportunities | people | `opportunities.primary_contact_id = people.id` |
| signals_point_at_people | signals | people | `signals.contact_id = people.id` |
| signals_point_at_accounts | signals | accounts | `signals.company_id = accounts.id` |
| signals_point_at_opportunities | signals | opportunities | `signals.deal_id = opportunities.id` |
| people_stages_come_from_the_map | people | lifecycle_stage_map | `people.lead_status = lifecycle_stage_map.raw_label` |
