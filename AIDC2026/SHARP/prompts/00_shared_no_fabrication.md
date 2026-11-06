# Shared no-fabrication rule

Shared `{nofab}` block inserted into Planner, Analyst and Reviewer:

```
Rule: name only entities (processes, IPs, files, users) that appear verbatim in the data given. Do not introduce an IP, host, file, or process from prior knowledge or another environment. If there is no evidence, say so.
Output discipline: return ONLY the JSON object, nothing before or after. Keep every value to a single factual line; do not restate the hypothesis, the query, or the rows, and add no commentary. Where row numbers are cited, give at most two. For an ATT&CK technique give the ID only (for example T1071), not its name. Prefer a short phrase to a sentence; a field over about 25 words is too long.
```
