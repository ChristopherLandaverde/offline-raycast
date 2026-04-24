# Offline Text Tools

Local-first grammar fixes and EN↔PT translation, triggered by a global hotkey,
with a mandatory diff review before any text is applied.

- **Local by default** via Ollama — zero network, zero API keys needed
- **Hosted optional** via any LiteLLM-supported provider (OpenAI, Anthropic,
  Groq, etc.) when quality matters more than locality
- **Every edit gets a diff review** — no auto-apply, ever
- **macOS + Linux**, same Python core, different popup frontends

## Status

**Design locked (v0.1), implementation pending.**

- Full spec → [`docs/design-v0.1.md`](docs/design-v0.1.md)
- Deferred work → [`TODOS.md`](TODOS.md)
- Eval criteria → [`docs/evaluation.md`](docs/evaluation.md)

## Commands (v0.1)

- `fix-grammar` — conservative, meaning-preserving edits
- `translate-en-to-pt` — English → Portuguese
- `translate-pt-to-en` — Portuguese → English

Adding a fourth command in v0.1 is: drop a prompt file in `prompts/`, add an
entry to `actions.toml`. No Python changes.

## How it works (summary)

```
Global hotkey
     │
     ▼
┌─────────────────────┐     ┌─────────────────────┐
│ Linux: rofi popup   │     │ macOS: choose popup │
│ + xclip / wl-copy   │     │ + pbpaste / pbcopy  │
└──────────┬──────────┘     └──────────┬──────────┘
           └────────────┬───────────────┘
                        ▼
            ┌────────────────────────┐
            │ python -m offline_text │
            │   (LiteLLM + safety +  │
            │    diff + actions)     │
            └──────────┬─────────────┘
                       ▼
              ┌─────────────────┐
              │ Local: Ollama   │
              │ Hosted: OpenAI, │
              │   Anthropic, … │
              └─────────────────┘
```

Full architecture diagram + repository layout in [`docs/design-v0.1.md`](docs/design-v0.1.md).

## Install (planned, per OS)

**Linux:**
```
git clone … && cd offline-text-tools && make install
# installs to ~/.local/bin, sets up rofi popup, prints keybind snippet
```

**macOS:**
```
git clone … && cd offline-text-tools && make install
# runs brew install choose-gui ollama, installs the Python package,
# prints a Raycast Script Command snippet for the hotkey
```

Both paths assume Ollama is installed locally if you want the default local
provider. Hosted providers need an API key stored via the OS keychain:
```
offline-text config set-key anthropic
```

Full install + per-machine config guide is in the design doc.

## License

MIT — see [`LICENSE`](LICENSE).
