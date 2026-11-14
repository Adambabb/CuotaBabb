# CuotaBabb

**Get paid without chasing anyone.**

CuotaBabb is a multi-tenant SaaS that helps small service businesses manage customers, plans, subscriptions and payments from a single dashboard.

**English** | [Español](README.es.md)

> **Status:** early development. Application code has not been written yet.
> **Project plan (Spanish):** [CuotaBabb_Plan.md](./CuotaBabb_Plan.md)

---

## Problem

Academies, gyms, clinics, studios and small consultancies manage their fees with spreadsheets, WhatsApp and manual reminders. The usual result:

- customers forget to pay;
- subscriptions expire without control;
- revenue does not add up;
- time is wasted chasing invoices;
- the business has little visibility.

## Target users

Service SMBs with 1 to 30 employees:

- academies and training centres;
- gyms and sports centres;
- clinics and health centres;
- creative studios;
- small consultancies;
- beauty and wellness centres.

## Value proposition

- Customers, plans, subscriptions and payments in one place.
- Know who owes money and when.
- Recurring payments without chasing anyone.
- History and audit trail for important actions.
- Each company only sees its own data.

## MVP scope

- Company and owner registration.
- Roles: `OWNER`, `MANAGER` and `STAFF`.
- Customer management.
- Plan management.
- Create, renew and cancel subscriptions.
- Manual payments and Stripe Checkout in test mode.
- Stripe webhook with signature verification and idempotency.
- Dashboard with active customers, MRR, upcoming renewals and recent payments.
- Audit log.
- Demo with fictional Academia Nova data.
- Responsive Spanish UI.

### Out of scope

- Stripe Connect and real payments.
- PDF invoicing.
- Classes, bookings or access control.
- Native mobile app.
- Multiple locations.
- Microservices, Kafka or Kubernetes.
- AI inside the product.

## Planned stack

| Layer | Technology |
|---|---|
| Backend | Java 21, Spring Boot 4.x, Spring Security, JPA/Hibernate |
| API | REST, Bean Validation, springdoc OpenAPI |
| Database | PostgreSQL 17, Flyway |
| Payments | Stripe Checkout and webhooks in test mode |
| Frontend | React 19, TypeScript, Vite, React Router, TanStack Query |
| Styling | Tailwind CSS 4 |
| Testing | JUnit 5, MockMvc, Testcontainers, Vitest, Playwright |
| Infrastructure | Docker, Docker Compose, GitHub Actions |
| Deployment | Managed backend and PostgreSQL + static frontend |

## Architecture

Modular monolith with feature-based packages:

```text
auth / tenant / user / customer / plan /
subscription / payment / dashboard / audit / demo / common
```

Shared-schema multitenancy with `tenant_id` on every business table. The tenant is always derived from the authenticated user, never from the request body.

## Planned structure

```text
CuotaBabb/
├── backend/            # Spring Boot API
├── frontend/           # React application
├── docs/               # ERD, decisions and documentation
├── .env.example
├── .gitignore
├── docker-compose.yml
├── LICENSE
├── README.md
├── README.es.md
└── CuotaBabb_Plan.md
```

## Getting started

Pending implementation. The startup plan is available in [CuotaBabb_Plan.md](./CuotaBabb_Plan.md) (Spanish).

## Documentation

- [Project and learning plan](./CuotaBabb_Plan.md) (Spanish)
- `docs/` will contain the ERD, technical decisions and wireframes.

## License

MIT. See [LICENSE](./LICENSE).
