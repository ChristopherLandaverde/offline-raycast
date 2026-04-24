# CLAUDE.md

Project context for Claude Code and any AI assistant working in this repo.

## What this is

Offline Text Tools — a cross-platform (macOS + Linux) command-line + popup tool
that runs local-first grammar fixes and EN↔PT translations via Ollama, with an
optional hosted-model path (OpenAI / Anthropic / any LiteLLM-supported provider)
selected per action. Every edit requires a diff review before anything is
applied. Clipboard in, clipboard out.

The authoritative spec is `docs/design-v0.1.md`. That doc is post-review and
post-approval — it's the source of truth for architecture and scope.

## Repo layout

```
src/offline_text/    # Python core (empty until implementation lands)
frontends/           # rofi (Linux) + choose (macOS) popup wrappers
prompts/             # action prompt files, one per command
docs/                # design, architecture, evaluation
TODOS.md             # v0.2 deferrals
```

## Testing

Test framework: **pytest** + **pytest-httpx** (for mocking LiteLLM calls).

```
pip install -e ".[dev]"   # once src/ lands with pyproject.toml
pytest                    # runs the full suite
pytest tests/safety.py    # single module
```

Test expectations:
- 100% coverage on the core modules (safety, diff, providers, actions).
  Greenfield repo — there's no excuse for uncovered paths.
- Every new function gets a test. Every new if/else gets tests for both paths.
- Every error handler gets a test that triggers the error.
- Regression tests are mandatory, no AskUserQuestion needed.
- Never commit code that makes existing tests fail.
- Mock all external calls (Ollama, OpenAI, Anthropic). Use pytest-httpx.

## Coding standards

- Python 3.11+
- Type hints throughout, `mypy --strict` in CI
- No clever one-liners when a named variable reads better
- No comments that restate the code; comments explain non-obvious WHY only
- Error paths use the discriminated JSON schema from design-v0.1.md

## Skill routing

When a user request matches an available gstack skill, invoke it using the Skill
tool as your FIRST action. Do NOT answer directly, do NOT use other tools first.

- Product ideas, "is this worth building", brainstorming → /office-hours
- Bugs, errors, "why is this broken" → /investigate
- Ship, deploy, push, create PR → /ship
- QA, test the site, find bugs → /qa
- Code review, check my diff → /review
- Update docs after shipping → /document-release
- Weekly retro → /retro
- Architecture review → /plan-eng-review
- Save progress, checkpoint, resume → /checkpoint
- Code quality, health check → /health

## Non-goals (from design-v0.1.md, worth repeating)

- Windows support (macOS + Linux only)
- Hosted-model fallback on the default path (local Ollama is always the default)
- Auto-apply edits without diff review (never, not even for small edits)
- Accessibility-API direct-replace in v0.1 (clipboard-only for now)
- Telemetry or phone-home of any kind
