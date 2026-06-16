DECISIONS.md
# Nexvoid Decisions Log

---

## DECISION-001

Date: 2026-06-16

Title: Knowledge Architecture

Status: Accepted

Decision:

GitHub является основным хранилищем знаний (Source of Truth).

PostgreSQL используется как индекс поиска и шина событий.

Rationale:

GitHub обеспечивает читаемость, прозрачность и версионирование знаний.

PostgreSQL обеспечивает быстрый поиск, фильтрацию и обработку событий между сервисами.

Consequences:

Все знания сохраняются в GitHub.
___________________________________________________________
---

## DECISION-002

Date: 2026-06-16

Title: Service-Oriented Architecture

Status: Accepted

Decision:

Nexvoid строится как набор независимых сервисов.

Основные сервисы:

- Commander Router
- Memory Service
- Signal Service
- Research Service
- Media Factory

Каждый сервис имеет собственную ответственность и может развиваться независимо.

Rationale:

Монолитный workflow сложно поддерживать и масштабировать.

Отказ одного сервиса не должен приводить к остановке всей системы.

Consequences:

Новые функции добавляются в существующие сервисы или создаются как отдельные сервисы.

Запрещено превращать Commander в монолит с бизнес-логикой.

Все сервисы используют PostgreSQL для поиска и межсервисного взаимодействия.
______________________________________________________
---

## DECISION-003

Date: 2026-06-16

Title: Memory Service API v1

Status: Accepted

Decision:

Memory Service v1 поддерживает следующие действия:

- save_idea
- search_ideas
- get_recent_ideas
- memory_stats

Rationale:

На первом этапе необходимо реализовать минимальный набор функций для сохранения, поиска и извлечения знаний.

Consequences:

Все запросы памяти должны использовать один из утверждённых actions.

Расширение API допускается только через новые версии.
