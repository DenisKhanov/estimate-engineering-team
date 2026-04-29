---
name: apartment-estimate-materials
description: "Use this skill whenever the user needs the materials tab of an apartment renovation estimate, material quantities from renovation work volumes, расход материалов, черновые материалы, запас/отходы, or conversion of works into a Russian Excel materials estimate."
---

# Apartment Estimate Materials

Convert work quantities into `agent-runtime/shared/materials-draft.json`.

Include rough materials by default: protection, masonry, GKL systems, floor mixes, waterproofing, primers, plaster/putty/paint, tile consumables, electrical rough-in, plumbing rough-in.

Do not include visible finish products unless requested. Do not choose suppliers or prices.

Every material row must include material name, unit, consumption norm, base work quantity, calculated quantity, rounding rule, waste factor, empty unit price, total formula placeholder, linked work ids and source note. Round purchasable units up and explain waste.
