# Nimble Foundry

The official [Ailloy](https://github.com/nimble-giant/ailloy) foundry index by [Nimble Giant](https://github.com/nimble-giant).

## What is a Foundry Index?

A foundry index is a YAML catalog of Ailloy molds that enables SCM-agnostic mold discovery.
Users can register this foundry with:

```bash
ailloy foundry add https://github.com/nimble-giant/foundry
```

Then search for molds across all registered foundries:

```bash
ailloy foundry search <query>
```

Molds from this official foundry are displayed with a **verified** badge in search results.

## Molds

| Name | Source | Description |
|------|--------|-------------|
| [nimble-mold](https://github.com/nimble-giant/nimble-mold) | External | Distributable, reusable, and agnostic AI instruction blanks for common SDLC workflows |
| [plugin-to-mold](https://github.com/kriscoleman/plugin-to-mold) | External | Conversion toolkit: skills and agent that automate converting Claude Code plugins to multi-target ailloy molds |
| [ghostwriter](./molds/ghostwriter) | Nested | Interactively interviews the user to capture their personal writing voice and emits a ready-to-install SKILL.md that teaches other AI agents to write in that voice |

Molds listed as **External** live in their own repositories. Molds listed as **Nested** live inside this repository under `molds/<name>` and are released independently via [release-please](https://github.com/googleapis/release-please).

## Contributing

### Submitting an External Mold

If your mold lives in its own repository, open a pull request that adds an entry to `foundry.yaml`:

```yaml
molds:
  - name: your-mold
    source: github.com/your-org/your-mold
    description: "One-line description of what your mold does"
    tags: ["ailloy-mold", "relevant", "tags"]
```

Then add a corresponding row to the Molds table above.

### Submitting a Nested Mold

Nested molds live under `molds/<name>` and are released from this repository. To add one:

1. Create `molds/<name>/` with at minimum a `mold.yaml` (see [ghostwriter](./molds/ghostwriter/mold.yaml) for an example).
2. Add an entry to `foundry.yaml` with `source: github.com/nimble-giant/foundry//molds/<name>`.
3. Add a row to the Molds table in this README.
4. Register the mold in `release-please-config.json` so it gets its own versioned release stream:

   ```json
   "molds/<name>": {
     "component": "<name>",
     "release-type": "simple",
     "extra-files": [
       { "type": "yaml", "path": "mold.yaml", "jsonpath": "$.version" }
     ]
   }
   ```

Use [Conventional Commits](https://www.conventionalcommits.org/) (e.g. `feat(<name>): ...`, `fix(<name>): ...`) so release-please can generate changelogs and bump versions automatically.

### Schema Reference

See the [foundry documentation](https://github.com/nimble-giant/ailloy/blob/main/docs/foundry.md) for the full schema reference.
