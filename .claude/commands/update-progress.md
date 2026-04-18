---
description: Update QEA progress.html from the latest weekly progress report, then commit and push to main
argument-hint: "[optional: YYYY-MM-DD — target a specific weekly report file instead of the latest]"
---

# Update QEA progress.html from the latest weekly report

You are updating `progress.html` in this repo (qea-coach) from Jag's latest QEA weekly progress report. The weekly report lives on his Mac at `~/Documents/Claude/LBA Programme./` and is named `QEA_Progress_Update_YYYY-MM-DD.md`. Each report embeds a ready-to-apply "Claude Code Prompt" block that describes exactly what to change in `progress.html`.

## Steps

1. **Locate the source report.**
   - If `$ARGUMENTS` is a date in `YYYY-MM-DD` form, use `~/Documents/Claude/LBA Programme./QEA_Progress_Update_$ARGUMENTS.md`.
   - Otherwise pick the most recently modified `QEA_Progress_Update_*.md` in that folder.
   - If no matching file exists, stop and tell Jag which path you checked.

2. **Extract the change instructions.**
   - Read the report and find the fenced code block under the "Claude Code Prompt" section (starts with a line like "Update progress.html in the qea-coach repo…").
   - If no such block is present, stop and tell Jag which file was read and why you couldn't find the block.

3. **Sanity-check the repo state.**
   - Run `git status` and `git branch --show-current`.
   - If the branch isn't `main`, stop and ask before continuing.
   - If there are uncommitted changes unrelated to `progress.html`, stop and flag them — don't sweep them into this commit.
   - Run `git pull --ff-only` to catch up with remote.

4. **Apply the changes to `progress.html`.**
   - Read `progress.html` first.
   - Apply each change in the Claude Code Prompt block using the `Edit` tool.
   - If an instruction is ambiguous against the actual HTML (e.g. the selector/wording doesn't match anything in the file), do NOT guess — stop and surface the mismatch with the exact old/new strings you were trying to use and what's actually in the file.
   - Scrub rule: if the report calls for stripping employer or stakeholder names (Path E posture), do it thoroughly — grep the final HTML for any remaining occurrence and confirm it's clean.

5. **Show the diff.**
   - Run `git diff -- progress.html` and print the output so Jag can eyeball it.
   - Keep it to the diff only — no additional narration unless something unusual came up.

6. **Commit and push.**
   - Stage only `progress.html`: `git add progress.html`.
   - Use the commit message from the report's Claude Code Prompt block. If none is specified, fall back to:
     `Update progress page — $REPORT_DATE: $SHORT_SUMMARY`
     where `$REPORT_DATE` is the date in the report filename and `$SHORT_SUMMARY` is a ≤10-word summary derived from the change list.
   - Never use `--amend` or `--no-verify`.
   - Push with `git push origin main`. Never force-push.

7. **Report back.**
   - One short message to Jag: which weekly report was used, which commit SHA was pushed, and the public URL `https://jagadeeshjayachandran.github.io/qea-coach/progress.html` (GitHub Pages rebuild is typically under a minute).

## Guardrails

- Never touch files other than `progress.html`.
- Never create a new branch or PR — this repo is simple and Jag pushes direct to `main`.
- Never rewrite history.
- If anything looks off — missing report, missing code block, ambiguous instruction, dirty working tree, wrong branch, failed push — stop and ask. Better to bounce back than ship a bad page.
- Don't add Claude/Co-Authored-By trailers unless Jag asks; this is his personal site.

## Success criteria

- `progress.html` reflects the latest weekly report.
- A single clean commit on `main` with the correct message.
- Pushed to GitHub.
- Jag has the commit SHA and the live URL.
