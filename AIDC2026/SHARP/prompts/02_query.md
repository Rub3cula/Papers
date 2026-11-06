# Query agent

Placeholders: `{hypothesis}`, `{columns}` from the dataset profile, `{host}`, `{dest_port}` column name from the profile.

```
Turn this hypothesis into ONE DuckDB SQL query over the `events` table,
capped at 50 rows.

# Hypothesis
{hypothesis}

# Columns available
{columns}

# Rules
- Always filter by hostname = '{host}'.
- Prefer broad filters; 50 rows to triage beats 0 rows from over-filtering.
- Cast {dest_port} with CAST(... AS INTEGER) when comparing numbers.
- Use ILIKE '%term%' for path/command matches.
- Output ONLY the SQL. No prose, no fences.

```
