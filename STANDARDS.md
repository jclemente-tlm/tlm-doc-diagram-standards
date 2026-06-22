# Estándares de Diagramas de Arquitectura

**Última actualización**: 2026-06-22
**Versión**: 2.0

---

## 📜 Historial de versiones

| Versión | Fecha       | Autor            | Cambios principales                                                                                       |
|---------|-------------|------------------|-----------------------------------------------------------------------------------------------------------|
| 2.0     | 2026-06-22  | Architecture Team | Revisión completa: 4 secciones nuevas, paleta ampliada, separación de agrupadores, sistema de tags de estado, glosario, anti-patrones y ejemplos end-to-end |
| 1.1     | 2026-06-21  | Architecture Team | Workflow CI export, convención de pestañas                                                                 |
| 1.0     | anterior a 2026-06 | Architecture Team | Versión inicial                                                                                     |

---

## 🎨 1. PALETA CORPORATIVA

| Categoría           | Color          | Código Hex | Uso                                                                                       |
| ------------------- | -------------- | ---------- | ----------------------------------------------------------------------------------------- |
| **System**          | Azul           | `#B2CEFF`  | Sistemas internos                                                                         |
| **App**             | Morado claro   | `#E1D5E7`  | APIs, servicios, workers, frontends                                                       |
| **Store**           | Coral claro    | `#F8CECC`  | DBs, colas, event bus, storage                                                            |
| **Component**       | Amarillo claro | `#FFF2CC`  | Componentes internos (C3)                                                                 |
| **Person**          | Verde claro    | `#D5E8D4`  | Usuarios internos, empleados                                                              |
| **External Person** | Gris           | `#DFDFDF`  | Usuarios externos, clientes                                                               |
| **External System** | Gris           | `#DFDFDF`  | Sistemas externos, third-party (genérico, shape variable)                                 |
| **Infrastructure**  | Lavanda        | `#E8EAF6`  | VMs/EC2 en Deployment                                                                     |
| **Boundary**        | Gris oscuro    | `#666666`  | Bordes de agrupadores y boundaries (sin relleno)                                          |

**Regla de precedencia (External)**: Cuando un componente es de terceros, el color gris (`#DFDFDF`) **sobrescribe** el color de categoría (morado/coral). El **shape semántico se preserva** para indicar la naturaleza del componente (cilindro para DB, folder para object storage, rectángulo para app/SaaS, cilindro horizontal para message broker). Esta regla cubre también los casos de `External App` (rectángulo redondeado gris) y `External Store` (cilindro o folder gris).

**Regla de color en Deployment**: Los componentes mantienen su color de **categoría lógica** cuando se despliegan como contenedores/pods propios. Solo cambian a color de infraestructura cuando son recursos cloud gestionados (ver §14).

### Accesibilidad

- La diferenciación "externo" depende del color gris (`#DFDFDF`). En impresiones B/N o para personas con ceguera al color, añadir etiqueta textual `<<External>>` en la línea 2 o usar shape distintivo.
- Verificar contraste mínimo WCAG AA (4.5:1) en etiquetas sobre fondos de color de categoría.

---

## 📦 2. COMPONENTES ESTÁNDAR

**47 elementos totales**

Librería (35):

- 2 actores (Person, External Person)
- 2 sistemas (System, External System)
- 3 agrupadores (boundaries)
- 10 App
- 7 Store
- 10 componentes C3
- 1 leyenda

Flechas (12) → ver §3

### Actores

| Componente          | Shape | Icono | Color     | Tipo                | Uso                          |
| ------------------- | ----- | ----- | --------- | ------------------- | ---------------------------- |
| **Person**          | Actor | user  | `#D5E8D4` | `[Person]`          | Usuarios internos, empleados |
| **External Person** | Actor | user  | `#DFDFDF` | `[External Person]` | Usuarios externos, clientes  |

### Sistemas

| Componente          | Shape                 | Icono  | Color     | Tipo                | Uso                             |
| ------------------- | --------------------- | ------ | --------- | ------------------- | ------------------------------- |
| **System**          | Rectángulo redondeado | server | `#B2CEFF` | `[System]`          | Sistemas internos               |
| **External System** | Rectángulo redondeado | cloud  | `#DFDFDF` | `[External System]` | Sistemas externos, third-party  |

### Agrupadores (Boundaries)

Los agrupadores son constructos de visualización, no entidades de primera clase: su función es delimitar alcances (sistema, container, dominio, infraestructura). Visualmente se representan como rectángulos **sin relleno**, con borde sólido o discontinuo según el tipo.

| Componente                  | Borde       | Icono          | Color (borde) | c4Type                       | Uso                                                                              |
| --------------------------- | ----------- | -------------- | ------------- | ---------------------------- | -------------------------------------------------------------------------------- |
| **Agrupador de Sistema**    | Discontinuo | layer-group    | `#666666`     | `SystemScopeBoundary`        | C2 — define el límite/alcance de un sistema mostrando sus containers internos    |
| **Agrupador de Aplicación** | Discontinuo | layer-group    | `#666666`     | `ContainerScopeBoundary`     | C3 — define el límite/alcance de un container mostrando sus componentes internos |
| **Agrupador**               | Sólido      | layer-group    | `#666666`     | `Boundary`                   | Multi-propósito: dominios, módulos, cloud, infraestructura                       |

**Nombres recomendados:**

- **System scope (C2)**: Identity System, Payment System, HR System
- **Container scope (C3)**: Identity API, Payment Service, HR Worker
- **Agrupación genérica**: Identity Domain, Payment Context, AWS us-east-1, Production EKS

❌ **Evitar**: nombres genéricos o ambiguos como "Módulo 1", "Backend", "Servicios", "Otros"

**Reglas:**

- Etiqueta descriptiva que indique el propósito del agrupamiento.
- NO anidar más de 2 niveles.
- Usar inglés para consistencia con nombres técnicos.
- **System scope**: muestra la frontera de un sistema con sus containers.
- **Container scope**: muestra la frontera de un container con sus componentes.
- **Boundary genérico**: para cualquier otra agrupación lógica o de infraestructura.

### App (morado `#E1D5E7`)

| Componente          | Shape                 | Icono        | Uso                                                                  |
| ------------------- | --------------------- | ------------ | -------------------------------------------------------------------- |
| **Web Application** | Rectángulo redondeado | monitor      | Aplicación web                                                       |
| **Mobile App**      | Rectángulo redondeado | smartphone   | Aplicación móvil                                                     |
| **API**             | Rectángulo redondeado | code         | API REST/gRPC                                                        |
| **Microservice**    | Rectángulo redondeado | boxes        | Microservicio                                                        |
| **Worker**          | Rectángulo redondeado | cog          | Background worker                                                    |
| **Batch**           | Rectángulo redondeado | play         | Proceso batch/programado                                             |
| **CDC Processor**   | Rectángulo redondeado | sync         | Procesador CDC (Change Data Capture)                                 |
| **API Gateway**     | Hexágono              | shield-half  | Gateway/Proxy de entrada                                             |
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

### Reglas para External

Las siguientes reglas consolidan el tratamiento de elementos externos (third-party) en cualquier contexto. Aplican a diagramas C1, C2, C3 y Deployment.

**Color y shape:**

| Tipo externo                | Shape                 | Icono       | Color     | Ejemplos                              |
| --------------------------- | --------------------- | ----------- | --------- | ------------------------------------- |
| **External App / SaaS**     | Rectángulo redondeado | cloud / app | `#DFDFDF` | Salesforce, Workday, Kong externo     |
| **External Store (DB)**     | Cilindro vertical     | database    | `#DFDFDF` | RDS externo, Aurora, Cosmos externo   |
| **External Store (Obj.)**   | Folder                | folder-open | `#DFDFDF` | S3 externo, Azure Blob                |
| **External Store (Broker)** | Cilindro horizontal   | radio-tower | `#DFDFDF` | Confluent Cloud, Amazon MQ            |
| **External System**         | Rectángulo redondeado | cloud       | `#DFDFDF` | Sistema genérico third-party          |
| **External Person**         | Actor                 | user        | `#DFDFDF` | Cliente final, usuario externo        |

**Reglas:**

- El color gris (`#DFDFDF`) **sobrescribe** el color de categoría (ver §1).
- El shape semántico se preserva (cilindro para DB, folder para object storage, cilindro horizontal para broker, etc.).
- En diagramas de **Deployment** (ver §14), los recursos cloud gestionados externos usan el mismo shape semántico y pueden llevar estereotipo del proveedor (`<<RDS>>`, `<<S3>>`, etc.) para clarificar.
- Los nombres externos deben identificar claramente al proveedor o sistema (ej. `AWS S3`, `Salesforce CRM`).

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

| Tipo                | Línea             | Formato Etiqueta                                                  |
| ------------------- | ----------------- | ----------------------------------------------------------------- |
| **Relación**        | Sólida →          | `<Propósito>`<br>`[<Protocolo>]`                                  |
| **Relación Simple** | Sólida →          | `<Propósito>`                                                     |
| **HTTPS**           | Sólida →          | `<Propósito>`<br>`[HTTPS]`                                        |
| **HTTP**            | Sólida →          | `<Propósito>`<br>`[HTTP]`                                         |
| **SOAP**            | Sólida →          | `<Propósito>`<br>`[SOAP]`                                         |
| **gRPC**            | Sólida →          | `<Propósito>`<br>`[gRPC]`                                         |
| **Event**           | Discontinua - - → | `<Nombre evento (formato §10)>`<br>`<evt.payment.invoice.created>`<br>`[Kafka]` |
| **Message**         | Discontinua - - → | `<Nombre mensaje>`<br>`[SQS]`                                     |
| **CDC**             | Discontinua - - → | `Capturar cambios`<br>`[CDC]`                                     |
| **Batch**           | Punteada · · · →  | `<Propósito proceso>`<br>`[Batch]`                                |
| **File Transfer**   | Sólida →          | `Transferir archivo`<br>`[SFTP]`                                  |
| **Database Access** | Sólida delgada →  | `<Propósito consulta>`<br>`[SQL]`                                 |

**Obligatorio:**

- Todas las flechas deben tener **Propósito** (texto descriptivo)
- Protocolo obligatorio en **C2 y Deployment**, opcional en **C1 y C3**
- Dirección clara de origen a destino

**Prohibido:**

- Flechas completamente vacías (sin propósito)
- Flechas bidireccionales (usar dos flechas separadas)

**Notas:**

- Los nombres de eventos siguen la nomenclatura de §10 — prefijo `tipo.dominio.entidad.acción`.
- `Message` se usa para flujos point-to-point (Queue como Store); `Event` para flujos pub/sub (Event Bus como Store).
- `Database Access` se permite solo en diagramas C3 o diagramas de auditoría/explicación de queries. En C1/C2 la comunicación directa con Stores debe pasar por la API o servicio responsable.

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

### Diagramas de Secuencia

**Estado**: Recomendado para flujos críticos.

**Cuándo crear uno:**

- Flujos críticos de negocio: autenticación, pagos, checkout, onboarding
- Integraciones complejas con sistemas externos
- Procesos donde el ordenamiento temporal es relevante

**Cuándo NO crear uno:**

- Flujos simples evidentes a partir del C2/C3
- Lógica interna trivial de un componente

**Nivel de detalle esperado:**

- Actores y sistemas externos relevantes
- Pasos de negocio, no llamadas triviales a métodos
- Condiciones, loops y excepciones cuando aplique
- Timeouts y reintentos cuando sean relevantes

**Herramientas:**

- PlantUML o Mermaid (no Draw.io)
- Guardar `.puml` / `.md` en Git junto al diagrama correspondiente

**Convenciones:**

- Nombres en inglés consistentes con el resto del estándar
- Mensajes verbos en presente ("Validate token", no "Validating token")
- Notas (`note left/right`) para condiciones especiales
- Diagramas < 30 pasos; exceder requiere división

---

## 📏 5. REGLAS VISUALES

### Tipografía

- **Fuente**: Helvetica
- **Nombre elemento**: 20px (bold)
- **Descripción elemento**: 13px
- **Estereotipo/Tipo**: 11px
- **Etiqueta flecha**: 12px

### Estructura de Componentes

**Todos los componentes siguen este formato estándar de 4 líneas (3 obligatorias + 1 opcional):**

```
Nombre del Componente
[Tipo: Tecnología]
Descripción breve de responsabilidades.
[Status: <estado>] <nota opcional: fecha/ADR/contexto>
```

- **Línea 1**: Nombre descriptivo del componente
- **Línea 2**: `[Categoría: Tecnología específica]`
- **Línea 3**: Descripción de responsabilidades y propósito
- **Línea 4 (opcional)**: Estado del elemento + nota contextual — solo si no es `Actual`

**Categorías base (línea 2):**

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

**Estados válidos (línea 4):**

| Tag                    | Significado | Cuándo usar                                            |
| ---------------------- | ----------- | ------------------------------------------------------ |
| `[Status: Planned]`    | Planeado    | Elementos futuros, en roadmap, no implementados        |
| `[Status: Deprecated]` | Deprecado   | Elementos en fase de retiro                            |

Notas:

- Formato recomendado: `[Status: Planned] 2026-Q3` o `[Status: Deprecated] 2026-Q4 — ADR-042`
- Solo se incluyen cuando el elemento **no** está en estado Actual (default sin tag).
- El tag se combina con el visual encoding de borde discontinuo (ver sección "Estados Visuales" abajo) — redundancia intencional para accesibilidad.
- La línea 4 es opcional: si el elemento está Actual, simplemente se omite.

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

```
Payments V2
[App: .NET 9 REST API]
Nueva versión del servicio de pagos con soporte multi-currency.
[Status: Planned] 2026-Q4 — ADR-038
```

```
Legacy Auth API
[App: .NET 6 REST API]
Servicio de autenticación heredado, reemplazado por Identity V2.
[Status: Deprecated] 2026-Q4 — ADR-035
```

### Shapes

- **Rectángulo redondeado**: APIs, servicios, workers, apps
- **Cilindro vertical**: Bases de datos, cache
- **Cilindro horizontal**: Message bus, colas, event bus
- **Folder**: Object storage, file storage
- **Hexágono**: API Gateway, Reverse Proxy
- **Boundary**: Agrupación de sistemas o módulos (sin relleno)

### Estilos

- **Border Width**: 1.5px
- **Border Radius**: 10px
- **Sombras**: NO
- **Gradientes**: NO
- **Transparencias**: NO

### Estados Visuales

**Regla fundamental**: Un significado visual = un único concepto. Los estados `Planned` y `Deprecated` combinan **visual encoding** (borde) con **tag textual** (línea 4) para redundancia y accesibilidad.

| Estado        | Borde            | Color          | Tag (línea 4)            | Uso                                            |
| ------------- | ---------------- | -------------- | ------------------------ | ---------------------------------------------- |
| **Actual**    | Sólido           | Categoría      | _(no tag)_               | Elementos implementados y en producción        |
| **Planeado**  | Discontinuo      | Categoría      | `[Status: Planned]`      | Elementos futuros, en roadmap, no implementados |
| **Externo**   | Sólido           | Gris `#DFDFDF` | _(no tag)_               | Usuarios y sistemas externos (third-party)     |
| **Deprecado** | Discontinuo      | Categoría      | `[Status: Deprecated]`   | Elementos en fase de retiro                    |

Notas:

- Elementos planeados mantienen el color de su categoría (morado/coral) + borde discontinuo + tag.
- Elementos deprecados mantienen el color de su categoría + borde discontinuo + tag.
- **Draw.io**: Dashed (1) + Border Width 1.5px.
- `Externo` se señaliza solo con color gris (ver §1); no requiere tag porque color + shape ya son suficientes para identificar el tipo.

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

### Espaciado

- **Entre elementos**: 40-60px
- **Margen**: 30px mínimo
- **Evitar**: Elementos pegados, cruces de líneas

> **Nota**: Las reglas de Boundaries (tipos, nombres, alcances) se documentan en §2 ("Agrupadores").

---

## ✅ 7. CHECKLIST DE VALIDACIÓN

1. [ ] **Título claro** - Nombre del sistema + tipo de diagrama
2. [ ] **Metadata** - Fecha (ISO 8601: `YYYY-MM-DD`), versión (SemVer), owner (equipo responsable)
3. [ ] **Colores correctos** - Según tabla corporativa (§1)
4. [ ] **Shapes correctos** - Cilindro para DB, folder para storage
5. [ ] **Estructura de componentes** - `[Tipo: Tecnología]` en todos los elementos; línea 4 con `[Status: ...]` si no es Actual
6. [ ] **TODAS las flechas etiquetadas** - Protocolo + propósito
7. [ ] **Nombres descriptivos** - NO "Service 1", "API", "DB"
8. [ ] **Nivel correcto** - C1 sin internos, C2 sin componentes
9. [ ] **Límite de elementos** - Cumple los límites por nivel definidos en §4 (tope absoluto en §8)
10. [ ] **Layout limpio** - Sin cruces innecesarios, flujo claro
11. [ ] **Actualizado** - Refleja el estado real del sistema
12. [ ] **Convención de pestañas** - Si el archivo `.drawio` tiene pestañas exportables, sus nombres siguen el patrón `[Nombre del Sistema] - [Tipo]` (ver §16). Tabs `borrador`, `WIP`, etc. deben existir solo como pestañas no exportables.

**Formato de metadata:**

- **Fecha**: `YYYY-MM-DD` (ISO 8601)
- **Versión**: SemVer (`MAJOR.MINOR`)
- **Owner**: Equipo responsable (ej. `Team: Identity Platform`)
- **Ubicación**: archivo README junto al `.drawio` + footer en el PNG exportado

---

## ❌ 8. PROHIBICIONES

### Prohibido

1. **Diagramas con más de 25 elementos** — Tope absoluto. Exceder requiere justificación documentada en el README del diagrama y aprobación de un revisor de Architecture. Límites recomendados por nivel: C1 ≤ 10, C2 ≤ 20, C3 ≤ 12. Deployment y diagramas de integración pueden acercarse al tope si documentan infraestructura real.
2. **Herramientas no autorizadas** - Usar exclusivamente Draw.io o las herramientas listadas en §12. Diagramas one-off en otras herramientas se degradan rápido (no se versionan, no se reutilizan, no se enlazan con código).
3. **Mezclar niveles C4** - Un nivel por diagrama
4. **Diagramas sin metadata** - Incluir owner, fecha, versión
5. **Diagramas sin etiquetas** - Todos los elementos deben tener nombre descriptivo

---

## ⚠️ 9. ANTI-PATRÓNES

Esta sección muestra ejemplos concretos de errores comunes. **Cada anti-patrón tiene su contraparte correcta**.

### Diagrama sin metadata

❌ Diagrama con título pero sin fecha, versión ni owner.

```
Identity - Container
[Sin metadata]
```

✅ Diagrama con metadata completa.

```
# Identity - Container
**Versión**: 1.2 | **Fecha**: 2026-06-15 | **Owner**: Team Identity Platform
```

### Mezcla de niveles C4

❌ C1 que muestra internals de un sistema (APIs, DBs visibles).

✅ C1 con el sistema como caja negra; los detalles van en C2.

### Flechas sin etiqueta

❌ Flecha entre API y DB sin texto.

```
[Identity API] ────────> [identity-db]
```

✅ Flecha con propósito y protocolo.

```
[Identity API] ──Consultar credenciales──> [identity-db]
                  [SQL]
```

### Componente sin tecnología

❌ `[App: Service]`, `[App: API]`, `[Store: Database]`.

✅ `[App: .NET 8 REST API]`, `[App: Node.js 20 gRPC]`, `[Store: PostgreSQL 15]`.

### Boundary mal nombrado

❌ "Módulo 1", "Backend", "Servicios", "Otros".

✅ "Identity Domain", "Payment Context", "AWS us-east-1", "Production EKS".

### Boundaries sobre-anidados

❌ Más de 2 niveles de boundaries anidados (dificulta lectura).

```
AWS > Production > EKS > Cluster > Identity Namespace > Identity System
```

✅ Máximo 2 niveles: ej. `AWS us-east-1 > Production EKS > Identity System`.

### C2 con componentes

❌ C2 que muestra clases o componentes internos de un container.

✅ C2 muestra containers; los componentes van en C3.

### Color de marca en lugar de paleta corporativa

❌ Uso de colores de marca del equipo o del producto.

✅ Solo colores de la paleta corporativa (§1). Cualquier color adicional requiere actualización del estándar.

### Shape incorrecto para el tipo

❌ Cubo o rectángulo para una base de datos.

✅ Cilindro vertical para DB relacional/NoSQL, cilindro horizontal para Event Bus/Queue.

### Tag de estado en lugar incorrecto

❌ Tag en línea 2 (donde va `[Tipo: Tecnología]`):

```
Identity V2
[App: .NET 9] [PLANNED]
...
```

✅ Tag en línea 4 dedicada:

```
Identity V2
[App: .NET 9 REST API]
Nueva versión del servicio de autenticación.
[Status: Planned] 2026-Q4 — ADR-038
```

---

## 🔧 10. NOMENCLATURA

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

## 📊 11. TIPOS DE DIAGRAMAS

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

## 🛠️ 12. HERRAMIENTAS

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

1. Por cada pestaña del `.drawio` que matchee el patrón de §16, se exporta un PNG
2. Las pestañas que **no** matcheen el patrón (§16) se ignoran silenciosamente
3. Si el PNG generado es byte-identical al ya existente en el branch, no se hace commit
4. Los PNGs se hacen commit al **mismo branch** del PR — nunca a `main`
5. El workflow nunca abre un nuevo PR ni crea issues

**Checklist PR** (nuevo item):

- [ ] Si se modificó un `.drawio`, ¿el workflow generó/actualizó el `.png` en este PR?

---

## 🚀 13. PROCESO DE CREACIÓN

El ciclo de vida de un diagrama sigue cinco fases. Cada fase tiene criterios de salida claros.

### 1. Draft

- Crear pestaña siguiendo convención §16 (ej. `Identity - Container`).
- Aplicar paleta (§1) y componentes estándar (§2).
- Etiquetar todos los elementos y flechas (§3).
- Versionar como `0.1-draft` en el archivo README asociado.
- Estado: **borrador**, no apto para revisión arquitectónica.

### 2. Review

- Self-review con checklist (§7).
- Verificar límites por nivel (§4) y anti-patrones (§9).
- PR al repositorio del equipo con tag `diagram-review`.
- Owners de los sistemas representados deben validar que el contenido refleja la realidad.

### 3. Architecture Review

- Revisión por Architecture Team (criterios §7 + §8 + §9).
- Verificar adherencia a la paleta, nomenclatura (§10) y formato.
- Verificar coherencia con otros diagramas del mismo sistema.
- Cambios solicitados → volver a fase `Draft`.

### 4. Approved

- Merge del PR.
- Bump de versión a `1.0` (o siguiente SemVer si es una revisión mayor).
- Workflow CI (§12) exporta PNG automáticamente.
- Tag en repo: `diagram-v1.0` (opcional).

### 5. Published

- PNG committeado en el mismo branch del PR (nunca a `main` directo).
- Link en README del sistema o en el ADR asociado.
- Comunicar a stakeholders vía Slack `#architecture`.

### Roles y Responsabilidades

| Rol                    | Responsabilidad                                                  |
| ---------------------- | ---------------------------------------------------------------- |
| Autor del diagrama     | Crear el diagrama, self-review, abrir PR                         |
| Owner del sistema      | Validar que el contenido refleja la realidad                     |
| Architecture Team      | Review final, aprobación, mantenimiento del estándar             |
| DevOps / Platform Team | Mantener el workflow CI de exportación (§12)                     |

---

## 🏗️ 14. DEPLOYMENT DIAGRAM

**Obligatorio** para TODO sistema en producción.

### Elementos de Infraestructura

| Elemento          | Shape                 | Color     | Uso                             |
| ----------------- | --------------------- | --------- | ------------------------------- |
| **Pod/Container** | Rectángulo redondeado | `#E1D5E7` | Contenedor individual           |
| **VM/EC2**        | Rectángulo            | `#E8EAF6` | Máquina virtual                 |
| **Load Balancer** | Hexágono              | `#E1D5E7` | Balanceador de carga (ALB, NLB). Color: morado App (categoría App — API Gateway managed). |

**Nota**: Para Databases, Object Storage y Message Brokers, usar los componentes **Store** definidos en §2.

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

## 📚 15. DOCUMENTACIÓN DE REFERENCIA

- [Validation Criteria](./reference/validation-criteria.md) - Checklists para PR y auditorías
- [Best Practices](./reference/c4-best-practices.md) - Principios y anti-patrones
- [Cheat Sheet](./reference/cheat-sheet.md) - Referencia rápida de una página
- [Contribution Guide](./reference/contribution-guide.md) - Cómo proponer cambios al estándar

---

## 📑 16. CONVENCIÓN DE PESTAÑAS

### Propósito

El workflow de CI/CD (ver §12) necesita identificar automáticamente qué pestañas de un archivo `.drawio` representan diagramas exportables. Esta convención define el patrón que el workflow usa para distinguir pestañas exportables de borradores y notas.

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

## 📖 17. GLOSARIO DE TÉRMINOS

Términos clave usados en este estándar.

- **App**: Container ejecutable (API, worker, frontend, función serverless). Color: morado.
- **Aplicación (genérico)**: Escape hatch en §2 para casos App no cubiertos por un componente específico. Usar con moderación.
- **Agrupador (Boundary)**: Constructo de visualización que delimita un alcance. No es una entidad, es un agrupador visual.
- **ADR**: Architecture Decision Record. Documento que captura una decisión arquitectónica significativa.
- **Boundary**: Sinónimo inglés de Agrupador. Usado en la jerga C4.
- **Component**: Unidad interna de un container con un único motivo de cambio. Se modela en diagramas C3.
- **Container**: Aplicación o Store que forma parte de un System. Unidad desplegable o proceso runtime.
- **C4 model**: Marco de modelado de arquitectura en 4 niveles (Context, Container, Component, Code). Este estándar no cubre C4 (Code).
- **Diagram Type**: Clasificación del diagrama (Context, Container, Deployment, Sequence, etc.). Ver §11.
- **External**: Atributo de un elemento que indica que es third-party o está fuera del alcance de nuestro sistema. Color gris.
- **Person**: Actor humano en el sistema. Interno (verde) o externo (gris).
- **Scope**: Sinónimo de Boundary orientado a C4 (SystemScopeBoundary, ContainerScopeBoundary).
- **Store**: Container de persistencia (DB, queue, cache, storage). Color: coral.
- **System**: Unidad de software autónoma con responsabilidades claras. Sujeto de un diagrama C1.
- **Tag (estado)**: Marcador en línea 4 del formato de componente que indica estado (`Planned`, `Deprecated`).

---

## 📋 18. EJEMPLOS END-TO-END

Ejemplos mínimos de cada nivel, siguiendo el estándar completo.

### Ejemplo C1 — Context Diagram (Identity System)

**Elementos (5):**

- 1 Person: Usuario Final
- 1 System: Identity System (nuestra caja negra)
- 1 External System: Google OAuth
- 1 External System: SMTP Provider

**Layout sugerido:**

```
┌─────────────────┐
│ Usuario Final   │
└────────┬────────┘
         │ login
         ▼
┌─────────────────┐      OAuth      ┌──────────────┐
│ Identity System │ ───────────────► │ Google OAuth │
│ (caja negra)    │                  └──────────────┘
└────────┬────────┘
         │ enviar email
         ▼
┌─────────────────┐
│ SMTP Provider   │
└─────────────────┘
```

**Notas:**

- Cubre los elementos obligatorios de §4 C1.
- 4 elementos, muy por debajo del límite recomendado de 10.
- Todas las flechas con propósito; OAuth especificado como protocolo.

---

### Ejemplo C2 — Container Diagram (Identity System)

**Elementos (6):**

- 1 Boundary: Identity System (scope)
- 3 App containers: Identity API (.NET 8), Admin Web (Angular 17), Notification Worker (.NET 8)
- 2 Store containers: identity-db (PostgreSQL), session-cache (Redis)
- 1 External System: SMTP Provider

**Layout sugerido:**

```
┌─ Identity System ─────────────────────────────────────────────────┐
│                                                                    │
│  ┌──────────────┐    HTTPS    ┌──────────────┐    SQL    ┌──────┐ │
│  │ Admin Web    │ ──────────► │ Identity API │ ────────► │ id-db│ │
│  └──────────────┘             └──────┬───────┘           └──────┘ │
│                                       │                            │
│                                       └──Redis──► ┌──────────┐    │
│                                                   │session-c │    │
│                                                   └──────────┘    │
│                                                                    │
│  ┌──────────────────┐                                              │
│  │ Notification     │ ────SMTP──► [SMTP Provider]                  │
│  │ Worker           │                                              │
│  └──────────────────┘                                              │
└────────────────────────────────────────────────────────────────────┘
```

**Notas:**

- 6 containers internos + 1 external = 7 elementos, bajo el límite recomendado de 20.
- Cumple §4 C2: App en morado, Store en coral, External en gris.
- No muestra componentes (irían en C3).
- Protocolos explícitos en cada flecha (obligatorio en C2).

---

### Ejemplo C3 — Component Diagram (Identity API)

**Elementos (6):**

- 1 Boundary: Identity API (scope)
- 5 Components: AuthController, CredentialValidator, AuthService, UserMapper, UserRepository
- 1 Store: identity-db (referencia al container padre)

**Layout sugerido:**

```
┌─ Identity API ──────────────────────────────────────────────┐
│                                                              │
│  ┌────────────────┐    validate    ┌─────────────────────┐ │
│  │ AuthController │ ─────────────► │ CredentialValidator │ │
│  └────────┬───────┘               └─────────────────────┘ │
│           │                                                  │
│           │ process                                          │
│           ▼                                                  │
│  ┌────────────────┐                                          │
│  │ AuthService    │ ──┐                                      │
│  └────────────────┘   │ map                                  │
│                        ▼                                      │
│               ┌────────────────┐                             │
│               │ UserMapper     │                             │
│               └────────────────┘                             │
│                                                              │
│           ┌────────────────┐    SQL    ┌─────────────┐     │
│           │ UserRepository │ ────────► │ identity-db │     │
│           └────────────────┘           └─────────────┘     │
└──────────────────────────────────────────────────────────────┘
```

**Notas:**

- Cumple §4 C3: componentes de UN SOLO container (Identity API).
- Una responsabilidad por componente (Controller/Validator/Service/Mapper/Repository).
- Flechas muestran dirección del dato.
- identity-db aparece como referencia al Store padre (no como componente).

---

### Ejemplo Deployment — Identity System en Production (AWS)

**Elementos:**

- 1 Boundary: AWS us-east-1 (Production)
  - 1 Boundary: Production EKS
    - 1 Load Balancer: Public ALB
    - 1 Store: identity-db (RDS PostgreSQL)
    - 1 Store: session-cache (ElastiCache Redis)
    - 1 App: identity-api (x3 réplicas)

**Layout sugerido:**

```
┌─ AWS us-east-1 (Production) ──────────────────────────────────────┐
│                                                                   │
│  ┌─ Production EKS ──────────────────────────────────────────┐   │
│  │                                                            │   │
│  │       ┌──────────────┐                                     │   │
│  │       │ Public ALB   │                                     │   │
│  │       └──────┬───────┘                                     │   │
│  │              │ HTTPS:443                                   │   │
│  │              ▼                                             │   │
│  │       ┌──────────────────┐                                │   │
│  │       │ identity-api x3  │                                │   │
│  │       └────┬─────────┬───┘                                │   │
│  │            │         │                                    │   │
│  │   SQL:5432 │         │ Redis:6379                         │   │
│  │            ▼         ▼                                    │   │
│  │   ┌─────────────┐ ┌──────────────┐                       │   │
│  │   │ identity-db │ │ session-cache│                       │   │
│  │   │ (RDS)       │ │ (ElastiCache)│                       │   │
│  │   └─────────────┘ └──────────────┘                       │   │
│  │                                                            │   │
│  └────────────────────────────────────────────────────────────┘   │
└───────────────────────────────────────────────────────────────────┘
```

**Notas:**

- Muestra infraestructura real (no conceptual): AWS, EKS, RDS, ElastiCache, ALB.
- Réplicas indicadas (x3 para identity-api).
- Protocolos y puertos en cada flecha (HTTPS:443, SQL:5432, Redis:6379).
- Estereotipos opcionales como `<<RDS>>`, `<<ALB>>` pueden añadirse al nombre del componente.

---

## 📞 Contacto y Soporte

- **Slack**: `#architecture`
- **Email**: <architecture@company.com>
- **Librería Draw.io**: [Actualizar aquí](./drawio-library/)

---

**Mantenedores**: Architecture Team