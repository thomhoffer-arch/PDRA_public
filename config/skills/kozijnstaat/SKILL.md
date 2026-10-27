---
name: kozijnstaat
description: Produce a Dutch kozijnstaat (window/door/curtain-wall schedule + one sheet per kozijn type with elevation, plan-section, vertical-section, schedule and dimensions). Use when the user asks for a kozijnstaat, a window/door schedule deliverable, or "make a sheet set for the kozijnen".
allowed_tools:
  - pdra_list_project_context
  - pdra_read_text_file
  - pdra_list_doors_windows_curtain_walls
  - pdra_create_kozijnstaat_sections
  - pdra_kozijnstaat_engineer_set
  - pdra_dimension_kozijnstaat_sheet
  - pdra_clone_sheet_for
  - pdra_export_view_image
done_criteria:
  - Every kozijnstaat sheet renders via pdra_export_view_image with all three views (Aanzicht, Plattegrond, Doorsnede) showing the wall geometry and no geometry clipped by a crop.
  - Dimensions and spot elevations are present on the Aanzicht and read against the geometry, not floating in empty space.
  - When cloning from a reference sheet, the clone is rendered and visually matches the reference (same view settings, crop sizing, alignment) — no known difference is left unfixed.
---

# Kozijnstaat

A kozijnstaat is a per-type door/window/curtain-wall sheet deliverable: one sheet per
kozijn type carrying a schedule, an elevation (**Aanzicht**), a horizontal section
(**Plattegrond**), a vertical section (**Doorsnede**), plus dimensions and spot elevations.

This skill decides *which* tools to run and *how to verify* the result. The tools compute
the geometry — never hand-compute coordinates, crops, or transforms.

## When to use
The user wants a kozijnstaat / window-door schedule sheet set, or asks to reproduce an
existing kozijnstaat sheet for another type ("make a K1.02 sheet like K1.01").

## Assumptions & inputs
- The model is the source of truth for which marks/types exist — never invent marks.
- The office naming, dimension and browser-folder conventions live in project context.
  Read them first; do not guess names or dimension styles.

## Solution strategy
1. **Load standards.** `pdra_list_project_context`, then `pdra_read_text_file` on the
   naming / dimension files before creating or naming anything.
2. **Inventory.** `pdra_list_doors_windows_curtain_walls` to see which marks need a sheet.
3. **Build the set.** Prefer the vetted composite `pdra_kozijnstaat_engineer_set` — it runs
   the create-sections → template → sheet → place → schedule sequence deterministically.
   (`pdra_create_kozijnstaat_sections` is the per-wall primitive if you need one type only.)
4. **Dimension.** `pdra_dimension_kozijnstaat_sheet` applies the Aanzicht/Plattegrond/
   Doorsnede dimension recipe. Pass references (wall/mark), never paper coordinates.
5. **Reproduce a reference exactly.** To clone an existing sheet for another type, use
   `pdra_clone_sheet_for` — it copies view settings and sizes/aligns by geometry. Reproduce
   the reference *exactly*; never leave a known difference and ask the user to fix it.

See [references/standards.md](references/standards.md) for the two-path detail (annotated
vs assembly) and the dimension recipe specifics.

## Verification recipe (mandatory)
Do not report the deliverable done until every `done_criteria` item above is satisfied.
Concretely: `pdra_export_view_image` each finished sheet (and, when cloning, the reference
too) and compare against the criteria. Coordinate numbers are not ground truth — the
rendered image is. If a render shows clipped geometry, fix the *content* scope first; only
widen the crop if geometry is genuinely excluded by it.
