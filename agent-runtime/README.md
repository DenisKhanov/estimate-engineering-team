# Agent Runtime

Рабочая директория сметного pipeline `router-estimator`.

- `shared/` — промежуточные данные между сметными агентами
- `outputs/` — финальные результаты, включая `final-estimate.xlsx`
- `state/` — статус выполнения pipeline
- `messages/` — handoff-сообщения между агентами

Ожидаемые файлы: `project-index.json`, `extracted-quantities.json`, `normalized-quantities.json`, `works-draft.json`, `materials-draft.json`, `norms-notes.md`, `audit-report.md`, `final-estimate.xlsx`, `pipeline-status.json`.
