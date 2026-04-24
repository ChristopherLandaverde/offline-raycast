> **Superseded by [`design-v0.1.md`](./design-v0.1.md).**
>
> This was the initial v0 sketch written before the office-hours + eng-review
> cycle. It's preserved for historical context. For current architecture,
> scope, premises, and the authoritative spec, read `design-v0.1.md`.

# Architecture (v0 sketch — historical)

## MVP

The first version should avoid agent layers and focus on deterministic text transforms.

Flow:

1. capture selected text or clipboard text
2. detect requested action
3. send text to a local model with a strict prompt
4. compare input and output
5. require review if the output changes too much
6. copy or paste the approved result

## Components

### UI shell

- global shortcut
- compact popup
- review dialog with diff

### Local inference adapter

- abstract one provider interface
- support `Ollama` first
- keep `llama.cpp` as a later adapter

### Safety layer

- reject empty output
- reject output with changed URLs, emails, or numbers
- flag outputs with high edit ratio
- keep grammar mode conservative

## Non-goals

- full Raycast clone
- plugin marketplace
- autonomous agent routing
- hosted-model fallback in the default design
