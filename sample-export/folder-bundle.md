# Take it with you: demo export


===== README.md =====

# Fixture Partners: everything you can take with you

Exported from Nynch on 2026-10-20T09:00:00.000Z. 127 rows across 74 datasets, 12 relationship memory files, 4 account memory files.

This folder is the complete business memory of the workspace in open formats. Nothing in it needs Nynch to read: CSV for spreadsheets, JSONL for code, Markdown for people and AI assistants.

## What is in the folder

| Path | What it holds |
|---|---|
| `README.md` | This file. |
| `manifest.json` | Every dataset, its row count as counted in the database beside the rows written, every file with its size and SHA-256, every excluded table with the reason. |
| `data/<dataset>.csv` | One spreadsheet per dataset (UTF-8, first row is the header). Big datasets continue in `<dataset>.2.csv`, `.3.csv`. |
| `data/<dataset>.jsonl` | The same rows as JSON, one object per line, with full nested values. |
| `memory/relationships/<name>-<id>.md` | One file per person: profile, companies, deals, relationship memory, known facts, commitments, notes, the 50 most recent interactions, signals. |
| `memory/accounts/<name>-<id>.md` | One file per company: people, deals, account memory, signals, proof points and problems that mention it. |
| `memory/learnings/*.md` | Deal learnings, proposal learnings, outcome patterns and belief observations, grouped by type. |
| `transcripts/<meeting-id>.md` | Every meeting with its summary, commitments and full transcript. |
| `proposals/<id>.json` and `.md` | Every proposal with its content and priced options. |
| `semantics/nynch-bd.ossie.yaml` | The business-development definitions (Apache Ossie format) the datasets follow. |

Definitions version: 2026-10-20.

## How to read it

- A **person** is a row in `people`; `id` is the key every other dataset points at (`contact_id`).
- A **company** is a row in `companies` (`company_id`).
- **Deals and opportunities are the same thing.** Both live in `opportunities`; the `rail` column says which table they came from (`deal` is the live rail, `opportunity` the retiring one). A deal's status is derived from `outcome`: null means open, otherwise won, lost, no_decision or pending.
- `signals` are revenue signals (expansion, upsell, referral angles). They are not deals.
- Teammate columns such as `owner_id` carry the person's name beside them (`owner_name`). Ids stay so the files join.
- `lead_status` is the raw label as typed. Where the workspace has a `lifecycle_stage`, that is the canonical stage.
- Soft-deleted and merged records are included with their `deleted_at` or `merged_into_id` set, so nothing is silently dropped.

## Datasets

- **companies** (from `companies`): Every company (account) with its firmographics, tier and enrichment.
- **people** (from `contacts`): Every person in the workspace with their profile, status, owner and relationship scores.
- **opportunities** (from `deals` and `opportunities`): Deals: the live pipeline rail. Status is derived from outcome (null = open). Opportunities: the retiring rail, kept read-only. Same thing as a deal; the rail column tells them apart.
- **pipelines** (from `pipelines`): Pipelines the deals move through.
- **pipeline_stages** (from `pipeline_stages`): Stages per pipeline. stage_type (active, won, lost) is the canonical stage mapping.
- **deal_people** (from `deal_contacts`): Which people are attached to which deals.
- **deal_stakeholders** (from `deal_stakeholders`): Stakeholder maps per deal: role, influence, support and concerns.
- **buying_committee** (from `buying_committee_members`): Buying committee members per deal.
- **deal_activities** (from `deal_activities`): Activities logged against deals.
- **deal_stage_history** (from `deal_stage_history`): Every stage move with how long the deal sat in the previous stage.
- **deal_comments** (from `deal_comments`): Comments on deals.
- **deal_lines** (from `deal_lines`): Service lines priced on each deal.
- **interactions** (from `contact_interactions`): Every recorded touch with a person: emails, meetings, calls, messages and manual notes of contact.
- **activities** (from `contact_activities`): The older activity log per person.
- **email_messages** (from `email_messages`): Imported email messages linked to a person (text bodies, no HTML). Messages not linked to a person are not reachable by workspace and are left out.
- **email_threads** (from `email_threads`): Thread-level summaries, commitments and signals.
- **sent_emails** (from `sent_emails`): Emails sent from Nynch (text bodies).
- **linkedin_messages** (from `linkedin_messages`): LinkedIn messages imported from the extension.
- **chat_messages** (from `chat_messages_sync`): WhatsApp, Slack and other chat messages synced to a person.
- **meetings** (from `meeting_transcripts`): Meeting records with summaries, commitments and full transcripts (also written to transcripts/).
- **calendar_events** (from `calendar_events`): Imported calendar events and their attendees.
- **meeting_attendees** (from `meeting_attendees`): Who attended which meeting.
- **notes** (from `contact_notes`): Notes written on people.
- **comments** (from `record_comments`): Teammate comments on people, companies and deals.
- **tasks** (from `team_tasks`): Team tasks.
- **action_items** (from `action_items`): Personal action items tied to people and deals.
- **commitments** (from `commitments`): Promises made and received.
- **genuine_commitments** (from `genuine_commitments`): Commitments confirmed as genuine from conversations.
- **company_people** (from `company_contact_associations`): Which people belong to which companies, including secondary associations.
- **employment_history** (from `contact_employment_history`): Past and present roles per person.
- **contact_emails** (from `contact_emails`): Every email address per person with consent and deliverability state.
- **contextual_roles** (from `contact_contextual_roles`): What a person is to the firm on an account, agreement, project or deal.
- **company_signals** (from `current_company_signals`): Company signals as the workspace sees them (funding, hiring, leadership, news).
- **contact_signals** (from `contact_signals`): Detected changes about a person (job moves, title changes).
- **social_signals** (from `social_signals`): Social activity signals per person.
- **opportunity_signals** (from `opportunity_signals`): Buying signals spotted in conversations.
- **signals** (from `revenue_opportunities`): Revenue signals (expansion, upsell, referral angles). These are signals, not deals.
- **intelligence_items** (from `intelligence_items`): Intelligence items surfaced about people and companies.
- **engagement_scores** (from `engagement_scores`): Two-way engagement scores per person; the inputs to the warmth tier.
- **ideal_client_profiles** (from `ideal_client_profiles`): Ideal client profiles.
- **services** (from `services`): The services the firm sells.
- **proof_points** (from `proof_points`): Proof points and evidence the firm uses.
- **gtm_problems** (from `gtm_problems`): The customer problems the firm solves.
- **competitors** (from `competitors`): Competitors and how the firm wins against them.
- **manifestos** (from `manifestos`): Positioning manifestos.
- **custom_fields** (from `custom_fields`): Custom field definitions.
- **custom_field_values** (from `custom_field_values`): Custom field values per record.
- **lead_source_options** (from `lead_source_options`): The workspace lead-source list.
- **relationship_memory** (from `contact_ai_profiles`): The synthesised relationship memory per person: history, facts, what not to assume.
- **account_memory** (from `company_ai_profiles`): The synthesised account memory per company.
- **agent_memories** (from `agent_memories`): Durable memories Nynch agents kept about people, workflows and preferences.
- **knowledge_entities** (from `kg_entities`): Knowledge-graph entities for people, companies and deals.
- **known_facts** (from `kg_relationships`): Known facts: relationships between people, companies and deals with their evidence.
- **beliefs** (from `canonical_beliefs`): Buyer beliefs the firm has catalogued.
- **belief_observations** (from `belief_observations`): Each time a belief was observed in a conversation.
- **deal_learnings** (from `deal_learnings`): Patterns learned from won and lost deals.
- **proposal_learnings** (from `proposal_learnings`): Patterns learned from proposals.
- **decisions** (from `decisions`): Decisions recorded with their reasoning.
- **introduction_requests** (from `introduction_requests`): Warm introduction requests and their outcomes.
- **team_intro_requests** (from `team_intro_requests`): Introduction asks between teammates.
- **relationship_paths** (from `relationship_paths`): Warm paths to target people through connectors.
- **circles** (from `circles_of_leverage`): Circles: the people around one target, each asked to help open a conversation.
- **circle_members** (from `circle_members`): Members of each circle.
- **proposals** (from `proposals`): Proposals with their full content (also written to proposals/).
- **proposal_versions** (from `proposal_versions`): Every proposal version as a frozen document.
- **proposal_options** (from `proposal_options`): Priced options inside proposals.
- **proposal_artifacts** (from `proposal_export_artifacts`): Where each proposal was exported to (links to the generated documents).
- **agreements** (from `commercial_agreements`): Commercial agreements per account.
- **agreement_versions** (from `commercial_agreement_versions`): Accepted versions of each agreement with frozen terms.
- **agreement_lines** (from `commercial_agreement_lines`): Service lines inside accepted agreement versions.
- **projects** (from `client_projects`): Delivery projects per account.
- **campaigns** (from `campaigns`): Outreach campaigns and their steps.
- **campaign_enrollments** (from `campaign_enrollments`): Who was enrolled in which campaign and what happened.
- **company_briefs** (from `company_briefs`): Company briefs written for prospects.



## Third-party personal data

Transcripts, email messages, chat and LinkedIn messages contain the words and personal data of people outside your firm. Your firm owns these records and is their data controller; they were collected under your firm's own basis for processing, and that responsibility travels with this folder. Keep it where you would keep your CRM.

## What is not in the folder, and why

Every table in the product that is not exported is listed here and in `manifest.json` with its reason. The groups:

- AI internals: Agent capability registry. System configuration, not user data. (1 table: agent_registry)
- AI internals: Agent handoff queue state. Operational. (1 table: agent_handoffs)
- AI internals: Agent runtime, queue, checkpoint, schedule, and execution state. (1 table: agent_*)
- AI internals: Agent scratch context store. Durable knowledge lives in agent_memories. (1 table: agent_context_store)
- AI internals: AI feedback telemetry (1 table: ai_recommendation_tracking)
- AI internals: AI response cache (1 table: ai_cache)
- AI internals: AI runtime, cache, evaluation, budget, and provider internals. (1 table: ai_*)
- AI internals: AI-drafted account version of a pitch deck awaiting human approval, same class as value_audit_outreach_drafts. Letting the model read its own unapproved deck copy back would turn generated claims about a buyer into recorded fact; what the account actually said stays canonical in meeting_transcripts, deal_stakeholders and the Deal research dossier. (1 table: deal_pitch_renders)
- AI internals: AI-drafted outreach awaiting human approval, same class as company_briefs. Unapproved drafts must not feed AI context or search, or the model would read its own unsent output back as fact; the underlying relationship facts stay canonical in contacts, companies, and company_signals, and a sent message becomes canonical in sent_emails. (1 table: value_audit_outreach_drafts)
- AI internals: AI-generated commercial offer hypotheses awaiting human judgement. Blocked until output quality is proven; an offer the user promotes becomes canonical in services. (1 table: offer_opportunities)
- AI internals: Append-only Today ranking exposure and outcome events for model evaluation. The Today queue records remain the customer-facing state. (1 table: today_rank_events)
- AI internals: Bucket recommendation queue state. Accepted outcomes live on contact_buckets. (1 table: bucket_recommendations)
- AI internals: Deal-intelligence calibration and model internals. (1 table: dii_*)
- AI internals: Deal-intelligence resonance work queue. Operational. (1 table: dii_resonance_queue)
- AI internals: Dedupe similarity scores, not user-authored relationship context. (1 table: contact_similarities)
- AI internals: Derived per-offer contact suggestions. The canonical relationship record is the contact itself; these rankings are regenerable AI output. (1 table: offer_shortlist_members)
- AI internals: Derived stage-baseline priors and counts. They are calculation internals, not customer-authored Deal context; customer-facing reads use the governed projection. (1 table: deal_outlook_priors)
- AI internals: DII stage medians. Model internals. (1 table: dii_user_stage_medians)
- AI internals: DII statistical priors. Model internals, not user content. (1 table: dii_user_priors)
- AI internals: Durable Account Research retry/resume state. User-facing reports are stored in ai_cache and surfaced through the reasoning-agent, not indexed as relationship context. (1 table: account_research_jobs)
- AI internals: Durable analyze-intelligence work queue. User-facing intelligence is stored in intelligence_items and opportunity_signals, not indexed from this operational queue. (1 table: intelligence_analysis_queue)
- AI internals: Durable retry state for automatic closed-Deal ICP learning. The resulting evidence lives in icp_learning_events. (1 table: icp_closed_deal_learning_queue)
- AI internals: Evaluation data (1 table: ai_golden_sets)
- AI internals: Evaluation tables (1 table: ai_evaluation_*)
- AI internals: Generated candidate hypotheses awaiting queue admission. Blocking them prevents unreviewed output from reinforcing later AI context. (1 table: daily_relationship_candidates)
- AI internals: Heartbeat calibration telemetry. (1 table: heartbeat_effectiveness)
- AI internals: Heartbeat proactive-scan run history. Outputs surface as ai_alerts/action_items. (1 table: proactive_scan_runs)
- AI internals: Internal ICP refinement work and decision history. The current ideal_client_profiles row is the canonical Assist context, so legacy Golden suggestions and pending AI output must not feed generic intelligence. (1 table: icp_learning_events)
- AI internals: Internal ICP suggestion-application history. Assist reads the current ideal_client_profiles row and canonical closed Deals instead of recycling historical or legacy Golden refinements as intelligence. (1 table: icp_refinement_history)
- AI internals: Legacy Golden Standard pattern retained temporarily for production rollout compatibility. Automatic ICP intelligence now comes from canonical closed Deals. (1 table: user_golden_icp)
- AI internals: Legacy probability history produced by the retired visible scorer. Its event model is not trusted and it cannot be used as generic AI evidence. (1 table: deal_probability_history)
- AI internals: Metrics buffer (1 table: ai_metrics_buffer)
- AI internals: Model prediction telemetry used for calibration. Not user-searchable content. (1 table: prediction_log)
- AI internals: Multi-agent coordination events. Operational observability. (1 table: agent_coordination_log)
- AI internals: One Offer Creator generation run: input snapshot, status, and AI cost metadata. Operational run history; the resulting offers live in offer_opportunities. (1 table: offer_ranking_runs)
- AI internals: Per-call AI cost ledger (our cost, billing-internal) (1 table: ai_call_costs)
- AI internals: Per-call data-provider cost ledger (1 table: data_provider_calls)
- AI internals: Playbook scan run snapshots. Suggestions surface via ai_playbook_suggestions. (1 table: playbook_scan_snapshots)
- AI internals: Probability calibration ledger containing shadow candidate observations and resolved outcomes. It remains evaluation-only until a model promotion contract explicitly approves a user-visible projection. (1 table: deal_prediction_outcomes)
- AI internals: Prompt templates (1 table: ai_prompts)
- AI internals: Prospect planning chat and an unexecuted research plan. It holds what the user asked for and what the assistant suggested, not verified facts about any person or company, so reading it back would present planning guesses as relationship context. (1 table: prospect_conversations)
- AI internals: Provider enrichment cache; accepted fields live on contacts and companies. (1 table: enrichment_cache)
- AI internals: Quality metrics (1 table: ai_quality_metrics)
- AI internals: Signal identity-verification audit ledger; customer reads use current_company_signals. (1 table: company_signal_trust_assessments)
- AI internals: Standard Derived Signal model output and diagnostics. Only explicit approved projections may become product context; raw envelopes never become generic AI evidence. (1 table: derived_signal_snapshots)
- AI internals: Statistical model-evaluation reports and shadow operational health. These internals cannot become customer relationship evidence or generic AI context. (1 table: derived_signal_validation_runs)
- AI internals: Temporary service-suggestion cache. Accepted services live in service_bundles, services, and service_tiers. (1 table: service_suggestions_cache)
- AI internals: Theme extraction cache keyed by question hash. Derived cache, not source content. (1 table: theme_extraction_cache)
- AI internals: Usage tracking (1 table: ai_usage)
- AI internals: User evaluation of a derived interpretation. It must not be recycled as source evidence or generic AI context. (1 table: derived_signal_feedback)
- AI internals: Vendor unit prices (1 table: data_provider_unit_costs)
- AI internals: Workspace usage-credit ledger, prices and background job state (1 table: usage_credit_*)
- Assist and agent conversations and run state; the memories they produced are exported as agent_memories. (21 tables: assist_conversations, assist_projects, assist_workpieces, assistant_actions, chat_messages, conversations, agent_activity_log, agent_project_outputs, agent_projects, agent_schedules, agent_settings, agent_tasks, …)
- Billing: Billing and Stripe plan configuration. (1 table: subscription_plans)
- Billing: Billing details (1 table: subscription_addons)
- Billing: Billing usage meters (1 table: usage_meters)
- Billing: Credit balance and billing state. (1 table: user_credit_wallet)
- Billing: Credit ledger movements. Billing data, not relationship context. (1 table: credit_transactions)
- Billing: Current Value Audit allowance reservation and consumption state. (1 table: audit_usage_operations)
- Billing: Immutable Value Audit allowance event ledger. (1 table: audit_usage_ledger)
- Billing: Internal credit pricing configuration. (1 table: credit_cost_config)
- Billing: Offer Creator trial/paid entitlement and quota counters. Billing state, not relationship context. (1 table: offer_browser_entitlements)
- Billing: Stripe customer, subscription, and charge data. (1 table: stripe_*)
- Billing: Usage metering events. (1 table: usage_events)
- Billing: User billing (1 table: user_addons)
- Billing: Workspace usage metering. (1 table: workspace_usage)
- Buyer-facing rooms and partner-network records shared with other accounts; the proposal and deal behind them are exported. (14 tables: deal_rooms, deal_room_sections, deal_room_items, deal_room_members, esignature_documents, proposal_edit_feedback, proposal_refinement_messages, proposal_option_services, partner_exchanges, partner_exchange_deal_links, partner_exchange_deal_link_events, partner_profiles, …)
- Client-foundation ledgers (billing periods, movements, handover and close events); the agreements, versions, lines and projects they roll up are exported. (13 tables: commercial_agreement_lineages, commercial_agreement_rebase_requirements, commercial_line_movements, commercial_billing_schedule_periods, commercial_external_finance_mappings, commercial_project_allocations, client_project_lifecycle_events, client_project_health_checkpoints, client_handover_runs, client_handover_run_steps, client_handover_run_events, client_close_events, …)
- Configuration: API keys and OAuth tokens (1 table: integration_config)
- Configuration: Chrome extension feature configuration. (1 table: extension_config)
- Configuration: Client-authored marketing framework slides. Content the client writes for their own audience, not relationship context. (1 table: framework_wheels)
- Configuration: Coach-curated skill templates copied into company_skills at signup. Service-role only, not relationship data. (1 table: partner_skill_templates)
- Configuration: Coach/affiliate partner records (service-role only) used by stripe-webhook to seed content into attributed workspaces. Not relationship data. (1 table: partners)
- Configuration: Company-level GTM value drivers (pain relievers and gain creators). Workspace strategy configuration, not relationship contact data. (1 table: gtm_value_drivers)
- Configuration: Connected-app OAuth client grants. Security configuration. (1 table: mcp_oauth_client_grants)
- Configuration: Connected-app OAuth metadata and encrypted tokens (1 table: mcp_user_connections)
- Configuration: Core record source registry used for database validation and UI labels. Product metadata, not relationship context. (1 table: record_source_types)
- Configuration: Custom-domain setup state for the public AI assistant; not relationship data. (1 table: workspace_assistant_domains)
- Configuration: Derived signal-weight configuration. Canonical feedback and deal outcomes remain the auditable learning evidence. (1 table: focus_signal_learning_weights)
- Configuration: Derived soft-ranking weights for Daily Relationship Queue strategies. Canonical assignment events hold the auditable first-party outcomes. (1 table: daily_relationship_generator_learning)
- Configuration: Global workshop admission, closing-time and open-state configuration is outside CRM tenant scope; never relationship context. (1 table: workshop_campaign)
- Configuration: Historical private presentation and webinar content; not relationship context. (1 table: pov_asset_variant_revisions)
- Configuration: Historical private profile facts and sales scripts; not relationship context. (1 table: launch_kit_sales_revisions)
- Configuration: Internal feature flags (1 table: feature_flags)
- Configuration: Internal onboarding state (1 table: onboarding_progress)
- Configuration: Internal transactional email templates. (1 table: system_email_templates)
- Configuration: Junction table linking services to their selected value drivers. Derived from services and gtm_value_drivers config; not relationship data. (1 table: service_value_driver_links)
- Configuration: Per-user list of people hidden from the post-commenter panel; a display preference, not relationship context. (1 table: post_commenter_exclusions)
- Configuration: Per-workspace branding + alert thresholds + lead owner routing for the public AI assistant. (1 table: workspace_assistant_configs)
- Configuration: Per-workspace slide styling (colours, font, logo). Configuration, not relationship context. (1 table: framework_brand_kits)
- Configuration: Private newsletter and video drafts with source research; not relationship context. (1 table: launch_kit_longform)
- Configuration: Private presentation and webinar drafts and generation context; not relationship context. (1 table: pov_asset_variants)
- Configuration: Private profile facts and sales scripts; not relationship context. (1 table: launch_kit_sales_assets)
- Configuration: Private publishing destination configuration. (1 table: launch_kit_publishing_settings)
- Configuration: Private strategy writing, generation state and revision history; not relationship context. (1 table: launch_kit_sprints)
- Configuration: Private UI layout configuration. (1 table: command_centre_layout)
- Configuration: Service-only limits and scheduling controls for Daily Relationship Queue generation; configuration, not relationship context. (1 table: daily_relationship_generator_config)
- Configuration: Service-only reversible briefing preference snapshots. Backup configuration, not relationship context. (1 table: briefing_test_preference_snapshots)
- Configuration: Service-only temporary briefing test controls. Operational configuration, not relationship context. (1 table: briefing_test_controls)
- Configuration: Short-lived OAuth verifier state (1 table: mcp_oauth_state)
- Configuration: System rule catalog. Installed and customized rules live in user_rules. (1 table: rule_templates)
- Configuration: System skill catalog. Installed skills live in user_skills and company_skills. (1 table: skill_library)
- Configuration: System skill templates. Installed skills live in user_skills and company_skills. (1 table: skill_templates)
- Configuration: Temporary workspace capability switch for the pre-launch Value Audit pilot. It grants access only and contains no relationship context. (1 table: value_audit_preview_workspaces)
- Configuration: The consultant's own published writing. Read through the dedicated voice-context loader for drafting prompts, never generic AI context. (1 table: workspace_content_items)
- Configuration: The workspace's own core problem statement. Read through the Strategy loader for generation prompts; the same statement stays on the profile for Assist. (1 table: workspace_strategy_profiles)
- Configuration: User-installed Preferences and Playbooks use dedicated scope-aware loaders and explicit consultation. Never expose them through generic context, search, or RAG. (1 table: user_skills)
- Configuration: Workspace Preferences and Playbooks use dedicated scope-aware loaders and explicit consultation. Never expose them through generic context, search, or RAG. (1 table: company_skills)
- Configuration: Workspace typical-vs-opposite strategy drafts. Adopted rows reach Assist only through the dedicated GTM context reader, never generic table dump or RAG. (1 table: gtm_market_contrasts)
- Configuration: Workspace-owned Value Audit generation and rendering configuration. It contains instructions rather than customer or relationship facts and must not enter assistant context. (1 table: value_audit_domain_packs)
- Configuration: Your Content source registry (blog, Substack, own LinkedIn). Configuration, not relationship context. (1 table: workspace_content_sources)
- Derived from deals, which is exported. (1 table: client_engagement_states)
- Derived from proposal_learnings, which is exported. (1 table: proposal_learning_plan_usage_attempts)
- Derived from proposals, which is exported. (5 tables: proposal_delivery_attempts, proposal_delivery_receipts, proposal_generation_operations, proposal_generation_plan_usage_attempts, proposal_export_reconciliation_events)
- Firm memory that carries only a user id, not a workspace id. Exporting it by membership would put the rows a teammate holds in another workspace into this one, so it waits until the table carries workspace_id. (5 tables: testimonials, user_case_studies, knowledge_base_items, methodologies, outcome_patterns)
- Import working tables: Import job tracking (1 table: csv_import_jobs)
- Import working tables: Temporary bucket-game/import staging; accepted contacts are canonical. (1 table: pending_contacts)
- Import working tables: Temporary dedupe candidates (1 table: csv_import_match_candidates)
- Import working tables: Temporary import data (1 table: csv_import_rows)
- Nynch administration: Administrative tables (1 table: admin_*)
- Nynch administration: Company revenue ledger — every Nynch Stripe payment classified by SaaS/services split. Super-admin only; company finance data, not user relationship context. (1 table: revenue_events)
- Nynch administration: Customer support issue workflow; admin-facing operational data, not relationship context. (1 table: bug_reports)
- Nynch administration: Daily company-wide KPI snapshot (Nynch Top 20). Internal founder/admin business metrics, not user relationship context. (1 table: company_kpi_snapshots)
- Nynch administration: Internal database-audit findings, not customer relationship context. (1 table: audit_results)
- Nynch administration: Internal product roadmap, not customer relationship data. (1 table: roadmap_items)
- Nynch administration: Private screenshots for customer support issue workflow; admin-facing operational evidence, not relationship context. (1 table: bug_report_attachments)
- Nynch administration: Support content and conversations use the dedicated support/wiki path. (1 table: support_*)
- Nynch commercial state: Append-only commercial classification and correction history. Reporting may reconstruct it explicitly, but generic AI context must use the canonical Deal motion rather than historical authority records. (1 table: deal_commercial_motion_history)
- Nynch commercial state: Effective-dated Account owner history. Generic AI context must use the canonical current Account projection, not reconstruct authority from historical rows. (1 table: account_owner_assignments)
- Nynch commercial state: Private approved-proof selection for Audit Playbooks. (1 table: audit_playbook_version_proofs)
- Nynch commercial state: Private Audit Playbook lifecycle configuration. (1 table: audit_playbooks)
- Nynch commercial state: Private consultant method and outreach configuration. (1 table: audit_playbook_versions)
- Nynch commercial state: Private immutable evidence provenance for customer-facing Value Audits. (1 table: value_audit_evidence_packs)
- Nynch commercial state: Private source excerpts, URLs, confidence, freshness, and visibility for Value Audits. (1 table: value_audit_evidence_items)
- Nynch commercial state: Private standing account hypotheses (pains, competitors, buying roles) drafted from audit research. Direction for generation, never assistant context: an unverified hypothesis read back as fact is exactly the fabrication class the registry exists to block. (1 table: value_audit_account_plans)
- Nynch commercial state: Private standing sector research reused across Value Audits; market items enter audits only through the evidence pipeline, never assistant context. (1 table: value_audit_sector_briefs)
- Operational state: Add/Skip decisions feeding the exec monitor trust model. Feature-internal learning state. (1 table: exec_suggestion_decisions)
- Operational state: Ambiguous historical Deal outcome repair queue. It is review workflow state, not customer-authored relationship context. (1 table: deal_outcome_repair_queue)
- Operational state: Append-only Daily Relationship Queue outcome and recipe-learning ledger; never generic AI context or RAG material. (1 table: daily_relationship_assignment_events)
- Operational state: Append-only Deal outcome landmarks for leakage-safe model evaluation. The canonical Deal already supplies customer-facing outcome context. (1 table: deal_outcome_events)
- Operational state: Backend-only merge values for campaign emails from website sign-ups (the calculator); the same result is written as a contact note, which is the copy Assist reads. (1 table: contact_signup_results)
- Operational state: Bulk processing job and item state; user-facing records live in their destination sources. (1 table: bulk_job*)
- Operational state: Calculated product reliability scoreboard snapshots for super-admin owners. They contain operational evidence only, never relationship context. (1 table: feature_reality_status_snapshots)
- Operational state: Campaign jobs-page watch state. User-visible journey state remains canonical on campaign_enrollments, pending_campaign_actions, and the drafted email. (1 table: campaign_job_watches)
- Operational state: Candidates held back pending a past-roles retry. Detected people surface as new_exec_suggestions once resolved. (1 table: exec_monitor_held_candidates)
- Operational state: Champion job-change watch state: user champion marks and last-checked times. The findings live on contact_job_changes and the alerts on ai_alerts. (1 table: champion_watch)
- Operational state: Coalescing flags and Gmail catch-up cursors for push-triggered syncs. (1 table: sync_push_triggers)
- Operational state: Contact merge audit trail; the surviving contact is canonical. (1 table: contact_merges)
- Operational state: Content-workspace run health and counters; source ideas and drafts are indexed separately. (1 table: content_analysis_status)
- Operational state: Customer-owned Resend provider event receipts and webhook ingestion ledger. Campaign delivery outcomes are canonical on campaign_email_delivery_claims. (1 table: resend_campaign_event_receipts)
- Operational state: Customer-visible website-scan progress receipt. It records process state only; safe website signals remain on the company and public cache contracts. (1 table: company_website_scan_receipts)
- Operational state: Daily Relationship Queue generation state. The user-facing people and relationship evidence remain on canonical Contact and Company records. (1 table: daily_relationship_days)
- Operational state: Database query performance telemetry. (1 table: query_performance_log)
- Operational state: Delivery receipts for the 30-minute meeting brief (claimed, delivered, skipped). The brief content lives on meeting_prep_docs and notifications. (1 table: meeting_brief_deliveries)
- Operational state: Derived contact meeting-count repair cursors are not relationship evidence. (1 table: contact_meeting_rollup_checkpoints)
- Operational state: Derived daily ranking and workflow state. Canonical Contact, Company, signal, and action records remain the relationship truth. (1 table: daily_relationship_assignments)
- Operational state: Derived prospecting signal response weights; canonical outreach and reply evidence remains on queue and interaction records. (1 table: prospecting_signal_response_weights)
- Operational state: Email enrichment execution log; accepted data lives on contacts. (1 table: email_enrichment_log)
- Operational state: Email generation execution log; sent/draft content has canonical sources. (1 table: email_generation_log)
- Operational state: Email send execution state; messages and sent emails are canonical. (1 table: email_sends)
- Operational state: End-of-day synthesis execution logs. (1 table: eod_synthesis_logs)
- Operational state: Enrichment work queue and retry state. (1 table: enrichment_jobs)
- Operational state: Firmographic queue control, job receipts, and health ledgers. Accepted customer-facing fields and provenance are stored on companies. (1 table: company_enrichment_*)
- Operational state: Frozen ranking snapshots derived from canonical company, signal, relationship, and activity records; avoid duplicating that evidence in AI context. (1 table: focus_company_members)
- Operational state: Granular campaign delivery and tracking telemetry; campaign outcomes are canonical. (1 table: campaign_email_*)
- Operational state: Grok Bot campaign assignment queue. User-visible journey state remains canonical on campaign_enrollments, pending_campaign_actions, and contact notes. (1 table: campaign_agent_jobs)
- Operational state: Historical employer-link remediation queue and audit status. Verified relationship truth remains on contacts and companies. (1 table: contact_company_review_queue)
- Operational state: How each target-market tier was built (filters, estimated size, confidence). The tiered companies themselves are canonical company records. (1 table: market_definitions)
- Operational state: Immutable engagement correction audit; read only through bounded parent-authorised lifecycle history. (1 table: client_engagement_events)
- Operational state: Import progress state; safe connection status is exposed through the connected-integration context wrapper. (1 table: integration_import_progress)
- Operational state: Internal identity-resolution conflict evidence; canonical user context remains on contacts. (1 table: contact_identity_conflicts)
- Operational state: Internal progress ledger for the bounded company identity shadow scan; never relationship context. (1 table: company_identity_observation_scan_state)
- Operational state: Internal reversible identity-link ledger; canonical user context remains on contacts. (1 table: contact_identity_links)
- Operational state: Internal shadow identity evidence and reviewer labels; canonical user context remains on companies. (1 table: company_identity_observations)
- Operational state: Last contact-duplicate scan receipt per workspace for honest empty states; accepted people remain on contacts. (1 table: workspace_dedupe_scan_state)
- Operational state: Learned skip patterns a user undid for the exec monitor. Feature-internal learning state. (1 table: exec_skip_pattern_overrides)
- Operational state: Lease and retry state for governed derived-signal calculation. It contains process health, not customer relationship knowledge. (1 table: derived_signal_recalc_queue)
- Operational state: Merge audit events; the surviving contact is canonical. (1 table: contact_merge_events)
- Operational state: Merge audit trail; the surviving company is canonical. (1 table: company_merges)
- Operational state: Multi-key identity graph (LinkedIn vanity/opaque/conversation, email, phone, name) for match/orphan attach; canonical person context remains on contacts. (1 table: contact_identity_keys)
- Operational state: Offline release-gate receipts for Today readiness; operational validation metadata. (1 table: daily_relationship_readiness_validation_receipts)
- Operational state: One-off website pre-qualification runs over market candidates. Not monitoring; the resulting companies are canonical records. (1 table: market_web_check_runs)
- Operational state: Operational no-resend delivery and history-reconciliation ledger. Canonical sent content is stored in sent_emails. (1 table: outbound_email_send_attempts)
- Operational state: Pending new-executive suggestions for the exec monitor. Surfaced through its own alert card, not Assist, until the feature is proven. (1 table: new_exec_suggestions)
- Operational state: Per-candidate website check outcome and a short quote, kept for the add step. Canonical company records carry the durable context. (1 table: market_web_check_results)
- Operational state: Per-contact relationship-memory refresh queue and rewrite timestamps. The memory itself lives on contact_ai_profiles. (1 table: relationship_memory_refresh_state)
- Operational state: Per-contact scan receipts proving the fan-out ran. Detected signals live in contact_signals and ai_alerts. (1 table: signal_scan_receipts)
- Operational state: Per-mailbox receive bookkeeping (workspace, last sync, last error) for secondary mailboxes. (1 table: email_account_sync_state)
- Operational state: Per-mailbox sync lease so push, drain and hourly poll never overlap. (1 table: mail_sync_leases)
- Operational state: Per-run progress and spend tracking for the weekly new-exec detector. Operational, never relationship context. (1 table: exec_monitor_progress)
- Operational state: Per-user approval rate for the exec monitor trust model. Feature-internal state. (1 table: exec_monitor_trust)
- Operational state: Per-user generation status and credit accounting. Materialised queue records hold the user-facing result. (1 table: daily_relationship_generator_runs)
- Operational state: Per-workspace watch allowance settings (per-person cap, private-watch toggle). Configuration, not relationship context. (1 table: workspace_watch_settings)
- Operational state: Point-in-time Focus market readiness diagnostics. Canonical companies, signals, and ICP/service records remain the source of market truth. (1 table: focus_market_snapshots)
- Operational state: Principal-independent private Storage deletion receipts for removed Audit introduction videos. Cleanup state must never become generic AI context. (1 table: value_audit_video_cleanup_tombstones)
- Operational state: Privacy-safe intent, provider-attempt, quality, and cost receipts for email resolution. Accepted addresses live on Contacts; these ledgers are operational evidence, not AI context. (1 table: email_resolution_*)
- Operational state: Private actor-owned email and meeting attribution receipts. Human attachments are not generic Assist or retrieval evidence; source access remains scoped to the attaching person and workspace. (1 table: deal_communication_links)
- Operational state: Private actor-scoped relationship snapshots; read only through the permissioned history RPC (1 table: contact_member_histories)
- Operational state: Private audience snapshots and resumable import progress; canonical journeys are campaign enrollments. (1 table: campaign_audience_imports)
- Operational state: Private durable Value Audit queue, lease, retry, and quality receipts. (1 table: value_audit_jobs)
- Operational state: Private engagement idempotency payload and exact replay receipts; never generic AI context. (1 table: client_engagement_receipts)
- Operational state: Private historical market reset recovery snapshots; never relationship context. (1 table: focus_reset_20261009)
- Operational state: Private history discovery cursors; operational progress, not generic CRM evidence. (1 table: relationship_memory_discovery)
- Operational state: Private per-alert selection and planned-action state. The customer-facing company alert carries the evidence and roster; this table must not become generic AI context. (1 table: company_alert_contact_actions)
- Operational state: Private publication and draft delivery receipts; canonical activity is exposed by Today. (1 table: launch_kit_asset_receipts)
- Operational state: Private reviewed repair audit and authorization state; never generic relationship context or retrieval evidence. (4 tables: deal_activity_repair_receipts, legacy_commercial_repair_receipts, historical_pipeline_repair_plans, historical_pipeline_repair_items)
- Operational state: Private source-backed continuity excerpts; read only through the actor-authorised evidence RPC. (1 table: relationship_memory_evidence)
- Operational state: Private target-market before-images and compare-and-set state; never relationship context. (1 table: target_market_reset_rows)
- Operational state: Private target-market confirmation and recovery receipts; never relationship context. (1 table: target_market_reset_operations)
- Operational state: Private transaction-bound recovery context; never relationship context. (1 table: target_market_restore_context)
- Operational state: Private unfinished composer state. The sent message becomes canonical in sent_emails; drafts must not influence AI context or search. (1 table: email_drafts)
- Operational state: Private upload and lifecycle ledger for Audit introduction videos. The approved Audit snapshot carries the public-facing media reference; upload state must never become generic AI context. (1 table: value_audit_video_assets)
- Operational state: Private Value Audit batch receipts, dispositions, and counts. (1 table: value_audit_batches)
- Operational state: Private visitor no-resend ledger; never relationship context. (1 table: visitor_email_delivery_intents)
- Operational state: Provider push subscription ids, expiries and per-subscription client secrets for mail and calendar sync. Never AI context. (1 table: sync_push_subscriptions)
- Operational state: Provider sync cursors and progress only; campaign engagement is read through the bounded Focus Companies workflow, not generic Assist context. (1 table: zenabm_engagement_syncs)
- Operational state: Provider, cost, and stage receipts for Daily Relationship Queue generation; operational evidence, not relationship context. (1 table: daily_relationship_receipts)
- Operational state: Public authority-scan rate limiting. (1 table: authority_scan_rate_limits)
- Operational state: Public endpoint rate limiting. (1 table: public_rate_limits)
- Operational state: Redacted contact-to-company reliability ledger. It records outcome and refresh state only; never relationship context. (1 table: company_link_live_proofs)
- Operational state: Redacted operator-attended Chrome proof ledger. It records only outcome, extension version, source commit, and time; never relationship context. (1 table: extension_supervised_proofs)
- Operational state: Repair audit trail; canonical company/contact records hold the corrected result. (1 table: company_mismatch_audit)
- Operational state: Retired Academy community forum. Product surface removed; rows retained. Not relationship context. (4 tables: academy_forum_categories, academy_forum_posts, academy_forum_comments, academy_forum_votes)
- Operational state: Retired Academy events calendar. Product surface removed; rows retained. Not relationship context. (1 table: academy_events)
- Operational state: Retry queue for deleting unreferenced storage objects after failed uploads. Canonical customer records and their live storage references remain the source of truth. (1 table: storage_orphan_cleanup_queue)
- Operational state: Retryable per-source generation work and provider receipts. Materialised queue records hold the user-facing result. (1 table: daily_relationship_generator_jobs)
- Operational state: Reviewed recovery audit and idempotency state is not relationship context. (1 table: deal_customer_recovery_receipts)
- Operational state: Rotation state for which watched company was last checked. Operational scheduling state, not relationship context. (1 table: exec_monitor_company_checks)
- Operational state: Scheduled-job locks and processor state. (1 table: cron_*)
- Operational state: Sealed Today readiness evaluation evidence and verdicts; operational proof, not generic AI context. (1 table: daily_relationship_readiness_evaluations)
- Operational state: Send ledger for inbound audit emails: recipient, attempts, and failure reason. The audit itself is canonical on company_briefs and the person who asked is canonical on the captured lead; this row only records whether it reached them. (1 table: inbound_audit_deliveries)
- Operational state: Server-owned Fathom delivery and polling checkpoints are operational state; meeting content remains on meeting_transcripts. (1 table: fathom_webhook_state)
- Operational state: Service-only archive of duplicate email rows removed by the one-row-per-message cleanup. (1 table: email_message_dedupe_archive)
- Operational state: Service-only campaign transfer lineage. User-visible journey state remains canonical on campaign_enrollments. (1 table: campaign_enrollment_transfers)
- Operational state: Service-only cursor per activity source for the team re-match backfill: last id, counts and pass timestamps. Holds no customer content; the re-attached activity lives on its own sources. (1 table: team_activity_rematch_progress)
- Operational state: Service-only ledger for handing the records of a departed teammate to their successor: planned and moved counts per record kind, the resume step and the ids of rows a guard refused. Operational handover state, not relationship context. (1 table: member_offboarding_handover*)
- Operational state: Service-only ledger for the Monday team summary: stored facts, the single AI narrative and a resume lease. Derived from deals, contacts and intro requests, which stay canonical; never relationship context. (1 table: team_weekly_summary_runs)
- Operational state: Service-only per-recipient delivery state for the Monday team summary (notification id, email intent). Delivery bookkeeping, not relationship context. (1 table: team_weekly_summary_deliveries)
- Operational state: Service-only private calendar delivery identity and recovery receipts are not relationship context. (1 table: private_calendar_prep_deliveries)
- Operational state: Service-only record of a disconnected mailbox (owner, provider account, address) so its stored mail can be re-attached when the same user reconnects it. (1 table: email_account_tombstones)
- Operational state: Service-only resume cursors for batch scans (kg-sync, ai-data-hygiene); operational state, never relationship context. (1 table: system_scan_cursors)
- Operational state: Service-only run ledger for a supervised workspace tenancy move: table checkpoints, batch counts and the parked identity keys of records being merged. Operational migration state, not relationship context, and the parked columns briefly hold an email or LinkedIn URL that must never become generic AI context. (1 table: workspace_tenancy_move_*)
- Operational state: Service-run queue that removes the mail and calendar data synced from a disconnected mailbox or calendar. (1 table: mailbox_data_purges)
- Operational state: Short-lived enrichment debit reservation and settlement state. Accepted profile data lives on contacts and durable balance movements live in credit_transactions. (1 table: contact_enrichment_credit_reservations)
- Operational state: Short-lived working list for a winnable-market refresh spanning several calls; removed when the refresh finishes. Company records carry the tiers. (1 table: market_som_refresh_progress)
- Operational state: Singleton cap/cursor control for the nightly signal fan-out. Operational, never relationship context. (1 table: signal_scan_config)
- Operational state: Snapshot of statuses changed by the one-time 2026-10-14 meeting insight sweep, kept so the sweep can be reversed; internal migration state, never relationship context. (1 table: meeting_insight_sweep_20261014)
- Operational state: Telegram send/delivery log; conversation history is canonical. (1 table: telegram_message_log)
- Operational state: Temporary dedupe candidates; accepted results live on contacts. (1 table: contact_dedupe_candidates)
- Operational state: Temporary enrichment staging; accepted contact data is canonical. (1 table: pending_contact_enrichments)
- Operational state: Throttle state for refreshing derived prospecting signal response weights; canonical outreach and reply evidence remains on queue and interaction records. (1 table: prospecting_signal_response_refresh_state)
- Operational state: TypeSafe Jev shadow-mode agreement ledger: compares Jev decisions with LLM decisions in background, never affects user-facing outputs. Operational diagnostic evidence only, not relationship context or AI input. (1 table: jev_shadow_comparisons)
- Operational state: UI dismissal state, not relationship knowledge. (1 table: flow_dismissals)
- Operational state: User-visible monthly focus-list workflow state. Canonical company, signal, deal, task, and campaign records already provide the underlying context. (1 table: focus_company_cohorts)
- Operational state: Webhook delivery and dedupe log. (1 table: webhook_processing_log)
- Operational state: Which pre-written webinar posts an owner removed. The posts themselves remain canonical on content_drafts. (1 table: campaign_post_dismissals)
- Operational state: Which teammate owns each exec in the New in Role campaign. Ownership state; the person themselves lives in contacts. (1 table: exec_campaign_enrollments)
- Operational state: Who was @mentioned in a record comment; notification routing state, not relationship context. (1 table: record_mentions)
- Product configuration, personal preferences or app housekeeping, not business memory. Workspace name and plan are in manifest.json. (45 tables: calendar_display_preferences, contact_field_layout, crm_views, deal_monitor_config, meeting_prep_settings, nudge_preferences, nudge_dismissals, signal_preferences, signal_definitions, signal_types, sync_preferences, time_blocking_preferences, …)
- Prospecting, content-studio and Academy working tables; anything that became a person, company or deal is exported there. (20 tables: prospects, prospect_deep_signals, prospect_outreach, sprint_opportunities, sprint_runs, connection_sprints, connector_scores, pending_campaign_actions, testimonial_requests, content_brief_hooks, content_drafts, content_outlines, …)
- Raw signal evidence; the verified view is exported as company_signals from current_company_signals. (1 table: company_signals)
- Regenerable AI output or derived score; the facts it was built from are exported. (54 tables: ai_alerts, ai_playbook_suggestions, ai_playbooks, ai_recommendations_log, aha_moments, calendar_event_insights, checklist_ai_suggestions, company_data_suggestions, contact_data_suggestions, contact_intelligence_cards, content_recommendations, deal_ai_suggestions, …)
- Reusable templates and packaging that only make sense inside the product; the services, proposals and campaigns built from them are exported. (20 tables: email_templates, email_templates_ai, campaign_templates, proposal_templates, service_templates, service_bundles, bundle_tiers, bundle_tier_services, service_tiers, gtm_customer_profiles, gtm_problem_links, user_deal_room_blocks, …)
- Security-sensitive: Confidential paid-diagnostic engagement state is read only through its own contracts, never as generic AI context. (1 table: client_diagnostics)
- Security-sensitive: Confidential paid-diagnostic sources, evidence, findings and history are read only through their own contracts, never as generic AI context. (1 table: client_diagnostic_*)
- Security-sensitive: Connected mailbox account and token metadata. (1 table: email_accounts)
- Security-sensitive: Connected-app keys, grants, subscriptions, logs, and OAuth state. (1 table: mcp_*)
- Security-sensitive: Dependent rows captured with a Company delete so Undo can restore them; never AI context. (1 table: company_delete_contract_secondary_items)
- Security-sensitive: Ephemeral private transaction authorization for canonical Account creator rehoming. It is a mutation fence, never customer knowledge or AI context. (1 table: account_creator_rehome_authorizations)
- Security-sensitive: Ephemeral transaction authority for advancing the campaign sender rotation counter during the trusted pool update. It is a security fence, never customer knowledge or AI context. (1 table: campaign_sender_counter_authorizations)
- Security-sensitive: Ephemeral transaction authority for assigning immutable campaign sender snapshots during the trusted pool update. It is a security fence, never customer knowledge or AI context. (1 table: campaign_sender_assignment_authorizations)
- Security-sensitive: Ephemeral transaction authority for closing current Account owners during archive or merge. It is a security fence, never customer knowledge or AI context. (1 table: account_lifecycle_owner_close_authorizations)
- Security-sensitive: HMAC-only erased-identifier tombstones; never user or AI context. (1 table: core_erased_*)
- Security-sensitive: Immutable outcome correction snapshots are audit evidence and must not enter generic Assist retrieval. (1 table: zenabm_outcome_link_reviews)
- Security-sensitive: Mailbox identities and provider configuration. (1 table: email_identities)
- Security-sensitive: Non-PII privacy-erasure proof. It is an operator compliance record, never relationship evidence or AI context. (1 table: personal_account_erasure_tombstones)
- Security-sensitive: OAuth tokens and pending authorization state. (1 table: oauth_*)
- Security-sensitive: Observed outcome links are scoped to explicit same-company deals and remain outside generic Assist retrieval until a dedicated reader is validated. (1 table: zenabm_outcome_links)
- Security-sensitive: Per-action execution receipts proving an automation step ran exactly once; the resulting record change is the customer-facing truth. (1 table: automation_action_receipts)
- Security-sensitive: Pre-delete Company snapshot retained only to power Undo; never AI context. (1 table: company_delete_contract_snapshots)
- Security-sensitive: Privacy-bounded mutation audit ledger; exposed only through the policy-scoped receipt contract, never as AI context. (1 table: core_mutation_receipt*)
- Security-sensitive: Private blocked-validation replay details for Deal close and correction. They must never become generic AI context. (1 table: deal_close_failure_replay_payloads)
- Security-sensitive: Private campaign mailbox routing configuration is not generic relationship context. (1 table: campaign_sender_pool)
- Security-sensitive: Private human coaching and delivery state must never become AI knowledge or public content. (1 table: human_coaching_*)
- Security-sensitive: Private idempotency and replay ledger for Account owner mutations; never relationship evidence or AI context. (1 table: account_owner_assignment_receipts)
- Security-sensitive: Private immutable Deal close and correction replay payloads. Canonical commercial truth remains on the Deal and its outcome event. (1 table: deal_outcome_mutation_replay_payloads)
- Security-sensitive: Private owner feedback; no generic AI indexing. (1 table: growth_review_decisions)
- Security-sensitive: Private repeatable growth state is available only through owner-scoped or explicitly consented readers. (12 tables: growth_plans, growth_plan_versions, growth_events, growth_event_variants, growth_contributions, growth_cadence_rules, growth_materialization, growth_occurrences, growth_evidence_links, growth_invitations, growth_invitation_evidence, growth_event_changes)
- Security-sensitive: Private reviewed drafts, task and send receipts, and usage events are not relationship evidence. (1 table: meeting_follow_through*)
- Security-sensitive: Private tenant-bound terminal RPC responses for exact retry replay. Product state remains on the Account or Deal; this ledger is never AI context. (1 table: client_contract_command_results)
- Security-sensitive: Private transaction-local authorization fence for the exact proposal delivery receipt transition; never customer knowledge or AI context. (1 table: proposal_delivery_mutation_context)
- Security-sensitive: Private transaction-local scope for exact campaign privacy erasure; never customer knowledge or AI context. (1 table: campaign_privacy_erasure_scopes)
- Security-sensitive: Private visitor opt-out capability state; never relationship context. (1 table: visitor_email_suppressions)
- Security-sensitive: Provider-sourced account metrics are exposed only through the authenticated Focus Companies workflow until a workspace-scoped Assist reader is validated. (1 table: zenabm_account_engagement)
- Security-sensitive: Raw ABM review rows include unfiltered provider payloads; Assist may read only the tenant-scoped view that returns user-reviewed rows and omits provider evidence. (1 table: zenabm_review_actions)
- Security-sensitive: Restricted keyed erasure registry material; never user or AI context. (1 table: core_erasure_*)
- Security-sensitive: Row-level payloads of a pre-delete Company snapshot, retained only to power Undo. (1 table: company_delete_contract_snapshot_items)
- Security-sensitive: Service-only workshop applicant emails, payloads, token hashes and referral receipts are outside CRM tenant scope; never generic AI context. (1 table: workshop_applications)
- Security-sensitive: Service-role-only allowlist for the invite-only OAuth signup gate. It is an access-control fence holding third-party emails, never customer knowledge or AI context. (1 table: approved_signup_emails)
- Security-sensitive: Single-row shared secret proving a request came from our own pg_cron jobs. Owner/service-role only; never customer knowledge or AI context. (1 table: internal_cron_auth)
- Security-sensitive: Stored terminal responses for replayed Contact contract calls; the saved Contact is the customer-facing truth. (1 table: contact_contract_replay_payloads)
- Security-sensitive: Stored terminal responses for replayed Deal delete/restore calls; the Deal row is the customer-facing truth. (1 table: deal_lifecycle_replay_payloads)
- Security-sensitive: Stored terminal responses for replayed Deal stage moves; the Deal stage is the customer-facing truth. (1 table: deal_stage_move_replay_payloads)
- Security-sensitive: Stored terminal responses for replayed Opportunity delete/restore calls; the Opportunity row is the customer-facing truth. (1 table: opportunity_lifecycle_replay_payloads)
- Security-sensitive: Telegram account-linking credentials. (1 table: telegram_link_*)
- Security-sensitive: Telegram connection credentials and status. (1 table: telegram_connections)
- Security-sensitive: Telegram session state. (1 table: telegram_sessions)
- Security-sensitive: Transaction-local erasure authority rows. Access control state, not relationship context. (1 table: growth_erasure_scopes)
- Security-sensitive: Unmatched and conflicting provider identities require explicit human review and must not enter generic Assist retrieval. (1 table: zenabm_identity_reviews)
- Security-sensitive: User AI provider configuration and encrypted credentials. (1 table: user_ai_*)
- Security-sensitive: Webhook endpoints and secrets. (1 table: user_webhooks)
- Security-sensitive: Zapier authorization and connection credentials. (1 table: zapier_*)
- Sign-in and authorisation state: Invitation delivery receipts and retry state; recipient addresses and process health, not relationship context. (1 table: workspace_invitation_delivery_attempts)
- Sign-in and authorisation state: Invitation tokens (1 table: workspace_invitations)
- Sign-in and authorisation state: Permission mappings (1 table: workspace_members)
- Sign-in and authorisation state: Private paid activation and failure receipts are operational evidence, not relationship context. (1 table: paid_purchase_activation_journeys)
- Sign-in and authorisation state: Private Team application decisions are authorization state, not relationship context. (1 table: team_checkout_approvals)
- Sign-in and authorisation state: Records which actor may change a custom field definition; an authority mapping, not relationship context. (1 table: custom_field_definition_delegations)
- Sign-in and authorisation state: Removed SSO providers and their verified domains; gates converting members to password accounts (1 table: workspace_sso_removed_connections)
- Sign-in and authorisation state: Role assignments (1 table: workspace_user_roles)
- Sign-in and authorisation state: Role definitions (1 table: user_roles)
- Sign-in and authorisation state: Security audit logs (1 table: workspace_access_log)
- Sign-in and authorisation state: Service-managed commercial capability assignments, not relationship context. (1 table: workspace_capability_grants)
- Sign-in and authorisation state: Service-only checkout intents contain billing identity and payment session identifiers. (1 table: stripe_checkout_attempts)
- Sign-in and authorisation state: Service-only immutable purchase-to-account receipts are authorization state, not AI context. (1 table: paid_subscription_owners)
- Sign-in and authorisation state: Service-only paid-checkout login delivery receipts can temporarily contain a bearer link and must never become AI context. (1 table: paid_checkout_login_email_receipts)
- Sign-in and authorisation state: Service-only Team-upgrade cancellation receipts contain billing identifiers and must never become AI context. (1 table: team_upgrade_proration_receipts)
- Sign-in and authorisation state: Service-only workspace billing provenance is not relationship context. (1 table: workspace_subscription_claim_receipts)
- Sign-in and authorisation state: Single-use paid-checkout login handoff receipts contain authentication state and must never become AI context. (1 table: paid_checkout_handoffs)
- Sign-in and authorisation state: SSO domain verification tokens (1 table: workspace_sso_domains)
- Sign-in and authorisation state: SSO identity provider configuration (1 table: workspace_sso_connections)
- System internals: Admin-defined per-step conversion targets for the growth/case-study funnels; configuration data, not relationship context. (1 table: growth_funnel_goals)
- System internals: Anonymous per-step page views for the public funnels; analytics only, not logged-in relationship context for Superbrain indexing. (1 table: growth_funnel_page_views)
- System internals: Assist source-access registry, not customer data. (1 table: superbrain_source_registry)
- System internals: Automation failure/debug log. Operational, not relationship context. (1 table: automation_failures)
- System internals: Background LinkedIn-search workqueue drained by the Chrome extension. Operational state, not relationship data. (1 table: linkedin_search_jobs)
- System internals: Chrome extension acceptance-check observability. User-facing relationship outcomes are stored on contacts and drip_connect_queue. (1 table: linkedin_connection_check_runs)
- System internals: Chrome extension parse telemetry. Operational observability. (1 table: extension_parse_telemetry)
- System internals: Config: sources that stay searchable but are never embedded (mirrors contract sources without the rag surface). (1 table: rag_indexing_exclusions)
- System internals: Connected-app execution audit logs (1 table: mcp_outbound_call_logs)
- System internals: Connected-app pending action state. Operational. (1 table: mcp_pending_actions)
- System internals: Connected-app tool execution logs. Operational observability. (1 table: mcp_execution_logs)
- System internals: Connector sync run logs. Operational observability, not relationship data. (1 table: integration_sync_logs)
- System internals: Daily ad spend/clicks per traffic source for the funnel-economics (EPC/ROAS/CAC) dashboard; marketing analytics, not logged-in relationship context for Superbrain indexing. (1 table: funnel_ad_daily)
- System internals: Daily Google Analytics 4 metrics (sessions, source/medium, conversions) synced by sync-analytics-daily. Company-level analytics, not per-user relationship data. (1 table: ga4_daily)
- System internals: Daily Mixpanel product-analytics metrics (event counts, unique users) synced by sync-analytics-daily. Company-level analytics, not per-user relationship data. (1 table: mixpanel_daily)
- System internals: Database migration metadata (1 table: schema_migrations)
- System internals: Demo walkthrough infrastructure — product demos, anonymous view/step analytics. Not relationship data. (1 table: demo_*)
- System internals: Derived routing table holding one public web address per Company. Assist already reads Audit content through company_briefs; the slug adds no relationship context and would only invite fabricated links. (1 table: value_audit_company_addresses)
- System internals: Durable LinkedIn post signal-extraction queue. User-facing signals are stored in intelligence_items and raw evidence in linkedin_posts. (1 table: linkedin_post_signal_jobs)
- System internals: Email import analysis/classification retry ledger and dead-letter visibility. User-facing intelligence is stored on emails and email_threads, not indexed from this operational failure log. (1 table: email_analysis_failures)
- System internals: Exit-overlay email opt-in log; the captured email becomes a contact via find_or_create_contact (which Superbrain already indexes). The log itself is operational. (1 table: video_optins)
- System internals: External provider id to Deal mapping that makes integration upserts idempotent; the Deal carries the customer-facing data. (1 table: deal_provider_identities)
- System internals: First 30 Days course instrumentation: dedupe ledger for stall nudges (one row per user, gate, step); delivery bookkeeping, not relationship context. (1 table: course_nudges)
- System internals: First 30 Days course instrumentation: per-week weekly-review completion rows used to count consecutive reviews for the operator_day30 graduation gate; analytics only, not relationship context. (1 table: course_weekly_reviews)
- System internals: High-volume granular playback telemetry (play/pause/seek/progress) for drop-off curves. Not relationship context. (1 table: video_watch_events)
- System internals: High-volume internal audit trail (1 table: entity_audit_log)
- System internals: Idempotent Actions evidence ledger. User-facing targets and scored results are exposed through weekly_action_plans and canonical Say/Do summaries. (1 table: action_activity_events)
- System internals: Immutable frozen Say/Do score snapshots derived from weekly_action_plans and the Actions evidence ledger; a cache that stops a finalized weekly score from drifting, not a source of relationship context. (1 table: action_say_do_snapshots)
- System internals: Immutable published calculator versions addressable by opaque link token. Public snapshots carry no answer results; they are definition+metadata snapshots only. (1 table: client_tool_public_snapshots)
- System internals: Internal activation-funnel instrumentation (time-to-value milestones for the admin Activation dashboard); analytics only, not logged-in relationship context for Superbrain indexing. (1 table: activation_milestones)
- System internals: Internal Assist pilot cohort and outcome ledger. It is operational measurement, never relationship context or AI source material. (1 table: assist_pilot_*)
- System internals: Internal automation queue (1 table: automation_event_queue)
- System internals: Internal background rescore requests raised by ICP changes and repair backfills; operational queue state, not relationship knowledge. (1 table: icp_rescore_requests)
- System internals: Internal browser identity attribution. Not AI context. (1 table: video_engagement_bindings)
- System internals: Internal calculator draft state: generation claims, definitions and status. User-facing calculated results are never persisted; this table is plumbing for the draft-ready transition. (1 table: client_tool_drafts)
- System internals: Internal dedup/audit log for hot-chat alerts (notify-hot-chat). Not relationship data. (1 table: chat_alert_log)
- System internals: Internal Jungler pilot dedupe and provider audit ledger; user-facing signals are stored in social_signals, linkedin_contact_engagements, and linkedin_social_leads. (1 table: jungler_pilot_events)
- System internals: Internal kill-switch and cap configuration for the guarded enrichment cron. Operational safety config, not relationship knowledge. (1 table: enrichment_cron_control)
- System internals: Internal latency telemetry for the post-meeting orchestrator (1 table: post_meeting_processing_metrics)
- System internals: Internal pilot playback telemetry and session capability hashes. CRM reads use scoped summary RPCs, never raw AI retrieval. (1 table: video_engagement_sessions)
- System internals: Internal pilot publishing configuration, not relationship context. (1 table: video_engagement_placements)
- System internals: Internal prospecting cache for recovered direct employers from recruiter job posts. User-facing evidence is stored on contacts and queue source metadata. (1 table: prospecting_recruiter_recoveries)
- System internals: Intro batch job state. The intros themselves live on contact activity. (1 table: intro_request_batches)
- System internals: LinkedIn drip work queue. Relationship outcomes live on contacts and linkedin_messages. (1 table: drip_connect_queue)
- System internals: Meeting-prep work queue. The user-facing artifact is meeting_prep_docs. (1 table: meeting_prep_queue)
- System internals: Notification dedup keys. Pure infrastructure. (1 table: telegram_notification_dedup)
- System internals: One-off audit snapshot from the 2026-05-19 LinkedIn message dedup cleanup migration. Backoffice recovery trail, not relationship data. (1 table: _linkedin_dedup_audit_*)
- System internals: One-off audit snapshot from the 2026-05-25 LinkedIn message misattribution cleanup migration. Backoffice recovery trail, not relationship data. (1 table: _linkedin_misattribution_audit_*)
- System internals: One-off work list for re-sorting mis-labelled email (2026-10). Holds only email ids; the result is stored on email_messages. (1 table: email_resort_queue)
- System internals: Operational completion ledger for the imported-email analysis retry worker; outcomes belong in email messages, threads, and the failure ledger. (1 table: email_analysis_retry_runs)
- System internals: Operational Firecrawl monitor state; user-visible website changes are stored in company_signals and intelligence_items. (1 table: company_website_watches)
- System internals: Operational queue/status for background Rhythm-brief research prep; the prepared research is stored on contacts.signaliz_data, not here. (1 table: rhythm_brief_prep)
- System internals: Operational run history for website monitoring; user-visible signals are stored in company_monitoring_snapshots and intelligence_items. (1 table: company_monitoring_runs)
- System internals: Per-user on/off and ordering for the sources feeding the daily Today list. UI configuration, not relationship context. (1 table: daily_source_preferences)
- System internals: PostgreSQL system tables (1 table: pg_*)
- System internals: Pre-signup Network Revenue Audit lead capture (anonymous scan results + assembled deliverable) for the free audit lead magnet; hydrated into onboarding at signup, not logged-in relationship context for Superbrain indexing. (1 table: connector_sessions)
- System internals: Precomputed Growth Agent opening turns, keyed by lead magnet and greeting branch. A latency cache over text the assistant wrote itself; holds no visitor and no relationship context. (1 table: growth_agent_greeting_cache)
- System internals: Private invitation retry deduplication keys. User-facing activity lives in action_activity_events and sprint totals. (1 table: connection_sprint_action_receipts)
- System internals: Private observation-to-invitation retry mappings; not user-facing activity. (1 table: manual_invitation_observation_receipts)
- System internals: Public /growth founding-member funnel leads and per-step analytics; not logged-in relationship context for Superbrain indexing. (1 table: growth_funnel_leads)
- System internals: Public authority lead-capture records, not logged-in relationship context. (1 table: authority_score_leads)
- System internals: Public Commercial Spine lead magnet submissions and generated reports; not logged-in relationship context for Superbrain indexing. (1 table: commercial_spine_leads)
- System internals: RAG embedding job queue (1 table: rag_embedding_jobs)
- System internals: RAG indexing queue (1 table: rag_index_queue)
- System internals: RAG internal storage (2 tables: rag_documents, rag_document_chunks)
- System internals: RAG storage, queues, caches, feedback, and ranking internals. (1 table: rag_*)
- System internals: Raw embeddings (1 table: content_embeddings)
- System internals: Request rate-limit log with non-UUID IDs (1 table: request_rate_log)
- System internals: Scan session state rows. Operational. (1 table: scan_sessions)
- System internals: Scoped resume capability hashes, not relationship context. (1 table: video_engagement_links)
- System internals: Scoring work queue state. Operational, not relationship data. (1 table: contact_scoring_queue)
- System internals: Scrape run log. Detected signals live in social_signals/linkedin_* sources. (1 table: social_scrape_log)
- System internals: Short-lived job bookkeeping for Strategy generations. The finished work lives in its own table; this row is plumbing, not relationship context. (1 table: strategy_generation_jobs)
- System internals: Skill version snapshots. The active skill content is indexed via company_skills/user skills sources. (1 table: skill_versions)
- System internals: Supabase internal tables (1 table: supabase_*)
- System internals: Superbrain configuration and status metadata (1 table: superbrain_*)
- System internals: UI flow session telemetry. Product analytics, not relationship data. (1 table: flow_sessions)
- System internals: Unmatched participant triage queue. Resolved participants become contacts. (1 table: unmatched_meeting_participants)
- System internals: Voice-of-market marketing pipeline. Backoffice, service-role only, not relationship data. (1 table: vom_*)
- System internals: Webhook debug logs (1 table: webhook_logs)
- System internals: Withdrawn LinkedIn invitations and their re-invite block, used by the connection queue. The user-facing record is the contact timeline entry written with each withdrawal. (1 table: linkedin_invitation_withdrawals)
- Tracking, telemetry and working copies of the retiring opportunities rail; the records they describe are exported. (44 tables: email_open_tracking, campaign_email_tracking, link_clicks, tracking_links, tracking_high_value_email_alerts, visitor_events, contact_identity, inferred_email_connections, deal_room_views, deal_room_view_notifications, deal_room_item_events, video_views, …)

Vector embeddings are not exported unless the export was started with embeddings switched on; they are derived from the text you already have.

## Bringing it back

The `data/*.jsonl` files and `manifest.json` are enough to rebuild the workspace: people, companies and deals through the product's own save contracts, child records with their foreign keys remapped. Nynch's own importer reads exactly this folder.

Questions: support@nynch.com


===== memory/accounts/greenfold-logistics-00000008.md =====

---
id: "00000008-0000-4000-8000-bbbbbbbbbbbb"
name: "Greenfold Logistics"
domain: "greenfoldlogistics.example.test"
industry: "Services"
account_type: "prospect"
owner: "Owner One"
market_tier: "tier_2"
employee_range: "11-50"
---
# Greenfold Logistics

## Profile

- Website
- Location · Leeds, GB
- LinkedIn
- Tags · fixture

Greenfold Logistics helps mid-market firms with operations.

## People

- Casey Stone · Finance Director · casey.stone@greenfoldlogistics.example.test
- Harper Stone · Managing Director · harper.stone@greenfoldlogistics.example.test · warmth warm
- Morgan Stone · Head of Operations · morgan.stone@greenfoldlogistics.example.test · warmth hot

## Deals

- Greenfold Logistics: growth programme · Won · won · 15000 GBP · 2026-06-30

## Account memory

Greenfold Logistics is growing its services team and reviewing suppliers this quarter.

- Market position · challenger
- Growth · Hiring an operations lead
- Entry point · Operations review

## Company signals

- 2026-05-10 · funding · Greenfold Logistics raised a growth round


===== memory/accounts/harbourlight-consulting-00000007.md =====

---
id: "00000007-0000-4000-8000-bbbbbbbbbbbb"
name: "Harbourlight Consulting"
domain: "harbourlightconsulting.example.test"
industry: "Software"
account_type: "prospect"
owner: "Owner One"
market_tier: "tier_2"
employee_range: "11-50"
---
# Harbourlight Consulting

## Profile

- Website
- Location · Leeds, GB
- LinkedIn
- Tags · fixture

Harbourlight Consulting helps mid-market firms with software delivery.

## People

- Blake Stone · Head of Operations · blake.stone@harbourlightconsulting.example.test · warmth warm
- Finley Stone · Finance Director · finley.stone@harbourlightconsulting.example.test · warmth hot
- Logan Stone · Managing Director · logan.stone@harbourlightconsulting.example.test

## Deals

- Harbourlight Consulting: operations review · Proposal · open · 13500 GBP · 2026-06-30

## Account memory

Harbourlight Consulting is growing its software team and reviewing suppliers this quarter.

- Market position · challenger
- Growth · Hiring an operations lead
- Entry point · Operations review

## Company signals

- 2026-05-10 · hiring · Harbourlight Consulting is hiring a Head of Operations


===== memory/accounts/meridian-ledger-00000009.md =====

---
id: "00000009-0000-4000-8000-bbbbbbbbbbbb"
name: "Meridian Ledger"
domain: "meridianledger.example.test"
industry: "Software"
account_type: "client"
owner: "Owner One"
market_tier: "tier_1"
employee_range: "11-50"
---
# Meridian Ledger

## Profile

- Website
- Location · Leeds, GB
- LinkedIn
- Tags · fixture

Meridian Ledger helps mid-market firms with software delivery.

## People

- Devon Stone · Managing Director · devon.stone@meridianledger.example.test
- Jordan Stone · Head of Operations · jordan.stone@meridianledger.example.test
- Parker Stone · Finance Director · parker.stone@meridianledger.example.test · warmth warm

## Account memory

Meridian Ledger is growing its software team and reviewing suppliers this quarter.

- Market position · challenger
- Growth · Hiring an operations lead
- Entry point · Operations review

## Company signals

- 2026-05-10 · hiring · Meridian Ledger is hiring a Head of Operations


===== memory/accounts/northwind-analytics-00000006.md =====

---
id: "00000006-0000-4000-8000-bbbbbbbbbbbb"
name: "Northwind Analytics"
domain: "northwindanalytics.example.test"
industry: "Services"
account_type: "client"
owner: "Owner One"
market_tier: "tier_1"
employee_range: "11-50"
---
# Northwind Analytics

## Profile

- Website
- Location · Leeds, GB
- LinkedIn
- Tags · fixture

Northwind Analytics helps mid-market firms with operations.

## People

- Avery Stone · Managing Director · avery.stone@northwindanalytics.example.test · warmth hot
- Emery Stone · Head of Operations · emery.stone@northwindanalytics.example.test
- Kendall Stone · Finance Director · kendall.stone@northwindanalytics.example.test

## Deals

- Northwind Analytics: growth programme · Discovery · open · 12000 GBP · 2026-06-30
- A legacy opportunity · opportunity rail · open · 5000 GBP

## Account memory

Northwind Analytics is growing its services team and reviewing suppliers this quarter.

- Market position · challenger
- Growth · Hiring an operations lead
- Entry point · Operations review

## Company signals

- 2026-05-10 · funding · Northwind Analytics raised a growth round

## Proof points

- Northwind Analytics cut onboarding time by 40% · Northwind Analytics cut onboarding time by 40% in one quarter.


===== memory/learnings/belief-observations.md =====

# Belief observations

1 entries from the belief_observations dataset.

## objection

- 2026-02-20 · Consultants slow us down. · "Last time a consultancy slowed us down for months." · negative


===== memory/learnings/deal-learnings.md =====

# Deal learnings

1 entries from the deal_learnings dataset.

## win

- Deals with an operations review in discovery close twice as often. · status validated · 2026-05-01


===== memory/relationships/avery-stone-0000000a.md =====

---
id: "0000000a-0000-4000-8000-aaaaaaaaaaaa"
name: "Avery Stone"
email: "avery.stone@northwindanalytics.example.test"
title: "Managing Director"
company: "Northwind Analytics"
owner: "Owner One"
warmth_tier: "hot"
lifecycle_stage: 
lead_status: "customer"
last_contacted_at: "2026-01-10T09:00:00Z"
---
# Avery Stone

## Profile

- Headline · Managing Director at Northwind Analytics
- Location · Leeds, UK
- Industry · Services
- LinkedIn · https://linkedin.example.test/in/fixture-0
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 20

## Companies

- Northwind Analytics

## Deals

- Northwind Analytics: growth programme · Discovery · open · 12000 GBP · 2026-06-30
- A legacy opportunity · opportunity rail · open · 5000 GBP

## Relationship memory

Avery Stone is the Managing Director at Northwind Analytics and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Known facts

- Avery Stone (person) works at Northwind Analytics (company) · strength 0.9 · LinkedIn profile · 2026-05-01
- Blake Stone (person) referred by Avery Stone (person) · strength 0.7 · Intro email · 2026-05-01

## Commitments

- [x] · Send Avery Stone the case study · us promised · 2026-04-15

## Notes

- 2026-03-05 · Avery Stone prefers a short weekly check-in and dislikes long decks.

## Recent interactions

- 2026-04-02 · email · inbound · Re: proposal for Northwind Analytics · Wants to discuss the timeline on Tuesday.
- 2026-03-03 · email · outbound · Catch-up 3 with Avery Stone · Discussed next steps on the proposal with Avery Stone.
- 2026-02-20 · meeting · Discovery with Avery Stone · Agreed to start with an operations review in March.
- 2026-02-20 · meeting · Discovery with Avery Stone · Agreed to start with an operations review in March.
- 2026-02-02 · email · inbound · Catch-up 2 with Avery Stone · Discussed next steps on the proposal with Avery Stone.
- 2026-01-01 · meeting · outbound · Catch-up 1 with Avery Stone · Discussed the growth programme with Avery Stone.

## Signals

- 2026-05-01 · job_change · Head of Operations → Managing Director


===== memory/relationships/blake-stone-0000000b.md =====

---
id: "0000000b-0000-4000-8000-aaaaaaaaaaaa"
name: "Blake Stone"
email: "blake.stone@harbourlightconsulting.example.test"
title: "Head of Operations"
company: "Harbourlight Consulting"
owner: "Shared Member"
warmth_tier: "warm"
lifecycle_stage: 
lead_status: "qualified"
last_contacted_at: "2026-02-11T09:00:00Z"
---
# Blake Stone

## Profile

- Headline · Operations lead at Harbourlight Consulting
- Location · Leeds, UK
- Industry · Software
- LinkedIn · https://linkedin.example.test/in/fixture-1
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 21

## Companies

- Harbourlight Consulting

## Deals

- Harbourlight Consulting: operations review · Proposal · open · 13500 GBP · 2026-06-30

## Relationship memory

Blake Stone is the Head of Operations at Harbourlight Consulting and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Known facts

- Blake Stone (person) referred by Avery Stone (person) · strength 0.7 · Intro email · 2026-05-01

## Recent interactions

- 2026-04-03 · email · outbound · Catch-up 3 with Blake Stone · Discussed next steps on the proposal with Blake Stone.
- 2026-04-02 · email · inbound · Re: proposal for Harbourlight Consulting · Wants to discuss the timeline on Tuesday.
- 2026-03-02 · email · inbound · Catch-up 2 with Blake Stone · Discussed next steps on the proposal with Blake Stone.
- 2026-02-01 · meeting · outbound · Catch-up 1 with Blake Stone · Discussed the growth programme with Blake Stone.


===== memory/relationships/casey-stone-0000000c.md =====

---
id: "0000000c-0000-4000-8000-aaaaaaaaaaaa"
name: "Casey Stone"
email: "casey.stone@greenfoldlogistics.example.test"
title: "Finance Director"
company: "Greenfold Logistics"
owner: "Owner One"
warmth_tier: 
lifecycle_stage: 
lead_status: "new"
last_contacted_at: "2026-03-12T09:00:00Z"
---
# Casey Stone

## Profile

- Headline · Operations lead at Greenfold Logistics
- Location · Leeds, UK
- Industry · Services
- LinkedIn · https://linkedin.example.test/in/fixture-2
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 22

## Companies

- Greenfold Logistics

## Deals

- Greenfold Logistics: growth programme · Won · won · 15000 GBP · 2026-06-30

## Relationship memory

Casey Stone is the Finance Director at Greenfold Logistics and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Notes

- 2026-03-05 · Casey Stone prefers a short weekly check-in and dislikes long decks.

## Recent interactions

- 2026-05-03 · email · outbound · Catch-up 3 with Casey Stone · Discussed next steps on the proposal with Casey Stone.
- 2026-04-02 · email · inbound · Catch-up 2 with Casey Stone · Discussed next steps on the proposal with Casey Stone.
- 2026-04-02 · email · inbound · Re: proposal for Greenfold Logistics · Wants to discuss the timeline on Tuesday.
- 2026-03-01 · meeting · outbound · Catch-up 1 with Casey Stone · Discussed the growth programme with Casey Stone.


===== memory/relationships/devon-stone-0000000d.md =====

---
id: "0000000d-0000-4000-8000-aaaaaaaaaaaa"
name: "Devon Stone"
email: "devon.stone@meridianledger.example.test"
title: "Managing Director"
company: "Meridian Ledger"
owner: "Shared Member"
warmth_tier: 
lifecycle_stage: 
lead_status: "new"
last_contacted_at: "2026-04-13T09:00:00Z"
---
# Devon Stone

## Profile

- Headline · Managing Director at Meridian Ledger
- Location · Leeds, UK
- Industry · Software
- LinkedIn · https://linkedin.example.test/in/fixture-3
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 23

## Companies

- Meridian Ledger

## Relationship memory

Devon Stone is the Managing Director at Meridian Ledger and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Commitments

- [ ] · Send Devon Stone the case study · us promised · 2026-04-15

## Recent interactions

- 2026-06-03 · email · outbound · Catch-up 3 with Devon Stone · Discussed next steps on the proposal with Devon Stone.
- 2026-05-02 · email · inbound · Catch-up 2 with Devon Stone · Discussed next steps on the proposal with Devon Stone.
- 2026-04-02 · email · inbound · Re: proposal for Meridian Ledger · Wants to discuss the timeline on Tuesday.
- 2026-04-01 · meeting · outbound · Catch-up 1 with Devon Stone · Discussed the growth programme with Devon Stone.


===== memory/relationships/emery-stone-0000000e.md =====

---
id: "0000000e-0000-4000-8000-aaaaaaaaaaaa"
name: "Emery Stone"
email: "emery.stone@northwindanalytics.example.test"
title: "Head of Operations"
company: "Northwind Analytics"
owner: "Owner One"
warmth_tier: 
lifecycle_stage: 
lead_status: "new"
last_contacted_at: "2026-05-14T09:00:00Z"
---
# Emery Stone

## Profile

- Headline · Operations lead at Northwind Analytics
- Location · Leeds, UK
- Industry · Services
- LinkedIn · https://linkedin.example.test/in/fixture-4
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 24

## Companies

- Northwind Analytics

## Relationship memory

Emery Stone is the Head of Operations at Northwind Analytics and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Notes

- 2026-03-05 · Emery Stone prefers a short weekly check-in and dislikes long decks.

## Recent interactions

- 2026-07-03 · email · outbound · Catch-up 3 with Emery Stone · Discussed next steps on the proposal with Emery Stone.
- 2026-06-02 · email · inbound · Catch-up 2 with Emery Stone · Discussed next steps on the proposal with Emery Stone.
- 2026-05-01 · meeting · outbound · Catch-up 1 with Emery Stone · Discussed the growth programme with Emery Stone.
- 2026-04-02 · email · inbound · Re: proposal for Northwind Analytics · Wants to discuss the timeline on Tuesday.

## Signals

- 2026-05-01 · job_change · Head of Operations → Head of Operations


===== memory/relationships/finley-stone-0000000f.md =====

---
id: "0000000f-0000-4000-8000-aaaaaaaaaaaa"
name: "Finley Stone"
email: "finley.stone@harbourlightconsulting.example.test"
title: "Finance Director"
company: "Harbourlight Consulting"
owner: "Shared Member"
warmth_tier: "hot"
lifecycle_stage: 
lead_status: "customer"
last_contacted_at: "2026-06-15T09:00:00Z"
---
# Finley Stone

## Profile

- Headline · Operations lead at Harbourlight Consulting
- Location · Leeds, UK
- Industry · Software
- LinkedIn · https://linkedin.example.test/in/fixture-5
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 25

## Companies

- Harbourlight Consulting

## Relationship memory

Finley Stone is the Finance Director at Harbourlight Consulting and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Recent interactions

- 2026-08-03 · email · outbound · Catch-up 3 with Finley Stone · Discussed next steps on the proposal with Finley Stone.
- 2026-07-02 · email · inbound · Catch-up 2 with Finley Stone · Discussed next steps on the proposal with Finley Stone.
- 2026-06-01 · meeting · outbound · Catch-up 1 with Finley Stone · Discussed the growth programme with Finley Stone.
- 2026-04-02 · email · inbound · Re: proposal for Harbourlight Consulting · Wants to discuss the timeline on Tuesday.


===== memory/relationships/harper-stone-00000010.md =====

---
id: "00000010-0000-4000-8000-aaaaaaaaaaaa"
name: "Harper Stone"
email: "harper.stone@greenfoldlogistics.example.test"
title: "Managing Director"
company: "Greenfold Logistics"
owner: "Owner One"
warmth_tier: "warm"
lifecycle_stage: 
lead_status: "qualified"
last_contacted_at: "2026-07-16T09:00:00Z"
---
# Harper Stone

## Profile

- Headline · Managing Director at Greenfold Logistics
- Location · Leeds, UK
- Industry · Services
- LinkedIn · https://linkedin.example.test/in/fixture-6
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 26

## Companies

- Greenfold Logistics

## Relationship memory

Harper Stone is the Managing Director at Greenfold Logistics and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Commitments

- [x] · Send Harper Stone the case study · us promised · 2026-04-15

## Notes

- 2026-03-05 · Harper Stone prefers a short weekly check-in and dislikes long decks.

## Recent interactions

- 2026-09-03 · email · outbound · Catch-up 3 with Harper Stone · Discussed next steps on the proposal with Harper Stone.
- 2026-08-02 · email · inbound · Catch-up 2 with Harper Stone · Discussed next steps on the proposal with Harper Stone.
- 2026-07-01 · meeting · outbound · Catch-up 1 with Harper Stone · Discussed the growth programme with Harper Stone.
- 2026-04-02 · email · inbound · Re: proposal for Greenfold Logistics · Wants to discuss the timeline on Tuesday.


===== memory/relationships/jordan-stone-00000011.md =====

---
id: "00000011-0000-4000-8000-aaaaaaaaaaaa"
name: "Jordan Stone"
email: "jordan.stone@meridianledger.example.test"
title: "Head of Operations"
company: "Meridian Ledger"
owner: "Shared Member"
warmth_tier: 
lifecycle_stage: 
lead_status: "new"
last_contacted_at: "2026-08-17T09:00:00Z"
---
# Jordan Stone

## Profile

- Headline · Operations lead at Meridian Ledger
- Location · Leeds, UK
- Industry · Software
- LinkedIn · https://linkedin.example.test/in/fixture-7
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 27

## Companies

- Meridian Ledger

## Relationship memory

Jordan Stone is the Head of Operations at Meridian Ledger and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Recent interactions

- 2026-09-02 · email · inbound · Catch-up 2 with Jordan Stone · Discussed next steps on the proposal with Jordan Stone.
- 2026-08-01 · meeting · outbound · Catch-up 1 with Jordan Stone · Discussed the growth programme with Jordan Stone.
- 2026-04-02 · email · inbound · Re: proposal for Meridian Ledger · Wants to discuss the timeline on Tuesday.
- 2026-01-03 · email · outbound · Catch-up 3 with Jordan Stone · Discussed next steps on the proposal with Jordan Stone.


===== memory/relationships/kendall-stone-00000012.md =====

---
id: "00000012-0000-4000-8000-aaaaaaaaaaaa"
name: "Kendall Stone"
email: "kendall.stone@northwindanalytics.example.test"
title: "Finance Director"
company: "Northwind Analytics"
owner: "Owner One"
warmth_tier: 
lifecycle_stage: 
lead_status: "new"
last_contacted_at: "2026-09-18T09:00:00Z"
---
# Kendall Stone

## Profile

- Headline · Operations lead at Northwind Analytics
- Location · Leeds, UK
- Industry · Services
- LinkedIn · https://linkedin.example.test/in/fixture-8
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 28

## Companies

- Northwind Analytics

## Relationship memory

Kendall Stone is the Finance Director at Northwind Analytics and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Notes

- 2026-03-05 · Kendall Stone prefers a short weekly check-in and dislikes long decks.

## Recent interactions

- 2026-09-01 · meeting · outbound · Catch-up 1 with Kendall Stone · Discussed the growth programme with Kendall Stone.
- 2026-04-02 · email · inbound · Re: proposal for Northwind Analytics · Wants to discuss the timeline on Tuesday.
- 2026-02-03 · email · outbound · Catch-up 3 with Kendall Stone · Discussed next steps on the proposal with Kendall Stone.
- 2026-01-02 · email · inbound · Catch-up 2 with Kendall Stone · Discussed next steps on the proposal with Kendall Stone.

## Signals

- 2026-05-01 · job_change · Head of Operations → Finance Director


===== memory/relationships/logan-stone-00000013.md =====

---
id: "00000013-0000-4000-8000-aaaaaaaaaaaa"
name: "Logan Stone"
email: "logan.stone@harbourlightconsulting.example.test"
title: "Managing Director"
company: "Harbourlight Consulting"
owner: "Shared Member"
warmth_tier: 
lifecycle_stage: 
lead_status: "new"
last_contacted_at: "2026-01-10T09:00:00Z"
---
# Logan Stone

## Profile

- Headline · Managing Director at Harbourlight Consulting
- Location · Leeds, UK
- Industry · Software
- LinkedIn · https://linkedin.example.test/in/fixture-9
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 29

## Companies

- Harbourlight Consulting

## Relationship memory

Logan Stone is the Managing Director at Harbourlight Consulting and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Commitments

- [ ] · Send Logan Stone the case study · us promised · 2026-04-15

## Recent interactions

- 2026-04-02 · email · inbound · Re: proposal for Harbourlight Consulting · Wants to discuss the timeline on Tuesday.
- 2026-03-03 · email · outbound · Catch-up 3 with Logan Stone · Discussed next steps on the proposal with Logan Stone.
- 2026-02-02 · email · inbound · Catch-up 2 with Logan Stone · Discussed next steps on the proposal with Logan Stone.
- 2026-01-01 · meeting · outbound · Catch-up 1 with Logan Stone · Discussed the growth programme with Logan Stone.


===== memory/relationships/morgan-stone-00000014.md =====

---
id: "00000014-0000-4000-8000-aaaaaaaaaaaa"
name: "Morgan Stone"
email: "morgan.stone@greenfoldlogistics.example.test"
title: "Head of Operations"
company: "Greenfold Logistics"
owner: "Owner One"
warmth_tier: "hot"
lifecycle_stage: 
lead_status: "customer"
last_contacted_at: "2026-02-11T09:00:00Z"
---
# Morgan Stone

## Profile

- Headline · Operations lead at Greenfold Logistics
- Location · Leeds, UK
- Industry · Services
- LinkedIn · https://linkedin.example.test/in/fixture-10
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 30

## Companies

- Greenfold Logistics

## Relationship memory

Morgan Stone is the Head of Operations at Greenfold Logistics and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Notes

- 2026-03-05 · Morgan Stone prefers a short weekly check-in and dislikes long decks.

## Recent interactions

- 2026-04-03 · email · outbound · Catch-up 3 with Morgan Stone · Discussed next steps on the proposal with Morgan Stone.
- 2026-04-02 · email · inbound · Re: proposal for Greenfold Logistics · Wants to discuss the timeline on Tuesday.
- 2026-03-02 · email · inbound · Catch-up 2 with Morgan Stone · Discussed next steps on the proposal with Morgan Stone.
- 2026-02-01 · meeting · outbound · Catch-up 1 with Morgan Stone · Discussed the growth programme with Morgan Stone.


===== memory/relationships/parker-stone-00000015.md =====

---
id: "00000015-0000-4000-8000-aaaaaaaaaaaa"
name: "Parker Stone"
email: "parker.stone@meridianledger.example.test"
title: "Finance Director"
company: "Meridian Ledger"
owner: "Shared Member"
warmth_tier: "warm"
lifecycle_stage: 
lead_status: "qualified"
last_contacted_at: "2026-03-12T09:00:00Z"
---
# Parker Stone

## Profile

- Headline · Operations lead at Meridian Ledger
- Location · Leeds, UK
- Industry · Software
- LinkedIn · https://linkedin.example.test/in/fixture-11
- Phone
- Source · fixture · referral
- Tags · fixture
- Connection strength · 31

## Companies

- Meridian Ledger

## Relationship memory

Parker Stone is the Finance Director at Meridian Ledger and cares most about predictable delivery.

**Our history.** Met at a Leeds breakfast event; three conversations since.

- Fact · Budget cycle resets in April
- Do not assume · Do not assume the CFO is the decision maker.
- Recommended approach · Lead with the operations review outcome.
- Best channel · email

## Recent interactions

- 2026-05-03 · email · outbound · Catch-up 3 with Parker Stone · Discussed next steps on the proposal with Parker Stone.
- 2026-04-02 · email · inbound · Catch-up 2 with Parker Stone · Discussed next steps on the proposal with Parker Stone.
- 2026-04-02 · email · inbound · Re: proposal for Meridian Ledger · Wants to discuss the timeline on Tuesday.
- 2026-03-01 · meeting · outbound · Catch-up 1 with Parker Stone · Discussed the growth programme with Parker Stone.


===== proposals/0000007c-0000-4000-8000-ffffffffffff.md =====

---
id: "0000007c-0000-4000-8000-ffffffffffff"
deal_id: "00000016-0000-4000-8000-eeeeeeeeeeee"
contact_id: "0000000a-0000-4000-8000-aaaaaaaaaaaa"
status: "sent"
version: "1"
currency: "GBP"
sent_at: "2026-03-01T00:00:00Z"
accepted_at: 
---
# Proposal 0000007c

## Options

- Review only · 6000 GBP · Two-week review.
- Review and programme · 24000 GBP · recommended · Review plus 90 days.

## Content

### executive summary

A two-week operations review for Northwind Analytics, then a 90-day programme.

### scope

```json
[
  "Operations review",
  "Weekly check-ins"
]
```



===== transcripts/0000007b-0000-4000-8000-ffffffffffff.md =====

---
id: "0000007b-0000-4000-8000-ffffffffffff"
title: "Discovery with Avery Stone"
date: "2026-02-20T10:00:00Z"
duration_minutes: "45"
contact_id: "0000000a-0000-4000-8000-aaaaaaaaaaaa"
deal_id: "00000016-0000-4000-8000-eeeeeeeeeeee"
source: "fathom"
---
# Discovery with Avery Stone

## Participants

- Avery Stone

## Summary

Agreed to start with an operations review in March.

## Action items

- Send the review scope

## Commitments

- Share the case study

## Transcript

[00:00] Avery Stone: Thanks for coming in. Last time a consultancy slowed us down for months.
[00:20] Us: Understood. We start with a two-week operations review so you see value before anything else.

