# CuotaBabb

**Cobra sin perseguir a nadie.**

CuotaBabb es un SaaS multiempresa para que pequeñas empresas de servicios gestionen clientes, planes, suscripciones y cobros desde un único panel.

[English](README.md) | **Español**

> **Estado:** en desarrollo. Todavía no hay código de aplicación.
> **Plan del proyecto:** [CuotaBabb_Plan.md](./CuotaBabb_Plan.md)

---

## Problema

Academias, gimnasios, clínicas, estudios y consultoras pequeñas gestionan sus cuotas con Excel, WhatsApp y recordatorios manuales. El resultado es habitual:

- clientes que se olvidan de pagar;
- suscripciones vencidas sin control;
- ingresos que no cuadran;
- tiempo perdido persiguiendo recibos;
- poca visibilidad del negocio.

## Usuario objetivo

Pymes de servicios de 1 a 30 empleados:

- academias y centros de formación;
- gimnasios y centros deportivos;
- clínicas y centros de salud;
- estudios creativos;
- consultoras pequeñas;
- centros de estética o bienestar.

## Propuesta de valor

- Clientes, planes, suscripciones y pagos en un solo lugar.
- Control de quién debe pagar y cuándo.
- Cobros recurrentes sin perseguir a nadie.
- Historial y auditoría de las acciones importantes.
- Cada empresa ve únicamente sus propios datos.

## Alcance del MVP

- Registro de empresa y usuario propietario.
- Roles `OWNER`, `MANAGER` y `STAFF`.
- Gestión de clientes.
- Gestión de planes.
- Altas, renovaciones y cancelaciones de suscripciones.
- Pagos manuales y Stripe Checkout en modo test.
- Webhook de Stripe con verificación de firma e idempotencia.
- Dashboard con clientes activos, MRR, vencimientos y pagos recientes.
- Registro de auditoría.
- Demo con datos ficticios de Academia Nova.
- Interfaz responsive en español.

### Fuera del MVP

- Stripe Connect y pagos reales.
- Facturación PDF.
- Clases, reservas o control de acceso.
- App móvil nativa.
- Multi-sucursal.
- Microservicios, Kafka o Kubernetes.
- IA dentro del producto.

## Stack previsto

| Capa | Tecnología |
|---|---|
| Backend | Java 21, Spring Boot 4.x, Spring Security, JPA/Hibernate |
| API | REST, Bean Validation, springdoc OpenAPI |
| Base de datos | PostgreSQL 17, Flyway |
| Pagos | Stripe Checkout y webhooks en modo test |
| Frontend | React 19, TypeScript, Vite, React Router, TanStack Query |
| Estilos | Tailwind CSS 4 |
| Tests | JUnit 5, MockMvc, Testcontainers, Vitest, Playwright |
| Infraestructura | Docker, Docker Compose, GitHub Actions |
| Despliegue | Backend y PostgreSQL gestionados + frontend estático |

## Arquitectura

Monolito modular con paquetes por funcionalidad:

```text
auth / tenant / user / customer / plan /
subscription / payment / dashboard / audit / demo / common
```

Multitenancy con esquema compartido y `tenant_id` en todas las tablas de negocio. El tenant se obtiene siempre del usuario autenticado, nunca del cuerpo de la petición.

## Estructura prevista

```text
CuotaBabb/
├── backend/            # API Spring Boot
├── frontend/           # Aplicación React
├── docs/               # ERD, decisiones y documentación
├── .env.example
├── .gitignore
├── docker-compose.yml
├── LICENSE
├── README.md
├── README.es.md
└── CuotaBabb_Plan.md
```

## Puesta en marcha

Pendiente de implementación. El plan de arranque está en [CuotaBabb_Plan.md](./CuotaBabb_Plan.md).

## Documentación

- [Plan de proyecto y aprendizaje](./CuotaBabb_Plan.md)
- `docs/` se irá completando con el ERD, las decisiones técnicas y los wireframes.

## Licencia

MIT. Consulta [LICENSE](./LICENSE).
