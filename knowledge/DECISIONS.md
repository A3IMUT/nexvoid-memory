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

Все сервисы используют PostgreSQL для поиска и межсервисного взаимодействия.
