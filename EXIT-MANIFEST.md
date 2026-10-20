# The exit manifest

"Take it with you" is Nynch's promise that a customer can leave with their whole business memory, in open formats, whenever they choose: every person, company, deal, conversation, transcript, proposal and learning, plus the definitions that say what the data means. [`exit-manifest.json`](exit-manifest.json) is that promise written down as data. It is generated from the same registry the export runs on, so it lists exactly what an export contains today and every table left out, with the reason.

Ask any vendor holding your company's context what you would get to take with you if you ever leave. This file is our answer.

## Today's promise

- **74 datasets**, each as CSV and JSON Lines, plus Markdown memory files, transcripts and proposals.
- **662 tables left out**, each with the reason it is left out.
- **Never exported:** `embedding (unless switched on)`, `*_token`, `oauth_data`.
- **Who can start it:** A workspace owner or admin, after two-step verification. Every export is written to the audit log.
- **How long it takes:** Under two minutes for a few hundred people. Ten to fifteen minutes for about 20,000 people and 60,000 conversations, across two or three background runs; a run that stops resumes from where it stopped.
- **Availability:** Ready to download for seven days, then deleted.
- **Definitions version:** 2026-10-20.
- **Personal data:** Transcripts, emails, chat and LinkedIn messages contain the words and personal data of people outside the firm. The firm owns these records and is their data controller.

[`sample-export/`](sample-export/) is a complete export of a generated demo workspace, produced by the real export engine. Every name in it is invented and every address ends in `example.test`.

## Fields

### Top level

| Key | Meaning |
|---|---|
| `format_version` | Version of this manifest's own shape. It changes only when a key is added, removed or changes meaning. |
| `definitions_version` | The date-stamped version of the business-development definitions (`nynch-bd.ossie.yaml`) the exported data follows. The same value is in every export. |
| `generated_from` | Where in Nynch's product the manifest is generated from: the export registry, the one list every export reads. The manifest cannot promise a table the export does not write. |
| `what_you_get` | The shape of an export: folder layout, file formats, who can start one, how long it takes and how long it stays available. |
| `datasets` | Every dataset in an export, one entry each. |
| `dataset_count` | How many datasets an export contains. |
| `exclusions` | Every product table that is not exported, grouped by the reason it is left out. |
| `excluded_table_count` | How many tables are left out. Every table in the product database is either a dataset source or listed here. |
| `never_exported_columns` | Columns never written to any export, whichever table they sit in (secrets such as access tokens, and search vectors unless the customer switches them on). |
| `third_party_data_note` | What the export says about personal data of people outside the customer's firm. |

### `what_you_get`

| Key | Meaning |
|---|---|
| `folder_layout` | The paths in an export folder. `<dataset>` is a dataset name below; memory files are one Markdown file per person, per account and per learning type. |
| `formats` | The open formats used. Nothing needs Nynch to read. |
| `who_can_start_it` | Who may start an export and what is recorded when they do. |
| `typical_duration` | How long an export takes for a small and a large workspace, and how long the finished export stays available. |
| `page_size_rows` | Rows read per database page while exporting. A run that stops resumes from the last finished page. |
| `retention_days` | Days a finished export stays available to download before it is deleted. |

### `what_you_get.typical_duration`

| Key | Meaning |
|---|---|
| `small_workspace` | Typical time for a workspace of a few hundred people. |
| `large_workspace` | Typical time for a workspace of about 20,000 people. |
| `retention` | How long the finished export can be downloaded. |

### Each entry in `datasets`

| Key | Meaning |
|---|---|
| `dataset` | The dataset name, which is also the file name under `data/`. |
| `description` | What the dataset holds, in one or two sentences. |
| `sources` | The product tables the dataset is read from. |
| `formats` | The files written for the dataset (`csv` and `jsonl`). |
| `memory_files` | Which Markdown memory files the dataset feeds (`relationship`, `account`, `learning`), if any. |
| `importable` | Whether the dataset can be imported back into a Nynch workspace from the export. |
| `reach_flagged` | True when the dataset reaches the workspace through a teammate's membership rather than a record that belongs to the workspace. No dataset does today. |

### Each entry in `datasets[].sources`

| Key | Meaning |
|---|---|
| `table` | The product table. |
| `reach` | How rows are tied to the exporting workspace: `workspace_id` directly, or `via_<parent>` through a parent record that belongs to it (for example `via_contact`). |
| `columns` | How many columns of the table are exported. |

### Each entry in `exclusions`

| Key | Meaning |
|---|---|
| `reason` | Why the tables are left out (for example system configuration or another customer's data). |
| `tables` | The table names, or name patterns, left out for that reason. |

## Datasets in an export

| Dataset | What it holds | Importable |
|---|---|---|
| `companies` | Every company (account) with its firmographics, tier and enrichment. | yes |
| `people` | Every person in the workspace with their profile, status, owner and relationship scores. | yes |
| `opportunities` | Deals: the live pipeline rail. Status is derived from outcome (null = open). Opportunities: the retiring rail, kept read-only. Same thing as a deal; the rail column tells them apart. | yes |
| `pipelines` | Pipelines the deals move through. | no |
| `pipeline_stages` | Stages per pipeline. stage_type (active, won, lost) is the canonical stage mapping. | no |
| `deal_people` | Which people are attached to which deals. | yes |
| `deal_stakeholders` | Stakeholder maps per deal: role, influence, support and concerns. | yes |
| `buying_committee` | Buying committee members per deal. | yes |
| `deal_activities` | Activities logged against deals. | yes |
| `deal_stage_history` | Every stage move with how long the deal sat in the previous stage. | no |
| `deal_comments` | Comments on deals. | yes |
| `deal_lines` | Service lines priced on each deal. | no |
| `interactions` | Every recorded touch with a person: emails, meetings, calls, messages and manual notes of contact. | yes |
| `activities` | The older activity log per person. | yes |
| `email_messages` | Imported email messages linked to a person (text bodies, no HTML). Messages not linked to a person are not reachable by workspace and are left out. | yes |
| `email_threads` | Thread-level summaries, commitments and signals. | yes |
| `sent_emails` | Emails sent from Nynch (text bodies). | yes |
| `linkedin_messages` | LinkedIn messages imported from the extension. | yes |
| `chat_messages` | WhatsApp, Slack and other chat messages synced to a person. | yes |
| `meetings` | Meeting records with summaries, commitments and full transcripts (also written to transcripts/). | yes |
| `calendar_events` | Imported calendar events and their attendees. | yes |
| `meeting_attendees` | Who attended which meeting. | yes |
| `notes` | Notes written on people. | yes |
| `comments` | Teammate comments on people, companies and deals. | yes |
| `tasks` | Team tasks. | yes |
| `action_items` | Personal action items tied to people and deals. | yes |
| `commitments` | Promises made and received. | yes |
| `genuine_commitments` | Commitments confirmed as genuine from conversations. | yes |
| `company_people` | Which people belong to which companies, including secondary associations. | yes |
| `employment_history` | Past and present roles per person. | yes |
| `contact_emails` | Every email address per person with consent and deliverability state. | yes |
| `contextual_roles` | What a person is to the firm on an account, agreement, project or deal. | yes |
| `company_signals` | Company signals as the workspace sees them (funding, hiring, leadership, news). | yes |
| `contact_signals` | Detected changes about a person (job moves, title changes). | yes |
| `social_signals` | Social activity signals per person. | yes |
| `opportunity_signals` | Buying signals spotted in conversations. | yes |
| `signals` | Revenue signals (expansion, upsell, referral angles). These are signals, not deals. | yes |
| `intelligence_items` | Intelligence items surfaced about people and companies. | yes |
| `engagement_scores` | Two-way engagement scores per person; the inputs to the warmth tier. | no |
| `ideal_client_profiles` | Ideal client profiles. | yes |
| `services` | The services the firm sells. | yes |
| `proof_points` | Proof points and evidence the firm uses. | yes |
| `gtm_problems` | The customer problems the firm solves. | yes |
| `competitors` | Competitors and how the firm wins against them. | yes |
| `manifestos` | Positioning manifestos. | yes |
| `custom_fields` | Custom field definitions. | yes |
| `custom_field_values` | Custom field values per record. | yes |
| `lead_source_options` | The workspace lead-source list. | yes |
| `relationship_memory` | The synthesised relationship memory per person: history, facts, what not to assume. | yes |
| `account_memory` | The synthesised account memory per company. | yes |
| `agent_memories` | Durable memories Nynch agents kept about people, workflows and preferences. | yes |
| `knowledge_entities` | Knowledge-graph entities for people, companies and deals. | yes |
| `known_facts` | Known facts: relationships between people, companies and deals with their evidence. | yes |
| `beliefs` | Buyer beliefs the firm has catalogued. | yes |
| `belief_observations` | Each time a belief was observed in a conversation. | yes |
| `deal_learnings` | Patterns learned from won and lost deals. | yes |
| `proposal_learnings` | Patterns learned from proposals. | yes |
| `decisions` | Decisions recorded with their reasoning. | yes |
| `introduction_requests` | Warm introduction requests and their outcomes. | yes |
| `team_intro_requests` | Introduction asks between teammates. | yes |
| `relationship_paths` | Warm paths to target people through connectors. | yes |
| `circles` | Circles of leverage around a target. | yes |
| `circle_members` | Members of each circle. | yes |
| `proposals` | Proposals with their full content (also written to proposals/). | yes |
| `proposal_versions` | Every proposal version as a frozen document. | yes |
| `proposal_options` | Priced options inside proposals. | yes |
| `proposal_artifacts` | Where each proposal was exported to (links to the generated documents). | no |
| `agreements` | Commercial agreements per account. | no |
| `agreement_versions` | Accepted versions of each agreement with frozen terms. | no |
| `agreement_lines` | Service lines inside accepted agreement versions. | no |
| `projects` | Delivery projects per account. | no |
| `campaigns` | Outreach campaigns and their steps. | yes |
| `campaign_enrollments` | Who was enrolled in which campaign and what happened. | yes |
| `company_briefs` | Company briefs written for prospects. | no |
