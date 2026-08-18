# Shadesmar Data

Spoiler-tagged lore dataset for [Shadesmar Companion](https://github.com/shadesmar-companion/shadesmar-app) — a spoiler-free _The Stormlight Archive_ companion web app.

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

---

## License

**CC BY-NC-SA 4.0** — Attribution-NonCommercial-ShareAlike.

See [LICENSE](./LICENSE) for the full terms.

## Content policy

All entity descriptions in this dataset are **independently authored** from the Stormlight Archive books by Brandon Sanderson.

[Coppermind](https://coppermind.net) (CC BY-SA 4.0) is used as a factual reference for verification purposes only. **No Coppermind text is reproduced.**

See [ATTRIBUTION.md](./ATTRIBUTION.md) for the complete attribution.

---

## Repository structure

```
stormlight/
  characters/     YAML files — named characters
  locations/      YAML files — named places and regions
  magic/          YAML files — magic systems (Surgebinding, Fabrials, etc.)
  spren/          YAML files — named spren
  orders/         YAML files — the ten Knights Radiant orders
  organizations/  YAML files — guilds, houses, armies, factions
  glossary/       YAML files — terms, concepts, and in-world vocabulary
  events/         YAML files — major historical and in-series events
schema/           JSON Schema for entity validation (git submodule → shadesmar-schema)
```

Each YAML file represents one entity. The schema governing the format is defined in [shadesmar-schema](https://github.com/shadesmar-companion/shadesmar-schema) and is used by the build pipeline to validate all entities before they reach the app.

---

## Contributing

Read [CONTRIBUTING.md](./CONTRIBUTING.md) before opening a Pull Request.

The most important principle: **when in doubt about a spoiler threshold, use the later position.** The dataset protects readers from accidental spoilers — conservative tagging is always the right call.

---

## Fan-created

_Fan-created, inspired by Brandon Sanderson. Not affiliated with or endorsed by Dragonsteel Entertainment, LLC._

_"The Stormlight Archive" is a trademark of Dragonsteel Entertainment, LLC._
