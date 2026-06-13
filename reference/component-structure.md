# Estructura Completa de Componentes

**Última actualización**: 2026-06-04

---

## Formato Estándar

Todos los componentes en los diagramas siguen este formato consistente:

```
Nombre del Componente
[Tipo: Tecnología]
Descripción breve de responsabilidades.
```

**Línea 1**: Nombre descriptivo del componente
**Línea 2**: `[Categoría: Tecnología específica]`
**Línea 3**: Descripción de responsabilidades y propósito

---

## Categorías Base

| Categoría           | Formato                             | Uso                                             |
| ------------------- | ----------------------------------- | ----------------------------------------------- |
| **Person**          | `[Person]`                          | Usuario o rol interno                           |
| **External Person** | `[External Person]`                 | Usuario o rol externo                           |
| **System**          | `[System]`                          | Sistema interno                                 |
| **External System** | `[External System]`                 | Sistema externo, third-party                    |
| **App**             | `[App: tecnología]`                 | Aplicación, API, servicio, worker, batch        |
| **Store**           | `[Store: tecnología]`               | Base de datos, cache, queue, event bus, storage |
| **Component**       | `[Component]` o `[Component: tipo]` | Componentes internos (C3)                       |

---

## Plantillas por Componente

### Actores

#### Person (Usuario Interno)

```
Nombre del Rol
[Person]
Descripción del rol y responsabilidades del usuario.
```

**Ejemplo:**

```
Analista de RRHH
[Person]
Gestiona información de empleados y procesos de recursos humanos.
```

---

#### External Person (Usuario Externo)

```
Nombre del Rol
[External Person]
Descripción del rol e interacciones del usuario.
```

**Ejemplo:**

```
Cliente
[External Person]
Accede a servicios de autogestión y consulta de productos.
```

---

### Sistemas

#### System (Sistema Interno)

```
Nombre del Sistema
[System]
Descripción del propósito y capacidades del sistema.
```

**Ejemplo:**

```
Plataforma de Identidad
[System]
Proporciona capacidades de gestión de identidad y acceso.
```

---

#### External System (Sistema Externo)

```
Nombre del Sistema Externo
[External System]
Descripción del propósito e integraciones del sistema externo.
```

**Ejemplo:**

```
SAP ERP
[External System]
Sistema de planificación de recursos empresariales.
```

---

### Container App (Aplicaciones)

#### Application (Genérico)

```
Nombre de la Aplicación
[App: e.g. Tecnología]
Descripción de las responsabilidades de la aplicación.
```

**Ejemplo:**

```
Rules Engine
[App: Drools]
Motor de reglas de negocio para validación de políticas.
```

**Cuándo usar:**

- Aplicaciones que no encajan en categorías específicas (Web, Mobile, API, etc.)
- Tecnologías emergentes no contempladas (AI Agents, Workflow Engines, etc.)
- Componentes especializados (OCR Processor, Document Extractor, etc.)

---

#### Web Application

```
Nombre de la Aplicación Web
[App: e.g. Angular]
Descripción de funcionalidades y usuarios objetivo.
```

**Ejemplo:**

```
Portal de Empleados
[App: Angular 17]
Portal de autoservicio para empleados corporativos.
```

---

#### Mobile Application

```
Nombre de la Aplicación Móvil
[App: e.g. Flutter]
Descripción de funcionalidades y usuarios objetivo.
```

**Ejemplo:**

```
App de Ventas
[App: Flutter]
Aplicación móvil para gestión de ventas en campo.
```

---

#### API

```
Nombre del API
[App: e.g. .NET 8 REST API]
Descripción de responsabilidades y capacidades expuestas.
```

**Ejemplo:**

```
Identity API
[App: .NET 8 REST API]
Gestiona autenticación, autorización y emisión de tokens.
```

---

#### Microservice

```
Nombre del Servicio
[App: e.g. .NET 8]
Descripción de responsabilidades y capacidades de negocio.
```

**Ejemplo:**

```
Notification Service
[App: .NET 8]
Procesa y envía notificaciones por correo, SMS y WhatsApp.
```

---

#### Worker

```
Nombre del Worker
[App: e.g. .NET Worker Service]
Descripción de procesamiento en segundo plano.
```

**Ejemplo:**

```
Email Processor
[App: .NET Worker Service]
Procesa cola de correos electrónicos y realiza envíos masivos.
```

---

#### Batch Process

```
Nombre del Proceso
[App: Airflow]
Descripción de proceso programado y frecuencia.
```

**Ejemplo:**

```
Nightly Report Generator
[App: Airflow]
Genera reportes consolidados de manera nocturna.
```

---

#### CDC Processor

```
Nombre del Procesador CDC
[App: e.g. Debezium]
Descripción de captura de cambios y flujo de datos.
```

**Ejemplo:**

```
Employee CDC Processor
[App: Debezium]
Captura cambios de empleados desde Oracle y publica eventos.
```

---

#### API Gateway

```
Nombre del Gateway
[App: e.g. Kong Gateway]
Descripción de routing, seguridad y gestión de APIs.
```

**Ejemplo:**

```
Corporate API Gateway
[App: Kong Gateway]
Proporciona routing, autenticación y rate limiting para APIs corporativas.
```

---

### Container Store (Almacenamiento)

#### Store (Genérico)

```
Nombre del Almacenamiento
[Store: e.g. Tecnología]
Descripción del almacenamiento.
```

**Ejemplo:**

```
Vector Database
[Store: Pinecone]
Almacena embeddings vectoriales para búsqueda semántica de documentos.
```

**Cuándo usar:**

- Almacenamiento que no encaja en categorías específicas (DB, Cache, Queue, etc.)
- Tecnologías emergentes (Vector DBs, Knowledge Bases, Data Lakes, etc.)
- Stores especializados (Search Indexes, Time-Series DBs, Graph DBs, etc.)

---

#### Relational Database

```
Nombre de la Base de Datos
[Store: e.g. PostgreSQL]
Descripción de datos almacenados y modelo de persistencia.
```

**Ejemplo:**

```
identity-db
[Store: PostgreSQL]
Almacena usuarios, roles, permisos y configuraciones de seguridad.
```

---

#### NoSQL Database

```
Nombre de la Base de Datos
[Store: e.g. DynamoDB]
Descripción de datos almacenados y patrones de acceso.
```

**Ejemplo:**

```
session-store
[Store: DynamoDB]
Almacena sesiones activas con alta disponibilidad.
```

---

#### Cache

```
Nombre del Cache
[Store: e.g. Redis]
Descripción de datos temporales y estrategia de caché.
```

**Ejemplo:**

```
session-cache
[Store: Redis]
Almacena sesiones de usuario activas para acceso rápido.
```

---

#### Event Bus

```
Nombre de la Plataforma de Eventos
[Store: e.g. Kafka]
Descripción de distribución de eventos y topics.
```

**Ejemplo:**

```
Event Streaming Platform
[Store: Kafka]
Publica y consume eventos de negocio en tiempo real.
```

---

#### Queue

```
Nombre de la Cola
[Store: e.g. SQS]
Descripción de procesamiento asíncrono y mensajería.
```

**Ejemplo:**

```
Notification Queue
[Store: SQS]
Encola solicitudes de notificación para procesamiento asíncrono.
```

---

#### Object Storage

```
Nombre del Almacenamiento
[Store: e.g. Amazon S3]
Descripción de archivos y objetos almacenados.
```

**Ejemplo:**

```
Document Storage
[Store: Amazon S3]
Almacena documentos generados y archivos cargados por usuarios.
```

---

### Componentes C3

#### Component (Genérico)

```
Nombre del Componente
[Component]
Descripción de responsabilidades e interacciones.
```

**Ejemplo:**

```
Authentication Component
[Component]
Maneja validación de credenciales y generación de tokens.
```

---

#### Controller

```
Nombre del Controller
[Component: Controller]
Descripción de manejo de requests y coordinación.
```

**Ejemplo:**

```
User Controller
[Component: Controller]
Maneja requests HTTP relacionados con gestión de usuarios.
```

---

#### Service Component

```
Nombre del Servicio
[Component: Service]
Descripción de lógica de negocio implementada.
```

**Ejemplo:**

```
User Service
[Component: Service]
Implementa reglas de negocio para creación y gestión de usuarios.
```

---

#### Repository

```
Nombre del Repository
[Component: Repository]
Descripción de operaciones de acceso a datos.
```

**Ejemplo:**

```
User Repository
[Component: Repository]
Gestiona persistencia y consultas de datos de usuarios.
```

---

#### Adapter

```
Nombre del Adapter
[Component: Adapter]
Descripción de integración con sistemas externos.
```

**Ejemplo:**

```
SAP Adapter
[Component: Adapter]
Integra con SAP ERP para sincronización de datos de empleados.
```

---

#### Client

```
Nombre del Client
[Component: Client]
Descripción de consumo de APIs o servicios externos.
```

**Ejemplo:**

```
Notification API Client
[Component: Client]
Consume Notification API para envío de correos electrónicos.
```

---

#### Publisher

```
Nombre del Publisher
[Component: Publisher]
Descripción de publicación de eventos o mensajes.
```

**Ejemplo:**

```
User Event Publisher
[Component: Publisher]
Publica eventos de creación, actualización y eliminación de usuarios.
```

---

#### Consumer

```
Nombre del Consumer
[Component: Consumer]
Descripción de consumo y procesamiento de eventos.
```

**Ejemplo:**

```
User Event Consumer
[Component: Consumer]
Consume eventos de usuarios y actualiza caché local.
```

---

#### Mapper

```
Nombre del Mapper
[Component: Mapper]
Descripción de transformación de datos entre modelos.
```

**Ejemplo:**

```
User Mapper
[Component: Mapper]
Transforma entre modelos internos y DTOs de API.
```

---

#### Validator

```
Nombre del Validator
[Component: Validator]
Descripción de validaciones de negocio y datos.
```

**Ejemplo:**

```
User Validator
[Component: Validator]
Valida reglas de negocio y restricciones de datos de usuario.
```

---

## Tabla de Referencia Rápida

| Componente              | Nombre por defecto                 | Tipo por defecto                  | Descripción por defecto                                        |
| ----------------------- | ---------------------------------- | --------------------------------- | -------------------------------------------------------------- |
| **Person**              | Nombre del Rol                     | `[Person]`                        | Descripción del rol y responsabilidades del usuario.           |
| **External Person**     | Nombre del Usuario Externo         | `[External Person]`               | Descripción del rol e interacciones del usuario.               |
| **System**              | Nombre del Sistema                 | `[System]`                        | Descripción del propósito y capacidades del sistema.           |
| **External System**     | Nombre del Sistema Externo         | `[External System]`               | Descripción del propósito e integraciones del sistema externo. |
| **Application**         | Nombre de la Aplicación            | `[App: e.g. Tecnología]`          | Descripción de las responsabilidades de la aplicación.         |
| **Web Application**     | Nombre de la Aplicación Web        | `[App: e.g. Angular]`             | Descripción de funcionalidades y usuarios objetivo.            |
| **Mobile Application**  | Nombre de la Aplicación Móvil      | `[App: e.g. Flutter]`             | Descripción de funcionalidades y usuarios objetivo.            |
| **API**                 | Nombre del API                     | `[App: e.g. .NET 8 REST API]`     | Descripción de responsabilidades y capacidades expuestas.      |
| **Microservice**        | Nombre del Servicio                | `[App: e.g. .NET 8]`              | Descripción de responsabilidades y capacidades de negocio.     |
| **Worker**              | Nombre del Worker                  | `[App: e.g. .NET Worker Service]` | Descripción de procesamiento en segundo plano.                 |
| **Batch**               | Nombre del Proceso                 | `[App: Airflow]`                  | Descripción de proceso programado y frecuencia.                |
| **CDC Processor**       | Nombre del Procesador CDC          | `[App: e.g. Debezium]`            | Descripción de captura de cambios y flujo de datos.            |
| **API Gateway**         | Nombre del Gateway                 | `[App: e.g. Kong Gateway]`        | Descripción de routing, seguridad y gestión de APIs.           |
| **Store**               | Nombre del Almacenamiento          | `[Store: e.g. Tecnología]`        | Descripción del almacenamiento.                                |
| **Relational Database** | Nombre de la Base de Datos         | `[Store: e.g. PostgreSQL]`        | Descripción de datos almacenados y modelo de persistencia.     |
| **NoSQL Database**      | Nombre de la Base de Datos         | `[Store: e.g. DynamoDB]`          | Descripción de datos almacenados y patrones de acceso.         |
| **Cache**               | Nombre del Cache                   | `[Store: e.g. Redis]`             | Descripción de datos temporales y estrategia de caché.         |
| **Event Bus**           | Nombre de la Plataforma de Eventos | `[Store: e.g. Kafka]`             | Descripción de distribución de eventos y topics.               |
| **Queue**               | Nombre de la Cola                  | `[Store: e.g. SQS]`               | Descripción de procesamiento asíncrono y mensajería.           |
| **Object Storage**      | Nombre del Almacenamiento          | `[Store: e.g. Amazon S3]`         | Descripción de archivos y objetos almacenados.                 |
| **Component**           | Nombre del Componente              | `[Component]`                     | Descripción de responsabilidades e interacciones.              |
| **Controller**          | Nombre del Controller              | `[Component: Controller]`         | Descripción de manejo de requests y coordinación.              |
| **Service**             | Nombre del Servicio                | `[Component: Service]`            | Descripción de lógica de negocio implementada.                 |
| **Repository**          | Nombre del Repository              | `[Component: Repository]`         | Descripción de operaciones de acceso a datos.                  |
| **Adapter**             | Nombre del Adapter                 | `[Component: Adapter]`            | Descripción de integración con sistemas externos.              |
| **Client**              | Nombre del Client                  | `[Component: Client]`             | Descripción de consumo de APIs o servicios externos.           |
| **Publisher**           | Nombre del Publisher               | `[Component: Publisher]`          | Descripción de publicación de eventos o mensajes.              |
| **Consumer**            | Nombre del Consumer                | `[Component: Consumer]`           | Descripción de consumo y procesamiento de eventos.             |
| **Mapper**              | Nombre del Mapper                  | `[Component: Mapper]`             | Descripción de transformación de datos entre modelos.          |
| **Validator**           | Nombre del Validator               | `[Component: Validator]`          | Descripción de validaciones de negocio y datos.                |

---

## Notas de Uso

1. **Consistencia**: Usar siempre el mismo formato en todos los diagramas
2. **Tecnología específica**: Incluir versión cuando sea relevante (e.g., `.NET 8`, `Angular 17`)
3. **Descripciones**: Usar lenguaje claro y orientado a responsabilidades, no implementación
4. **Nombres**: En español para facilitar comprensión por equipos de negocio
5. **Tipos**: En inglés para mantener compatibilidad con C4 y estándares internacionales
