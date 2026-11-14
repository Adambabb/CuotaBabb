# CuotaBabb — Plan de proyecto y aprendizaje

> **Documento de trabajo, no código.**
> CuotaBabb se construye desde cero. La IA planifica, revisa y pregunta; **Adam escribe el código**.
> Última actualización: 14/11/2026.

---

## Índice

1. [Reglas de colaboración](#1-reglas-de-colaboración)
2. [El producto](#2-el-producto)
3. [Decisiones cerradas](#3-decisiones-cerradas)
4. [Lenguajes y stack](#4-lenguajes-y-stack)
5. [Arquitectura](#5-arquitectura)
6. [Modelo de datos](#6-modelo-de-datos)
7. [API y pantallas](#7-api-y-pantallas)
8. [Seguridad y multitenancy](#8-seguridad-y-multitenancy)
9. [Roadmap paso a paso](#9-roadmap-paso-a-paso)
10. [Testing](#10-testing)
11. [Despliegue y demo](#11-despliegue-y-demo)
12. [Portfolio y entrevistas](#12-portfolio-y-entrevistas)
13. [Uso de IA en el desarrollo](#13-uso-de-ia-en-el-desarrollo)
14. [Búsqueda de prácticas en paralelo](#14-búsqueda-de-prácticas-en-paralelo)
15. [Riesgos y control de alcance](#15-riesgos-y-control-de-alcance)
16. [Definición de terminado](#16-definición-de-terminado)
17. [Primer paso](#17-primer-paso)

---

## 1. Reglas de colaboración

Esto existe porque el proyecto es **tu portfolio** y lo vas a defender tú en una entrevista. El objetivo no es tener código rápido: es que puedas explicar cada decisión.

### Reparto de trabajo

| Tarea | Adam | IA |
|---|---|---|
| Código de la aplicación | Lo escribe | Revisa y pregunta |
| Tests | Los escribe | Propone casos y revisa |
| Commits | Los hace | Revisa el diff |
| Decisiones técnicas | Las toma y las justifica | Plantea alternativas |
| README y documentación | Los revisa y valida | Puede redactar borradores |
| Entrevista técnica | La practica | Hace de entrevistador |

### Reglas

1. **La IA no escribe código de la aplicación salvo que lo pidas explícitamente.**
2. Antes de crear un archivo, se pregunta: ¿lo escribe Adam o la IA?
3. Cada paso sigue este ciclo:
   - La IA explica el objetivo y los criterios de aceptación.
   - Adam implementa.
   - Adam ejecuta los tests.
   - La IA revisa el diff y hace preguntas.
   - Adam corrige y hace commit.
4. Si te bloqueas, pide **pistas**, no la solución. Si después de 20-30 minutos sigues bloqueado, se permite un ejemplo pequeño, que deberás explicar y reescribir.
5. **No se copia código de IA sin entenderlo.** Si no puedes explicar una línea, no entra en el proyecto.
6. Cada commit debe poder explicarse en dos minutos: qué hace, por qué así y qué alternativa descartaste.
7. Nada de secretos en el repositorio. `.env` siempre fuera de Git.
8. No se añade una tecnología solo porque suene bien. Primero el problema, después la herramienta.

### Frases útiles para pedir ayuda

- "Revisa este diff y hazme preguntas, no me des la solución."
- "Explícame este error sin reescribir el archivo."
- "¿Qué casos de test faltan para esta funcionalidad?"
- "Hazme una entrevista técnica sobre esta parte del proyecto."
- "¿Qué alternativa más simple existe?"

---

## 2. El producto

### Nombre

**CuotaBabb**. Sigue la familia de nombres de tus proyectos (`ReproBabb`) y se asocia a cuotas, suscripciones y cobros.

### Una frase

> **CuotaBabb es el SaaS que ayuda a pequeñas empresas de servicios a cobrar sus cuotas sin perseguir a nadie.**

### Problema

Academias, gimnasios, clínicas, estudios y consultoras pequeñas gestionan clientes, planes y cobros con hojas de Excel, WhatsApp y recordatorios manuales. Eso provoca:

- clientes que se olvidan de pagar;
- suscripciones vencidas sin control;
- ingresos que no cuadran;
- tiempo perdido persiguiendo recibos;
- poca visibilidad del negocio.

### Usuario objetivo

Pymes de servicios de 1 a 30 empleados:

- academias y centros de formación;
- gimnasios y centros deportivos;
- clínicas y centros de salud;
- estudios creativos;
- consultoras pequeñas;
- centros de estética o bienestar.

### Demo

Para que el producto se entienda en 10 segundos, la demo pública será **Academia Nova**, una academia ficticia.

- 20-30 clientes de ejemplo.
- 3-4 planes.
- Suscripciones activas, vencidas y canceladas.
- Pagos manuales y pagos de Stripe en modo test.
- Dashboard con MRR, altas, bajas y próximos vencimientos.

**Importante:** la demo es un `tenant` separado. Una empresa nueva que se registra ve su espacio vacío, no los datos de Academia Nova.

### Propuesta de valor

- Todo en un panel: clientes, planes, suscripciones y pagos.
- Sabes quién debe pagar y cuándo.
- Cobros recurrentes sin perseguir a nadie.
- Historial y auditoría de lo que ha pasado.
- Cada empresa ve solo sus datos.

### Alcance del MVP

**Dentro:**

- Registro de empresa y usuario propietario.
- Roles: `OWNER`, `MANAGER`, `STAFF`.
- Clientes: crear, editar, buscar, desactivar.
- Planes: nombre, precio, periodicidad.
- Suscripciones: alta, renovación, cancelación y vencimiento.
- Pagos manuales y Stripe Checkout en modo test.
- Webhook de Stripe con verificación de firma e idempotencia.
- Dashboard: clientes activos, MRR, próximos vencimientos, pagos recientes.
- Registro de auditoría.
- Datos de demo y cuenta de demo.
- Interfaz responsive en español.

**Fuera del MVP:**

- Stripe Connect y pagos reales.
- Facturación PDF.
- Clases, reservas o control de acceso.
- App móvil nativa.
- Multi-sucursal.
- Microservicios, Kafka o Kubernetes.
- IA dentro del producto.
- Integraciones con contabilidad.

### No objetivos

- No es un ERP.
- No es un CRM genérico para cualquier empresa.
- No es una herramienta de gestión de proyectos.
- No es una app de gimnasio con entrenamientos.

El producto es **horizontal en el modelo** (clientes, planes, suscripciones, pagos) y **vertical en la demo** (una academia concreta).

---

## 3. Decisiones cerradas

| Decisión | Elección | Motivo |
|---|---|---|
| Público | Pymes de servicios | Mercado amplio, problema claro, demo creíble |
| Demo | Academia Nova | Se entiende rápido y no encasilla en gimnasios |
| Arquitectura | Monolito modular | Adecuado para junior; fácil de explicar y desplegar |
| Multitenancy | Esquema compartido con `tenant_id` | Menos complejidad que una base por cliente |
| Aislamiento | `tenant_id` desde el JWT, nunca desde el body | Evita que una empresa lea datos de otra |
| Autenticación | JWT corto + refresh token en cookie HttpOnly | Seguridad razonable y explicable |
| Pagos | Manuales + Stripe test | Se demuestra el flujo sin arriesgar dinero |
| Facturación PDF | Fuera del MVP | Añade complejidad legal y técnica |
| IA en el producto | No | No es el objetivo del proyecto |
| IA en el desarrollo | Tutor socrático y revisor | Aprendizaje y defensa en entrevista |
| Idioma de la UI | Español, preparado para i18n | Usuarios de Madrid y España |
| Idioma del código | Inglés | Convención profesional y entrevistas |
| Idioma de commits | Inglés | Historial legible y estándar |
| Documentación | README principal en inglés + `README.es.md`; plan interno en español | Alcance internacional y comodidad local |
| Despliegue | Backend + Postgres gestionado; frontend estático | Bajo coste y suficiente para el portfolio |
| Check-in | Fuera del MVP | Es lo único que huele a gimnasio |

### Sobre el nombre y la estética

- Nombre: `CuotaBabb`.
- Tono: directo, cercano, sin lenguaje corporativo vacío.
- Dirección visual: fondo hueso, texto casi negro, acento lima y cian, tipografía con carácter, tablas densas pero aireadas, sidebar oscura.
- Evitar el aspecto de plantilla de admin genérica.

---

## 4. Lenguajes y stack

### Lenguajes de programación

| Lenguaje | Uso |
|---|---|
| **Java 21** | Backend, lógica de dominio, API REST |
| **TypeScript** | Frontend React con tipado |
| **SQL** | Esquema, consultas, índices, constraints |
| **YAML** | Configuración, Docker Compose, CI |
| **Bash / PowerShell** | Comandos de desarrollo y despliegue |
| **Markdown** | README, documentación y este plan |

Python no se usa en CuotaBabb. Python ya está demostrado en ReproBabb.

### Idiomas humanos

- **Español:** interfaz, textos de producto, `README.es.md` y este plan interno.
- **Inglés:** nombres de clases, variables, endpoints, commits, `README.md` principal y documentación pública.
- **Objetivo personal:** practicar inglés técnico hasta un B2 funcional usando el README principal y los mensajes de commit.
- La UI estará preparada para i18n, pero el MVP sale en español.

### Backend

- **Java 21 LTS**.
- **Spring Boot 4.x**.
- **Spring Web MVC** para la API REST.
- **Spring Security** para autenticación y autorización.
- **Spring Data JPA / Hibernate** para persistencia.
- **Bean Validation** para validar entrada.
- **PostgreSQL 17**.
- **Flyway** para migraciones.
- **springdoc OpenAPI** para documentar la API.
- **Maven** con wrapper (`mvnw`).
- **JUnit 5 + MockMvc + Testcontainers** para tests.
- **Stripe Java SDK** para Checkout y webhooks.
- **Actuator** para health checks.

### Frontend

- **React 19**.
- **TypeScript**.
- **Vite**.
- **React Router**.
- **TanStack Query** para datos del servidor.
- **React Hook Form + Zod** para formularios y validación.
- **Tailwind CSS 4**.
- **shadcn/ui** solo si aporta; no convertir el proyecto en una plantilla.
- **TanStack Table** para tablas.
- **Recharts** para gráficos.
- **Vitest + Testing Library** para tests.
- **Playwright** para el flujo E2E principal.

### Infraestructura

- **Docker** y **Docker Compose** para desarrollo local.
- **PostgreSQL** en contenedor para desarrollo.
- **GitHub Actions** para CI.
- **GitHub Container Registry** si se publica la imagen del backend.
- **Railway / Render / Fly.io** para backend y base de datos.
- **Vercel / Netlify** para el frontend.
- **Stripe CLI** para probar webhooks en local.

### Versiones

- Fijar versiones en `pom.xml`, `package.json` y la imagen de PostgreSQL.
- No usar `latest` en producción.
- Antes de empezar, comprobar la versión estable actual y anotarla en el README.
- Si una versión da problemas, bajar a la última estable compatible, no a una beta.

### Lo que NO se añade al principio

- Redis.
- Kafka.
- Kubernetes.
- Microservicios.
- GraphQL.
- WebSockets.
- IA generativa dentro del producto.
- Facturación electrónica.

Primero terminar el MVP. Después se evalúa.

---

## 5. Arquitectura

### Estilo

**Monolito modular** con paquetes por funcionalidad. Es más fácil de entender, probar y desplegar que una arquitectura de microservicios, y sigue siendo una decisión profesional si se justifica.

### Paquetes del backend

```text
com.adambabb.cuotababb
├── auth
├── tenant
├── user
├── customer
├── plan
├── subscription
├── payment
├── dashboard
├── audit
├── demo
└── common
```

Cada paquete agrupa:

- entidad;
- repositorio;
- servicio;
- controlador;
- DTOs;
- mapper;
- excepciones propias.

### Capas

```text
Controller  ->  Service  ->  Repository  ->  PostgreSQL
     |             |
     v             v
   DTOs        reglas de negocio
```

Reglas:

- El controlador no contiene lógica de negocio.
- El servicio no devuelve entidades JPA al exterior.
- Se usan DTOs de entrada y salida.
- Las entidades no se serializan directamente.
- Las transacciones se declaran en la capa de servicio.
- Los errores se centralizan en un manejador global.

### Multitenancy

- Una sola base de datos.
- Una sola tabla por entidad de negocio.
- Todas las tablas de negocio tienen `tenant_id`.
- El `tenant_id` se obtiene del usuario autenticado, nunca de un parámetro de la petición.
- Las consultas se filtran siempre por `tenant_id`.
- Los tests de integración crean **dos tenants** y verifican que no se ven entre sí.
- Más adelante se puede añadir Row Level Security de PostgreSQL, pero no es obligatorio para el MVP.

### Flujo de una petición

1. El cliente envía el access token.
2. Spring Security valida el token.
3. Se carga el usuario y su tenant.
4. El servicio recibe el `tenant_id` desde el contexto de seguridad.
5. El repositorio filtra por ese `tenant_id`.
6. Se devuelve un DTO, nunca la entidad.

### API

- Prefijo `/api/v1`.
- JSON.
- Fechas en UTC ISO-8601.
- Dinero en céntimos de euro (`integer`), nunca `float`.
- Paginación con `page`, `size`, `sort`.
- Errores con un formato común.
- Documentación OpenAPI accesible en desarrollo.

---

## 6. Modelo de datos

### Tablas principales

| Tabla | Propósito |
|---|---|
| `tenants` | Cada empresa cliente del SaaS |
| `users` | Usuarios de una empresa |
| `refresh_tokens` | Sesiones renovables |
| `customers` | Clientes de la empresa |
| `service_plans` | Planes que se venden |
| `subscriptions` | Suscripciones de clientes a planes |
| `payments` | Pagos manuales o de Stripe |
| `stripe_events` | Eventos recibidos de Stripe |
| `audit_logs` | Historial de acciones |

### Campos importantes

**`tenants`**

- `id` UUID.
- `name`.
- `slug` único.
- `status`.
- `is_demo`.
- `created_at`, `updated_at`.

**`users`**

- `id` UUID.
- `tenant_id`.
- `email` único.
- `password_hash`.
- `role`: `OWNER`, `MANAGER`, `STAFF`.
- `status`.
- `created_at`, `updated_at`.

**`customers`**

- `id` UUID.
- `tenant_id`.
- Nombre y apellidos.
- Email y teléfono.
- `status`.
- Notas.
- `deleted_at` para baja lógica.
- Único `(tenant_id, email)` entre los no borrados.

**`service_plans`**

- `id` UUID.
- `tenant_id`.
- `name`, `description`.
- `price_cents`.
- `billing_period`: `MONTHLY`, `QUARTERLY`, `YEARLY`.
- `status`.

**`subscriptions`**

- `id` UUID.
- `tenant_id`.
- `customer_id`.
- `plan_id`.
- `status`: `ACTIVE`, `PAST_DUE`, `CANCELLED`, `EXPIRED`.
- `start_date`, `end_date`, `next_billing_date`.
- `cancel_at_period_end`.
- Solo una suscripción `ACTIVE` por cliente.

**`payments`**

- `id` UUID.
- `tenant_id`.
- `subscription_id`.
- `amount_cents`, `currency`.
- `method`: `CASH`, `CARD`, `TRANSFER`, `STRIPE`.
- `status`: `PENDING`, `PAID`, `FAILED`, `REFUNDED`.
- `paid_at`.
- IDs de Stripe.
- Nunca guardar datos de tarjeta.

**`stripe_events`**

- `stripe_event_id` único.
- `type`.
- `payload` JSONB.
- `processed_at`.
- Sirve para que un evento repetido no cobre dos veces.

**`audit_logs`**

- `tenant_id`.
- `user_id`.
- `action`.
- `entity_type`, `entity_id`.
- `metadata` JSONB.
- `created_at`.

### Constraints que no se negocian

- Todas las tablas de negocio tienen `tenant_id`.
- Las claves foráneas de negocio incluyen el tenant o se validan en servicio.
- Email de usuario único.
- Email de cliente único dentro del tenant.
- Precios y duraciones no negativos.
- Una sola suscripción activa por cliente.
- `stripe_event_id` único.
- `stripe_checkout_session_id` único.
- Borrado lógico solo donde tenga sentido.
- `updated_at` actualizado por trigger o por la aplicación, no a mano en cada consulta.

---

## 7. API y pantallas

### Endpoints del MVP

**Auth**

- `POST /api/v1/auth/register`
- `POST /api/v1/auth/login`
- `POST /api/v1/auth/refresh`
- `POST /api/v1/auth/logout`
- `GET /api/v1/auth/me`

**Clientes**

- `GET /api/v1/customers`
- `POST /api/v1/customers`
- `GET /api/v1/customers/{id}`
- `PUT /api/v1/customers/{id}`
- `DELETE /api/v1/customers/{id}`

**Planes**

- `GET /api/v1/plans`
- `POST /api/v1/plans`
- `PUT /api/v1/plans/{id}`
- `DELETE /api/v1/plans/{id}`

**Suscripciones**

- `GET /api/v1/subscriptions`
- `POST /api/v1/subscriptions`
- `POST /api/v1/subscriptions/{id}/renew`
- `POST /api/v1/subscriptions/{id}/cancel`
- `GET /api/v1/subscriptions/{id}`

**Pagos**

- `GET /api/v1/payments`
- `POST /api/v1/payments`
- `POST /api/v1/payments/checkout`
- `POST /api/v1/webhooks/stripe`

**Dashboard**

- `GET /api/v1/dashboard/summary`

**Auditoría**

- `GET /api/v1/audit-logs` solo para `OWNER`.

### Reglas de API

- El `tenant_id` no se acepta en el body para operaciones normales.
- El cliente no puede elegir otro tenant.
- Los errores de validación devuelven 400 con detalles.
- Los recursos de otro tenant devuelven 404, no 403, para no filtrar información.
- Las operaciones sensibles se registran en auditoría.

### Pantallas

1. **Landing** con propuesta de valor, demo y CTA.
2. **Login** y **registro**.
3. **Dashboard** con métricas y últimos movimientos.
4. **Clientes** con búsqueda, filtros, paginación y ficha.
5. **Planes** con precio y periodicidad.
6. **Suscripciones** con estado y próxima renovación.
7. **Pagos** con método, estado e importe.
8. **Ajustes** con datos de la empresa y usuarios.
9. **Auditoría** solo para propietarios.

### Estados de UI

Cada pantalla debe tener:

- carga;
- vacío;
- error;
- éxito;
- permisos insuficientes.

Un formulario no está terminado si solo funciona en el caso feliz.

---

## 8. Seguridad y multitenancy

### Autenticación

- Contraseñas con **BCrypt** (coste 12).
- Access token JWT de 15 minutos.
- Refresh token de 30 días, hasheado en base de datos.
- Refresh token en cookie `HttpOnly`, `Secure` y `SameSite`.
- Rotación de refresh token.
- Logout revoca el refresh token.
- Nunca guardar tokens en `localStorage` si se puede evitar.

### Autorización

- Roles `OWNER`, `MANAGER`, `STAFF`.
- `OWNER`: todo, incluidos usuarios y auditoría.
- `MANAGER`: clientes, planes, suscripciones y pagos.
- `STAFF`: consulta y pagos, sin borrar clientes.
- La autorización se comprueba en el backend, no solo en la UI.

### Aislamiento

- El `tenant_id` sale del usuario autenticado.
- Los repositorios reciben el tenant como parámetro obligatorio.
- Tests con dos tenants en cada funcionalidad sensible.
- No confiar en IDs enviados por el cliente.
- Revisar que los `JOIN` también filtren por tenant.

### Otros controles

- Validación de entrada con Bean Validation.
- CORS limitado al frontend.
- Rate limiting en login y endpoints públicos.
- Secretos por variables de entorno.
- HTTPS en producción.
- Stripe: verificar firma del webhook e idempotencia.
- No usar claves reales de Stripe.
- Datos de demo ficticios.
- Política básica de privacidad y protección de datos.
- Logs sin contraseñas, tokens ni datos sensibles.

---

## 9. Roadmap paso a paso

> Tiempo estimado: **5-6 semanas a tiempo parcial**. Si aprendes React sobre la marcha, añade 1-2 semanas. Si te retrasas, recorta alcance; no amplíes el plazo.

### Semana 0 — Decisiones y arranque

**Objetivo:** tener claro qué se construye y un repositorio limpio.

- [ ] 0.1 Crear el repositorio `CuotaBabb` en GitHub.
- [ ] 0.2 Escribir el README inicial: problema, usuario, alcance, stack y cómo se ejecutará.
- [ ] 0.3 Crear `.gitignore` y `.env.example`.
- [ ] 0.4 Dibujar el ERD y cuatro wireframes: login, dashboard, clientes, suscripciones.
- [ ] 0.5 Escribir `docs/decisions.md` con las decisiones de la tabla anterior.
- [ ] 0.6 Configurar el tablero de tareas y las etiquetas del repo.

**Revisión:** la IA revisa el README, el ERD y las decisiones. Tú corriges.

**Hecho cuando:** otra persona puede leer el README y entender qué vas a construir sin preguntarte.

---

### Semana 1 — Base técnica y autenticación

**Objetivo:** un usuario puede registrarse, entrar, refrescar sesión y salir; cada empresa queda aislada.

- [ ] 1.1 Levantar PostgreSQL con Docker Compose.
- [ ] 1.2 Crear el proyecto Spring Boot con Maven Wrapper y Java 21.
- [ ] 1.3 Configurar perfiles `local` y `test`, variables de entorno y Flyway.
- [ ] 1.4 Crear la migración inicial de `tenants`, `users` y `refresh_tokens`.
- [ ] 1.5 Crear entidades, repositorios y DTOs de tenant y usuario.
- [ ] 1.6 Implementar registro de empresa + propietario.
- [ ] 1.7 Implementar login, refresh y logout.
- [ ] 1.8 Configurar Spring Security y el filtro JWT.
- [ ] 1.9 Implementar el contexto de tenant.
- [ ] 1.10 Añadir tests de aislamiento, token inválido y roles.

**Revisión:** la IA revisa el modelo, el flujo de tokens, la configuración de seguridad y los tests.

**Hecho cuando:**

- dos tenants no pueden verse;
- el access token caduca;
- el refresh token rota y se revoca;
- un usuario sin rol no puede acceder a lo que no le corresponde;
- los tests pasan en local y en CI.

**No avances si:** estás guardando contraseñas en texto plano, aceptando `tenant_id` del cliente o devolviendo entidades con el hash de la contraseña.

---

### Semana 2 — Clientes, planes y suscripciones

**Objetivo:** el núcleo del negocio funciona.

- [ ] 2.1 Migración y CRUD de clientes.
- [ ] 2.2 Búsqueda, paginación y baja lógica.
- [ ] 2.3 Migración y CRUD de planes.
- [ ] 2.4 Migración de suscripciones.
- [ ] 2.5 Alta de suscripción con validación de plan y cliente del mismo tenant.
- [ ] 2.6 Renovación y cancelación.
- [ ] 2.7 Cálculo de próxima fecha de cobro.
- [ ] 2.8 Regla: una sola suscripción activa por cliente.
- [ ] 2.9 Tarea programada para marcar vencidas.
- [ ] 2.10 Tests de transiciones y aislamiento.

**Revisión:** la IA revisa la máquina de estados, las constraints y los tests de dos tenants.

**Hecho cuando:** puedes crear un cliente, asignarle un plan, renovar, cancelar y ver el estado correcto sin tocar la base de datos a mano.

---

### Semana 3 — Pagos y Stripe

**Objetivo:** registrar pagos y cobrar en modo test.

- [ ] 3.1 Migración de pagos.
- [ ] 3.2 Pago manual en efectivo, tarjeta o transferencia.
- [ ] 3.3 Crear sesión de Stripe Checkout en modo test.
- [ ] 3.4 Webhook con verificación de firma.
- [ ] 3.5 Idempotencia con `stripe_event_id`.
- [ ] 3.6 Actualizar suscripción cuando el pago se completa.
- [ ] 3.7 Tests de firma inválida, evento repetido y pago fallido.
- [ ] 3.8 Documentar cómo probar con Stripe CLI.

**Revisión:** la IA revisa el flujo completo, los estados y la seguridad del webhook.

**Hecho cuando:** un pago de prueba activa o renueva una suscripción y un evento repetido no duplica nada.

**No avances si:** usas claves reales, no verificas la firma o confías en el frontend para marcar un pago como pagado.

---

### Semana 4 — Dashboard, auditoría y frontend

**Objetivo:** el producto se puede usar y enseñar.

- [ ] 4.1 Consulta de métricas: clientes activos, MRR, próximos vencimientos, pagos recientes.
- [ ] 4.2 Registro de auditoría en operaciones sensibles.
- [ ] 4.3 Layout responsive con sidebar y cabecera.
- [ ] 4.4 Login y registro.
- [ ] 4.5 Dashboard.
- [ ] 4.6 Clientes: listado, búsqueda, formulario y ficha.
- [ ] 4.7 Planes.
- [ ] 4.8 Suscripciones.
- [ ] 4.9 Pagos.
- [ ] 4.10 Ajustes y auditoría.
- [ ] 4.11 Estados de carga, vacío y error.
- [ ] 4.12 Datos de demo para Academia Nova.

**Revisión:** la IA revisa la UI, la accesibilidad básica, los estados y la consistencia con el diseño.

**Hecho cuando:** puedes hacer el flujo completo desde la UI sin usar Postman.

---

### Semana 5 — Tests, CI, despliegue y demo

**Objetivo:** el proyecto está online y se puede probar.

- [ ] 5.1 Tests unitarios de servicios críticos.
- [ ] 5.2 Tests de integración con Testcontainers.
- [ ] 5.3 Test E2E del flujo principal.
- [ ] 5.4 GitHub Actions: backend, frontend y migraciones.
- [ ] 5.5 Dockerfile multi-stage del backend.
- [ ] 5.6 Desplegar PostgreSQL y backend.
- [ ] 5.7 Desplegar el frontend.
- [ ] 5.8 Variables de entorno y CORS de producción.
- [ ] 5.9 Cuenta demo y datos de Academia Nova.
- [ ] 5.10 Reset periódico de la demo.
- [ ] 5.11 Health checks y logs.

**Revisión:** la IA revisa el pipeline, el Dockerfile, las variables y el checklist de seguridad.

**Hecho cuando:** un reclutador puede entrar en la URL, usar la cuenta demo y ver el producto sin instalar nada.

---

### Semana 6 — Portfolio y entrevista

**Objetivo:** que el proyecto se entienda y se pueda defender.

- [ ] 6.1 README final con problema, arquitectura, decisiones, seguridad, demo y límites.
- [ ] 6.2 Diagrama de arquitectura y ERD.
- [ ] 6.3 Capturas y GIF.
- [ ] 6.4 Vídeo de 2-3 minutos.
- [ ] 6.5 Caso de estudio en tu portfolio.
- [ ] 6.6 Enlazar desde GitHub, LinkedIn y CV.
- [ ] 6.7 Preparar respuestas de entrevista.
- [ ] 6.8 Hacer una entrevista simulada con la IA.
- [ ] 6.9 Revisar que no haya secretos ni datos reales.

**Hecho cuando:** puedes explicar el proyecto en 2 minutos y responder 10 preguntas técnicas sin mirar el código.

---

## 10. Testing

### Pirámide

- **Unitarios:** reglas de suscripción, cálculo de fechas, estados, validaciones.
- **Integración:** repositorios, seguridad, migraciones, aislamiento entre tenants.
- **E2E:** registro, login, alta de cliente, plan, suscripción y pago.

### Tests críticos

1. Un tenant no puede leer ni modificar datos de otro.
2. Un usuario sin token no accede.
3. Un token caducado no accede.
4. Un refresh token revocado no renueva.
5. Un `STAFF` no puede borrar clientes.
6. Un cliente no puede tener dos suscripciones activas.
7. Una suscripción cancelada no se renueva.
8. Un evento de Stripe repetido no duplica el pago.
9. Una firma inválida de Stripe se rechaza.
10. Un pago fallido no activa la suscripción.
11. Los importes se guardan en céntimos.
12. Las migraciones se aplican desde cero en una base vacía.

### Regla

Si un bug llega a producción, primero se escribe un test que lo reproduce; después se corrige.

---

## 11. Despliegue y demo

### Entornos

- **Local:** Docker Compose + backend + frontend.
- **Test:** Testcontainers, sin servicios externos.
- **Producción:** backend y PostgreSQL gestionados + frontend estático.

### Variables de entorno

- `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`.
- `JWT_SECRET`.
- `CORS_ALLOWED_ORIGINS`.
- `STRIPE_SECRET_KEY` de test.
- `STRIPE_WEBHOOK_SECRET` de test.
- `DEMO_SEED`.
- `APP_BASE_URL`.

Nunca se commitea el `.env` real.

### Demo

- Tenant `Academia Nova` con `is_demo = true`.
- Usuario demo con credenciales públicas en el README.
- Datos ficticios: clientes, planes, suscripciones y pagos.
- Banner visible de "Entorno de demostración".
- Stripe en modo test.
- Reset periódico para que la demo no se rompa.
- Una empresa nueva que se registra ve su espacio vacío y un onboarding.

### Checklist de producción

- [ ] HTTPS.
- [ ] CORS restringido.
- [ ] Secretos fuera del repo.
- [ ] Migraciones automáticas y verificadas.
- [ ] Health check.
- [ ] Logs sin datos sensibles.
- [ ] Backup de base de datos.
- [ ] Rate limiting en login.
- [ ] Stripe en modo test.
- [ ] Cuenta demo separada.
- [ ] Datos de demo ficticios.
- [ ] Aviso de privacidad básico.

---

## 12. Portfolio y entrevistas

### Qué demuestra CuotaBabb

- Diseño de producto y alcance.
- Backend Java/Spring Boot.
- API REST y validación.
- PostgreSQL, migraciones y constraints.
- Multitenancy y aislamiento.
- Autenticación y roles.
- Pagos y webhooks.
- React y TypeScript.
- Docker, CI y despliegue.
- Tests y decisiones técnicas.

### Narrativa

> "Mi primer proyecto, Subscription Management System, era un CRUD de suscripciones. CuotaBabb es su evolución: un SaaS multiempresa con aislamiento de datos, autenticación, pagos y despliegue. Aprendí que el problema no era gestionar suscripciones, sino hacerlo de forma segura para varias empresas."

Esa historia conecta tus proyectos y demuestra evolución.

### README del proyecto

El README principal (`README.md`) estará en inglés y tendrá una versión en español (`README.es.md`) enlazada desde la cabecera.

Debe incluir:

- problema y usuario;
- demo y credenciales;
- funcionalidades;
- arquitectura;
- modelo de datos;
- decisiones y tradeoffs;
- seguridad;
- tests;
- cómo ejecutarlo;
- límites y lo que harías después;
- capturas y vídeo.

### Vídeo de demo

2-3 minutos:

1. Qué problema resuelve.
2. Login en la demo.
3. Dashboard.
4. Alta de cliente.
5. Alta de suscripción.
6. Pago de prueba.
7. Aislamiento: mención a que cada empresa ve solo sus datos.
8. Cierre: stack y lo que aprendiste.

### Preguntas de entrevista

- ¿Por qué monolito modular y no microservicios?
- ¿Cómo garantizas que un tenant no vea otro?
- ¿Por qué `tenant_id` desde el JWT?
- ¿Qué pasa si el token caduca?
- ¿Por qué refresh token en cookie HttpOnly?
- ¿Cómo evitas duplicar un pago de Stripe?
- ¿Qué estados tiene una suscripción?
- ¿Qué pasa si el webhook llega antes que el usuario vuelva del checkout?
- ¿Cómo pruebas el aislamiento?
- ¿Qué harías con 10.000 empresas?
- ¿Qué decisiones cambiarías si esto fuera production?
- ¿Qué parte te costó más y cómo la resolviste?

### Lo que NO debes decir

- "Usé microservicios" si no los usaste.
- "Tengo IA" si no la tiene.
- "Cobro pagos reales" si es Stripe test.
- "Tengo clientes" si son datos ficticios.
- "Tengo tests" si no cubren lo crítico.
- "Es escalable" sin explicar por qué.

---

## 13. Uso de IA en el desarrollo

### Permitido

- Explicar conceptos.
- Revisar código y señalar problemas.
- Proponer casos de test.
- Hacer preguntas socráticas.
- Comparar alternativas.
- Redactar borradores de documentación.
- Simular entrevistas.
- Ayudar a leer un error.

### No permitido

- Generar una funcionalidad completa y copiarla sin entenderla.
- Aceptar código que no puedas explicar.
- Añadir dependencias porque la IA lo sugiere.
- Delegar decisiones de arquitectura sin revisarlas.
- Usar IA para escribir el README final sin revisarlo.

### Flujo socrático

1. Tú describes qué quieres conseguir.
2. La IA pregunta qué has intentado y qué esperas.
3. Tú implementas.
4. La IA revisa y pregunta "¿por qué?".
5. Tú corriges.
6. Tú explicas el resultado en voz alta.

### Futuro proyecto de IA

La idea de un tercer proyecto para demostrar ingeniería de IA es buena, pero **no ahora**. Primero:

1. Terminar CuotaBabb.
2. Desplegarlo.
3. Aplicar a prácticas.
4. Después, si acaso, un proyecto pequeño de harness/evaluación/revisión con IA, no otro chatbot.

---

## 14. Búsqueda de prácticas en paralelo

No esperes a terminar CuotaBabb para solicitar prácticas. ReproBabb ya es un proyecto suficiente para empezar a aplicar.

### Ritmo semanal

- 4-5 días de construcción.
- 1 día de aplicación y networking.
- 1 día de descanso.

### Acciones

- [ ] Actualizar el CV con ReproBabb y "CuotaBabb en desarrollo" solo cuando exista repo.
- [ ] Actualizar LinkedIn.
- [ ] Revisar la lista `Empresas_Practicas_DAM_Adam.txt`.
- [ ] Aplicar a 5-10 empresas por semana.
- [ ] Guardar cada solicitud en una tabla.
- [ ] Preparar una respuesta para "háblame de un proyecto".
- [ ] Practicar entrevistas con la IA.

### Prioridad

1. Prácticas.
2. CuotaBabb.
3. Portfolio visual.
4. Tercer proyecto.

No al revés.

---

## 15. Riesgos y control de alcance

| Riesgo | Señal | Mitigación |
|---|---|---|
| Scope creep | Añades clases, reservas o PDF | Volver al MVP; apuntar la idea para después |
| Auth infinita | Llevas una semana con tokens | Implementar lo mínimo seguro y seguir |
| Stripe | Webhooks que no llegan | Probar con Stripe CLI y logs |
| Fuga entre tenants | Consultas sin `tenant_id` | Tests de dos tenants y revisión de repos |
| Frontend perfecto | Cambias colores durante días | Diseño aprobado; primero funcionalidad |
| Deploy caro | Servicios de pago | Free tier o coste máximo mensual fijado |
| Datos reales | Metes clientes de verdad | Solo datos ficticios |
| Burnout | No avanzas | Semanas de 4 días y alcance recortable |
| Copiar IA | No puedes explicar el código | Reescribir y explicar antes del commit |

### Regla de recorte

Si una semana se retrasa, se recorta en este orden:

1. Auditoría avanzada.
2. i18n.
3. Email.
4. QR/check-in.
5. Stripe Checkout (se queda el pago manual).
6. Animaciones.

Nunca se recorta:

- aislamiento entre tenants;
- autenticación;
- tests críticos;
- README;
- demo;
- despliegue.

---

## 16. Definición de terminado

### Por funcionalidad

- [ ] Funciona en local.
- [ ] Tiene tests.
- [ ] Tiene validación.
- [ ] Tiene manejo de errores.
- [ ] Respeta el tenant.
- [ ] Está documentada.
- [ ] Puedes explicarla en 2 minutos.
- [ ] Está commiteada.

### Por proyecto

- [ ] README completo.
- [ ] Demo online.
- [ ] Cuenta demo.
- [ ] Datos ficticios.
- [ ] Tests críticos.
- [ ] CI en verde.
- [ ] Migraciones desde cero.
- [ ] Docker.
- [ ] Variables de entorno.
- [ ] Sin secretos.
- [ ] Vídeo.
- [ ] Capturas.
- [ ] Caso de estudio.
- [ ] Enlaces en GitHub, portfolio, CV y LinkedIn.
- [ ] Entrevista simulada superada.

---

## 17. Primer paso

**No empieces por el código.**

### Hoy

1. Lee este plan y marca lo que quieras cambiar.
2. Confirma el alcance del MVP.
3. Crea el repositorio `CuotaBabb`.
4. Escribe el README inicial con:
   - problema;
   - usuario;
   - alcance;
   - stack;
   - cómo se ejecutará.
5. Dibuja el ERD y los cuatro wireframes.

### Después

Cuando tengas el README y el ERD, pide revisión. La IA te hará preguntas y te señalará huecos. Tú corriges. Solo entonces se pasa al Paso 1.1: levantar PostgreSQL con Docker.

### Regla final

> Si no puedes defenderlo, no está terminado.
