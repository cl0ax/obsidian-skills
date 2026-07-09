---
name: obsidian-annotate
description: Use when the user has an Obsidian note with unclear, uncertain, or incomplete parts - phrases like "I think", "find what that means", "(not exactly but...)", or quoted statements that need explanation. Adds clarifying annotations in place.
---

# obsidian-annotate

Adds clarifying annotations directly to an Obsidian note. Explains uncertain or incomplete parts without rewriting the original content or the student's voice.

## Annotation Format

```
- original note line
	- *`Clarifying annotation indented directly under the line it explains.`*
```

Every annotation must be:
- Wrapped in backticks (inline code)
- Italicized with `* *`
- Indented one level below the line it clarifies

## What to Annotate

Target lines that signal uncertainty or incompleteness:

| Signal | Example |
|---|---|
| Uncertainty | "I think he means...", "I think stuff went down b/c..." |
| Explicit gaps | "(find what that means)", "(not exactly but...)" |
| Unexplained references | A real-world incident mentioned briefly |
| Quoted statements | Professor quote with no follow-up explanation |
| Unnamed tools/terms | Terms introduced without definition |

**Do NOT annotate:**
- Lines that are already self-explanatory
- Lines the student already clarified in their own words below
- Logistical notes (exam format, dates, room info)

## Steps

1. **Read the note** - identify all uncertain or incomplete lines
2. **Add annotations in place** - insert one `- *\`...\`*` line, indented directly under each line that needs clarifying
3. **Preserve everything** - do not remove, reorder, or rephrase any original content
