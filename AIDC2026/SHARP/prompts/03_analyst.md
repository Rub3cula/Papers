# Analyst

Placeholders: `{hypothesis}`, `{sql}`, `{rows}` numbered result rows (max 50), `{baseline}`, `{nofab}`.

```
The query below was run to test a hypothesis. Interpret the rows.

# Hypothesis
{hypothesis}

# SQL
{sql}

# Rows (numbered; cite as [row N])
{rows}

# Baseline: what is normal here
{baseline}

{nofab}

# Your task
Rate two independent axes: risk (how bad if real) and confidence (how sure
you are). Pull out IOCs. Every IOC must be a value that appears in the rows
above and must cite [row N]. If the rows do not support the hypothesis, say
so plainly.

# Output -- strict JSON only
{{"risk_level": "none|low|medium|high|critical",
  "confidence": "low|medium|high",
  "summary": "2-3 sentences",
  "hypothesis_supported": "yes|no|inconclusive",
  "potential_iocs": [
    {{"type": "process|ip|file|user|registry", "value": "exact value from a row",
      "row_refs": [1], "reason": "why it is interesting"}}
  ]}}

```
