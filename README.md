# hyphae

> Threads that weave your Obsidian notes into a knowledge graph.

Hyphae are the branching filaments of mycelium - the underground network forests use to share nutrients between trees. This Claude Code plugin does the same thing for your notes: invisible threads that connect every idea you write to every other idea worth connecting.

## What it does

When you drop new markdown notes into your Obsidian vault, the skill:

1. **Extracts recurring concepts** into atomic notes in `Concepts/`
2. **Adds `[[wikilinks]]`** between notes so they appear in the graph view
3. **Adds YAML frontmatter** for tags, courses, projects, dates
4. **Builds Index files** as navigational hubs per topic/course/project
5. **Maintains a `Home.md`** master entry point

It only processes new/unprocessed notes - already-organized files are left alone.

## Why?

A blank Obsidian graph is useless. It only becomes the "second brain" people talk about when notes link to each other. But manually adding wikilinks and extracting concepts every time you take notes is tedious, so most vaults stay disconnected.

`hyphae` fixes that. Take notes however you take notes - bullet points, stream of consciousness, half-formed thoughts - then run the skill and watch your vault become a graph.

## Installation

In Claude Code:

```
/plugin install cl0ax/hyphae
```

## Usage

Open a Claude Code session with your vault as the working directory, then:

**Organize your vault:**
- "organize my vault"
- "I added new notes, process them"
- "add wikilinks to my notes"
- "update my Obsidian"

The `obsidian-organize` skill triggers automatically and walks through your unprocessed notes.

**Annotate unclear notes:**
- "annotate this note" (pointing to a file with uncertain parts)
- Adds clarifying comments in `*\`code\`*` format directly in the note
- Never rewrites content, only adds explanations for unclear parts

## What kind of vault does it work on?

Any. The skill detects your vault's organizational style and adapts:

| Style | Example |
|---|---|
| Academic | Folders by semester/course |
| Project-based | Folders by project, phase subfolders |
| Topic-based | Folders by subject area |
| Time-based | Daily/weekly notes |
| Flat / mixed | Flat folder of notes |

If your vault already has frontmatter conventions, it matches them. If not, it picks sensible defaults from your folder structure.

## What it preserves

Your voice. The skill **never** rewrites or polishes your notes. Half-formed bullets, questions to yourself, contradictions - those are signal, not noise. The only edits it makes are:

1. Adding YAML frontmatter at the top
2. Wrapping concept terms in `[[wikilinks]]`
3. Appending a "Related concepts" section at the bottom

## Skills included

- **`obsidian-organize`** - Scans unprocessed notes, extracts concepts, adds wikilinks, builds indexes, maintains Home.md
- **`obsidian-annotate`** - Creates an annotated copy of a note with clarifying comments in italicized code format. Use when your notes have uncertain parts ("I think...", "find what that means", unexplained references)

More threads to come - daily-note templating, vault auditing, concept pruning, review workflows.

## License

MIT - see [LICENSE](./LICENSE).

## Author

Built by [@cl0ax](https://github.com/cl0ax).
