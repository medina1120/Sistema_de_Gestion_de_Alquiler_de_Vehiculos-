# 🚗 Sistema de Gestión de Alquiler de Vehículos

> Flota vehicular, reservas y contratos — Trabajo en Clase · Nivel Intermedio

[![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=for-the-badge&logo=nestjs&logoColor=white)](https://nestjs.com/)
[![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)](https://nextjs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)](http://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=prisma&logoColor=white)](https://www.prisma.io/)

**Autores:** Cristhian Gaitán Guzmán · Yilver Isnardo Medina Urrea

---

## 📋 Tabla de Contenidos

- [Descripción del Proyecto](#-descripción-del-proyecto)
- [Stack Tecnológico](#-stack-tecnológico)
- [Modelo de Datos](#-modelo-de-datos)
- [Plan de Releases](#-plan-de-releases)
- [Sprints e Historias de Usuario](#-sprints-e-historias-de-usuario)
- [Cronograma](#-cronograma)
- [Definition of Done (DoD)](#-definition-of-done-dod)
- [Instalación y Ejecución](#-instalación-y-ejecución)

---

## 📖 Descripción del Proyecto

El **Sistema de Gestión de Alquiler de Vehículos** es una aplicación web full-stack que permite a empresas de alquiler sistematizar sus procesos de reserva, entrega y devolución de vehículos: gestión de flota, registro de clientes, creación de reservas con verificación de disponibilidad, generación de contratos y cálculo automático de cargos adicionales.

### Contexto

Una empresa de alquiler de vehículos con una flota de 80 unidades necesita sistematizar sus procesos. El registro actual en papel genera doble reservación de vehículos, falta de control sobre el estado mecánico de la flota y dificultades para generar facturas. Esta plataforma permite gestionar la flota, realizar reservas, generar contratos de alquiler y llevar el historial de mantenimiento de cada vehículo.

### Alcance

| Aspecto | Detalle |
|---|---|
| **Tipo** | Trabajo en clase — Nivel Intermedio |
| **Entidades** | 7 entidades con relaciones (ver modelo de datos) |
| **Historias de Usuario** | 10 HUs organizadas en 5 sprints |
| **Releases** | 2 releases alineados con los cortes académicos |
| **Casos de Uso** | 5 CUs (vehículos, clientes, reservas, contratos, devoluciones) |

### Casos de Uso

| # | Descripción |
|---|---|
| **CU-01** | Registrar vehículo con marca, modelo, año, placa, tipo (sedán, SUV, camioneta) y estado. |
| **CU-02** | Registrar cliente con datos personales, licencia de conducción y contacto. |
| **CU-03** | Crear reserva seleccionando vehículo, fecha de inicio, fecha de fin y verificando disponibilidad. |
| **CU-04** | Generar contrato de alquiler al entregar el vehículo con condiciones y valor total. |
| **CU-05** | Registrar devolución del vehículo con revisión de estado y cálculo de cargos adicionales. |

### Funcionalidades Principales

- ✅ CRUD completo de Vehículos, Tipos de Vehículo y Clientes
- ✅ Gestión de Reservas con verificación de disponibilidad y detección de solapamiento de fechas
- ✅ Generación de Contratos de Alquiler con cálculo automático de valor total
- ✅ Registro de Entrega con check-in de estado del vehículo
- ✅ Registro de Devolución con comparación de estado entrega/devolución
- ✅ Cálculo automático de Cargos Adicionales (días extra, daños, combustible)
- ✅ Common Module: Filtros de excepción, Interceptores y Pipes globales
- ✅ Integración completa Frontend ↔ Backend con Docker Compose

---

## 🛠 Stack Tecnológico

| Capa | Tecnología | Propósito |
|---|---|---|
| **Backend** | NestJS (Node.js + TypeScript) | API REST con arquitectura en capas |
| **Frontend** | Next.js 14+ (React + TypeScript) | Interfaz de usuario con App Router |
| **Base de Datos** | PostgreSQL 16 | Almacenamiento relacional |
| **ORM** | Prisma | Modelado de datos, migraciones y queries |
| **Contenedores** | Docker + Docker Compose | Orquestación de servicios |
| **Validación** | class-validator + class-transformer | DTOs y validación de entrada |

---

## 📊 Modelo de Datos

### Diagrama de Relaciones
```
TipoVehiculo    1 ──── N  Vehiculo
Vehiculo        1 ──── N  Reserva
Cliente         1 ──── N  Reserva
Reserva         1 ──── 1  Contrato
Contrato        1 ──── 1  Devolucion
Devolucion      1 ──── N  CargoAdicional
```

### Entidades

| Entidad | Campos Principales |
|---|---|
| **Vehiculo** | id, marca, modelo, año, placa (unique), color, estado (disponible \| reservado \| mantenimiento), tipoVehiculoId |
| **TipoVehiculo** | id, nombre (unique), descripción, tarifaBaseDia |
| **Cliente** | id, nombres, apellidos, documentoIdentidad (unique), correo (unique), teléfono, dirección, numeroLicencia |
| **Reserva** | id, clienteId, vehiculoId, fechaInicio, fechaFin, estado (pendiente \| activa \| cancelada), fechaCreacion, costoEstimado |
| **Contrato** | id, reservaId (unique), valorTotal, condiciones, nivelCombustibleEntrega, kilometrajeEntrega, observacionesEntrega, estado (activo \| cerrado) |
| **Devolucion** | id, contratoId (unique), fechaDevolucion, nivelCombustibleDevolucion, kilometrajeDevolucion, observacionesDevolucion |
| **CargoAdicional** | id, devolucionId, tipo (dias_extra \| daño \| combustible), descripción, monto |

---

## 🚀 Plan de Releases

### Release 1 — Segundo Corte: Backend + Frontend Base

> **📅 Cierre: 17 de Abril de 2026** · Sprints 1, 2 y 3

**Objetivo:** Entregar la API REST completa con arquitectura en capas (Controller → Service → Repository) y el frontend con las vistas de CRUD para todas las entidades base del sistema de alquiler.

| Sprint | Período | HUs | Alcance |
|---|---|---|---|
| [Sprint 1](#sprint-1--infraestructura-y-entidades-base) | Mar 16 → Mar 29 | HU-01, HU-02 | Docker, Prisma, Vehículo, TipoVehículo |
| [Sprint 2](#sprint-2--clientes-reservas-y-disponibilidad) | Mar 30 → Abr 10 | HU-03, HU-04, HU-05, HU-06 | Cliente, Reserva, Disponibilidad |
| [Sprint 3](#sprint-3--contratos-devoluciones-y-frontend-base) | Abr 13 → Abr 17 | HU-07, HU-08, HU-09 | Contrato, Devolución, Cargos, Frontend base |

### Release 2 — Tercer Corte: Integración y Despliegue

> **📅 Cierre: 22 de Mayo de 2026** · Sprints 4 y 5

**Objetivo:** Integración completa frontend ↔ backend, flujos avanzados de reserva-contrato-devolución y despliegue funcional con Docker.

| Sprint | Período | HUs | Alcance |
|---|---|---|---|
| [Sprint 4](#sprint-4--frontend-avanzado-e-integración) | Abr 20 → May 8 | HU-07, HU-08 | Frontend reservas, contratos, navegación completa |
| [Sprint 5](#sprint-5--cierre-y-despliegue) | May 11 → May 22 | HU-09, HU-10 | Integración de flujos, pruebas, Docker Compose, README |

---

## 📌 Sprints e Historias de Usuario

### Sprint 1 — Infraestructura y entidades base

> 📅 **Mar 16 → Mar 29** · 🚫 Festivo: Mar 23 (San José)

| # | Historia de Usuario | Labels | Issue |
|---|---|---|---|
| HU-01 | Registro de Vehículos | `user-story` `backend` `frontend` | <!-- TODO --> |
| HU-02 | Gestión de Tipos de Vehículo | `user-story` `backend` `frontend` | <!-- TODO --> |

**Entregables:**
- Docker Compose con PostgreSQL, NestJS y Next.js
- Prisma schema con entidades Vehiculo y TipoVehiculo
- Migraciones ejecutadas
- CRUD completo (Controller → Service → Repository) para las 2 entidades
- Frontend: listados y formularios básicos

---

#### HU-01 — Registro de Vehículos (CU-01)

> Como administrador de flota, quiero registrar, consultar, editar y eliminar vehículos con su información completa, para mantener actualizado el inventario de la flota vehicular de la empresa.

**Criterios de Aceptación**
- [ ] Se puede crear un vehículo con: marca, modelo, año, placa (única), tipo de vehículo (sedán, SUV, camioneta), color y estado (disponible, reservado, en mantenimiento).
- [ ] La placa del vehículo es única; si se duplica, el sistema retorna un error claro.
- [ ] El tipo de vehículo debe existir previamente en el catálogo; si no existe, se retorna error.
- [ ] Se puede consultar la lista de todos los vehículos con paginación y filtro por estado y tipo.
- [ ] Se puede consultar un vehículo por su ID con sus datos completos incluyendo el tipo asociado.
- [ ] Se puede editar la información de un vehículo existente (excepto la placa).
- [ ] Se puede eliminar un vehículo que no tenga reservas activas; caso contrario se retorna error.

**Tareas Técnicas**
- Backend: `CreateVehiculoDto`, `UpdateVehiculoDto` con validación class-validator
- Backend: `VehiculoRepository` (Prisma queries con relación a TipoVehiculo)
- Backend: `VehiculoService` (lógica de negocio + validación de unicidad de placa)
- Backend: `VehiculoController` (GET, GET/:id, POST, PUT/:id, DELETE/:id)
- Frontend: Página de listado `/vehiculos` con filtros por estado y tipo
- Frontend: Formulario de creación/edición con select dinámico de tipo
- Frontend: Página de detalle `/vehiculos/[id]`

---

#### HU-02 — Gestión de Tipos de Vehículo (CU-01)

> Como administrador de flota, quiero gestionar el catálogo de tipos de vehículo con sus tarifas base, para poder categorizar la flota y definir precios diferenciados por tipo de unidad.

**Criterios de Aceptación**
- [ ] Se puede crear un tipo de vehículo con: nombre (sedán, SUV, camioneta, etc.), descripción y tarifa base por día.
- [ ] El nombre del tipo de vehículo es único.
- [ ] Se puede listar todos los tipos de vehículo.
- [ ] Se puede editar el nombre, descripción o tarifa de un tipo existente.
- [ ] No se puede eliminar un tipo de vehículo que tenga vehículos asociados; se retorna error descriptivo.

**Tareas Técnicas**
- Backend: DTOs, Repository, Service, Controller del módulo `TipoVehiculo`
- Frontend: Listado y formulario de tipos de vehículo con campo de tarifa

---

### Sprint 2 — Clientes, Reservas y Disponibilidad

> 📅 **Mar 30 → Abr 10** · 🚫 Festivos: Abr 2-3 (Semana Santa)

| # | Historia de Usuario | Labels | Issue |
|---|---|---|---|
| HU-03 | Registro de Clientes | `user-story` `backend` `frontend` | <!-- TODO --> |
| HU-04 | Consulta y Actualización de Clientes | `user-story` `backend` `frontend` | <!-- TODO --> |
| HU-05 | Creación de Reserva | `user-story` `backend` `frontend` | <!-- TODO --> |
| HU-06 | Verificación de Disponibilidad de Vehículos | `user-story` `backend` `frontend` | <!-- TODO --> |

**Entregables:**
- CRUD de Cliente con validación de documento y correo únicos
- Módulo de Reserva con lógica de verificación de solapamiento de fechas
- Endpoint de disponibilidad con cálculo de tarifa por rango de fechas

---

#### HU-03 — Registro de Clientes (CU-02)

> Como agente de alquiler, quiero registrar clientes con sus datos personales y licencia de conducción, para validar que el cliente está habilitado para alquilar un vehículo y mantener un historial de sus contratos.

**Criterios de Aceptación**
- [ ] Se puede crear un cliente con: nombres, apellidos, documento de identidad, correo electrónico, teléfono, dirección y número de licencia de conducción.
- [ ] El documento de identidad y el correo electrónico son únicos; si se duplican, el sistema retorna error.
- [ ] El correo electrónico debe tener formato válido.
- [ ] Se puede consultar la lista de todos los clientes con paginación.
- [ ] Se puede consultar un cliente por su ID y ver sus datos completos.
- [ ] Se puede editar la información de un cliente existente.
- [ ] Se puede desactivar un cliente; si tiene reservas activas, el sistema retorna error.

**Tareas Técnicas**
- Backend: `CreateClienteDto`, `UpdateClienteDto` con class-validator
- Backend: `ClienteRepository` (Prisma queries), `ClienteService`, `ClienteController`
- Frontend: Página de listado `/clientes`
- Frontend: Formulario de creación `/clientes/new` y detalle `/clientes/[id]`

---

#### HU-04 — Consulta y Actualización de Clientes (CU-02)

> Como agente de alquiler, quiero buscar, editar y actualizar la información de los clientes registrados, para mantener los datos de contacto y la vigencia de la licencia de conducción al día.

**Criterios de Aceptación**
- [ ] Se puede buscar clientes por nombre, documento o correo desde la interfaz web.
- [ ] Se puede editar la dirección, teléfono y datos de licencia de un cliente existente.
- [ ] Se puede ver el historial de reservas y contratos asociados a un cliente en su página de detalle.
- [ ] El sistema indica visualmente si la licencia de un cliente está próxima a vencer (menos de 30 días).
- [ ] Los cambios se reflejan inmediatamente en el listado y en los formularios de reserva.

**Tareas Técnicas**
- Backend: Endpoint `GET /clientes?search=...` con filtro por nombre/documento/correo
- Backend: Endpoint `GET /clientes/:id/reservas` para historial de reservas del cliente
- Frontend: Barra de búsqueda en listado de clientes
- Frontend: Indicador de licencia próxima a vencer en detalle del cliente

---

#### HU-05 — Creación de Reserva (CU-03)

> Como agente de alquiler o cliente, quiero crear una reserva seleccionando el vehículo, fecha de inicio y fecha de fin, para garantizar la disponibilidad del vehículo en el período deseado y calcular el costo preliminar.

**Criterios de Aceptación**
- [ ] Se puede crear una reserva seleccionando: cliente, vehículo, fecha de inicio y fecha de fin.
- [ ] El sistema verifica que el vehículo esté disponible (estado = disponible y sin reservas solapadas); si no, retorna error descriptivo.
- [ ] La fecha de fin debe ser posterior a la fecha de inicio.
- [ ] El sistema calcula automáticamente el costo estimado (días × tarifa base del tipo de vehículo).
- [ ] La reserva registra la fecha de creación y queda en estado 'pendiente' hasta la entrega del vehículo.
- [ ] Se puede cancelar una reserva pendiente; el vehículo vuelve a estado disponible.
- [ ] Se puede consultar el listado de reservas con filtro por estado y por cliente.

**Tareas Técnicas**
- Backend: `CreateReservaDto` con validación de fechas y FK de cliente y vehículo
- Backend: `ReservaService` — lógica de verificación de solapamiento de fechas
- Backend: `ReservaController` (GET, GET/:id, POST, PATCH/:id/cancelar)
- Frontend: Formulario de reserva con date pickers y cálculo de costo en tiempo real
- Frontend: Listado de reservas con filtros por estado y cliente

---

#### HU-06 — Verificación de Disponibilidad de Vehículos (CU-03)

> Como agente de alquiler, quiero consultar qué vehículos están disponibles para un rango de fechas y tipo específico, para ofrecer al cliente alternativas disponibles de forma ágil y precisa.

**Criterios de Aceptación**
- [ ] El resultado excluye vehículos con reservas solapadas o que estén en mantenimiento.
- [ ] La respuesta incluye la tarifa calculada para el rango de fechas solicitado.
- [ ] Si no hay vehículos disponibles del tipo solicitado, el sistema retorna mensaje claro con sugerencia de tipos alternativos.
- [ ] El frontend muestra los vehículos disponibles como tarjetas con foto, tipo, año y precio estimado.

**Tareas Técnicas**
- Backend: Query Prisma con lógica `NOT IN` reservas solapadas y estado ≠ mantenimiento
- Backend: Cálculo dinámico de tarifa en la respuesta del endpoint
- Frontend: Vista de disponibilidad con filtros de tipo y rango de fechas
- Frontend: Tarjetas de vehículo disponible con acción directa 'Reservar'

---

### Sprint 3 — Contratos, Devoluciones y Frontend base

> 📅 **Abr 13 → Abr 17** · 📝 Cierre Segundo Corte: Abr 17

| # | Historia de Usuario | Labels | Issue |
|---|---|---|---|
| HU-07 | Generación de Contrato de Alquiler | `user-story` `backend` | <!-- TODO --> |
| HU-08 | Registro de Entrega de Vehículo | `user-story` `backend` | <!-- TODO --> |
| HU-09 | Registro de Devolución del Vehículo | `user-story` `backend` `frontend` | <!-- TODO --> |

**Entregables:**
- Módulo de Contrato con cálculo de valor total y cambio de estado del vehículo
- Módulo de Devolución con comparación de estados y generación de cargos
- Common Module global (filtros, interceptores, pipes)
- Frontend: estructura Next.js, listados y formularios de entidades base

---

#### HU-07 — Generación de Contrato de Alquiler (CU-04)

> Como agente de alquiler, quiero generar automáticamente el contrato de alquiler al entregar el vehículo al cliente, para formalizar las condiciones pactadas, el valor total y proteger a la empresa ante posibles incidentes.

**Criterios de Aceptación**
- [ ] Al confirmar la entrega de un vehículo, el sistema genera un contrato vinculado a la reserva correspondiente.
- [ ] El contrato incluye: datos del cliente, datos del vehículo, fechas, valor total, condiciones y estado del vehículo al momento de entrega.
- [ ] El valor total se calcula como: (número de días) × (tarifa base del tipo de vehículo).
- [ ] El contrato queda en estado 'activo' desde la entrega y el vehículo cambia a estado 'reservado'.
- [ ] Solo se puede generar un contrato por reserva; si ya existe, el sistema retorna error.
- [ ] Se puede consultar el contrato por su ID o por la reserva asociada.

**Tareas Técnicas**
- Backend: `CreateContratoDto` con FK a `reservaId`, validación de contrato único por reserva
- Backend: `ContratoService` — cálculo de valor total y cambio de estado del vehículo
- Backend: `ContratoController` (GET, GET/:id, POST)
- Frontend: Formulario de generación de contrato con vista previa de datos
- Frontend: Página de detalle del contrato con opción de imprimir/exportar

---

#### HU-08 — Registro de Entrega de Vehículo (CU-04)

> Como agente de alquiler, quiero registrar el estado del vehículo al momento de la entrega al cliente, para tener un registro formal de las condiciones iniciales y evitar disputas al momento de la devolución.

**Criterios de Aceptación**
- [ ] Al generar el contrato se registra el estado actual del vehículo: nivel de combustible, kilómetros y observaciones generales.
- [ ] El agente puede marcar hasta 5 puntos de daño previo en un diagrama simplificado del vehículo.
- [ ] Las observaciones de entrega se almacenan en el contrato y son visibles al momento de la devolución.
- [ ] El cliente o agente confirma la entrega; el contrato pasa a estado 'activo'.
- [ ] No se puede modificar el registro de entrega una vez confirmado.

**Tareas Técnicas**
- Backend: Campos `nivelCombustibleEntrega`, `kilometrajeEntrega`, `observacionesEntrega` en modelo Contrato
- Backend: Endpoint `PATCH /contratos/:id/confirmar-entrega`
- Frontend: Formulario de check-in del vehículo con campos de estado
- Frontend: Vista de confirmación con resumen de condiciones registradas

---

#### HU-09 — Registro de Devolución del Vehículo (CU-05)

> Como agente de alquiler, quiero registrar la devolución del vehículo con revisión detallada del estado actual, para identificar daños o condiciones que generen cargos adicionales y cerrar el contrato formalmente.

**Criterios de Aceptación**
- [ ] Se puede registrar la devolución vinculada a un contrato activo, indicando: fecha real de devolución, nivel de combustible, kilómetros y observaciones de estado.
- [ ] El sistema compara el estado de devolución con el estado de entrega para identificar diferencias.
- [ ] Si la devolución se realiza después de la fecha fin pactada, se calcula automáticamente el cargo por días adicionales.
- [ ] Al confirmar la devolución, el contrato pasa a estado 'cerrado' y el vehículo a estado 'disponible'.
- [ ] Se puede consultar el historial de devoluciones de un vehículo.
- [ ] Si hay cargos adicionales, se genera automáticamente un resumen de cobro.

**Tareas Técnicas**
- Backend: `CreateDevolucionDto` con FK a `contratoId` y validaciones de estado
- Backend: `DevolucionService` — comparación de estados entrega/devolución, cambio de estado del vehículo y contrato
- Backend: `DevolucionController` (GET, GET/:id, POST, PATCH/:id/confirmar)
- Frontend: Formulario de devolución con comparación visual de estados
- Frontend: Resumen de cargos adicionales antes de confirmar

---

### Sprint 4 — Frontend avanzado e integración

> 📅 **Abr 20 → May 8** · 🚫 Festivo: May 1 (Día del Trabajo)

| # | Historia de Usuario | Labels | Issue |
|---|---|---|---|
| HU-10 | Cálculo y Gestión de Cargos Adicionales | `user-story` `backend` `frontend` | <!-- TODO --> |

**Entregables:**
- Formularios con selects dinámicos encadenados (tipo vehículo → vehículo → cliente)
- Vista de disponibilidad con tarjetas de vehículo y acción directa de reserva
- Tabla editable de cargos adicionales con totalización en tiempo real
- Layout general con sidebar/navbar y navegación entre secciones
- Componentes de feedback (toast/alert de éxito/error)

---

#### HU-10 — Cálculo y Gestión de Cargos Adicionales (CU-05)

> Como administrador del sistema, quiero que el sistema calcule automáticamente los cargos adicionales por daños, días extra o combustible faltante, para emitir la factura final precisa al cliente y mantener un registro auditable de los cobros.

**Criterios de Aceptación**
- [ ] El sistema calcula automáticamente el cargo por días adicionales: días extra × tarifa base × 1.5 (penalización).
- [ ] El agente puede registrar cargos manuales por daños con descripción y monto.
- [ ] El cargo por combustible faltante se calcula proporcionalmente según la diferencia entre nivel de entrega y devolución.
- [ ] El total final = valor base del contrato + cargos adicionales automáticos + cargos manuales.
- [ ] El sistema genera un resumen detallado de cargos que puede exportarse o imprimirse.
- [ ] Los cargos quedan registrados en la devolución y son parte del historial del cliente.

**Tareas Técnicas**
- Backend: Modelo `CargoAdicional` en Prisma (devolucionId, tipo, descripción, monto)
- Backend: Lógica de cálculo automático en `DevolucionService` (días extra, combustible)
- Backend: Endpoint `POST /devoluciones/:id/cargos` para cargos manuales
- Frontend: Tabla editable de cargos adicionales con totalización en tiempo real
- Frontend: Vista de resumen final antes de confirmar cierre del contrato

---

### Sprint 5 — Cierre y despliegue

> 📅 **May 11 → May 22** · 🚫 Festivo: May 18 (Día de la Ascensión) · 📝 Cierre Tercer Corte: May 22

| # | Historia de Usuario | Labels | Issue |
|---|---|---|---|
| HU-11 | Integración Final y Despliegue con Docker | `user-story` `infraestructura` | <!-- TODO --> |

**Entregables:**
- Integración de flujos completos (registrar vehículo → crear cliente → crear reserva → generar contrato → registrar devolución → calcular cargos)
- Pruebas de integración
- Docker Compose validación final
- README y documentación

---

## 📅 Cronograma
```
┌──────────────────────────────────────────────────────────────────────────────┐
│              SEGUNDO CORTE (Release 1) — Cierre: 17 Abr 2026               │
│                        Backend + Frontend Base                              │
├─────────────────────┬─────────────────────┬──────────────────────────────────┤
│      Sprint 1       │      Sprint 2       │          Sprint 3               │
│  Mar 16 → Mar 29    │  Mar 30 → Abr 10    │    Abr 13 → Abr 17             │
│                     │                     │                                 │
│ • Docker + Prisma   │ • Cliente           │ • Contrato                      │
│ • Vehiculo          │ • Reserva           │ • Devolución                    │
│ • TipoVehiculo      │ • Disponibilidad    │ • Cargos Adicionales            │
│                     │                     │ • Frontend: listados y forms    │
│                     │                     │                                 │
│ 🚫 Mar 23          │ 🚫 Abr 2-3         │                                 │
│   (San José)        │   (Semana Santa)    │                                 │
├─────────────────────┴─────────────────────┴──────────────────────────────────┤
│              TERCER CORTE (Release 2) — Cierre: 22 May 2026                │
│                        Integración y Despliegue                            │
├────────────────────────────────────┬─────────────────────────────────────────┤
│          Sprint 4                  │             Sprint 5                   │
│       Abr 20 → May 8              │          May 11 → May 22               │
│                                    │                                        │
│ • Frontend reservas completas      │ • Integración de flujos               │
│ • Formularios con selects          │ • Pruebas de integración              │
│   dinámicos                        │ • Docker Compose validación           │
│ • Cargos adicionales               │ • README y documentación              │
│ • Navegación y layout              │                                        │
│                                    │                                        │
│ 🚫 May 1                          │ 🚫 May 18                             │
│   (Día del Trabajo)               │   (Día de la Ascensión)               │
└────────────────────────────────────┴─────────────────────────────────────────┘
```

### Festivos Colombianos (Marzo — Mayo 2026)

| Fecha | Festivo | Sprint Afectado |
|---|---|---|
| Lunes 23 de Marzo | Día de San José | Sprint 1 |
| Jueves 2 de Abril | Jueves Santo | Sprint 2 |
| Viernes 3 de Abril | Viernes Santo | Sprint 2 |
| Viernes 1 de Mayo | Día del Trabajo | Sprint 4 |
| Lunes 18 de Mayo | Día de la Ascensión | Sprint 5 |

---

## ✅ Definition of Done (DoD)

Cada Historia de Usuario se considera **terminada** cuando cumple **todos** los siguientes criterios:

### Backend
- [ ] Endpoint(s) implementados con arquitectura en capas: Controller → Service → Repository
- [ ] DTOs con validaciones usando `class-validator` y `class-transformer`
- [ ] Manejo de errores con excepciones HTTP apropiadas (`NotFoundException`, `ConflictException`, `BadRequestException`)
- [ ] Respuestas con formato uniforme (interceptor aplicado)
- [ ] Endpoint probado manualmente con Postman/Thunder Client

### Frontend
- [ ] Página(s) implementada(s) con componentes reutilizables
- [ ] Consumo del API a través de la capa de `services/`
- [ ] Manejo de estados: carga (loading), éxito y error
- [ ] Formularios con validación del lado del cliente
- [ ] Diseño responsivo y navegable

### Infraestructura y Código
- [ ] Código versionado en GitHub con commits descriptivos
- [ ] El servicio funciona correctamente con `docker compose up`
- [ ] No hay errores de consola ni advertencias críticas
- [ ] Las migraciones de Prisma están aplicadas y el esquema es consistente

---

## ⚙ Instalación y Ejecución

### Prerrequisitos

- [Docker](https://www.docker.com/products/docker-desktop/) y Docker Compose instalados
- [Git](https://git-scm.com/downloads)

### Clonar el repositorio
```bash
git clone https://github.com/equipos/gestion-alquiler-vehiculos.git
cd gestion-alquiler-vehiculos
```

### Configurar variables de entorno
```bash
cp .env.example .env
```
```env
# .env.example
DB_USER=admin
DB_PASSWORD=admin123
DB_NAME=alquiler_vehiculos_db
```

### Levantar los servicios
```bash
# Levantar todos los servicios con Docker Compose
docker compose up

# O en modo detached (segundo plano)
docker compose up -d
```

### Acceder a los servicios

| Servicio | URL |
|---|---|
| **Frontend (Next.js)** | http://localhost:3000 |
| **Backend (NestJS)** | http://localhost:3001 |
| **PostgreSQL** | `localhost:5432` |

---

## 📊 Estado del Proyecto

- [x] Plan de releases revisado y aprobado
- [x] Historias de Usuario revisadas y aprobadas
- [x] Criterios de Aceptación revisados y aprobados
- [x] Definition of Done revisado y aprobado
- [x] Modelo de datos revisado y aprobado
- [x] Repositorio GitHub creado con Issues y Milestones
