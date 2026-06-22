# Estándares de Diagramas de Arquitectura

**Última actualización**: 2026-06-21
**Versión**: 1.1

---

## 🎨 1. PALETA CORPORATIVA

| Categoría           | Color          | Código Hex | Uso                                                              |
| ------------------- | -------------- | ---------- | ---------------------------------------------------------------- |
| **System**          | Azul           | `#B2CEFF`  | Sistemas internos (boundaries ver §6)                            |
| **App**             | Morado claro   | `#E1D5E7`  | APIs, servicios, workers, frontends                              |
| **Store**           | Coral claro    | `#F8CECC`  | DBs, colas, event bus, storage                                   |
| **Component**       | Amarillo claro | `#FFF2CC`  | Componentes internos (C3)                                        |
| **Person**          | Verde claro    | `#D5E8D4`  | Usuarios internos, empleados                                     |
| **External Person** | Gris           | `#DFDFDF`  | Usuarios externos, clientes                                      |
| **External System** | Gris           | `#DFDFDF`  | Sistemas externos, third-party (genérico, shape variable)        |

**Regla de precedencia (External)**: Cuando un componente es de terceros, el color gris (`#DFDFDF`) **sobrescribe** el color de categoría (morado/coral). El **shape semántico se preserva** para indicar la naturaleza del componente (cilindro para DB, folder para object storage, rectángulo para app/SaaS, cilindro horizontal para message broker). Esta regla cubre también los casos de `External App` (rectángulo redondeado gris) y `External Store` (cilindro o folder gris) — no se necesitan filas separadas en §2.

---

## 📦 2. COMPONENTES ESTÁNDAR

**46 elementos totales — 30 componentes + 12 flechas + 3 boundaries + 1 leyenda**

### Actores

| Componente          | Shape | Icono | Color     | Tipo                | Uso                          |
| ------------------- | ----- | ----- | --------- | ------------------- | ---------------------------- |
| **Person**          | Actor | user  | `#D5E8D4` | `[Person]`          | Usuarios internos, empleados |
| **External Person** | Actor | user  | `#DFDFDF` | `[External Person]` | Usuarios externos, clientes  |

### Sistemas

| Componente          | Shape                                                           | Icono       | Color     | Tipo                | Uso                             |
| ------------------- | --------------------------------------------------------------- | ----------- | --------- | ------------------- | ------------------------------- |
| **System**          | Rectángulo redondeado                                           | server      | `#B2CEFF` | `[System]`          | Sistemas internos               |
| **External System** | Rectángulo redondeado                                           | cloud       | `#DFDFDF` | `[External System]` | Sistemas externos, third-party  |
| **Agrupador de Sistema** | Rectángulo (borde discontinuo, sin relleno) | layer-group | `#666666` (borde) | c4Type=`SystemScopeBoundary` | C2 — define el límite/alcance de un sistema mostrando sus containers internos |
| **Agrupador de Aplicación** | Rectángulo (borde discontinuo, sin relleno) | layer-group | `#666666` (borde) | c4Type=`ContainerScopeBoundary` | C3 — define el límite/alcance de un container mostrando sus componentes internos |
| **Agrupador** | Rectángulo (borde sólido, sin relleno) | layer-group | `#666666` (borde) | c4Type=`Boundary` | Multi-propósito: dominios, módulos, cloud, infraestructura |

### App (morado `#E1D5E7`)

| Componente          | Shape                 | Icono        | Uso                                                                 |
| ------------------- | --------------------- | ------------ | ------------------------------------------------------------------- |
| **Web Application** | Rectángulo redondeado | monitor      | Aplicación web                                                      |
| **Mobile App**      | Rectángulo redondeado | smartphone   | Aplicación móvil                                                    |
| **API**             | Rectángulo redondeado | code         | API REST/gRPC                                                       |
| **Microservice**    | Rectángulo redondeado | boxes        | Microservicio                                                       |
| **Worker**          | Rectángulo redondeado | cog          | Background worker                                                   |
| **Batch**           | Rectángulo redondeado | play         | Proceso batch/programado                                            |
| **CDC Processor**   | Rectángulo redondeado | sync         | Procesador CDC (Change Data Capture)                                |
| **API Gateway**     | Hexágono              | shield-half  | Gateway/Proxy de entrada                                            |
| **Function**        | Rectángulo redondeado | zap          | Función serverless (AWS Lambda, Azure Functions, GCP Cloud Functions) |
| **Aplicación**      | Rectángulo redondeado | app-window   | Aplicación genérica (escape hatch para casos no cubiertos) — c4Technology=`ej. Tecnología` |

### Store (coral `#F8CECC`)

| Componente              | Shape               | Icono        | Uso                                                                                  |
| ----------------------- | ------------------- | ------------ | ------------------------------------------------------------------------------------ |
| **Relational Database** | Cilindro vertical   | database     | Base de datos relacional (PostgreSQL, Oracle, SQL Server)                            |
| **NoSQL Database**      | Cilindro vertical   | table-2      | Base de datos NoSQL (DynamoDB)                                                       |
| **Cache**               | Cilindro vertical   | bolt         | Cache distribuido (Redis)                                                            |
| **Event Bus**           | Cilindro horizontal | radio-tower  | Bus de eventos pub/sub (Kafka, NATS, Pulsar)                                         |
| **Queue**               | Cilindro horizontal | list-ordered | Cola de mensajes (Point-to-Point)                                                    |
| **Object Storage**      | Folder              | folder-open  | Almacenamiento de objetos (S3, File Server)                                          |
| **Almacenamiento**      | Cilindro vertical   | archive      | Almacenamiento genérico (no específico) — c4Technology=`ej. Tecnología`              |

**Nota sobre patrones**: `Event Bus` implementa semántica pub/sub (varios consumers), `Queue` implementa point-to-point (un solo consumer). El resto de los Stores son neutrales respecto al patrón de acceso.

### Componentes C3 (amarillo `#FFF2CC`)

| Componente     | Shape                 | Icono       | Uso                                      |
| -------------- | --------------------- | ----------- | ---------------------------------------- |
| **Component**  | Rectángulo redondeado | box         | Componente genérico                      |
| **Controller** | Rectángulo redondeado | route       | Controller (MVC) - maneja requests       |
| **Service**    | Rectángulo redondeado | cog         | Service - lógica de negocio              |
| **Repository** | Rectángulo redondeado | database    | Repository - acceso a datos              |
| **Adapter**    | Rectángulo redondeado | plug        | Adapter - integración con externos       |
| **Client**     | Rectángulo redondeado | download    | Client - consume APIs de otros servicios |
| **Publisher**  | Rectángulo redondeado | send        | Publisher - publica eventos              |
| **Consumer**   | Rectángulo redondeado | inbox       | Consumer - consume eventos/mensajes      |
| **Mapper**     | Rectángulo redondeado | shuffle     | Mapper - transformación de datos         |
| **Validator**  | Rectángulo redondeado | check       | Validator - validación de datos          |

### Leyenda

La **Leyenda** es un meta-elemento opcional que explica la clave de colores y formas del diagrama. Se representa como un componente con `Title="Leyenda"`.

| Componente | Shape | Icono | Color | Uso |
| --- | --- | --- | --- | --- |
| **Leyenda** | Meta-elemento | n/a | n/a | Explica la clave de colores/shapes del diagrama |

---

## 🔗 3. FLECHAS Y CONECTORES

**Regla general**: Todas las flechas DEBEN incluir al menos **Propósito**.

**Por nivel:**

| Nivel      | Propósito   | Protocolo   |
| ---------- | ----------- | ----------- |
| **C1**     | Obligatorio | Opcional    |
| **C2**     | Obligatorio | Obligatorio |
| **C3**     | Obligatorio | Opcional    |
| **Deploy** | Obligatorio | Obligatorio |

### Tipos de flechas disponibles

| Tipo                | Línea             | Formato Etiqueta                                              |
| ------------------- | ----------------- | ------------------------------------------------------------- |
| **Relación**        | Sólida →          | `<Propósito>`<br>`[<Protocolo>]`                              |
| **Relación Simple** | Sólida →          | `<Propósito>`                                                 |
| **HTTPS**           | Sólida →          | `<Propósito>`<br>`[HTTPS]`                                    |
| **HTTP**            | Sólida →          | `<Propósito>`<br>`[HTTP]`                                     |
| **SOAP**            | Sólida →          | `<Propósito>`<br>`[SOAP]`                                     |
| **gRPC**            | Sólida →          | `<Propósito>`<br>`[gRPC]`                                     |
| **Evento**          | Discontinua - - → | `<Nombre evento (formato §9)>`<br>`<evt.payment.invoice.created>`<br>`[Kafka]` |
| **Queue**           | Discontinua - - → | `<Nombre mensaje>`<br>`[SQS]`                                 |
| **CDC**             | Discontinua - - → | `Capturar cambios`<br>`[CDC]`                                 |
| **Batch**           | Punteada · · · →  | `<Propósito proceso>`<br>`[Batch]`                            |
| **File Transfer**   | Sólida →          | `Transferir archivo`<br>`[SFTP]`                              |
| **Database Access** | Sólida delgada →  | `<Propósito consulta>`<br>`[SQL]`                             |

**Obligatorio:**

- Todas las flechas deben tener **Propósito** (texto descriptivo)
- Protocolo obligatorio en **C2 y Deployment**, opcional en **C1 y C3**
- Dirección clara de origen a destino

**Prohibido:**

- Flechas completamente vacías (sin propósito)
- Flechas bidireccionales (usar dos flechas separadas)

**Nota sobre eventos**: Los nombres de eventos siguen la nomenclatura de §9 — prefijo `tipo.dominio.entidad.acción`.

---

## 🎯 4. MODELO C4

### **C1 - Context Diagram**

**Incluir:**

- ✅ Tu sistema (como caja negra)
- ✅ Usuarios (internos y externos)
- ✅ Sistemas externos con los que interactúa
- ✅ Relaciones de alto nivel

**Excluir:**

- ❌ Internos del sistema (APIs, DBs)
- ❌ Tecnologías específicas
- ❌ Componentes
- ❌ **Límite recomendado**: 10 elementos. Exceder requiere división en múltiples diagramas.
- ❌ **Tope absoluto**: 25 elementos por diagrama (ver §8). Excepciones documentadas en el README del diagrama.

**Propósito**: Mostrar el sistema en su contexto - qué hace, quién lo usa, con qué se integra.

---

### **C2 - Container Diagram**

**Incluir:**

- ✅ Todos los containers del sistema (Apps y Stores)
- ✅ Tecnologías principales (.NET, PostgreSQL, Kafka)
- ✅ Relaciones entre containers
- ✅ Sistemas externos relevantes

**Separar containers:**

- **App** (morado): APIs, servicios, workers, frontends
- **Store** (coral): Bases de datos, colas, caches, storage

**Excluir:**

- ❌ Componentes internos
- ❌ Infraestructura (eso va en Deployment)
- ❌ **Límite recomendado**: 20 elementos. Exceder requiere división en múltiples diagramas.
- ❌ **Tope absoluto**: 25 elementos por diagrama (ver §8). Excepciones documentadas en el README del diagrama.

**Propósito**: Mostrar la arquitectura interna del sistema - aplicaciones, servicios, bases de datos y cómo se comunican.

---

### **C3 - Component Diagram**

**Crear C3 cuando se cumple AL MENOS UNA de estas condiciones:**

- ✅ **Servicio crítico de negocio** (auth, pagos, checkout, datos personales)
- ✅ **3 o más componentes con responsabilidades distintas** visibles en la estructura del código (no un CRUD thin)
- ✅ **2 o más integraciones externas no triviales** (no solo HTTP REST estándar sobre recursos propios)

**Excluir explícitamente (no crear C3):**

- ❌ CRUD simple sobre una sola tabla
- ❌ Thin wrapper / proxy sobre otro servicio
- ❌ El C2 ya muestra la responsabilidad del container con claridad

**Incluir:**

- ✅ Componentes de UN SOLO container
- ✅ Una responsabilidad principal por componente (un componente = un motivo de cambio)
- ✅ Relaciones entre componentes
- ✅ Stores que lee/escribe (la flecha indica la dirección del dato, no el componente)

**Separar componentes por capa:**

- **Capa de entrada** (Controller, Adapter, Client): recibe requests externos
- **Capa de negocio** (Service, Validator, Mapper): lógica y transformación
- **Capa de datos** (Repository): acceso a stores
- **Capa de integración** (Publisher, Consumer): eventos y colas

**Excluir:**

- ❌ Clases o métodos (C4 modela componentes, no unidades de código)
- ❌ Componentes de múltiples containers
- ❌ **Límite recomendado**: 12 componentes. Exceder requiere división en múltiples diagramas.
- ❌ **Tope absoluto**: 25 elementos por diagrama (ver §8). Excepciones documentadas en el README del diagrama.

**Propósito**: Mostrar la organización interna de un servicio/API específico - sus componentes y responsabilidades.

---

### **C4 - Code Diagram**

❌ **NO CREAR** diagramas de Nivel 4 (Code). Usar código fuente como documentación.

---

## 📏 5. REGLAS VISUALES

### Tipografía

- **Fuente**: Helvetica
- **Nombre elemento**: 20px (bold)
- **Descripción elemento**: 13px
- **Estereotipo/Tipo**: 11px
- **Etiqueta flecha**: 12px

### Estructura de Componentes

**Todos los componentes siguen este formato estándar:**

```
Nombre del Componente
[Tipo: Tecnología]
Descripción breve de responsabilidades.
```

**Línea 1**: Nombre descriptivo del componente
**Línea 2**: `[Categoría: Tecnología específica]`
**Línea 3**: Descripción de responsabilidades y propósito

**Categorías base:**

- `[Person]` - Usuario/rol interno
- `[External Person]` - Usuario/rol externo
- `[System]` - Sistema interno
- `[External System]` - Sistema externo
- `[App: tecnología]` - Aplicación, API, servicio, worker
- `[Store: tecnología]` - Base de datos, cache, queue, event bus, storage
- `[Component]` o `[Component: tipo]` - Componentes C3

**Regla importante**: La línea 2 debe especificar **tecnología real**, no tipo de componente.

✅ **Correcto**: `[App: .NET 8]`, `[App: Angular 17]`, `[Store: PostgreSQL]`
❌ **Incorrecto**: `[App: API]`, `[App: Service]`, `[Store: Database]`

**Excepción (C3)**: Para componentes C3, el formato es `[Component: tipo]` donde "tipo" indica la responsabilidad (Controller, Service, Repository, etc.). La tecnología se hereda del container padre documentado en C2.

**Ejemplos:**

```
Identity API
[App: .NET 8 REST API]
Gestiona autenticación, autorización y emisión de tokens.
```

```
identity-db
[Store: PostgreSQL]
Almacena usuarios, roles, permisos y configuraciones.
```

```
SAP ERP
[External System]
Sistema de planificación de recursos empresariales.
```

```
Analista de RRHH
[Person]
Gestiona información de empleados y procesos de RRHH.
```

### Shapes

- **Rectángulo redondeado**: APIs, servicios, workers, apps
- **Cilindro vertical**: Bases de datos, cache
- **Cilindro horizontal**: Message bus, colas, event bus
- **Folder**: Object storage, file storage
- **Hexágono**: API Gateway, Reverse Proxy
- **Boundary**: Agrupación de sistemas o módulos

### Estilos

- **Border Width**: 1.5px
- **Border Radius**: 10px
- **Sombras**: NO
- **Gradientes**: NO
- **Transparencias**: NO

### Estados Visuales

**Regla fundamental**: Un significado visual = un único concepto.

| Estado        | Representación                       | Uso                                          |
| ------------- | ------------------------------------ | -------------------------------------------- |
| **Actual**    | Borde sólido                         | Elementos implementados y en producción      |
| **Planeado**  | Borde discontinuo                    | Elementos futuros, roadmap, no implementados |
| **Externo**   | Color gris `#DFDFDF`                 | Usuarios y sistemas externos (third-party)   |
| **Deprecado** | Borde discontinuo + `<<Deprecated>>` | Elementos en fase de retiro (opcional)       |

**Notas**:

- Elementos planeados mantienen el color de su categoría (morado/coral) + borde discontinuo
- **Draw.io**: Dashed (1) + Border Width 1.5px
- **Deprecado (opcional)**: No existe elemento en la librería para este estado. Se representa con un componente estándar (cualquier categoría) con borde discontinuo + estereotipo `<<Deprecated>>` en la línea 2 entre corchetes, p. ej. `[App: .NET 8] <<Deprecated>>`. Es optativo — los equipos que no lo necesiten pueden omitirlo.

### Tamaños Estándar

**Tamaño mínimo recomendado**: 280px × 140px

| Categoría      | Tamaño mínimo     | Notas                                          |
| -------------- | ----------------- | ---------------------------------------------- |
| Sistemas       | 280px × 140px     | Ajustar según longitud de nombre y descripción |
| App            | 280px × 140px     | Ajustar según longitud de nombre y descripción |
| Store          | 280px × 140px     | Ajustar según longitud de nombre y descripción |
| Componentes C3 | 280px × 140px     | Ajustar según longitud de nombre y descripción |
| Actores        | 200px × 140px     | Suficiente para nombre de rol                  |
| Boundary       | Ajustar contenido | Debe contener todos los elementos agrupados    |

**Nota**: El tamaño puede ajustarse según el contenido manteniendo consistencia visual. Priorizar legibilidad sobre tamaño fijo.

### Iconografía

- **Icono**: Monocromático (gris o color del elemento)
- **Tamaño**: 18-20px
- **C1/C2**: Iconos genéricos (database, api, user)
- **Deployment**: Iconos tecnológicos (AWS, Azure) solo si es necesario

---

## 📐 6. LAYOUT Y COMPOSICIÓN

### Reglas de Layout

1. **Usuarios arriba o izquierda**
2. **Frontend arriba**
3. **Backend centro**
4. **Stores (DBs, queues) abajo**
5. **Sistemas externos a los lados**
6. **Flujo: izquierda → derecha, arriba → abajo**

### Boundaries

**Tipos de boundaries disponibles:**

| Boundary                    | Borde       | Uso principal                                                                                 | Ejemplo                                                            |
| --------------------------- | ----------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Agrupador de Sistema**    | Discontinuo | C2 - Define el límite/alcance de un sistema mostrando sus containers (apps y stores) internos | Identity System agrupa Identity API, Identity Service, identity-db |
| **Agrupador de Aplicación** | Discontinuo | C3 - Define el límite/alcance de un container mostrando sus componentes internos              | Identity API agrupa Controller, Service, Repository                |
| **Agrupador**               | Sólido      | Multi-propósito:<br>- C1/C2: Dominios, módulos<br>- Deployment: Cloud, infraestructura        | Identity Domain, AWS us-east-1, On-Premise Data Center             |

**Nombres recomendados:**

✅ **Usar**: Nombres descriptivos que indiquen el propósito
✅ **Ejemplos**:

- **System scope (C2)**: Identity System, Payment System, HR System
- **Container scope (C3)**: Identity API, Payment Service, HR Worker
- **Agrupación genérica**: Identity Domain, Payment Context, AWS us-east-1, Production EKS

❌ **Evitar**: Nombres genéricos o ambiguos como "Módulo 1", "Backend", "Servicios", "Otros"

**Reglas**:

- Etiqueta descriptiva que indique el propósito del agrupamiento
- NO anidar más de 2 niveles
- Usar inglés para consistencia con nombres técnicos
- **System scope**: Muestra la frontera de un sistema con sus containers
- **Container scope**: Muestra la frontera de un container con sus componentes
- **Boundary genérico**: Para cualquier otra agrupación lógica o de infraestructura

### Espaciado

- **Entre elementos**: 40-60px
- **Margen**: 30px mínimo
- **Evitar**: Elementos pegados, cruces de líneas

---

## ✅ 7. CHECKLIST DE VALIDACIÓN

1. [ ] **Título claro** - Nombre del sistema + tipo de diagrama
2. [ ] **Metadata** - Fecha, versión, owner
3. [ ] **Colores correctos** - Según tabla corporativa
4. [ ] **Shapes correctos** - Cilindro para DB, folder para storage
5. [ ] **Estructura de componentes** - `[Tipo: Tecnología]` en todos los elementos
6. [ ] **TODAS las flechas etiquetadas** - Protocolo + propósito
7. [ ] **Nombres descriptivos** - NO "Service 1", "API", "DB"
8. [ ] **Nivel correcto** - C1 sin internos, C2 sin componentes
9. [ ] **Límite de elementos** - Cumple los límites por nivel definidos en §8
10. [ ] **Layout limpio** - Sin cruces innecesarios, flujo claro
11. [ ] **Actualizado** - Refleja el estado real del sistema
12. [ ] **Convención de pestañas** - Si el archivo `.drawio` tiene pestañas exportables, sus nombres siguen el patrón `[Nombre del Sistema] - [Tipo]` (ver §15). Tabs `borrador`, `WIP`, etc. deben existir solo como pestañas no exportables.

---

## ❌ 8. PROHIBICIONES

### Prohibido

1. **Diagramas con más de 25 elementos** — Tope absoluto. Exceder requiere justificación documentada en el README del diagrama y aprobación de un revisor de Architecture. Límites recomendados por nivel: C1 ≤ 10, C2 ≤ 20, C3 ≤ 12. Deployment y diagramas de integración pueden acercarse al tope si documentan infraestructura real.
2. **Herramientas no autorizadas** - Usar exclusivamente Draw.io o las herramientas listadas en Sección 11. Diagramas one-off en otras herramientas se degradan rápido (no se versionan, no se reutilizan, no se enlazan con código).
3. **Mezclar niveles C4** - Un nivel por diagrama
4. **Diagramas sin metadata** - Incluir owner, fecha, versión
5. **Diagramas sin etiquetas** - Todos los elementos deben tener nombre descriptivo

---

## 🔧 9. NOMENCLATURA

### Servicios y APIs

✅ **Formato**: `[Dominio] Service` o `[Dominio] API`
✅ **Ejemplos**: Identity Service, Notification API, Payment Service
❌ **Evitar**: auth-svc, svc-notif, PaymentMS

### Patrones Frontend (BFF, MFE, SSR)

Para arquitecturas frontend complejas, aplicar el mismo estándar con el sufijo del patrón. Representar como un `[App]` estándar dentro de un **Boundary por dominio**.

✅ **Formato y ejemplos:**

| Patrón            | Formato         | Ejemplo       | Tipo                |
| ----------------- | --------------- | ------------- | ------------------- |
| **BFF**           | `[Dominio] BFF` | Checkout BFF  | `[App: Node.js]`    |
| **Microfrontend** | `[Dominio] MFE` | Billing MFE   | `[App: React 18]`   |
| **SSR Web**       | `[Dominio] Web` | Marketing Web | `[App: Next.js 14]` o `[App: Angular 17 SSR]` |

❌ **Evitar**: `bff-checkout-svc`, `mfe-billing-frontend`, `webapp-1`

**Nota**: El campo tecnología captura el stack real del equipo (Next.js, Angular SSR, Nuxt, Remix). El framework SSR ya implica React/Vue subyacente; no duplicar. No se añaden nuevos componentes a la librería oficial. BFF/MFE/SSR usan los mismos componentes `App` estándar. El ejemplo `[App: Next.js 14]` es ilustrativo — cualquier framework SSR válido se acepta como `[App: <framework> <versión>]`.

### Bases de Datos

✅ **Formato**: `[dominio]-db` o `[Dominio] Database`
✅ **Ejemplos**: identity-db, payment-db, notification-db
❌ **Evitar**: DB1, database, payments_production

### Cache

✅ **Formato**: `[dominio]-cache` o `[Dominio] Cache`
✅ **Ejemplos**: session-cache, payment-cache, identity-cache
❌ **Evitar**: redis, cache1, temp-storage

### Eventos Kafka

✅ **Formato**: `[tipo].[dominio].[entidad].[acción]`
✅ **Ejemplos**: `cdc.hr.employee.changed`, `evt.payment.invoice.created`, `cmd.notification.email.send`
❌ **Evitar**: event1, notification, kafka-topic

**Prefijos permitidos:**

| Prefijo | Significado         | Ejemplo                       |
| ------- | ------------------- | ----------------------------- |
| `evt.`  | Evento de dominio   | `evt.payment.invoice.created` |
| `cmd.`  | Comando (intención) | `cmd.notification.email.send` |
| `cdc.`  | Change Data Capture | `cdc.hr.employee.changed`     |
| `dlq.`  | Dead Letter Queue   | `dlq.payment.failed`          |

### Reglas Generales

- ✅ Nombres en **singular** (User Service, no Users Service)
- ✅ **Inglés** para elementos técnicos
- ✅ Descriptivo (qué hace, no tecnología)
- ❌ NO abreviaciones (excepto: API, DB, HTTP, CDC, ETL)

---

## 📊 10. TIPOS DE DIAGRAMAS

| Tipo de Diagrama      | Estado                   | Cuándo Usar                                                                                              | Herramienta              |
| --------------------- | ------------------------ | -------------------------------------------------------------------------------------------------------- | ------------------------ |
| **C1 - Context**      | ✅ Obligatorio           | Todo sistema nuevo. Sistemas existentes sin diagrama C1 deben crearlo.                                   | Draw.io                  |
| **C2 - Container**    | ✅ Obligatorio           | Todo sistema nuevo. Sistemas existentes sin diagrama C2 deben crearlo.                                   | Draw.io                  |
| **C3 - Component Diagram** | ⚠️ Condicional     | Servicio crítico, 3+ componentes o 2+ integraciones no triviales (ver §4)                               | Draw.io                  |
| **Deployment**        | ✅ Obligatorio           | Todo sistema en producción, nuevo o existente.                                                          | Draw.io                  |
| **Sequence**          | ✅ Recomendado           | Flujos críticos (auth, payments, checkout)                                                               | PlantUML, Mermaid        |
| **Integration**       | ✅ Recomendado           | Integraciones complejas con múltiples sistemas                                                           | Draw.io                  |
| **Data Flow**         | ✅ Recomendado           | Arquitecturas data-intensive (CDC, ETL, pipelines)                                                       | Draw.io                  |
| **Network**           | ✅ Recomendado           | Diseño de conectividad y seguridad de red                                                                | Draw.io                  |
| **Infrastructure**    | ✅ Recomendado           | Recursos cloud específicos (complementa Deployment)                                                      | Draw.io, AWS/Azure Icons |
| **C4 - Code**         | ❌ No crear              | Usar código fuente como documentación                                                                    | N/A                      |
| **UML Class**         | ❌ No recomendado        | C4 no es UML - usar C3 Component en su lugar                                                             | N/A                      |
| **UML Component**     | ❌ No recomendado        | Usar C3 Component con notación C4                                                                        | N/A                      |
| **Flowchart/Proceso** | ⚠️ Usar con precaución   | Solo para procesos de negocio, no arquitectura técnica                                                   | Draw.io, Lucidchart      |

### Notas

**Obligatorio** = Requerido para todo sistema en ADRs y documentación arquitectónica

**Condicional** = Requerido solo cuando aplica la condición especificada

**Recomendado** = Útil y recomendado cuando agrega valor, no obligatorio

**No crear** = Explícitamente prohibido por este estándar

**No recomendado** = Permitido técnicamente pero preferir alternativas sugeridas

---

## 🛠️ 11. HERRAMIENTAS

### Herramienta Oficial: Draw.io

**Setup obligatorio:**

1. Instalar desde: <https://draw.io>
2. Cargar librería: File → Open Library → `TLM - Librería C4.xml`
3. Crear nuevo diagrama en Draw.io

**Versionamiento:**

- Guardar `.drawio` en Git (obligatorio)
- Exportar `.png` para README/documentación

### Herramientas Autorizadas (casos específicos)

- **Sequence Diagrams**: PlantUML, Mermaid
- **Cloud Diagrams**: AWS Architecture Icons, Azure Icons

### Workflow de Exportación Automática

El export manual de `.drawio` → `.png` se desactualiza fácilmente. Se usa un **workflow compartido** para automatizar la exportación de PNGs por cada PR que modifica un archivo `.drawio`.

**Repositorio compartido**: `tlm-org/diagram-export-workflow` (pendiente de crear — el equipo de Arquitectura confirmará el path antes de habilitar la adopción por equipos)

**Adopción**: Cada equipo opta por usar el workflow referenciándolo en su `.github/workflows/`.

**Trigger**:

```yaml
on:
  pull_request:
    paths:
      - '**.drawio'
```

**Permisos**: `contents: write`

**Comportamiento**:

1. Por cada pestaña del `.drawio` que matchee el patrón de §15, se exporta un PNG
2. Las pestañas que **no** matcheen el patrón (§15) se ignoran silenciosamente
3. Si el PNG generado es byte-identical al ya existente en el branch, no se hace commit
4. Los PNGs se hacen commit al **mismo branch** del PR — nunca a `main`
5. El workflow nunca abre un nuevo PR ni crea issues

**Checklist PR** (nuevo item):

- [ ] Si se modificó un `.drawio`, ¿el workflow generó/actualizó el `.png` en este PR?

---

## 🚀 12. PROCESO DE CREACIÓN

1. Crear nuevo diagrama en Draw.io
2. Usar componentes de librería corporativa
3. Aplicar paleta de colores (Sección 1)
4. Etiquetar todos los elementos y flechas
5. Validar con checklist (Sección 7)
6. Guardar `.drawio` en Git con el PR

---

## 🏗️ 13. DEPLOYMENT DIAGRAM

**Obligatorio** para TODO sistema en producción.

### Elementos de Infraestructura

| Elemento          | Shape                 | Color     | Uso                             |
| ----------------- | --------------------- | --------- | ------------------------------- |
| **Pod/Container** | Rectángulo redondeado | `#E1D5E7` | Contenedor individual           |
| **VM/EC2**        | Rectángulo            | `#E8EAF6` | Máquina virtual                 |
| **Load Balancer** | Hexágono | `#E1D5E7` | Balanceador de carga (ALB, NLB). Color: morado App (categoría App — API Gateway managed). |

**Nota**: Para Databases, Object Storage y Message Brokers, usar los componentes **Store** definidos en la Sección 2.

**Regla de color en Deployment**: Los componentes mantienen su color de **categoría lógica** cuando se despliegan como contenedores/pods propios. Solo cambian a color de infraestructura cuando son recursos cloud gestionados.

**Aplicación:**

- Kong/API Gateway (pod propio) → **morado** (es App)
- AWS ALB / NLB (gestionado) → **morado** (es API Gateway managed, no infraestructura de red)
- Debezium CDC (pod propio) → **morado** (es App)
- AWS RDS (gestionado) → **coral** (es Store)

### Reglas de Deployment

1. **Mostrar infraestructura real** - No conceptual
2. **Incluir regiones/AZs** - Para HA/DR
3. **Etiquetar conexiones** - Puertos, protocolos, security groups
4. **Indicar réplicas** - `x3` para múltiples instancias
5. **Separar ambientes** - Prod, staging, dev en diagramas distintos

### Estereotipos de Infraestructura (opcionales)

| Tipo              | Estereotipo           | Ejemplo                        |
| ----------------- | --------------------- | ------------------------------ |
| **Pod**           | `<<Pod>>`             | `<<Pod>>` identity-api (x3)    |
| **VM**            | `<<EC2>>` o `<<VM>>`  | `<<EC2>>` Bastion Host         |
| **Load Balancer** | `<<ALB>>` o `<<NLB>>` | `<<ALB>>` Public Load Balancer |

**Nota**: Para Databases (RDS, DynamoDB), usar componentes **Store** con estereotipo cloud opcional: `<<RDS>>`, `<<DynamoDB>>`, etc.

### Información Requerida

- **Nombre del ambiente**: Production, Staging, Development
- **Región/AZ**: us-east-1a, us-east-1b
- **Réplicas**: Número de instancias (x3, x5)
- **Puertos**: 443, 8080, 5432
- **Protocolos**: HTTPS, TCP
- **Volúmenes**: EBS, persistent volumes

### Ejemplo de Etiquetado

```
Load Balancer → Pod: HTTPS:443
Pod → RDS: PostgreSQL:5432
Pod → Kafka: TCP:9092
```

---

## 📚 14. DOCUMENTACIÓN DE REFERENCIA

- [Validation Criteria](./reference/validation-criteria.md) - Checklists para PR y auditorías
- [Best Practices](./reference/c4-best-practices.md) - Principios y anti-patrones
- [Cheat Sheet](./reference/cheat-sheet.md) - Referencia rápida de una página
- [Contribution Guide](./reference/contribution-guide.md) - Cómo proponer cambios al estándar

---

## 📑 15. CONVENCIÓN DE PESTAÑAS

### Propósito

El workflow de CI/CD (ver §11) necesita identificar automáticamente qué pestañas de un archivo `.drawio` representan diagramas exportables. Esta convención define el patrón que el workflow usa para distinguir pestañas exportables de borradores y notas.

### Patrón de nombres

`^([^ ].*) - (Context|Container|Component|Deployment|Sequence|Integration|Data Flow|Network|Infrastructure)$`

| Parte | Descripción |
| --- | --- |
| `([^ ].*)` | `[Nombre del Sistema]`: cualquier string no vacío que no empiece con espacio |
| ` - ` | Separador literal (espacio-guión-espacio) |
| `[Tipo]` | Enum exacto: `Context`, `Container`, `Component`, `Deployment`, `Sequence`, `Integration`, `Data Flow`, `Network`, `Infrastructure` |

### Ejemplos correctos ✅

- `Payment System - Container`
- `Identity - Context`
- `Real-Time Order Processing - Context`

### Ejemplos incorrectos ❌

- `borrador` — falta el separador ` - ` y el Tipo
- ` - Container` — nombre de sistema vacío
- `Payment_Container` — separador incorrecto (debe ser ` - ` no `_`)
- `Payment - Wireframe` — `Wireframe` no es un Tipo permitido
- `Identity - C2` — `C2` no es un Tipo válido (usar `Container`)

### Comportamiento del workflow

Pestañas que **no** matchean el patrón son **ignoradas silenciosamente** — sin warning, sin error, sin PNG generado. Pestañas `borrador`, `WIP`, `WIP - Context`, etc. deben crearse como pestañas **no exportables** en Draw.io.

---

## 📞 Contacto y Soporte

- **Slack**: `#architecture`
- **Email**: <architecture@company.com>
- **Librería Draw.io**: [Actualizar aquí](./drawio-library/)

---

**Mantenedores**: Architecture Team
