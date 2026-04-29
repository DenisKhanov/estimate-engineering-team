---
name: apartment-estimate-works
description: "Use this skill whenever the user needs the works tab of an apartment renovation estimate, a list of construction work items from extracted PDF quantities, or a Russian renovation локальная смета without unit prices."
---

# Apartment Estimate Works

Convert normalized quantities into `agent-runtime/shared/works-draft.json`.

Default sections: preparation/demolition, partitions, GKL and embedded items, ceilings, floors, waterproofing, wall rough finish, wall finish, ceiling finish, floor coverings, doors, plumbing, electrical, lighting, low-current, additional works.

Each row must contain section, work name, unit, quantity or manual-check flag, quantity formula text, empty unit price, total formula placeholder, source note, linked quantity ids, confidence and optional norm reference.

Do not add prices. Do not add materials. Keep apartment-scale rows practical and traceable.
