## D-015 — Task Registry (async pattern) в Nexvoid_Memory_Service

Date: 2026-06-25

Decision:
Добавлены действия `create_task` и `update_task_status` в `Nexvoid_Memory_Service`. Postgres подключён к n8n впервые (credential `Nexvoid Postgres`, контейнер `nexvoid-postgres`, БД `nexvoid_db`), создана таблица `tasks` (текущий статус, без журнала истории — осознанное упрощение).

Архитектура: Commander остаётся единственной точкой контакта с пользователем. Агенты/сервисы не пишут в Telegram напрямую — пишут статус/результат в `tasks` через `update_task_status`, затем делают callback в Commander (`notify_user`, пока не реализован — следующий шаг).

Reason:
Нужен паттерн "принято -> передано агенту -> выполнено" с асинхронной обработкой без блокировки. Файлы (GitHub) не годятся для часто обновляемого статуса — sha-перезапись создаёт race condition; Postgres уже был в инфраструктуре, просто не подключён к n8n.

Impact:
Полная схема и SQL — в knowledge/TASK_REGISTRY.md (CORE).

Status:
ACTIVE

---

## D-016 — Внешний проект "Городские мастера" на общем VPS

Date: 2026-06-25

Decision:
На той же VPS, где работает Nexvoid, развёрнут полностью изолированный внешний проект "Городские мастера" (Flutter-маркетплейс, package ID ru.gormaster.app). Это НЕ часть экосистемы Nexvoid.

Изоляция: отдельная Docker-сеть gormaster-net, отдельная БД gormaster-db (Postgres 16, база gormaster_prod, юзер gormaster_admin), отдельный n8n gormaster-n8n (внутренний порт 5678, хостовый 5680, не публикуется напрямую). Домены: n8n.gormaster.nexvoid.ru (UI, owner-аккаунт создан) и api.gormaster.nexvoid.ru (единый /v1 gateway, action в body, rewrite на /webhook/gormaster-api, всё остальное — 404). Секреты — через POSTGRES_PASSWORD_FILE / N8N_ENCRYPTION_KEY_FILE (не -e, чтобы не светиться в docker inspect).

Reason:
Основатель ведёт два проекта параллельно на одной VPS. Полная инфраструктурная изоляция (сеть, БД, n8n, домены) предотвращает повторение путаницы, уже случившейся внутри Nexvoid (Nexvoid_Commander vs Nexvoid Commander_html — два воркфлоу с похожим именем и разной логикой).

Impact:
Полное описание инфраструктуры — в knowledge/EXTERNAL_PROJECTS.md. Контейнеры/сети/credentials с префиксом gormaster- никогда не пересекаются с nexvoid-*/n8n-tunneling/client-1-n8n-app. NEXVOID_CONSTITUTION_V2 §9 (Observer mode) применяется к агенту, работающему на VPS, в том же духе, что и к MCP-агентам внутри Nexvoid.

Status:
ACTIVE

---

DECISIONS.md
# Nexvoid Decisions Log

# DECISIONS

Status: ACTIVE

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

Agents:

* Signal Watcher
* Workflow Hunter
* Void Explorer
* Automation Architect
* Content Synthesist
* Digital Guardian

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

Reference:
NEXVOID_CONSTITUTION_V2.md

Rules:

* Report First
* Wait For Approval
* Never Modify Core Automatically

Status:
ACTIVE

---

## D-006 — Разделение Core и Experimental

Date: 2026-06

Decision:
Все workflow классифицируются по уровням критичности.

Classes:

* CORE
* EXPERIMENTAL
* TEST
* OBSOLETE

Impact:
Критические workflow запрещено изменять без одобрения.

Status:
ACTIVE

---

## D-007 — AI Media Factory становится главным продуктом

Date: 2026-06

Decision:
AI Media Factory выбран основным направлением развития Nexvoid.

Reason:
Позволяет одновременно:

* строить активы
* создавать контент
* привлекать аудиторию
* тестировать агентов
* генерировать трафик

Status:
ACTIVE

---

## D-008 — Монетизация важнее экспериментов

Date: 2026-06

Decision:
Приоритет разработки смещён с исследований на получение первых доходов.

Priority:

1. Доход
2. Автоматизация
3. Масштабирование
4. Исследования

Status:
ACTIVE

---

## D-009 — Минимизация ручных продаж

Date: 2026-06

Decision:
Основатель не строит бизнес через холодные продажи и постоянные переговоры.

Основные каналы:

* SEO
* Telegram
* Ghost
* Dzen
* Партнёрские программы
* Контент-маркетинг
* Лидогенерационные сайты

Reason:
Система должна привлекать клиентов автоматически.

Status:
ACTIVE

---

## D-010 — Сначала активы, потом SaaS

Date: 2026-06

Decision:
Развитие идёт по этапам:

1. Контентные активы
2. Трафик
3. Лиды
4. Автоматизированные услуги
5. SaaS

Reason:
Снижает риски и требования к бюджету.

Status:
ACTIVE

---

## D-011 — MiMo получает режим Observer

Date: 2026-06

Decision:
Внешние MCP-агенты работают только в режиме анализа.

Allowed:

* Read
* Audit
* Inspect

Forbidden:

* Production Changes
* Deletion
* Autonomous Modification

Reference:
NEXVOID_CONSTITUTION_V2.md

Status:
ACTIVE

---

## D-012 — Nexvoid строится как сеть систем

Date: 2026-06

Decision:
Проект развивается как экосистема взаимосвязанных компонентов.

Current Systems:

* Commander
* Memory Service
* AI Media Factory
* GitHub Knowledge Base
* MCP Layer

Future Systems:

* Signal Watcher
* Workflow Hunter
* Content Factory
* Revenue Engine

Status:
ACTIVE

---
## D-013 — Memory Record Schema v1

Date: 2026-06

Decision:
Вводится формальная схема записи для `knowledge/IDEAS.md`, отдельная от решения о действиях Memory Service API (save_idea, search_ideas, get_recent_ideas, memory_stats).

Обязательные поля: `Date`, `Title`, `Description`.

Расширенные поля v1.0: `ID`, `Source`, `Type`, `Status`.

Полное описание схемы зафиксировано в `knowledge/MEMORY_SCHEMA.md` (классификация CORE).

Действие `save_idea` в `Nexvoid_Memory_Service` реализовано в соответствии с этой схемой (ранее являлось заглушкой и не выполняло реальной записи).

GitHub Personal Access Token перенесён из открытых параметров HTTP-нод в n8n Credentials (`GitHub PAT`, httpHeaderAuth).

Reason:
Действия API определяют, что можно сделать с памятью, но не определяют формат самих записей. По мере подключения новых агентов (Signal Watcher, Market Oracle, Content Synthesist и др.) к Memory Service возникает риск, что каждый агент будет писать в IDEAS.md в своём собственном формате, что сломает парсинг для существующих read-действий.

Схема вводится как версионируемый контракт (additive-only для минорных версий) до подключения новых писателей — это предотвращает переделку парсинга постфактум.

Хранение токена доступа в открытом виде в параметрах workflow было риском безопасности независимо от схемы данных; перенос в Credentials устраняет этот риск без изменения функциональности.

Impact:
Любой агент или workflow, пишущий в `knowledge/IDEAS.md`, должен соответствовать `MEMORY_SCHEMA.md` v1.0.

Прямые записи в IDEAS.md, минующие `Nexvoid_Memory_Service.save_idea`, не рекомендуются.

Изменение состава полей (переименование, удаление) требует новой записи в DECISIONS.md и version bump схемы до 2.0.

Status:
ACTIVE

---
END OF DOCUMENT
