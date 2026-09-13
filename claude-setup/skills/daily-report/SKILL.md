---
name: daily-report
description: Create and open today's daily report in the Obsidian vault at __VAULT_PATH__/Reports, then open the previous report in a split pane beside it. Use this skill whenever the user says "/daily-report", "daily report วันนี้", "สร้าง daily report", "เปิด daily report", "เริ่ม daily report", or anything about starting/opening today's work report.
---

# Daily report

Obsidian does the work. Run the block below **verbatim** as one Bash call — do not compute dates yourself, do not write the note body, do not read the file back.

```bash
cd __VAULT_PATH__
T=$(date +%Y-%m-%d); M=$(date +%Y-%m); F="Reports/Daily/$M/$T.md"
P=$(find Reports/Daily -name "*.md" ! -name "$T.md" | sort | tail -1)

mkdir -p "Reports/Daily/$M"
if [ -f "$F" ]; then STATUS="opened"; else
  obsidian create vault=__VAULT_NAME__ path="$F" template="daily_report" silent >/dev/null
  STATUS="created"
fi

if [ -n "$P" ]; then
  obsidian open vault=__VAULT_NAME__ path="$P" >/dev/null
  sleep 1
  open "obsidian://open?vault=__VAULT_NAME__&file=${F%.md}&paneType=split"
else
  obsidian open vault=__VAULT_NAME__ path="$F" >/dev/null
fi

echo "$STATUS $F"
echo "previous ${P:-none}"
```

## Why it is shaped this way

- **`obsidian create ... template=daily_report`** fills `type`, `date`, and the header from `_Templates/daily_report.md`. Keep the frontmatter contract there, not here.
- **`[ -f "$F" ]` guard is required.** Without it `obsidian create` does not fail or skip on an existing file — it silently makes `2026-09-08 1.md`.
- **Previous note opens first, today second.** `paneType=split` splits to the *right* of the active pane, so this order puts previous on the left and leaves today focused. Reversing it puts them on the wrong sides.
- **`obsidian open` cannot create.** A missing path returns `Error: File ... not found`, so open-then-create is strictly worse than the `-f` test.
- **`obsidian open` has no `paneType`** (only `newtab`), which is why the second pane still goes through the `obsidian://` URI.
- **Daily notes are weekday-only**, so the previous note is not `today - 1`. `find | sort | tail -1` resolves it in one shot — no `previous:` pointer is stored in the note.
- **The template stamps the clock, not the filename.** `{{date}}` always resolves to the moment the file is created, so this skill can only ever produce *today's* note correctly — it cannot backfill a missed day. To write a past day, create the file by hand and set `date:` to match the filename.

## Output format

Report the two echoed lines back as:

1. "✅ Today's report created: `<path>`" — or "opened" if it already existed
2. "📅 Opened previous (`<date>`) in split pane" — or "no previous report found"

Do not print the note body.
