---
name: monthly-report
description: Gather all daily reports from a given month, compile completed work into a draft, and create a monthly report file at Monthly/YYYY/YYYY-MM.md with the draft at the top and a blank report template at the bottom. Use this skill whenever the user says "/monthly-report", "monthly report", "สรุปเดือน", "รายงานเดือน", or names a specific month (e.g. "May", "last month", "April 2026").
---

# Monthly report

## 1. Gather — substitute the target month, then run verbatim

`TM` is `YYYY-MM`: no argument means this month, `$(date -v-1m +%Y-%m)` is last month.

```bash
cd __VAULT_PATH__
TM="<YYYY-MM>"
echo "OUT=Reports/Monthly/${TM%-*}/$TM.md"
echo "DATE=$TM-01"
echo "LABEL=$(date -j -f %Y-%m-%d "$TM-01" +'%B %Y')"
find "Reports/Daily/$TM" -name '*.md' 2>/dev/null | sort | while read -r f; do
  echo "=== $(basename "$f" .md)"; cat "$f"; echo
done
```

The daily folder is `Reports/Daily/YYYY-MM` — numeric, not `May-2026`. No `===` lines means no daily reports that month — say so and stop.

## 2. Write

Write this to the printed `OUT` path, using the printed `DATE` and `LABEL`:

```markdown
---
type: Monthly note
date: <DATE>
---

# Draft:

- <Project or service>
	- <task>

---
# <LABEL>

**Monthly Report : <LABEL>**

```

Compile the draft from the **สิ่งที่ทำเสร็จแล้ว** sections only. Group by project, merging and deduplicating sub-tasks across days. Strip wikilinks (`[[A|b]]` → `b`, `[[A]]` → `A`). A month is long — do not over-compress; this is raw material the user edits into the real report by hand. Overwrite any existing file.

## 3. Open

```bash
obsidian open vault=__VAULT_NAME__ path="<OUT>"
```

## Constraints

- `date:` is the first of the **target** month — running this in October for September still stamps `2026-09-01`.
- The `---` above `# <LABEL>` is a horizontal rule, not frontmatter; Obsidian closes frontmatter at the first `---` pair.
- Only reads `Reports/Daily/<TM>/`, so a daily note filed in the wrong month folder is missed silently.
- No Obsidian template: the draft sits mid-file so `create template=` would be overwritten anyway, and core Templates has no date offset for past months.
- Requires Obsidian to be running.

## Output

1. Show the compiled draft
2. "✅ Saved: `<OUT>`"
