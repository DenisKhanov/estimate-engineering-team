---
name: apartment-project-pdf-takeoff
description: "Use this skill whenever the user asks to prepare a renovation estimate from apartment design PDF files, extract quantities from vector PDF drawings, analyze apartment project sheets, or perform construction takeoff for partitions, ceilings, lighting, electrical, plumbing, furniture plans, doors, finishes, or room schedules."
---

# Apartment Project PDF Takeoff

Turn vector apartment design PDFs into `agent-runtime/shared/extracted-quantities.json` and `takeoff-summary.md`.

Classify pages: visualizations, partition mounting plan, furniture layout, plumbing layout, lighting layout, electrical layout, ceiling scheme, room schedules, doors, finishes.

Extract categories: rooms, partitions, doors, ceilings, floors, walls, lighting, electrical, plumbing, built-in furniture, demolition if present.

Each quantity must include id, category, item, unit, quantity, formula, location, source file/page/sheet/evidence, confidence and manual-check flag. Use high confidence for schedules or explicit dimensions, medium for reliable calculations, low for inference. Low-confidence items require manual check.

Do not create the estimate and do not fill prices.
