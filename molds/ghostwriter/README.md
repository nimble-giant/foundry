# ghostwriter

An [Ailloy](https://github.com/nimble-giant/ailloy) mold that interactively interviews you to capture your personal writing voice — tone, rhythm, vocabulary, formatting, quirks — and emits a ready-to-install `SKILL.md` that teaches other AI agents to write in that voice.

## What it does

Most AI writing sounds like AI writing. Ghostwriter fixes that by giving you a reusable style asset, not a one-off prompt. Run it once; install the resulting skill anywhere an agent reads skills; future agents write like you.

The mold installs a single skill (`ghostwriter`) that runs a short, conversational interview in three phases:

1. **Interview** — 15-ish forced-choice questions across voice, stance, rhythm, vocabulary, formatting, and quirks. Batched 2–4 per turn, not one at a time.
2. **Optional sample analysis** — paste 1–3 short writing samples and the skill pulls out concrete patterns (sentence rhythms, signature moves, punctuation habits) and reflects them back for you to confirm.
3. **Generate** — produces a personalized `SKILL.md` you can drop into Claude (or any agent that reads skills) so it writes like you going forward.

The output is the artifact. The interview is the means.

## Install

```bash
ailloy mold install github.com/nimble-giant/foundry//molds/ghostwriter
```

Requires `ailloy >= 0.6.16`.

## Trigger phrases

The installed skill activates on signals like:

- "I want Claude / my agent to write like me"
- "Help me create a writing style skill"
- "My agent's comments / emails / posts don't sound like me"
- "Stop sounding like AI"
- "Capture my voice in a skill"

If you only want help writing *one* thing in your voice (a single email, a single post), this skill isn't the right tool — just ask for the writing directly. Ghostwriter is for building a *reusable* style asset.

## Source

This mold lives in the [nimble-giant/foundry](https://github.com/nimble-giant/foundry) repository under [`molds/ghostwriter`](https://github.com/nimble-giant/foundry/tree/main/molds/ghostwriter) and is released independently via [release-please](https://github.com/googleapis/release-please). See [CHANGELOG.md](./CHANGELOG.md) for release history.
