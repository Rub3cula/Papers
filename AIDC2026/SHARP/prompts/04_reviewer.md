# Reviewer

Placeholders: `{observations}`, `{baseline}`, `{ioc_table}` deterministic baseline-related tags, `{nofab}`. The tactic list is the public 14-tactic ATT&CK space.

```
You are writing the final verdict for an investigation of one host.

# All observations
{observations}

# Baseline: what is normal here
{baseline}

# IOC annotations (deterministic; authoritative)
Each surfaced IOC is tagged baseline_related true/false by a substring check
against the baseline. baseline_related=true means the entity is normal here.
{ioc_table}

{nofab}

# How to decide
- This score measures confidence that the host is COMPROMISED.
- Inconclusive (0.40-0.59) is a first-class, correct verdict when the data is
  weird but not clearly malicious. Use it freely.
- If every IOC you would rely on is baseline_related=true, you cannot exceed
  0.79; the baseline already explains those entities.
- Reaching 0.80+ requires at least one concrete in-data artefact that is NOT
  baseline_related and has no benign explanation.
- If the queries mostly returned no rows, the right verdict is inconclusive at
  low confidence (pipeline could not gather evidence), not clean.

# For each validated finding, also record intent
You have the full context, so name the intent here rather than leaving it to a
later pass with only the one-line finding.
- intent: one plain line for what the attacker gets the target (a person or a
  system) to do. Describe the goal in behaviour terms; no IPs, hosts, or hashes.
- tactic: the single best ATT&CK tactic the finding serves (the goal). Pick from
  the list below. This is general methodology, not a claim about what is present.
  Tactics: Reconnaissance, Resource Development, Initial Access, Execution, Persistence, Privilege Escalation, Defense Evasion, Credential Access, Discovery, Lateral Movement, Collection, Command and Control, Exfiltration, Impact

# Output -- strict JSON only
{{"conclusion": "compromised|inconclusive|clean",
  "confidence": 0.00,
  "verdict_rationale": "at most two sentences; chain observations to verdict; only in-data entities",
  "validated_findings": [
    {{"finding": "specific", "intent": "one line: what the attacker induces",
      "tactic": "one ATT&CK tactic from the list", "evidence": "row/observation refs",
      "risk": "low|medium|high|critical"}}
  ],
  "should_conclude": true}}

```
