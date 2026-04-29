# Apartment Renovation Estimate Agent Team

Проект предназначен только для подготовки строительных смет ремонта квартир по проектным PDF.

## Основной сценарий

Пользователь передает один или несколько PDF проекта квартиры. Команда извлекает объемы, формирует работы и материалы, собирает Excel-смету без цен и проверяет результат.

## Запуск

```bash
tmux new -s estimate-team
claude
```

`.claude/settings.json` должен содержать `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` и `teammateMode: tmux`.

## Точка входа

Главный управляющий агент:

```text
router-estimator
```

Пример задачи:

```text
Создай Agent Team с router-estimator.
Задание: "По PDF проекта квартиры составь Excel-смету работ и черновых материалов без цен"
```

## Агенты

| # | Агент | Назначение | Модель |
|---|---|---|---|
| 1 | `router-estimator` | Управляет сметным pipeline и финальной выдачей | opus |
| 2 | `pdf-takeoff-agent` | Извлекает листы проекта и первичные объемы из векторных PDF | opus |
| 3 | `quantity-surveyor-agent` | Проверяет и нормализует объемы | opus |
| 4 | `works-estimator-agent` | Формирует вкладку `Работы` без цен | opus |
| 5 | `materials-estimator-agent` | Формирует вкладку `Материалы` без цен | opus |
| 6 | `norms-research-agent` | Проверяет нормативную логику ГЭСН/ФЕР и расходные нормы | opus |
| 7 | `xlsx-builder-agent` | Собирает итоговый Excel | sonnet |
| 8 | `estimate-auditor-agent` | Проверяет формулы, источники объемов, пропуски и двойной счет | opus |

## Цепочка

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

## Skills

Доменные skills проекта:

- `.agents/skills/apartment-project-pdf-takeoff/`
- `.agents/skills/apartment-estimate-works/`
- `.agents/skills/apartment-estimate-materials/`
- `.agents/skills/gesn-fer-renovation-norms/`

Внешние зависимости skills: `pdf`, `xlsx`. Они зафиксированы в `skills-lock.json`. Прочие универсальные skills не используются.

## Runtime

```text
agent-runtime/shared/project-index.json
agent-runtime/shared/extracted-quantities.json
agent-runtime/shared/normalized-quantities.json
agent-runtime/shared/works-draft.json
agent-runtime/shared/materials-draft.json
agent-runtime/shared/norms-notes.md
agent-runtime/shared/audit-report.md
agent-runtime/outputs/final-estimate.xlsx
```

## Правила

- Цены агентами не заполняются.
- В Excel должны быть колонки `Цена за ед., руб.` и `Сумма, руб.`.
- Суммы и итоги должны быть формулами.
- Каждый объем должен иметь источник: PDF, страницу, лист проекта или явное допущение.
- Неподтвержденные объемы и нормы выносятся на проверку.
- ГЭСН/ФЕР используются для нормативной логики и проверки единиц/состава работ, а не для автоматического заполнения цен.
