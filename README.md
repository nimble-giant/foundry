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

| Name | Description |
|------|-------------|
| [nimble-mold](https://github.com/nimble-giant/nimble-mold) | Distributable, reusable, and agnostic AI instruction blanks for common SDLC workflows |

## Adding Molds

To submit a mold for inclusion in the official index, open a pull request adding your mold to `foundry.yaml`.

See the [foundry documentation](https://github.com/nimble-giant/ailloy/blob/main/docs/foundry.md) for the full schema reference.
