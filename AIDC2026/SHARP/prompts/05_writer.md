# Writer (behavioural rule generation)

Placeholders: `{vocab}` object/action values present in the dataset, `{behaviour}` one validated finding, `{tactic}` its tactic label, `{feedback}` gate rejection reason on attempts 2 and 3 (empty on attempt 1). The deterministic gate that follows this prompt is described in Section 4 of the paper.

```
You write a behavioural detection for one observed attacker behaviour. Output a
host-agnostic boolean predicate over the `events` table: the WHERE condition
that matches the behaviour pattern, using generic terms and not a specific IP,
file, or hostname, so it transfers to other hosts. Write only the condition, not
SELECT or FROM or WHERE. Also output an equivalent Sigma rule.

Columns: object (PROCESS|FILE|FLOW|REGISTRY), action, prop_image_path,
prop_parent_image_path, prop_command_line, prop_dest_ip, prop_dest_port,
prop_key (file/registry path).

Note: the same behaviour can be recorded as a file path (prop_key) OR as a
command line (prop_command_line, e.g. 'cat /etc/passwd'). When in doubt, check
both with OR.

# Examples of GOOD predicates
1) a service spawns a shell:
   object='PROCESS'
   AND (prop_image_path ILIKE '%/sh' OR prop_image_path='sh' OR prop_image_path ILIKE '%bash%'
        OR prop_image_path ILIKE '%cmd.exe%' OR prop_image_path ILIKE '%powershell%')
   AND (prop_parent_image_path ILIKE '%httpd%' OR prop_parent_image_path ILIKE '%nginx%'
        OR prop_parent_image_path ILIKE '%redis%' OR prop_parent_image_path ILIKE '%java%'
        OR prop_parent_image_path ILIKE '%tomcat%' OR prop_parent_image_path ILIKE '%gs')
2) access of a sensitive credential file:
   prop_key ILIKE '%/etc/passwd' OR prop_key ILIKE '%/etc/shadow'
   OR prop_key ILIKE '%lsass%' OR prop_key ILIKE '%SAM%'
   OR prop_command_line ILIKE '%/etc/passwd%' OR prop_command_line ILIKE '%/etc/shadow%'
3) execution from a temporary or unusual directory:
   object='PROCESS' AND (prop_image_path ILIKE '%/tmp/%' OR prop_image_path ILIKE '%\\\\Temp\\\\%'
        OR prop_command_line ILIKE '%/tmp/%')
4) recon utility execution:
   object='PROCESS' AND (prop_image_path ILIKE '%whoami%' OR prop_image_path='id' OR prop_image_path='cat'
        OR prop_image_path ILIKE '%arp%' OR prop_image_path ILIKE '%/uname' OR prop_image_path ILIKE '%hostname%'
        OR prop_command_line ILIKE '%whoami%')

# Event vocabulary in this dataset (real object/action values; use only these)
{vocab}
prop_dest_port is VARCHAR: compare with CAST(prop_dest_port AS INTEGER).

# The behaviour to detect
{behaviour}
(intent / tactic: {tactic})

{feedback}

# Output -- strict JSON only
{{"detection_predicate": "boolean condition over the columns; no SELECT/FROM/WHERE",
  "sigma": "title: ...\\nlogsource: ...\\ndetection:\\n  selection:\\n    ...\\n  condition: selection"}}
```
