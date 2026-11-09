# Prose dataset cleaning
> (Introduction to Digital Humanities, KU Leuven 2026–2027)

This repository holds the cleaning of the **Prose** part of the British Library "Microsoft Books"
metadata dataset, done in OpenRefine. It is part of the Group 2 project (Omar & Bert).

- **Source:** British Library, *Microsoft Books* digitised books metadata - https://doi.org/10.23636/1140
- **Input:** `data/raw/MicrosoftBooks-Prose.xlsx`. It has 8 sheets, but only the main `Prose` sheet
  (10,532 records) will be used: the subgenre sheets are subsets of it.

## Structure
| Path | Contents |
|---|---|
| `data/raw/` | Original file as downloaded. **Never modified.** |
| `data/interim/` | Versions exported from OpenRefine at each cleaning stage |
| `data/clean/` | Final cleaned Prose dataset, ready to merge |
| `openrefine/` | Exported OpenRefine operation histories (Undo/Redo → Extract → JSON) |

## Workflow (after every cleaning step)
1. Clear all facets/filters (OpenRefine only exports the rows you see)
2. Export → CSV → `data/interim/NN_step.csv`
3. Undo/Redo → Extract → only the new operations → `openrefine/NN_step.json`
4. Add a line to `NOTES.md`: what changed and how many cells
5. Commit

## Reproducing
1. Create an OpenRefine project from `data/raw/MicrosoftBooks-Prose.xlsx`
2. Undo/Redo → Apply → paste the JSON files from `openrefine/` in order.
