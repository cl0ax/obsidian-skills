---
name: obsidian-organize
description: Organizes an Obsidian vault by processing new or unprocessed markdown notes. Adds YAML frontmatter, wraps key terms in [[wikilinks]], creates atomic concept notes in Concepts/, builds Index files, and keeps a Home note current so the graph view shows meaningful connections. Use whenever the user mentions Obsidian, vaults, wikilinks, knowledge graphs, or note organization - including phrases like "organize my vault", "I added new notes", "process my notes", "add wikilinks", "update my Obsidian", "make my graph view useful", or anytime they drop new markdown notes into a vault and want them connected to existing ones.
---

# obsidian-organize

Incrementally organizes an Obsidian vault so the graph view reflects all meaningful connections between notes. Works on new notes only - doesn't touch already-processed files unless explicitly asked.

## Why this skill exists

A blank Obsidian graph is useless. It only becomes the "second brain" people talk about when notes link to each other via `[[wikilinks]]`. But manually adding wikilinks every time you write a note is tedious, and most people don't extract recurring ideas into atomic concept notes - so concepts that appear across many notes never get a hub to link to.

This skill automates that process. It scans the vault for unprocessed notes, extracts cross-cutting concepts into their own files, and weaves wikilinks throughout - turning a pile of disconnected markdown into a navigable knowledge graph.

## The philosophy (short version)

1. **Atomic concept notes** - every recurring idea gets its own short note in `Concepts/`. Definition + key points + links to related concepts.
2. **Wikilinks** - `[[Term]]` syntax in any note creates an edge in the graph. The more edges, the richer the graph.
3. **Index notes** - one per topic/course/project, acts as a navigational hub.
4. **Home note** - the master entry point that links to all indexes and concepts.
5. **Frontmatter** - YAML metadata at the top of each note enables tag filtering, queries, and Dataview-style features.
6. **Incremental** - never re-process already-organized notes. Only touch what's new.

## Step 1: Survey the vault

Read every `.md` file in the vault (skip `.obsidian/`, `node_modules/`, dotfiles). Separate into:
- **Already processed**: has YAML frontmatter at the top (between `---` markers)
- **Unprocessed**: no frontmatter, or frontmatter exists but lacks a primary grouping field (`course`, `project`, `topic`, etc.)

Also read everything in `Concepts/` and `Index/` (or whatever they're called in this vault) to know what's already there before creating duplicates.

If there are no unprocessed notes, tell the user and stop. Don't invent work.

## Step 2: Detect the vault's organizational style

Different vaults are organized differently. Read the folder structure and existing frontmatter to detect what's going on:

| Style | Signals | Primary grouping |
|---|---|---|
| **Academic** | folders like `Y4S2`, `Spring2026`, course codes (`CSC3510`) | `course` field |
| **Project-based** | folders named after projects, phase/sprint subfolders | `project` field |
| **Topic-based** | folders by subject (`Machine Learning`, `Cooking`) | `topic` field |
| **Time-based** | daily/weekly notes, date-named files | `date` field |
| **Mixed/None** | flat structure, ambiguous | ask the user once, then proceed |

Match the convention already used by processed notes if any exist. Don't impose a different scheme.

## Step 3: Extract concepts from new notes

For each unprocessed note, identify which terms deserve their own concept note. A concept earns a standalone note if it:
- Appears across **multiple** notes (now or likely soon), OR
- Is **substantive enough** that someone would want to look it up independently

**Good concept candidates:** frameworks, tools, patterns, theories, recurring people/places/projects, technical terms, named methodologies.

**NOT concept candidates:** one-off facts, professor names, assignment logistics, dates, casual phrases, anything that only makes sense in the context of one note.

Before creating a new concept note, check if one already exists. If it does, see whether the new note adds anything worth appending to it.

## Step 4: Create or update concept notes

For each genuinely new concept, create `Concepts/<Concept Name>.md`:

```markdown
---
type: concept
tags: [relevant, tags]
related: ["[[Related Concept]]", "[[Another]]"]
---

# Concept Name

Plain-language definition. What is this? One or two sentences max.

## Key points
- Pulled from the user's own words where possible
- Keep their phrasing intact

## Related concepts
- [[Concept A]]
- [[Concept B]]
```

If an existing concept note is missing connections newly revealed, update its `related:` array. Don't rewrite its body unless the user asks.

## Step 5: Rewrite unprocessed notes

For each unprocessed note, produce an enhanced version that adds structure **without changing the user's content**.

### Add YAML frontmatter at the top:

```yaml
---
<primary-grouping>: <value>      # course: CSC3510, project: SmartPrep, topic: ML, etc.
<secondary>: <value>             # week: 7, phase: planning, date: 2026-04-24, etc.
tags: [lowercase-hyphenated, topic-keywords]
---
```

Match whatever convention the vault is already using. If unprocessed notes are the first ever, infer from filename and folder.

### Add [[wikilinks]] inline

- Wrap the **first meaningful occurrence** of any concept-noted term in `[[]]`
- Don't link every instance - repetition clutters
- Don't link trivial/passing mentions
- If a term clearly deserves a concept note but doesn't have one yet, create the concept note (Step 4) and link it

### Add a "Related concepts" section at the bottom

```markdown
## Related concepts
- [[Concept A]]
- [[Concept B]]
```

### Preserve the user's voice

This is critical. **Do not** rephrase, polish, fix grammar, or delete half-formed thoughts. Questions, contradictions, and incomplete bullet points are intentional - they show where the user's understanding is. Sanitizing them removes signal.

The only allowed edits are: adding frontmatter, wrapping terms in `[[]]`, and appending the related-concepts section.

## Step 6: Update Index files

For each grouping (course, project, topic, etc.) that had new notes processed:
- Open `Index/<Group> Index.md`, or create it if missing
- Add wikilinks for any new notes not already listed
- Add wikilinks to concept notes newly associated with this group

### Index template (if creating new):

```markdown
---
type: index
<grouping>: <value>
tags: [<grouping-value>]
---

# <Group Name>

## Notes
- [[Note Title]]

## Concepts
- [[Concept A]]
```

## Step 7: Update the Home note

Open `Home.md` (create it if missing) and:
- Ensure every active grouping has an entry pointing to its Index note
- Ensure every concept note is listed
- Add new entries; **never** remove existing ones (the user may have added things manually)

### Home template (if creating new):

```markdown
---
type: home
tags: [index]
---

# Vault Home

## <Grouping plural, e.g. Courses / Projects / Topics>

| Group | Index |
|---|---|
| <Name> | [[<Group> Index]] |

## Concepts
- [[Concept A]]
```

## Naming conventions

| Thing | Convention | Example |
|---|---|---|
| Concept notes | Title Case, spaces OK | `Dependency Injection.md` |
| Index notes | `<Group> Index.md` | `CSC3510 Index.md` |
| Wikilinks | Exact filename minus `.md` | `[[Dependency Injection]]` |
| Tags | lowercase-with-hyphens | `dependency-injection` |
| Frontmatter keys | lowercase | `course:`, `tags:` |

## What NOT to do

- Don't delete or rename existing files
- Don't rewrite or polish the user's original content - preserve their voice exactly
- Don't create concept notes for one-off or trivial things
- Don't wikilink every single mention - only the meaningful first occurrences
- Don't re-process notes that already have proper frontmatter unless the user explicitly asks
- Don't impose your own organizational scheme if the vault already has one
- Don't invent content - if a note is sparse, leave it sparse (just add structure)

## When done

Report back to the user with a concise summary:
- How many new notes were processed
- Which concept notes were created (new ones)
- Which concept notes were updated (added connections)
- Which Index files were touched
- Any notes that were skipped and why

Then suggest they open the graph view in Obsidian to see the new connections.
