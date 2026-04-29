# Estimate Engineering Team

Agent Team for preparing apartment renovation estimates from project PDF files.

The project is intentionally scoped to one workflow:

1. Read vector PDF design documentation.
2. Extract and normalize construction quantities.
3. Prepare works and rough-materials estimate drafts.
4. Keep unit prices empty for manual contractor coordination.
5. Build an Excel workbook with formulas and traceable sources.
6. Audit the estimate for missing quantities, formula issues, and double counting.

Main entry point:

```text
router-estimator
```

Project instructions are in `CLAUDE.md`.
