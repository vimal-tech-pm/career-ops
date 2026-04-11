---
name: career-ops
description: Use when the user wants to run the career-ops workflow in Codex for job evaluation, tailored CV generation, application help, portal scanning, tracking, or batch processing. Do not use for unrelated coding tasks.
---

# career-ops for Codex

Use this skill to translate the original Claude/OpenCode workflow into Codex-native behavior.

## When to use this skill

- The user pastes a JD URL or JD text and wants the full workflow.
- The user wants a tailored CV or PDF for a role.
- The user wants help applying to a role.
- The user wants to scan portals, process the pipeline inbox, or batch-evaluate roles.
- The user wants to inspect or update the application tracker.

If the request is unrelated to the career-ops workflow, do not use this skill.

## Router

Infer the mode from the user's request:

| User intent | Mode |
|-------|------|
| No clear task yet | `discovery` |
| JD text or job URL | `auto-pipeline` |
| Evaluate one offer only | `oferta` |
| Compare multiple offers | `ofertas` |
| Generate or regenerate a tailored CV/PDF | `pdf` |
| Help fill a live application form | `apply` |
| Show or update application tracking | `tracker` |
| Process URLs from `data/pipeline.md` | `pipeline` |
| Scan `portals.yml` for new jobs | `scan` |
| Batch-evaluate many jobs | `batch` |
| Research a company | `deep` |
| Draft outreach | `contacto` |
| Evaluate training or a course | `training` |
| Evaluate a project idea | `project` |

If the user only invokes the skill name without a task, show this command-center summary:

```
career-ops -- Command Center

Main uses:
  - Paste a JD URL or JD text → full auto-pipeline
  - Ask for a tailored CV/PDF for a role
  - Ask for application-form help
  - Ask to scan, batch-evaluate, or track roles

Core files:
  - cv.md
  - config/profile.yml
  - modes/_profile.md
  - data/applications.md
  - reports/
  - output/
```

## Context loading

Load only the files needed for the chosen mode.

- For `auto-pipeline`, `oferta`, `ofertas`, `pdf`, `contacto`, `apply`, `pipeline`, `scan`, and `batch`:
  read `modes/_shared.md` and `modes/{mode}.md`
- For `tracker`, `deep`, `training`, and `project`:
  read only `modes/{mode}.md`

Also load user context when relevant:
- `cv.md` is the canonical CV source
- `config/profile.yml` and `modes/_profile.md` hold user-specific customization
- `article-digest.md` and `interview-prep/story-bank.md` are supporting context
- `data/applications.md` is the tracker
- `portals.yml` drives portal scanning

## Behavior notes

- Do the work directly in Codex rather than expecting `/career-ops ...` slash commands.
- Prefer Playwright or browser-based verification when the mode instructions require it.
- Never auto-submit an application; always stop before the final send/apply action.
- Respect the data contract in `DATA_CONTRACT.md`: user-specific customization belongs in `config/profile.yml` or `modes/_profile.md`, not in shared system files.
- For batch work, keep tracker integrity rules intact and use the existing scripts when needed.

Follow the selected mode instructions and produce the corresponding reports, PDFs, tracker updates, or application guidance.
