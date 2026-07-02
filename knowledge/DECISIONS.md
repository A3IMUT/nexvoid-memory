# Nexvoid Decisions Log

Status: ACTIVE

---

## D-001 — Nexvoid Positioning

Date: 2026-06

Decision:
Nexvoid определяется как AI Orchestration Operating System, а не Telegram-бот, набор workflow или AI-ассистент.

Reason:
Такое позиционирование позволяет строить долгосрочную экосистему агентов, автоматизаций и сервисов.

Impact:
Все новые компоненты рассматриваются как части единой операционной системы.

Status:
ACTIVE

---

## D-002 — GitHub как первичная память проекта

Date: 2026-06

Decision:
Основная долговременная память хранится в GitHub Markdown-файлах.

Reason:
Прозрачность, версионность, резервирование и простота работы через AI-агентов.

Impact:
Все знания проекта должны фиксироваться через систему knowledge/.

Status:
ACTIVE

---

## D-003 — Commander является координатором

Date: 2026-06

Decision:
Nexvoid Commander выполняет роль координатора и маршрутизатора задач.

Reason:
Экспертные функции постепенно выносятся в отдельных специализированных агентов.

Impact:
Commander отвечает за маршрутизацию, память и координацию.

Status:
ACTIVE

---

## D-004 — Архетипы становятся будущими AI-агентами

Date: 2026-06

Decision:
Персонажи Nexvoid рассматриваются как будущие функциональные AI-агенты.

Agents: Signal Watcher, Workflow Hunter, Void Explorer, Automation Architect, Content Synthesist, Digital Guardian.

Reason:
Лор проекта должен отражать реальную архитектуру системы.

Impact:
Каждый новый агент должен иметь понятную бизнес-функцию.

Status:
ACTIVE

---

## D-005 — Безопасность важнее автоматизации

Date: 2026-06

Decision:
Любые изменения инфраструктуры требуют предварительного анализа.

Rules: Report First, Wait For Approval, Never Modify Core Automatically.

Status:
ACTIVE

---

## D-006 — Разделение Core и Experimental

Date: 2026-06

Decision:
Все workflow классифицируются по уровням критичности: CORE, PRODUCTION, EXPERIMENTAL, TEST, OBSOLETE.

Impact:
Критические workflow запрещено изменять без одобрения.

Status:
ACTIVE

---

## D-007 — AI Media Factory становится главным продуктом

Date: 2026-06

Decision:
AI Media Factory выбран основным направлением развития Nexvoid — строит активы, создаёт контент, привлекает аудиторию, тестирует агентов, генерирует трафик.

Status:
ACTIVE

---

## D-008 — Монетизация важнее экспериментов

Date: 2026-06

Decision:
Приоритет: 1. Доход, 2. Автоматизация, 3. Масштабирование, 4. Исследования.

Status:
ACTIVE

---

## D-009 — Минимизация ручных продаж

Date: 2026-06

Decision:
Основные каналы: SEO, Telegram, Ghost, Dzen, партнёрские программы, контент-маркетинг, лидогенерационные сайты. Система должна привлекать клиентов автоматически.

Status:
ACTIVE

---

## D-010 — Сначала активы, потом SaaS

Date: 2026-06

Decision:
Этапы: 1. Контентные активы, 2. Трафик, 3. Лиды, 4. Автоматизированные услуги, 5. SaaS.

Status:
ACTIVE

---

## D-011 — MCP-агенты работают в режиме Observer

Date: 2026-06

Decision:
Внешние MCP-агенты: Read/Audit/Inspect — разрешено. Production Changes/Deletion/Autonomous Modification — запрещено. Default mode: OBSERVER.

Status:
ACTIVE

---

## D-012 — Nexvoid строится как сеть систем

Date: 2026-06

Decision:
Проект развивается как экосистема взаимосвязанных компонентов: Commander, Memory Service, AI Media Factory, GitHub Knowledge Base, MCP Layer.

Status:
ACTIVE

---

## D-013 — Memory Record Schema v1

Date: 2026-06

Decision:
Введена формальная схема записи для knowledge/IDEAS.md. Обязательные поля: Date, Title, Description. Расширенные v1.0: ID, Source, Type, Status. GitHub PAT перенесён в n8n Credentials (httpHeaderAuth). Полное описание — knowledge/MEMORY_SCHEMA.md (CORE).

Impact:
Любой агент/workflow, пишущий в knowledge/IDEAS.md, обязан соответствовать MEMORY_SCHEMA.md v1.0.

Status:
ACTIVE

---

## D-014 — Approval Layer для MCP-действий на CORE/PRODUCTION workflow

Date: 2026-06

Decision:
Любое MCP-действие, изменяющее workflow классов CORE или PRODUCTION, требует чек-листа ДО выполнения:
1. Назвать workflow и классификацию.
2. Описать, что меняется и зачем.
3. Назвать риск.
4. Получить явное подтверждение оператора.
5. Только после — выполнять.

Read-only действия (get_workflow_details, search_workflows, get_execution, test_workflow с pin data) — чек-лист не требуется.

Status:
ACTIVE

---

## D-015 — Task Registry (async pattern) в Nexvoid_Memory_Service

Date: 2026-06-25

Decision:
Добавлены действия create_task и update_task_status в Nexvoid_Memory_Service. Postgres подключён к n8n впервые (credential Nexvoid Postgres, контейнер nexvoid-postgres, БД nexvoid_db), создана таблица tasks (текущий статус, без журнала истории — осознанное упрощение).

Архитектура: Commander — единственная точка контакта с пользователем. Агенты не пишут в Telegram напрямую — пишут статус в tasks через update_task_status, потом callback в Commander (notify_user, пока не реализован).

Reason:
Нужен паттерн "принято → передано агенту → выполнено" с асинхронной обработкой без блокировки.

Status:
ACTIVE

---

## D-016 — Внешний проект "Городские мастера" на общем VPS

Date: 2026-06-25

Decision:
На той же VPS развёрнут изолированный проект "Городские мастера" (Flutter-маркетплейс). НЕ часть Nexvoid.

Изоляция: Docker-сеть gormaster-net, БД gormaster-db (Postgres 16, gormaster_prod), n8n gormaster-n8n (порт 5680). Домены: n8n.gormaster.nexvoid.ru, api.gormaster.nexvoid.ru. Секреты через *_FILE паттерн.

Status:
ACTIVE

---

## D-017 — HTTP credential binding требует ручной привязки в UI после каждого update_workflow

Date: 2026-06-28

Decision:
Зафиксировано как архитектурное ограничение: инструмент update_workflow (n8n MCP SDK) не сохраняет привязку credentials к HTTP Request нодам при программном обновлении workflow. После каждого update_workflow или create_workflow_from_code, добавляющего новые HTTP Request ноды, оператор обязан вручную привязать нужный credential в n8n UI.

Это не баг, требующий workaround — это известное ограничение SDK, которое необходимо учитывать в процессе разработки.

Исключение по ответственности: правило "за GitHub отвечает только Claude через Memory Service" (D-018) содержит исключение — ручная привязка credentials/секретов в UI остаётся за оператором, поскольку Claude не имеет доступа к реальным значениям credentials (SDK возвращает только метаданные: id, имя, тип).

Reason:
Ситуация неоднократно возникала в ходе разработки (при добавлении delete_file и create_file в Memory Service). Без явной записи каждая новая сессия будет повторно обнаруживать это ограничение методом проб и ошибок, теряя время и генерируя лишний шум в error-workflow.

Impact:
При планировании изменений CORE workflow через update_workflow — всегда закладывать шаг "ручная привязка credentials в UI" как часть процесса, а не как неожиданность.

Список нод Memory Service (BmlFZkYzamGDvgoB), требующих ручной привязки GitHub PAT после каждого update:
Get IDEAS.md (for save), Commit Idea to GitHub, Get IDEAS.md (search), Get IDEAS.md (recent), Get IDEAS.md (stats), Fetch File from GitHub, Fetch File for Update, Commit File Update, Fetch File for Delete, Commit File Delete, Commit File Create.

Status:
ACTIVE

---

## D-018 — Единственный канал записи в GitHub — через Memory Service

Date: 2026-06-28

Decision:
Репозиторий A3IMUT/nexvoid-memory является единственным местом хранения знаний Nexvoid. Ответственность за содержимое репозитория закреплена за Claude (AI-архитектором). Единственный разрешённый канал записи/изменения/удаления файлов в репозитории — через Nexvoid_Memory_Service (actions: create_file, update_file, delete_file, save_idea). Прямые коммиты в репозиторий в обход Memory Service не рекомендуются.

Исключение: ручная привязка credentials/секретов в n8n UI остаётся за оператором (см. D-017) — это единственный вид действия, требующего доступа человека к инфраструктуре для поддержания работы канала записи.

Reason:
Централизованный канал гарантирует: единый credential (GitHub PAT) хранится только в n8n Credentials и не дублируется в других местах; все операции с репозиторием проходят через один аудируемый путь; будущие агенты (Signal Watcher, Content Synthesist и т.д.) не получают прямого доступа к GitHub — только через Memory Service API.

Impact:
Любой агент или workflow, которому нужно записать/прочитать/удалить файл в репозитории, вызывает Nexvoid_Memory_Service через executeWorkflow с соответствующим action. Прямые HTTP-вызовы к GitHub API из других workflow допустимы только для чтения (GET) и только в диагностических/временных (TEST_) workflow, которые удаляются после использования.

Status:
ACTIVE

---

END OF DOCUMENT