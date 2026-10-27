# Kozijnstaat — detail reference (loaded on demand)

This is the deeper layer of the kozijnstaat skill. Read it only when the SKILL.md body
points you here. For the full canonical sequences see `docs/KOZIJNSTAAT_WORKFLOW.md` in the
repo (developer-facing); this file is the model-facing summary.

## Two paths — pick deliberately

**Path A — annotated kozijnstaat (default).** Views are project-level `ViewSection`s, so
API-placed dimensions and spot elevations behave normally. Pick this whenever the
deliverable needs dimensions/spot elevations on the views.

**Path B — assembly kozijnstaat.** Views come from a Revit assembly. Use only when the
office specifically wants assembly views; assembly-view rendering has quirks for annotation.

When in doubt, Path A.

## Dimension recipe (what `pdra_dimension_kozijnstaat_sheet` encodes)
- **Aanzicht (elevation):** outer width + height chains; opening dimensions along the
  bottom; spot elevations on sill/head.
- **Plattegrond (plan section):** horizontal opening widths and reveal depths.
- **Doorsnede (vertical section):** vertical heights, sill and lintel.

All offsets/targets come from the per-category `dimension_standards.csv` in project context
— the tool reads them; do not pass paper-space millimetres by hand.

## Cloning a sheet for another type
`pdra_clone_sheet_for` reproduces a reference sheet for a new mark: it copies the
reference's view settings and sizes/aligns the new views by *geometry* (so the same world
feature lands at the same place on paper), and returns a manifest of what it did and what it
could not. Read the manifest; reproduce the reference exactly; render both and compare.
