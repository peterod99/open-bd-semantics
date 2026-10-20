# Open BD semantics

What Nynch means by its business-development words, published openly so that any tool, any vendor and any customer can read the same definitions. Plus the exit manifest: what a Nynch customer takes with them if they ever leave.

> Ask any vendor holding your company's context what you'd get to take with you if you ever leave. Here's our answer.

Definitions version **2026-10-20**. Licensed under [Apache-2.0](LICENSE).

## What is here

| File | What it is |
|---|---|
| [`nynch-bd.ossie.yaml`](nynch-bd.ossie.yaml) | The definitions, in Apache Ossie format (spec draft `0.2.0.dev0`). This is the canonical file. |
| [`nynch-bd.ossie-0.1.1.yaml`](nynch-bd.ossie-0.1.1.yaml) | The same definitions in the Ossie `0.1.1` release shape, for tools that read the released spec. |
| [`GLOSSARY.md`](GLOSSARY.md) | Every defined term in plain English with the rule that decides it. Generated from the YAML. |
| [`exit-manifest.json`](exit-manifest.json) | What a full-workspace export contains, what is left out and why. |
| [`EXIT-MANIFEST.md`](EXIT-MANIFEST.md) | The "Take it with you" promise and what every field in the manifest means. |
| [`sample-export/`](sample-export/) | A complete export of a generated demo workspace. No real person or company is in it. |

## What Ossie is

[Apache Ossie](https://github.com/apache/ossie) (incubating at the Apache Software Foundation, formerly Open Semantic Interchange) is a vendor-neutral YAML format for semantic metadata: which datasets exist, what each field means, how datasets join, and how metrics are computed, with expressions in SQL. A tool that reads Ossie can understand a model it did not write, and an AI assistant can be handed the file as context.

## What these definitions cover

The model `nynch_business_development` describes 5 datasets: `people`, `accounts`, `opportunities`, `signals`, `lifecycle_stage_map`, 7 joins between them and 4 metrics. The terms that need a rule, rather than a stored value, are:

| Term | Meaning |
|---|---|
| `people.lifecycle_stage` | Where the person is in the relationship, as one of seven canonical stages: lead (a person we want a relationship with and have not yet qualified), qualified (a fit for what we sell, with a reason to talk), opportunity (an active commercial conversation, usually with a proposal), client (buying from us now or in the past), partner (a referrer, advocate or collaborator rather than a buyer), lost (a conversation that ended without a sale) and dormant (a relationship deliberately parked). |
| `people.relationship_warmth_tier` | How warm the relationship is, computed every fifteen minutes from connection_strength and the most recent two-way touch (the older of the last inbound and last outbound message, per channel, taking the most recent channel). |
| `opportunities.status` | Whether the opportunity is open, won, lost, no_decision or pending, derived from outcome alone. |
| `opportunities.is_qualified` | True when all four SPEC qualification elements are confirmed on the opportunity: Strategic Commitment (recorded as challenges), Problem Resonance (authority), Economic Fit (money) and Committee Gravity (prioritization). |

Two principles run through the file. Every word has one meaning: deals and opportunities are the same thing, and a status is never inferred from the name of a stage. And raw values are never destroyed: the label someone typed is kept beside the canonical meaning derived from it. [`GLOSSARY.md`](GLOSSARY.md) has every term.

Nynch runs the same rules inside its product (database triggers, scheduled jobs and the write contracts) and its test suite checks this file against them, so a definition cannot drift from the rule it describes. File paths mentioned in YAML comments refer to Nynch's product repository, which is private.

## Validate it yourself

You need Python 3.11 or later and Git. The Apache project's own validator checks the schema, unique names, references and relationship arity.

```bash
git clone https://github.com/apache/ossie.git
git -C ossie checkout b42ce0924eda8d03d9fd6b5ffdaa852343ad7ecc
python -m pip install pyyaml jsonschema
python -P ossie/validation/validate.py --schema ossie/core-spec/ossie-schema.json nynch-bd.ossie.yaml
```

The released `0.1.1` variant validates against the `0.1.1` schema:

```bash
git -C ossie show faf581054dcf7964d5fe0ceae7d6f415c8ce32a5:core-spec/osi-schema.json > osi-schema-0.1.1.json
python -P ossie/validation/validate.py --schema osi-schema-0.1.1.json nynch-bd.ossie-0.1.1.yaml
```

The draft spec changes, so the validator commit is pinned; `b42ce0924eda` is the commit these files were last checked against. Install `sqlglot` as well and the validator also parses every SQL expression.

## How to use them

- **A customer leaving Nynch.** Your export folder carries this YAML under `semantics/`. It says what every column means, so the data stays readable in a spreadsheet, a warehouse or another CRM. `definitions_version` in the file tells you which rules your export was written under.
- **A vendor or integrator.** Load the YAML into any Ossie-aware tool, or map your own fields to these terms. The 0.1.1 variant is the one to use if your tool reads the released spec only. Nynch-specific detail sits in `custom_extensions` entries marked `NYNCH` (in the 0.1.1 variant, `COMMON` entries whose data carries `"vendor": "NYNCH"`), so a tool that does not know Nynch can ignore it.
- **An AI assistant.** Give it the YAML (or the glossary) beside an export. The `ai_context` blocks tell it which field to use for which question, for example `lifecycle_stage` rather than the raw `lead_status`, and that a missing warmth tier means no two-way contact, not a cold relationship.
- **Anyone defining their own pipeline.** Copy the rules. They are Apache-2.0.

## The exit manifest and the sample export

[`exit-manifest.json`](exit-manifest.json) lists the 74 datasets an export contains and the 662 tables it leaves out, each with a reason. [`EXIT-MANIFEST.md`](EXIT-MANIFEST.md) explains every field. [`sample-export/`](sample-export/) is what an export looks like, produced by the real export engine from a generated workspace: invented names, `example.test` addresses and no customer data. A real export also holds transcripts, emails and messages that contain the words and personal data of people outside the customer's firm; the firm owns those records and is their data controller.

## Versioning

Two version numbers matter.

- **The Ossie spec version** is the `version` key. The canonical file tracks the unreleased `0.2.0.dev0` draft, pinned to the Apache commit above; when that draft changes, the file is re-checked against the new commit and fixed in the same change. `nynch-bd.ossie-0.1.1.yaml` is generated from it for the `0.1.1` release: the model moves under a `semantic_model` array, `datatype` is dropped (0.1.1 has none) and `NYNCH` extensions become `COMMON` extensions. When Ossie releases 0.2.0 the canonical file moves to it and the 0.1.1 variant is retired.
- **The definitions version** is `definitions_version` in the file's `NYNCH` extension (today `2026-10-20`). It is a date, bumped whenever a rule changes meaning, not when wording improves.

This repository is published from Nynch's product repository, where a test fails if this copy falls behind the definitions the product runs on. Changes are made there and published here; questions and suggestions are welcome as issues.

## Licence

Apache-2.0; see [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE). Apache Ossie is a project of the Apache Software Foundation; Nynch is not affiliated with it.
