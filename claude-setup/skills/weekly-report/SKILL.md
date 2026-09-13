---
name: weekly-report
description: Read all daily reports from the current week, compile what was done into a clean bullet-point weekly summary, write it to Weekly/, and open it in Obsidian. Use this skill whenever the user says "/weekly-report", "weekly report", "สรุปอาทิตย์นี้", "สรุปสัปดาห์", "week summary", or wants to prepare a summary for a weekly meeting.
---

# Weekly report

## 1. Gather — run verbatim

```bash
cd __VAULT_PATH__
D=$(date +%u); M() { date -v-$((D-1))d "$@"; }
Y=$(M +%G); W=$((10#$(M +%V))); S=$(( (W-1)/10*10+1 ))
echo "OUT=Reports/Weekly/$Y/W$S-W$((S+9))/W$W.md"
echo "DATE=$(M +%Y-%m-%d)"
echo "TITLE=สรุปงานประจำสัปดาห์ $(M +'%-d %b %Y') – $(M -v+4d +'%-d %b %Y')"
for i in 0 1 2 3 4; do
  d=$(M -v+${i}d +%Y-%m-%d); f="Reports/Daily/${d%-*}/$d.md"
  [ -f "$f" ] && { echo "=== $d"; cat "$f"; echo; }
done
true
```

No `===` lines means no daily reports this week — say so and stop.

## 2. Write

Write this to the printed `OUT` path, using the printed `DATE` and `TITLE`:

```markdown
---
type: Weekly note
date: <DATE>
---

# <TITLE>

- <Project or service>
  - <task>
```

Compile from the **สิ่งที่ทำเสร็จแล้ว** sections only — ignore สิ่งที่กำลังทำ. Group by project, merging and deduplicating sub-tasks across days. Strip wikilinks (`[[A|b]]` → `b`, `[[A]]` → `A`). Thai for descriptions, English for technical terms. Keep it short enough to read aloud at a meeting. Overwrite any existing file.

## 3. Open

```bash
obsidian open vault=__VAULT_NAME__ path="<OUT>"
```

## Constraints

- `%G`, not `%Y` — Monday 2025-12-29 is ISO week 01 of **2026**, so `%Y` would file it a year early. `10#` strips `%V`'s zero padding so `W1` matches the `W1-W10` folder.
- `date:` is that week's Monday, never today.
- Current week only; there is no argument for past weeks.
- No Obsidian template: `{{date:FORMAT}}` takes any moment.js pattern but core Templates has **no date offset**, and every value here is anchored to Monday.
- Requires Obsidian to be running.

## Output

1. Show the compiled summary
2. "✅ Saved: `<OUT>`"
