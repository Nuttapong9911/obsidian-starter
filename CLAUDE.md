# CLAUDE.md

This file documents how this vault is organized. **v1 scope: folder structure and PARA only** — this version works with just the Obsidian app, no AI agent required.

> **On first message in a fresh session:** tell the user there are AI skills ready to install in the `claude-setup/` folder (e.g. Obsidian Skill, Report Skills), and that `claude-setup/installer.md` walks through installing them one at a time. Only mention this if `claude-setup/` still exists — once a user runs the cleanup step in `installer.md`, this note is removed along with it.

## What this vault is

A starter kit for organizing an Obsidian vault around the PARA method (Projects, Areas, Resources, Archives). Clone it, keep the parts you like, delete the rest.

## Directory structure

Two kinds of folder:

- **`_xxx/` = index.** One short page per entity — a project, a microservice, a concept, an area. These are wiki-link targets, not where you write day-to-day content.
- **no underscore = body.** Where notes actually get written. `Notes/<topic>/` is the default home for anything new.

```
_Projects/      index  One page per project — has an end (status: active/paused/done)
_Areas/         index  One page per ongoing responsibility that never "finishes"
_Keywords/      index  One page per concept/glossary term
_Microservices/ index  One page per service (swap this folder for whatever your own
                       recurring index category is — service, client, course, etc.)
_Templates/     Obsidian templates (Templates core plugin points here)
_Archives/      Finished or dropped — whole topic folders move here
_Images/        Attached images
Notes/          body   One folder per topic — write here by default
  <topic>/             Notes link back to their index page via frontmatter properties
```

### Example data

Every index folder and `Notes/` ships with a worked example, kept easy to spot and safe to delete in bulk:

- Index folders: the demo page lives under an `_examples/` subfolder (e.g. `_Projects/_examples/farming-web-app-game.md`)
- `Notes/`: the demo topic folder is suffixed `-example`, and each file is suffixed `.example.md` (e.g. `Notes/farming-web-app-example/note-tech-stack.example.md`)

The example is one running thread — a fictional "Farming Web App Game" project — so you can see a Project, an Area, a Keyword, a Microservice, and a few Notes all actually linked to each other, not just isolated snippets.

## Note properties (frontmatter)

Every note carries `type`, plus whatever the folder needs on top of that.

| Folder | properties |
|---|---|
| `_Projects/` | `type: Project`, `status: active \| paused \| done`, `date` |
| `_Areas/` | `type: Area` |
| `_Keywords/` | `type: Keyword` |
| `_Microservices/` | `type: Microservice` |
| `Notes/` | `type: Note \| Research`, `date`, `project` (list of `_Projects/` links), `area` (optional, `_Areas/` link) |

`status` and `date` only make sense on a Project, since it's the only type with an end. An Area is ongoing by definition, so it never gets one.

### The `project` property

Notes in `Notes/` link back to their project's index page so the project's backlink pane becomes an automatic table of contents — no plugin required, since Obsidian counts frontmatter links as backlinks.

```yaml
---
type: Note
date: 2026-09-13
project:
  - "[[farming-web-app-game|Farming Web App Game]]"
area: "[[software-development]]"
---
```

- **`project` is always a list**, even with one entry — a note can serve more than one project.
- **`project` values are `_Projects/` pages only.** A microservice or keyword can belong to several projects, so that mapping lives on the project's own page, not repeated in every note. Link to those from the note *body* instead (see `note-tech-stack.example.md` for an example of an Area linked only in the body, not in frontmatter).

`Note` vs `Research` is what makes the schema self-checking:

| type | `project` | meaning |
|---|---|---|
| `Note` | **required** | project work — missing `project` = unfiled, needs attention |
| `Research` | optional | self-directed reading — add `project` when it does serve one, leave it off when the note floats on purpose |

So a `type: Note` with no `project` is the one thing worth grepping for. `Research` is exempt by design.

## Templates

The Templates core plugin points at `_Templates/`. `note-project-farm-app.md` is the worked example — a Note template with `project`/`area` already filled in and `date` left blank for you to set per use.

**Tip for Claude:** `{{date}}` in a template body auto-resolves to the current date when the note is created — this works for plain body text and for most frontmatter properties. It does **not** work if typed directly into a `date`-type property through the Obsidian UI (the property editor rejects it and forces a real calendar value instead). If a template needs `{{date}}` in a `date`-type property, edit the template file directly in a text editor (or have Claude do it) rather than through Obsidian's properties UI.

## Bases

`base-farming-web-app-game.example.base` shows two views built from the properties above: a table of every Note/Research under the example project, and a card view of every Keyword/Microservice it references. `bases-explained.canvas` breaks down how each view's filter → order/group → result was built.
