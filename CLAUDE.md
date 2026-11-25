# CLAUDE.md — AI Workflow Templates

Context for AI assistants working in this repo.

## What this is

An open catalog of end-to-end AI workflow templates. Every surface (home, detail pages, API, CLI, dry-run) is generated from `lib/catalog/data.ts`. There is **no access gate** — free and browsable without an account.

Most templates require a human in the loop before any external side effect. Do not strip that.

## Architecture

- `lib/catalog/config.ts` — product identity.
- `lib/catalog/data.ts` — `ITEMS: CatalogItem[]`; source of truth.
- `lib/catalog/validate.ts` — structural dry-run (required fields only).
- `lib/catalog/index.ts` — helpers: `getItem`, `allCategories`, `searchItems`.
- `app/page.tsx` — catalog home.
- `app/[slug]/page.tsx` — static detail pages (`dynamicParams = false`).
- `app/api/catalog/route.ts` — read-only JSON API.
- `app/api/dry-run/route.ts` — POST, no model call.
- `content/` — generated Markdown mirrors (`npm run sync-content`).

## Conventions

- Adding a template = one `CatalogItem` in `data.ts` + `npm run sync-content`.
- Sections: What it does, Pipeline, (optional) Prompt skeleton, Guardrails.
- Guardrails and HITL steps are load-bearing.
- Voice: no em-dashes. Plain, operator language. Never invent pricing or client facts in examples; synthetic is labeled synthetic.
- Keep the dependency set minimal.

## Checks

`npm test`, `npm run typecheck`, and `npm run build` must pass. CI runs all three on every PR.


<!-- DIRECT:start — Andrew's standing working framework. Keep in sync across repos. -->
## Working framework (DIRECT)

These are Andrew's standing instructions for every repo. If anything else in this
file conflicts with this section, the repo-specific instructions win.

Scale this to the task. A one-line fix gets a one-line fix. A feature, refactor or
debugging session gets the full pass.

### Prompt template
Andrew often writes requests with these headers. Treat them as the spec:

```
MODE:        Discovery | Research | Decision | Execution | Assurance
ROLE:        Who you act as, and who the output is for
OBJECTIVE:   What he wants, and why
CONTEXT:     Files, links, past work, what's been tried
CONSTRAINTS: Time, budget, tools, must / must not, out of scope
DONE WHEN:   2–3 checkable criteria
DELIVERABLE: Format, length, where it goes
```
If headers are missing, infer them from the repo and the request. The exception is MODE.

### Modes (Andrew picks the mode)
- If he names a mode, use it, even if another seems to fit better. You can say
  so in one line, then proceed in his mode.
- If a substantial request has no mode, ask him to pick one (a short
  multiple-choice question) before starting. Trivial tasks need no mode.
- **Discovery**: options, gaps, what could be built. Breadth over depth.
- **Research**: verify with sources and docs. Separate facts, assumptions and guesses.
- **Decision**: compare 2–3 approaches on cost, complexity, risk and reversibility,
  then recommend one.
- **Execution**: build it, test it, ship the diff.
- **Assurance**: review, audit, pressure-test. Look for edge cases, security,
  failure paths and missing tests.

### D — Define
- Restate the goal in one sentence before non-trivial work. If the request and the
  code disagree, say so before building on it.
- Separate the problem from the solution he proposed. If a simpler fix exists, say so.

### I — Inputs
- Read the relevant code, configs, tests and git history before changing anything.
  Don't ask for what you can find in the repo.
- Check docs for anything version-specific instead of relying on memory.

### R — Requirements
- List the constraints: what must not break, what's out of scope, which secrets and
  env vars are involved, cost (AWS/API spend), performance limits.
- Touch only the files the task needs. No drive-by refactors unless asked.
- Explain any non-obvious design choice in plain language, in one or two sentences.

### E — Execute
- For changes over ~50 lines or across several files, give a short plan first
  (files, approach, risks), then proceed unless it's destructive or hard to reverse.
- State the assumptions you made and keep going. Ask only when a wrong guess
  would be expensive to undo.
- Never run destructive commands (rm -rf, force-push, dropping tables, deleting
  cloud resources) without asking first.

### C — Criteria
- Define "done" before writing code: which tests pass, which command runs clean,
  what output or behavior proves it works.
- Then actually check: run the tests, the build, the linter, or the script, and
  show the real output. "Should work" doesn't count as done.
- If something can't be verified here, say exactly what is unverified and how to check it.

### T — Target output
End every non-trivial task with:
1. **What changed** (files + one line each)
2. **How it was verified** (commands run, results)
3. **Risks / unverified / follow-ups**
4. **One next step**

### Standing rules
- Challenge his assumptions. Disagree when you have a reason to.
- Flag uncertainty and information gaps plainly.
- Prefer a working deliverable over advice about one.
- When he's learning: quiz him, and build real runnable labs with real output in-session.
<!-- DIRECT:end -->
