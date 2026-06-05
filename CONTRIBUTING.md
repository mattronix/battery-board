# Contributing

Improvements are welcome — this is an open hardware project and the design files
are public so they can be studied, fixed and built on.

## How to propose a change

1. **Open an issue first** for anything non‑trivial (a footprint change, a new
   revision, a BOM swap) so it can be discussed before you spend time on it.
   Small fixes (typos, BOM part numbers, footprint drill tweaks) can go straight
   to a pull request.
2. **Fork / branch**, make the change in KiCad 10, and keep edits focused — one
   logical change per pull request.
3. **Describe what and why.** Include the board affected (*Battery Top* /
   *Radio Bottom*), and for electrical changes a note on the reasoning (e.g.
   "shunt changed 5 mΩ → 2 mΩ to extend current range").
4. **Re‑export fabrication output** if you changed a PCB — run the Fabrication
   Toolkit plugin so `production/` (gerbers, BOM, positions) stays in sync with
   the design, and mention it in the PR.

## Guidelines

- **Keep the custom library portable.** If you add a footprint, put it in
  `Kicad Footprint Libaries/Radio Library.pretty/` and reference it as
  `Radio Library:<name>`. Don't introduce new absolute paths — prefer a path
  variable so the project stays buildable on other machines (see the README).
- **Don't commit churn.** Editor backups (`.history/`) and KiCad lock files
  (`~*.lck`) shouldn't be part of a change.
- **Mechanical contacts matter.** Pogo‑pin and bolt footprints have drill sizes
  matched to physical parts — if you change one, say which part it now fits.

## Licensing of contributions

By submitting a contribution you agree it is licensed under the project's
**CERN‑OHL‑W v2** licence (see [`LICENSE`](LICENSE)). Because this is a *weakly
reciprocal* licence, if you distribute modified versions of these boards you must
make your modified design files available under the same licence.
