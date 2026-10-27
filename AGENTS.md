# Volo AI / tts-mcp


## Coding

You are responsible for managing the final outcomes of tasks. Use the Devin CLI—specifically the `swe-2-max` model with full permissions—to execute these tasks. Your role is to oversee the results; if any issues arise, instruct the `swe-2-max` model to fix them until the task is complete. Stick closely to the plan, and if you are unsure about anything or wish to propose an approach, ask for clarification. Since the `swe-2-max` model is free to use, leverage it for as much work as possible to conserve your own quota.

## Purpose

This repository contains the local OmniVoice TTS engine exposed through a
terminal CLI, an MCP server, and a Volo AI Tauri desktop app.

## Layout

- `src/tts_mcp/`: Python engine, audio conversion, CLI, MCP server, and the
  JSONL desktop sidecar.
- `apps/desktop/`: React + TypeScript UI and Tauri 2 shell.
- `scripts/`: sidecar packaging helpers.
- `tests/`: stdlib `unittest` coverage for engine, conversion, and sidecar
  protocol.
- `docs/agent-devkit/`: approved feature specs and execution plans.
- `docs/llm/`: source-grounded codebase wiki.

## Runtime requirements

- Python 3.10 or newer.
- Node.js 22 or newer.
- Rust toolchain for Tauri.
- FFmpeg for MP3 export during source development; release sidecars bundle it.

## VieNeu dependency

Desktop and MCP VieNeu support requires `vieneu>=3.8.3,<4` from the
`desktop-vieneu` extra; VieNeu 2.x does not provide the v3 Turbo engine used by
this project. Install the extra into the exact Python environment running the
desktop/MCP process or building the sidecar with
`python -m pip install -e '.[desktop-vieneu]'`. When diagnosing a missing
VieNeu module, check that interpreter's package version first. Rebuild a
bundled sidecar after changing its Python dependencies.

## Verified commands

```sh
python -m unittest discover -s tests -v
python -m compileall -q src tests
npm --prefix apps/desktop run build
cargo check --manifest-path apps/desktop/src-tauri/Cargo.toml
```

The desktop sidecar uses newline-delimited JSON on stdin/stdout and writes
diagnostic logs to stderr. Model assets and generated audio live in the
directory selected by `TTS_MCP_DATA_DIR`; packaged apps use their Tauri app
data directory.

## Code intelligence

OpenEZ is indexed for this repository. Prefer MCP tools
(`code_query`, `code_context`, `graph_neighbors`) for semantic code
questions. Use `memory_recall` for previously recorded decisions.
Fall back to direct file reads and `rg` when OpenEZ is unavailable.

## GitHub issue tracking

Before starting a task, create a GitHub issue for it or reuse an existing issue
that already tracks the work. Record its number as `#<number>` in the active
task description or plan, include the numeric ID in any Agent Devkit change
folder name according to the
[using-devkit workflow](.agents/skills/using-devkit/SKILL.md), and reference
the issue in the pull request.

Reference the issue number in commit messages (`Refs #<number>` for partial
work). When a pull request fully resolves the issue, write `Closes #<number>`
in the PR body so GitHub closes it on merge; when work completes without a
pull request, close it with `gh issue close <number> --comment "<result>"`.
Keep the issue open while any of its scope remains unmerged.

## Devin CLI delegation

The coordinating agent owns the final result: plan adherence, acceptance
criteria, review, and evidence. Delegate all suitable research,
implementation, tests, documentation, and repairs to the local Devin CLI to
reduce coordinator quota. Use model `swe-2-max` (preferred because the user
wants to use their free allowance; do not assume it is permanently free) and
permission mode `dangerous`. Do not delegate on `swe-2-max` once the free
allowance is gone or unknown; ask the user before running a paid model.

```sh
rtk proxy devin --model swe-2-max --permission-mode dangerous -p "<task prompt>"
rtk proxy devin --model swe-2-max --permission-mode dangerous --resume SESSION_ID -p "<follow-up prompt>"
```

When delegating, give the executor the issue or approved plan, scope,
acceptance criteria, existing changes to preserve, and verification
commands. Ask it to report changed files, checks run, remaining issues, and
its session ID. Afterwards, inspect the final diff and check evidence
yourself — never accept a self-report as proof — and return precise defects
to the same Devin session via `--resume`, repeating repair and verification
until done.

Ask the user when requirements are uncertain or a change goes beyond the
plan; routine in-scope repairs continue without repeated approval. Full tool
permissions stay within the authorized task and do not permit unsolicited
commits, pushes, deployments, or destructive operations. Devin must not
recursively delegate to itself. If the CLI, model, or auth is unavailable,
report the blocker and ask before switching model or taking over execution.

## Skills

- `brainstorm-feature`: `.agents/skills/brainstorm-feature/SKILL.md`
- `context-handoff`: `.agents/skills/context-handoff/SKILL.md`
- `document-wiki`: `.agents/skills/document-wiki/SKILL.md`
- `estimate-feature`: `.agents/skills/estimate-feature/SKILL.md`
- `implement-task`: `.agents/skills/implement-task/SKILL.md`
- `plan-feature`: `.agents/skills/plan-feature/SKILL.md`
- `read-codebase-context`: `.agents/skills/read-codebase-context/SKILL.md`
- `review-and-verify`: `.agents/skills/review-and-verify/SKILL.md`
- `setup-codebase`: `.agents/skills/setup-codebase/SKILL.md`
- `setup-openez`: `.agents/skills/setup-openez/SKILL.md`
- `systematic-debugging`: `.agents/skills/systematic-debugging/SKILL.md`
- `using-devkit`: `.agents/skills/using-devkit/SKILL.md`

## Documentation

The verified codebase wiki is at `docs/llm/INDEX.md`.

For behavior, workflow, or domain questions:

1. Read `docs/llm/INDEX.md`.
2. Open the relevant wiki page.
3. Verify important claims against current source and tests.

The wiki describes verified behavior only; source and tests remain
authoritative.
