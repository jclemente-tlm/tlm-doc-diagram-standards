# Estándares de Diagramas de Arquitectura

**Última actualización**: 2026-06-04
**Versión**: 1.0

---

## 🎨 1. PALETA CORPORATIVA

| Categoría           | Color          | Código Hex | Uso                                 |
| ------------------- | -------------- | ---------- | ----------------------------------- |
| **System**          | Azul           | `#B2CEFF`  | Sistemas internos, boundaries       |
| **App**             | Morado claro   | `#E1D5E7`  | APIs, servicios, workers, frontends |
| **Store**           | Coral claro    | `#F8CECC`  | DBs, colas, event bus, storage      |
| **Component**       | Amarillo claro | `#FFF2CC`  | Componentes internos (C3)           |
| **Person**          | Verde claro    | `#D5E8D4`  | Usuarios internos, empleados        |
| **External Person** | Gris           | `#DFDFDF`  | Usuarios externos, clientes         |
| **External System** | Gris           | `#DFDFDF`  | Sistemas externos, third-party      |

---

## 📦 2. COMPONENTES ESTÁNDAR

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
| **Boundary**        | Rectángulo redondeado (sin relleno, borde sólido o discontinuo) | layer-group | Ninguno   | N/A                 | Agrupación de módulos/contextos |

**Nota sobre Boundaries**: Existen 3 tipos - **Agrupador de Sistema** (discontinuo, C2, muestra alcance de un sistema con sus containers), **Agrupador de Aplicación** (discontinuo, C3, muestra alcance de un container con sus componentes), y **Agrupador** genérico (sólido, para dominios, módulos y deployment). Ver Sección 6 para detalles.

### App (morado `#E1D5E7`)

| Componente          | Shape                 | Icono      | Uso                                  |
| ------------------- | --------------------- | ---------- | ------------------------------------ |
| **Web Application** | Rectángulo redondeado | monitor    | Aplicación web                       |
| **Mobile App**      | Rectángulo redondeado | smartphone | Aplicación móvil                     |
| **API**             | Rectángulo redondeado | globe      | API REST/gRPC                        |
| **Microservice**    | Rectángulo redondeado | box        | Microservicio                        |
| **Worker**          | Rectángulo redondeado | cog        | Background worker                    |
| **Batch**           | Rectángulo redondeado | clock      | Proceso batch/programado             |
| **CDC Processor**   | Rectángulo redondeado | sync       | Procesador CDC (Change Data Capture) |
| **API Gateway**     | Hexágono              | shield     | Gateway/Proxy de entrada             |

### Store (coral `#F8CECC`)

| Componente              | Shape               | Icono        | Uso                                                       | Patrón         |
| ----------------------- | ------------------- | ------------ | --------------------------------------------------------- | -------------- |
| **Relational Database** | Cilindro vertical   | database     | Base de datos relacional (PostgreSQL, Oracle, SQL Server) | -              |
| **NoSQL Database**      | Cilindro vertical   | database     | Base de datos NoSQL (DynamoDB)                            | -              |
| **Cache**               | Cilindro vertical   | bolt         | Cache (Redis)                                             | -              |
| **Event Bus**           | Cilindro horizontal | waves        | Bus de eventos (Kafka)                                    | Pub/Sub        |
| **Queue**               | Cilindro horizontal | list-ordered | Cola de mensajes                                          | Point-to-Point |
| **Object Storage**      | Folder              | folder       | Almacenamiento (S3, File Server)                          | -              |

### Componentes C3 (amarillo `#FFF2CC`)

| Componente     | Shape                 | Icono       | Uso                                      |
| -------------- | --------------------- | ----------- | ---------------------------------------- |
| **Component**  | Rectángulo redondeado | box         | Componente genérico                      |
| **Controller** | Rectángulo redondeado | route       | Controller (MVC) - maneja requests       |
| **Service**    | Rectángulo redondeado | cog         | Service - lógica de negocio              |
| **Repository** | Rectángulo redondeado | database    | Repository - acceso a datos              |
| **Adapter**    | Rectángulo redondeado | plug        | Adapter - integración con externos       |
| **Client**     | Rectángulo redondeado | arrow-right | Client - consume APIs de otros servicios |
| **Publisher**  | Rectángulo redondeado | send        | Publisher - publica eventos              |
| **Consumer**   | Rectángulo redondeado | inbox       | Consumer - consume eventos/mensajes      |
| **Mapper**     | Rectángulo redondeado | shuffle     | Mapper - transformación de datos         |
| **Validator**  | Rectángulo redondeado | check       | Validator - validación de datos          |

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

| Tipo                | Línea             | Formato Etiqueta                   | Uso                            |
| ------------------- | ----------------- | ---------------------------------- | ------------------------------ |
| **Relación**        | Sólida →          | `<Propósito>`<br>`[<Protocolo>]`   | Genérico con protocolo         |
| **Relación Simple** | Sólida →          | `<Propósito>`                      | Genérico sin protocolo (C1/C3) |
| **REST/HTTP**       | Sólida →          | `<Propósito>`<br>`[HTTPS]`         | Llamadas HTTPS                 |
| **HTTP**            | Sólida →          | `<Propósito>`<br>`[HTTP]`          | Llamadas HTTP                  |
| **SOAP**            | Sólida →          | `<Propósito>`<br>`[SOAP]`          | Servicios SOAP/XML             |
| **gRPC**            | Sólida →          | `<Propósito>`<br>`[gRPC]`          | Llamadas gRPC                  |
| **Evento**          | Discontinua - - → | `<Nombre evento>`<br>`[Kafka]`     | Publicación de eventos         |
| **Queue**           | Discontinua - - → | `<Nombre mensaje>`<br>`[SQS]`      | Mensajes en cola               |
| **CDC**             | Discontinua - - → | `Capturar cambios`<br>`[CDC]`      | Change Data Capture            |
| **Batch**           | Punteada · · · →  | `<Propósito proceso>`<br>`[Batch]` | Procesos batch                 |
| **File Transfer**   | Sólida →          | `Transferir archivo`<br>`[SFTP]`   | Transferencia de archivos      |
| **Database Access** | Sólida delgada →  | `<Propósito consulta>`<br>`[SQL]`  | Acceso a BD                    |

**Obligatorio:**

- Todas las flechas deben tener **Propósito** (texto descriptivo)
- Protocolo obligatorio en **C2 y Deployment**, opcional en **C1 y C3**
- Dirección clara de origen a destino

**Prohibido:**

- Flechas completamente vacías (sin propósito)
- Flechas bidireccionales (usar dos flechas separadas)

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
- ❌ Preferiblemente no más de 10 elementos (si excede, evaluar dividir)

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
- ❌ Preferiblemente no más de 20 elementos (si excede, evaluar dividir)

**Propósito**: Mostrar la arquitectura interna del sistema - aplicaciones, servicios, bases de datos y cómo se comunican.

---

### **C3 - Component Diagram**

**Crear cuando:**

- ✅ Servicio complejo (>3 responsabilidades)
- ✅ Patrones arquitectónicos específicos
- ✅ Onboarding de equipo nuevo

**Incluir:**

- ✅ Componentes de UN SOLO container
- ✅ Responsabilidades claras
- ✅ Relaciones entre componentes
- ✅ Stores que usa

**Excluir:**

- ❌ Clases o métodos (C4 no es UML)
- ❌ Componentes de múltiples containers
- ❌ Preferiblemente no más de 12 componentes (si excede, evaluar dividir)

**Propósito**: Mostrar la organización interna de un servicio/API específico - sus componentes y responsabilidades.

---

### **C4 - Code Diagram**

❌ **NO CREAR** diagramas C4. Usar código fuente como documentación.

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

**Nota**: Ver [estructura completa de componentes](./reference/component-structure.md) para todos los tipos disponibles.

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

### Tamaños Estándar

**Tamaño mínimo recomendado**: 280pt × 140pt

| Categoría      | Tamaño mínimo     | Notas                                          |
| -------------- | ----------------- | ---------------------------------------------- |
| Sistemas       | 280pt × 140pt     | Ajustar según longitud de nombre y descripción |
| App            | 280pt × 140pt     | Ajustar según longitud de nombre y descripción |
| Store          | 280pt × 140pt     | Ajustar según longitud de nombre y descripción |
| Componentes C3 | 280pt × 140pt     | Ajustar según longitud de nombre y descripción |
| Actores        | 200pt × 140pt     | Suficiente para nombre de rol                  |
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
9. [ ] **Límite de elementos** - Preferiblemente C1 ≤10, C2 ≤20, C3 ≤12
10. [ ] **Layout limpio** - Sin cruces innecesarios, flujo claro
11. [ ] **Actualizado** - Refleja el estado real del sistema

---

## ❌ 8. PROHIBICIONES

### Prohibido

1. **Diagramas con más de 30 elementos** - Evaluar dividir en múltiples diagramas para mejorar legibilidad
2. **PowerPoint o herramientas no autorizadas** - Usar Draw.io
3. **Mezclar niveles C4** - Un nivel por diagrama
4. **Diagramas sin metadata** - Incluir owner, fecha, versión
5. **Diagramas sin etiquetas** - Todos los elementos deben tener nombre descriptivo

---

## 🔧 9. NOMENCLATURA

### Servicios y APIs

✅ **Formato**: `[Dominio] Service` o `[Dominio] API`
✅ **Ejemplos**: Identity Service, Notification API, Payment Service
❌ **Evitar**: auth-svc, svc-notif, PaymentMS

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
✅ **Ejemplos**: `cdc.oracle.hr.employee`, `evt.payment.created`, `cmd.notification.send`
❌ **Evitar**: event1, notification, kafka-topic

### Reglas Generales

- ✅ Nombres en **singular** (User Service, no Users Service)
- ✅ **Inglés** para elementos técnicos
- ✅ Descriptivo (qué hace, no tecnología)
- ❌ NO abreviaciones (excepto: API, DB, HTTP, CDC, ETL)

---

## 📋 10. DIAGRAMAS OBLIGATORIOS

### Para TODO sistema nuevo

1. **C1 - Context Diagram** ✅ Obligatorio
2. **C2 - Container Diagram** ✅ Obligatorio
3. **Deployment Diagram** ✅ Obligatorio (ver Sección 13)

### Condicionales

- **C3 - Component Diagram**: Solo para servicios complejos
- **Integration Diagram**: Si hay integraciones complejas con múltiples sistemas
- **Sequence Diagram**: Para flujos críticos (auth, payments, checkout)
- **Data Flow Diagram**: Para arquitecturas data-intensive (CDC, ETL, pipelines)

---

## 🛠️ 11. HERRAMIENTAS

### Herramienta Oficial: Draw.io

**Setup obligatorio:**

1. Instalar desde: <https://draw.io>
2. Cargar librería: File → Open Library → `TLM - Librería C4.xml`
3. Usar templates de `/templates/`

**Versionamiento:**

- Guardar `.drawio` en Git (obligatorio)
- Exportar `.png` para README/documentación

### Herramientas Autorizadas (casos específicos)

- **Sequence Diagrams**: PlantUML, Mermaid
- **Diagrams-as-Code**: Structurizr DSL
- **Cloud Diagrams**: AWS Architecture Icons, Azure Icons

---

## 🚀 12. PROCESO DE CREACIÓN

1. Abrir template correspondiente de `/templates/`
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
| **Load Balancer** | Hexágono              | `#B2CEFF` | Balanceador de carga (ALB, NLB) |

**Nota**: Para Databases, Object Storage y Message Brokers, usar los componentes **Store** definidos en la Sección 2.

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

## 📦 15. COMPONENT LIBRARY

**Catálogo oficial de componentes** para librería Draw.io y diagramas corporativos.

### Actores

| Componente          | Tipo                | Ejemplo tecnología | Uso                          |
| ------------------- | ------------------- | ------------------ | ---------------------------- |
| **Person**          | `[Person]`          | N/A                | Usuarios internos, empleados |
| **External Person** | `[External Person]` | N/A                | Usuarios externos, clientes  |

### Sistemas

| Componente          | Tipo                | Ejemplo tecnología | Uso                            |
| ------------------- | ------------------- | ------------------ | ------------------------------ |
| **System**          | `[System]`          | N/A                | Sistemas internos              |
| **External System** | `[External System]` | N/A                | Sistemas externos, third-party |

### App (Aplicaciones y Servicios)

| Componente          | Tipo                   | Ejemplo tecnología | Uso                                  |
| ------------------- | ---------------------- | ------------------ | ------------------------------------ |
| **Application**     | `[App: tecnología]`    | Tecnología         | Aplicación genérica (fallback)       |
| **Web Application** | `[App: tecnología]`    | Angular 17         | Aplicación web                       |
| **Mobile App**      | `[App: tecnología]`    | Flutter            | Aplicación móvil                     |
| **API**             | `[App: tecnología]`    | .NET 8 REST API    | API REST/gRPC                        |
| **Microservice**    | `[App: tecnología]`    | .NET 8             | Microservicio                        |
| **Worker**          | `[App: tecnología]`    | .NET Worker        | Background worker                    |
| **Batch**           | `[App: Batch Process]` | Batch Process      | Proceso batch/programado             |
| **CDC Processor**   | `[App: tecnología]`    | Debezium           | Procesador CDC (Change Data Capture) |
| **API Gateway**     | `[App: tecnología]`    | Kong Gateway       | Gateway/Proxy de entrada             |

### Store (Almacenamiento y Mensajería)

| Componente              | Tipo                  | Ejemplo tecnología | Uso                                                       |
| ----------------------- | --------------------- | ------------------ | --------------------------------------------------------- |
| **Store**               | `[Store: tecnología]` | Tecnología         | Almacenamiento genérico (fallback)                        |
| **Relational Database** | `[Store: tecnología]` | PostgreSQL         | Base de datos relacional (PostgreSQL, Oracle, SQL Server) |
| **NoSQL Database**      | `[Store: tecnología]` | DynamoDB           | Base de datos NoSQL (DynamoDB)                            |
| **Cache**               | `[Store: tecnología]` | Redis              | Cache (Redis)                                             |
| **Event Bus**           | `[Store: tecnología]` | Kafka              | Bus de eventos (Kafka)                                    |
| **Queue**               | `[Store: tecnología]` | SQS                | Cola de mensajes                                          |
| **Object Storage**      | `[Store: tecnología]` | Amazon S3          | Almacenamiento (S3, File Server)                          |

### Componentes C3 (Componentes Internos)

| Componente     | Tipo                      | Uso                                      |
| -------------- | ------------------------- | ---------------------------------------- |
| **Component**  | `[Component]`             | Componente genérico                      |
| **Controller** | `[Component: Controller]` | Controller (MVC) - maneja requests       |
| **Service**    | `[Component: Service]`    | Service - lógica de negocio              |
| **Repository** | `[Component: Repository]` | Repository - acceso a datos              |
| **Adapter**    | `[Component: Adapter]`    | Adapter - integración con externos       |
| **Client**     | `[Component: Client]`     | Client - consume APIs de otros servicios |
| **Publisher**  | `[Component: Publisher]`  | Publisher - publica eventos              |
| **Consumer**   | `[Component: Consumer]`   | Consumer - consume eventos/mensajes      |
| **Mapper**     | `[Component: Mapper]`     | Mapper - transformación de datos         |
| **Validator**  | `[Component: Validator]`  | Validator - validación de datos          |

**Notas**:

- Esta tabla define los **30 componentes oficiales** de la librería corporativa
- No crear componentes personalizados sin aprobación del Architecture Team
- Para casos no cubiertos, usar el componente genérico más cercano y documentar en ADR

---

## 📊 16. TIPOS DE DIAGRAMAS PERMITIDOS

| Tipo de Diagrama      | Estado                 | Cuándo Usar                                            | Herramienta              |
| --------------------- | ---------------------- | ------------------------------------------------------ | ------------------------ |
| **C1 - Context**      | ✅ Obligatorio         | Todo sistema nuevo                                     | Draw.io                  |
| **C2 - Container**    | ✅ Obligatorio         | Todo sistema nuevo                                     | Draw.io                  |
| **C3 - Component**    | ⚠️ Condicional         | Servicios complejos (>3 responsabilidades)             | Draw.io                  |
| **Deployment**        | ✅ Obligatorio         | Todo sistema en producción                             | Draw.io                  |
| **Sequence**          | ✅ Permitido           | Flujos críticos (auth, payments, checkout)             | PlantUML, Mermaid        |
| **Integration**       | ✅ Permitido           | Integraciones complejas con múltiples sistemas         | Draw.io                  |
| **Data Flow**         | ✅ Permitido           | Arquitecturas data-intensive (CDC, ETL, pipelines)     | Draw.io                  |
| **Network**           | ✅ Permitido           | Diseño de conectividad y seguridad de red              | Draw.io                  |
| **Infrastructure**    | ✅ Permitido           | Recursos cloud específicos (complementa Deployment)    | Draw.io, AWS/Azure Icons |
| **C4 - Code**         | ❌ No crear            | Usar código fuente como documentación                  | N/A                      |
| **UML Class**         | ❌ No recomendado      | C4 no es UML - usar C3 Component en su lugar           | N/A                      |
| **UML Component**     | ❌ No recomendado      | Usar C3 Component con notación C4                      | N/A                      |
| **Flowchart/Proceso** | ⚠️ Usar con precaución | Solo para procesos de negocio, no arquitectura técnica | Draw.io, Lucidchart      |

### Notas

**Obligatorio** = Requerido para todo sistema en ADRs y documentación arquitectónica

**Condicional** = Requerido solo cuando aplica la condición especificada

**Permitido** = Útil y recomendado cuando agrega valor, no obligatorio

**No crear** = Explícitamente prohibido por este estándar

**No recomendado** = Permitido técnicamente pero preferir alternativas sugeridas

---

## 📚 17. DOCUMENTACIÓN DE REFERENCIA

- [Estructura de componentes](./reference/component-structure.md)
- [Validation Criteria](./reference/VALIDATION-CRITERIA.md)
- [Best Practices](./reference/c4-guidelines/best-practices.md)
- [Guías C4](./reference/c4-guidelines/)

---

## 📞 Contacto y Soporte

- **Slack**: `#architecture`
- **Email**: <architecture@company.com>
- **Librería Draw.io**: [Actualizar aquí](./drawio-library/)
- **Templates**: [Actualizar aquí](./templates/)

---

**Versión**: 1.0
**Última actualización**: 2026-06-04
**Mantenedores**: Architecture Team
