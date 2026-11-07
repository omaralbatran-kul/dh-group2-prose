# Prose dataset cleaning — Group 2 (Introduction to Digital Humanities, KU Leuven 2026–2027)

This repository holds the cleaning of the **Prose** part of the British Library "Microsoft Books"
metadata dataset, done in OpenRefine. It is part of the Group 2 project (Omar & Bert).

- **Source:** British Library, *Microsoft Books* digitised books metadata — https://doi.org/10.23636/1140
- **Input:** `data/raw/MicrosoftBooks-Prose.xlsx`. It has 8 sheets, but only the main `Prose` sheet
  (10,532 records) will be used: the subgenre sheets are subsets of it.

## Structure
| Path | Contents |
|---|---|
| `data/raw/` | Original file as downloaded. **Never modified.** |
| `data/interim/` | Versions exported from OpenRefine at each cleaning stage |
| `data/clean/` | Final cleaned Prose dataset, ready to merge |
| `openrefine/` | Exported OpenRefine operation histories (Undo/Redo → Extract → JSON) |

## Reproducing
1. Create an OpenRefine project from `data/raw/MicrosoftBooks-Prose.xlsx`
2. Undo/Redo → Apply → paste the JSON files from `openrefine/` in order.
