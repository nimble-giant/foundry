# Nimble Foundry

The official [Ailloy](https://github.com/nimble-giant/ailloy) foundry index by [Nimble Giant](https://github.com/nimble-giant).

## What is a Foundry Index?

A foundry index is a YAML catalog of Ailloy molds (and other foundries) that enables SCM-agnostic discovery. Users can register this foundry with:

```bash
ailloy foundry add https://github.com/nimble-giant/foundry
```

Then search for molds across all registered foundries:

```bash
ailloy foundry search <query>
```

Molds from this official foundry are displayed with a **verified** badge in search results.

This foundry is also a built-in default — it appears in `ailloy foundry list` and is searched automatically even before you register any other foundries.

## Catalog

### Molds

| Name | Source | Description |
|------|--------|-------------|
| [nimble-mold](https://github.com/nimble-giant/nimble-mold) | External | Distributable, reusable, and agnostic AI instruction blanks for common SDLC workflows |
| [plugin-to-mold](https://github.com/kriscoleman/plugin-to-mold) | External | Conversion toolkit: skills and agent that automate converting Claude Code plugins to multi-target ailloy molds |
| [ghostwriter](./molds/ghostwriter) | Nested | Interactively interviews the user to capture their personal writing voice and emits a ready-to-install SKILL.md that teaches other AI agents to write in that voice |

Molds listed as **External** live in their own repositories. Molds listed as **Nested** live inside this repository under `molds/<name>` and are released independently via [release-please](https://github.com/googleapis/release-please).

### Ingots

Ingots are reusable template partials (chunks of blank content) that molds include via `{{ingot "name"}}`. They can ship inside a mold or as standalone packages.

| Name | Location | Description |
|------|----------|-------------|
| _none yet_ | | |

### Ores

Ores are reusable flux partials — structured data shapes (with an `enabled` toggle) that molds opt into. They can ship inside a mold's flux schema or as standalone packages installed via `ailloy ore add`.

| Name | Location | Description |
|------|----------|-------------|
| _none yet_ | | |

### Nested Foundries

This foundry can transitively aggregate other foundries via the top-level `foundries:` block in `foundry.yaml`. When a parent foundry is fetched, every child foundry it lists is resolved transitively and all of its molds become discoverable through the parent.

| Name | Source | Description |
|------|--------|-------------|
| _none yet_ | | |

## Contributing

Pick the path that matches what you're contributing:

| You want to contribute… | Path |
|-------------------------|------|
| Your entire foundry (so all its molds flow into this one) | [Your Own Foundry](#contributing-your-own-foundry) |
| A mold in its own repo | [External Mold](#contributing-an-external-mold) |
| A mold released from this repo | [Nested Mold](#contributing-a-nested-mold) |
| A standalone ingot | [Standalone Ingot](#contributing-a-standalone-ingot) |
| A standalone ore | [Standalone Ore](#contributing-a-standalone-ore) |

Ingots and ores can also live *inside* a mold rather than as standalone packages — see [Nested Mold](#contributing-a-nested-mold) for that.

### Contributing Your Own Foundry

If you maintain your own foundry with multiple molds, contributing it as a nested foundry is preferable to per-mold entries: every mold you add to your own foundry afterwards flows into this one automatically.

Open a pull request that adds an entry to `foundry.yaml` under `foundries:`:

```yaml
foundries:
  - name: your-foundry
    source: github.com/your-org/your-foundry
    description: "One-line description of your foundry"
```

Then add a corresponding row to the **Nested Foundries** table above. Your foundry's `name:` must be unique across the resolved tree — collisions are fatal.

### Contributing an External Mold

If your mold lives in its own repository, open a pull request that adds an entry to `foundry.yaml`:

```yaml
molds:
  - name: your-mold
    source: github.com/your-org/your-mold
    description: "One-line description of what your mold does"
    tags: ["ailloy-mold", "relevant", "tags"]
```

Then add a corresponding row to the **Molds** table above.

### Contributing a Nested Mold

Nested molds live under `molds/<name>` and are released from this repository. To add one:

1. Create `molds/<name>/` with at minimum a `mold.yaml` (see [ghostwriter](./molds/ghostwriter/mold.yaml) for an example).
2. Add an entry to `foundry.yaml` with `source: github.com/nimble-giant/foundry//molds/<name>`.
3. Add a row to the **Molds** table in this README.
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

**Ingots and ores inside your mold.** A nested mold can ship its own ingots under `molds/<name>/ingots/<ingot-name>/` and define ores in its `flux.schema.yaml` (grouped under a `# --- Ore Models ---` header). These travel with the mold and don't need separate release entries — they version with the mold itself.

### Contributing a Standalone Ingot

Standalone ingots live at the repo root under `ingots/<name>/` and are released independently. Each one is a directory with an `ingot.yaml` manifest and its content files:

```
ingots/<name>/
├── ingot.yaml
└── content.md
```

```yaml
# ingot.yaml
apiVersion: v1
kind: ingot
name: <name>
version: 0.1.0
description: "One-line description"
files:
  - content.md
```

To add one:

1. Create `ingots/<name>/` with the layout above.
2. Add a row to the **Ingots** table in this README pointing at `./ingots/<name>`.
3. Register the ingot in `release-please-config.json`:

   ```json
   "ingots/<name>": {
     "component": "<name>",
     "release-type": "simple",
     "extra-files": [
       { "type": "yaml", "path": "ingot.yaml", "jsonpath": "$.version" }
     ]
   }
   ```

The whole repo is itself a valid multi-ingot package per Ailloy's multi-ingot layout: `ailloy ingot add github.com/nimble-giant/foundry` installs every ingot under `ingots/`. To install just one, append a subpath: `ailloy ingot add github.com/nimble-giant/foundry//ingots/<name>`.

### Contributing a Standalone Ore

Standalone ores live at the repo root under `ores/<name>/` and are released independently. Each one is a directory with three files:

```
ores/<name>/
├── ore.yaml
├── flux.schema.yaml
└── flux.yaml
```

```yaml
# ore.yaml
apiVersion: v1
kind: ore
name: <name>            # claimed namespace: ore.<name>.*
version: 0.1.0
description: "One-line description"
```

`flux.schema.yaml` entries are **unprefixed** (ailloy prepends `ore.<name>.`), and your schema must include an `enabled: bool` entry. See the [Ailloy ore guide](https://github.com/nimble-giant/ailloy/blob/main/docs/ore.md) for the full authoring contract.

To add one:

1. Scaffold with `ailloy ore new ores/<name>` (or hand-create the layout above).
2. Add a row to the **Ores** table in this README pointing at `./ores/<name>`.
3. Register the ore in `release-please-config.json`:

   ```json
   "ores/<name>": {
     "component": "<name>",
     "release-type": "simple",
     "extra-files": [
       { "type": "yaml", "path": "ore.yaml", "jsonpath": "$.version" }
     ]
   }
   ```

Consumers install with `ailloy ore add github.com/nimble-giant/foundry//ores/<name>`.

### Validating Your Contribution

Before opening a PR, validate locally:

```bash
# Validate foundry.yaml — schema + every mold source + every nested foundry
ailloy foundry temper

# Schema check only — no network
ailloy foundry temper --offline

# Validate a nested mold, ingot, or ore directory
ailloy temper ./molds/<name>
ailloy temper ./ingots/<name>
ailloy temper ./ores/<name>
```

CI runs `ailloy foundry temper` on every PR that touches `foundry.yaml`.

### Schema Reference

See the [Ailloy foundry documentation](https://github.com/nimble-giant/ailloy/blob/main/docs/foundry.md) for the full `foundry.yaml` schema, and the [ingot](https://github.com/nimble-giant/ailloy/blob/main/docs/ingots.md) and [ore](https://github.com/nimble-giant/ailloy/blob/main/docs/ore.md) guides for package authoring details.
