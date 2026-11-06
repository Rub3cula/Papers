# Introducing SHARP: Autonomous, Host-Agnostic Behavioural Detection from Raw Endpoint Telemetry

Companion material for the paper of the same name, accepted at AIDC 2026
(the AI for Cyber Defence workshop, co-located with ACSAC 2026).
This repository holds the agent prompts, the behavioural rules the system
generated, and the per-rule and per-run results behind the paper's tables.
It does not include the pipeline code or the datasets.

## Contents

```
prompts/
  00_shared_no_fabrication.md   rule block shared by Planner, Analyst and Reviewer
  01_planner.md                 hypothesis generation
  02_query.md                   one SQL query per hypothesis
  03_analyst.md                 row interpretation, risk and confidence
  04_reviewer.md                final verdict, intent and tactic per finding
  05_writer.md                  behavioural rule generation (predicate + Sigma)
rules/
  optc_behaviour_rules.json     the 28 gate-passing OpTC rules with their test results
results/
  optc_rules_four_column.csv    one row per rule: self, rename, rename+port,
                                second victim, held-out host-days
  runs_summary.csv              one row per investigation: model, scenario,
                                verdict, confidence, rules proposed and passed
```

## How the prompts are used

The pipeline runs a deterministic setup first (provenance graph, host
orientation, benign baseline) and then four agents in a loop. The Planner
proposes hypotheses, the Query agent writes one DuckDB query each, the
Analyst reads the rows, and the Reviewer issues a verdict. A deterministic
guard removes any indicator whose value does not appear in the host's data.
The Writer turns each validated finding into a behavioural predicate and a
Sigma rule. A deterministic gate then accepts or rejects it. The gate is
described in Section 4 of the paper and is not an LLM.

Every prompt placeholder (`{orientation}`, `{baseline}`, `{rows}`, and so on)
is filled from the data at run time. The prompts contain no dataset facts.

## How to read the rules file

Each entry in `rules/optc_behaviour_rules.json` has the generating model, the
tactic and technique the Reviewer assigned, the one-line behaviour, the SQL
predicate over the `events` table, the Sigma rule, and a `test` block:

| field | meaning |
|---|---|
| `self_events` | events matched on the attacked host |
| `survives_rename` | still matches after the attacker renames files and keys |
| `survives_rename_and_port` | still matches after the port is changed as well |
| `second_victim_events` | events matched on a second attacked host never seen |
| `gate_benign_hosts_fired` | benign hosts matched on the attack day (always 0, the gate requires it) |
| `held_out_host_days_fired` | benign host-days matched on held-out recordings |
| `silent_on_held_out` | true if the rule never fires on held-out hosts |

Two caveats on the fields. The `behaviour` line is the finding as the
runner stored it, cut at 100 characters. The `sigma` text is the model's own
output and was not validated or executed. Several entries are not valid Sigma
syntax. Every test count comes from the SQL `predicate`, which is the artefact
the paper evaluates.

Rules 2 and 4 embed an ephemeral port and rule 18 embeds the
victim's user name. The paper discusses these as known flaws that a human
analyst would remove.

## Datasets

DARPA OpTC (Windows) and ProvSec (Linux). Both are public. Rows in
`runs_summary.csv` with a `provsec_` scenario are the ProvSec runs.

## Models

`claude-haiku-4-5-20251001`, `claude-sonnet-5`, `gemma4:12b`, `gemma4:e4b`,
`phi4:latest`. The local models ran through Ollama.

## Citation

To be added when the proceedings appear.

## Licence

To be added.
