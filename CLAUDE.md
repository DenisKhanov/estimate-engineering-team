# Apartment Renovation Estimate Subagents

Этот проект предназначен только для подготовки строительных смет ремонта квартир по проектным PDF.

## Режим работы

Проект использует стандартные Claude Code subagents:

- subagents лежат в `.claude/agents/`;
- project skills лежат в `.claude/skills/`;
- главный агент `router-estimator` управляет специализированными subagents.

Отдельный командный режим с несколькими равноправными участниками в сценарии работы не используем. Пользователь обращается к `router-estimator`, а он сам делегирует этапы нужным subagents.

## Главный агент

Точка входа:

```text
router-estimator
```

Пример запроса:

```text
Используй router-estimator.
По PDF проекта квартиры составь Excel-смету работ и черновых материалов без цен.
PDF лежат здесь: /path/to/project-pdfs
```

## Subagents

| Агент | Роль |
|---|---|
| `router-estimator` | Главный управляющий агент |
| `pdf-takeoff-agent` | Извлекает листы проекта и первичные объемы из PDF |
| `quantity-surveyor-agent` | Проверяет и нормализует объемы |
| `works-estimator-agent` | Формирует черновик вкладки `Работы` |
| `materials-estimator-agent` | Формирует черновик вкладки `Материалы` |
| `norms-research-agent` | Проверяет нормативную логику ГЭСН/ФЕР и нормы расхода |
| `xlsx-builder-agent` | Собирает итоговый Excel |
| `estimate-auditor-agent` | Проверяет смету перед выдачей |

## Последовательность

```text
router-estimator
  -> pdf-takeoff-agent
  -> quantity-surveyor-agent
  -> works-estimator-agent
  -> materials-estimator-agent
  -> norms-research-agent
  -> xlsx-builder-agent
  -> estimate-auditor-agent
```

При `critical` замечаниях аудитора `router-estimator` возвращает задачу на нужный этап.

## Skills

Project skills:

```text
.claude/skills/apartment-project-pdf-takeoff/
.claude/skills/apartment-estimate-works/
.claude/skills/apartment-estimate-materials/
.claude/skills/gesn-fer-renovation-norms/
```

Внешние зависимости skills:

- `pdf`;
- `xlsx`.

Они зафиксированы в `skills-lock.json`.

## Runtime

Агенты передают результаты через `agent-runtime/`:

```text
agent-runtime/shared/project-index.json
agent-runtime/shared/extracted-quantities.json
agent-runtime/shared/takeoff-summary.md
agent-runtime/shared/normalized-quantities.json
agent-runtime/shared/quantity-checks.md
agent-runtime/shared/works-draft.json
agent-runtime/shared/materials-draft.json
agent-runtime/shared/norms-notes.md
agent-runtime/shared/audit-report.md
agent-runtime/outputs/final-estimate.xlsx
```

## Обязательные правила

- Цены агентами не заполняются.
- В Excel должны быть колонки `Цена за ед., руб.` и `Сумма, руб.`.
- Суммы и итоги должны быть формулами.
- Каждый объем должен иметь источник: PDF, страницу, лист проекта или явное допущение.
- Неподтвержденные объемы и нормы выносятся на проверку.
- ГЭСН/ФЕР используются для проверки логики, единиц и состава работ, а не для автоматического заполнения цен.
