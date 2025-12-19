# Secondary-Structure-Prediction

A simple implementation of the Chou–Fasman algorithm for protein secondary-structure prediction.

---

## 🔧 Overview

This repository contains a small Python implementation (file: `choufasman.python`) of the Chou–Fasman rules to predict alpha-helices (H) and beta-strands (S) from a single-sequence of amino-acids. The program identifies nucleation sites, extends them, and resolves conflicts where helix and strand predictions overlap.

## ✅ Features

- Uses standard Chou–Fasman propensity tables for residues (Pa for helices and Pb for strands).
- Detects helix nucleation windows (6 residues, ≥4 with Pa ≥ 1) and strand nucleation windows (5 residues, ≥3 with Pb ≥ 1).
- Extends predicted regions using sliding 4-residue average threshold (average > 1).
- Resolves overlapping helix/strand segments by comparing summed propensities.

## 🧪 Requirements

- Python 3.x

> Note: the main file is named `choufasman.python`. To import it as a module you may want to rename it to `choufasman.py` first, or run it directly as a script.

## ▶️ Usage (interactive)

Run the script and paste/type a single-line amino-acid sequence (single-letter uppercase codes):

```bash
python choufasman.python
# or
python choufasman.py  # if you rename the file
```

The script will prompt:

```
Enter your sample in normal text (Not FastQ):
```

It prints:
- Predicted helices (start-end: sequence)
- Predicted strands (start-end: sequence)
- Final secondary structure string where H = helix, S = strand, - = neither

## 🧾 Example

If you use the included long example sequence (commented inside the file), the program prints detected helix/strand regions and a final structure map like:

```
Predicted helices:
12-30: LQQLDTRYLEQLHQLYS

Predicted strands:
45-50: VQIAC

Final Secondary Structure Prediction[H: Helix, S:Strand , -: Neither Helix Nor Strand]:
---HHHHHHHHHHHHH----SSS------
```

(Exact numbers and output depend on the input sequence.)

## Implementation details

- Propensities are defined in two dictionaries: `Pa` (alpha-helix) and `Pb` (beta-strand).
- Helix nucleation: window size 6; success if at least 4 residues have Pa >= 1.
- Strand nucleation: window size 5; success if at least 3 residues have Pb >= 1.
- Extension rule for both uses a sliding 4-residue window with average propensity > 1.
- Conflicts (overlapping H and S): contiguous overlapping residues are assigned to the structure (H or S) whose summed propensity across that contiguous segment is greater.

## API / Functions (in `choufasman.python`)

- `chel(window)` — checks a 6-residue window for helix nucleation (returns 1 or 0).
- `cstr(window)` — checks a 5-residue window for strand nucleation (returns 1 or 0).
- `hel(sequence)` — returns list of helix regions as `[start, end]` (0-based indices, end exclusive).
- `strand(sequence)` — returns list of strand regions as `[start, end]` (0-based indices, end exclusive).
- `resolve_conflicts(sequence, hel_list, strand_list)` — prints conflicting segments and returns the final combined structure string.

Other notes:
- Input is expected to be a string of single-letter amino-acid codes (uppercase). Non-standard characters or lower-case letters may cause KeyError or unexpected behavior.
- The script is small and intended for educational/demonstration use — it is not optimized for performance or large-scale batch predictions.


Happy predicting! 🔬
