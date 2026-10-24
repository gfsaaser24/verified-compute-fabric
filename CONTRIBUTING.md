# Changing Verified Compute Fabric

This repository is the source of truth for the concept.
If something is not here, it is not part of Verified Compute Fabric.

## What lives where

- **Text:** `spec/*.md`, one file per section. The number in the file name is the section number.
- **Sources:** `references/references.yaml`.
- **Figures:** `figures/*.svg`, referenced from the text.
- **PDFs:** `releases/` holds copies of published versions. The Markdown is the source; a PDF is never the place to make a change.

## Proposing a change

1. For anything beyond wording, open an issue first. Say what should change, why, and which sections or requirements it touches.
2. Make the change on a branch and open a pull request. Fill in the template.
3. The owner reviews and merges.

## Rules for content

- **Cite primary sources.** Every claim about a real system, incident, standard or date needs an entry in `references.yaml`: the publisher's page, the RFC, the advisory or the paper. Use news coverage only when nothing primary exists. Set `checked` to the date you verified the entry.
- **Label models.** Numbers that come from stated parameters rather than measurements are marked *illustrative*.
- **Keep identifiers stable.** Requirement IDs (R-01 to R-17), test names and section ids are never reused or renumbered. Retire an item by marking it withdrawn.
- **Use MUST and SHOULD** only for requirements of this architecture.
- **Write plainly.** Short sentences and everyday words. Put one sentence per line in the Markdown, so reviews and diffs point at single sentences.
- **Keep summaries in step.** The README and the executive brief (`spec/00-brief.md`) must agree with the sections they summarise.

## Markdown conventions

| Write | Meaning |
|---|---|
| `[@key]` or `[@a; @b]` | Citation of entries in `references/references.yaml` |
| `[§15](15-revocation.md)` | Cross-reference to a section |
| `[Figure 7](../figures/revtimeline.svg)` | Reference to a figure |
| ``$`T_{\text{stop}}`$`` and fenced `math` blocks | Math, rendered by GitHub |
| `> [!NOTE]` or `> [!WARNING]` | A callout |

If you renumber a section or figure, update every link that names it.

## Versions

Versions use MAJOR.MINOR.PATCH, applied to the concept:

- **Major** (3.0): changes a design rule, what a requirement means, a profile's trust assumptions, or a bound.
- **Minor** (2.1): adds a section, requirement, test or source without changing what existing ones mean.
- **Patch** (2.0.1): wording, typos, broken links, figure fixes.

To release a version:

1. Add an entry to `CHANGELOG.md`. For a major version, also update Appendix B.
2. If a PDF is published, add a copy to `releases/vX.Y/`.
3. Commit, then tag the commit `vX.Y`.
