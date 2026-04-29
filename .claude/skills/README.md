# Project Skills

В этой папке лежат project skills Claude Code, которые используются сметными subagents.

Активные skills:

- `apartment-project-pdf-takeoff` — извлечение объемов из проектных PDF;
- `apartment-estimate-works` — формирование черновика вкладки `Работы`;
- `apartment-estimate-materials` — формирование черновика вкладки `Материалы`;
- `gesn-fer-renovation-norms` — проверка нормативной логики ГЭСН/ФЕР и расходных норм.

Внешние зависимости `pdf` и `xlsx` не хранятся здесь как копии. Они зафиксированы в `skills-lock.json`.
