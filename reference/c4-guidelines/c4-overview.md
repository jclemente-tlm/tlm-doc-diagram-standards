# Modelo C4 - Overview

## ¿Qué es C4?

El **modelo C4** es un enfoque jerárquico para visualizar la arquitectura de software mediante diagramas en **4 niveles de abstracción**:

- **C1 - Context**: El sistema en su contexto
- **C2 - Container**: Contenedores de alto nivel (aplicaciones, servicios, bases de datos)
- **C3 - Component**: Componentes dentro de un contenedor
- **C4 - Code**: Clases e implementación detallada

## Por qué C4

### Problemas que Resuelve

1. **Inconsistencia**: Cada equipo dibuja diagramas de forma diferente
2. **Niveles mezclados**: Diagramas que mezclan infraestructura, negocio y código
3. **Demasiado detalle**: Diagramas ilegibles con todo junto
4. **Muy abstracto**: Diagramas que no ayudan a los developers
5. **Falta de estándar**: No existe un lenguaje común

### Ventajas

- ✅ **Simple**: Fácil de aprender y aplicar
- ✅ **Escalable**: Funciona desde monolitos hasta microservicios
- ✅ **Universal**: Lo entienden devs, arquitectos y stakeholders
- ✅ **Flexible**: Se adapta a cloud, eventos, APIs, legacy
- ✅ **Jerárquico**: Permite zoom in/out según la audiencia

## Adaptación Corporativa

Nuestra implementación de C4 está **adaptada** a las necesidades de la empresa:

### Modificaciones al C4 Estándar

1. **Simplificación visual**
   - Colores estandarizados por categoría
   - Iconografía corporativa
   - Shapes específicos por tipo

2. **Separación semántica**
   - **Container-App**: Aplicaciones con lógica (APIs, services, workers)
   - **Container-Store**: Almacenamiento pasivo (DBs, queues, storage)

3. **Enfoque pragmático**
   - C1 y C2 son **obligatorios**
   - C3 es **opcional** (solo para servicios complejos)
   - C4 es **raramente usado** (herramientas automáticas preferidas)

4. **Integración con tecnologías actuales**
   - Soporte para microservicios
   - Event-driven architecture
   - CDC (Change Data Capture)
   - Cloud-native patterns
   - API Gateways
   - Message buses (Kafka, etc.)

## Los 4 Niveles en Detalle

### Nivel 1 - Context Diagram

**Zoom Level**: 🔭 Muy alejado

**Pregunta**: ¿Cómo se relaciona el sistema con el mundo exterior?

**Elementos**:

- El sistema como **caja negra**
- Usuarios (internos y externos)
- Sistemas externos

**Ejemplo Mental**: Vista satelital de una ciudad

**Obligatorio**: ✅ SÍ

---

### Nivel 2 - Container Diagram

**Zoom Level**: 🔍 Alejado

**Pregunta**: ¿Cómo está construido el sistema internamente?

**Elementos**:

- APIs y microservicios
- Bases de datos
- Message buses
- Object storage
- Frontends
- Workers

**Ejemplo Mental**: Plano de edificios en una ciudad

**Obligatorio**: ✅ SÍ

---

### Nivel 3 - Component Diagram

**Zoom Level**: 🔬 Cercano

**Pregunta**: ¿Cómo está organizado internamente un servicio?

**Elementos**:

- Controllers
- Services
- Repositories
- Handlers
- Adapters
- Processors

**Ejemplo Mental**: Plano de habitaciones dentro de un edificio

**Obligatorio**: ⚠️ OPCIONAL

---

### Nivel 4 - Code Diagram

**Zoom Level**: 🔬🔬 Muy cercano

**Pregunta**: ¿Cómo está implementado el código?

**Elementos**:

- Clases
- Interfaces
- Métodos
- Relaciones de herencia

**Ejemplo Mental**: Plano de instalaciones eléctricas dentro de una habitación

**Obligatorio**: ❌ NO (preferir herramientas automáticas)

## Jerarquía Visual

```
┌─────────────────────────────────────────┐
│         C1 - Context Diagram            │  👥 Stakeholders, Ejecutivos
│                                         │
│  ┌───────────────────────────────────┐ │
│  │    C2 - Container Diagram         │ │  🏗️ Arquitectos, Tech Leads
│  │                                   │ │
│  │  ┌─────────────────────────────┐ │ │
│  │  │  C3 - Component Diagram     │ │ │  👨‍💻 Developers
│  │  │                             │ │ │
│  │  │  ┌───────────────────────┐ │ │ │
│  │  │  │ C4 - Code Diagram     │ │ │ │  🔧 Developers (detalle)
│  │  │  └───────────────────────┘ │ │ │
│  │  └─────────────────────────────┘ │ │
│  └───────────────────────────────────┘ │
└─────────────────────────────────────────┘
```

## Cuándo Usar Cada Nivel

| Situación                        | Diagrama        |
| -------------------------------- | --------------- |
| Presentación ejecutiva           | C1              |
| Revisión de arquitectura         | C2              |
| Diseño de nuevo sistema          | C1 + C2         |
| Onboarding de equipo             | C1 + C2         |
| Diseño de microservicio complejo | C2 + C3         |
| Análisis de integración          | C1 + C2         |
| Documentación de infraestructura | C2 + Deployment |

## Relación con Otros Diagramas

C4 se complementa con otros tipos de diagramas:

```
C4 Model (estructura estática)
├── C1 - Context
├── C2 - Container
└── C3 - Component

Diagramas Complementarios
├── Deployment Diagram (dónde se despliega)
├── Sequence Diagram (cómo interactúa en el tiempo)
├── Data Flow Diagram (cómo fluyen los datos)
└── Integration Diagram (cómo se integra)
```

## Principios Fundamentales

### 1. Abstracción Adecuada

Cada nivel tiene su propósito:

- No mezclar niveles
- No poner demasiado detalle
- No ser demasiado abstracto

### 2. Audiencia Clara

Cada diagrama debe tener una audiencia específica:

- C1: Stakeholders, ejecutivos
- C2: Arquitectos, tech leads
- C3: Developers del servicio específico

### 3. Mantenibilidad

Los diagramas deben ser:

- Fáciles de actualizar
- Versionados en Git
- Vinculados a la realidad del código

### 4. Propósito Sobre Completitud

Los diagramas deben:

- Comunicar ideas
- **NO** ser un inventario exhaustivo
- Omitir detalles irrelevantes

## Elementos Comunes en Todos Los Niveles

### 1. Personas (Actores)

Representan usuarios o roles:

- Usuarios internos
- Usuarios externos
- Sistemas externos que actúan como actores

**Color**: Verde (interno), Gris (externo)

### 2. Software Systems

En C1: El sistema completo
En C2: Sistemas externos
En C3: No aparecen

**Color**: Azul claro (externos)

### 3. Containers

Solo en C2 y C3:

- Apps: Morado
- Stores: Rojo/Rosado

### 4. Components

Solo en C3:
**Color**: Amarillo

### 5. Relationships (Flechas)

Indican comunicación:

- Sólidas: Sync
- Dashed: Async
- Etiquetadas con protocolo/propósito

## Notación Visual (Nuestro Estándar)

### Colores

| Elemento        | Color          | Hex     |
| --------------- | -------------- | ------- |
| External System | Azul claro     | #DAE8FC |
| Container-App   | Morado claro   | #E1D5E7 |
| Container-Store | Rojo claro     | #F8CECC |
| Component       | Amarillo claro | #FFF2CC |
| Internal User   | Verde claro    | #D5E8D4 |
| External User   | Gris           | #EEEEEE |

### Shapes

| Tipo            | Shape               |
| --------------- | ------------------- |
| App/Service     | Rectángulo          |
| Database        | Cilindro vertical   |
| Message Bus     | Cilindro horizontal |
| Object Storage  | Folder              |
| Frontend        | Shape web           |
| External System | Hexágono            |

## Anti-Patrones

### ❌ NO Hacer

1. **Mezclar niveles**
   - Poner componentes en un C2
   - Poner microservicios en un C1

2. **Demasiado detalle**
   - Mostrar todas las tablas de DB en C2
   - Mostrar todos los métodos en C3

3. **Muy genérico**
   - Cajas sin nombre descriptivo
   - Flechas sin etiquetar

4. **Tecnología como identidad**
   - Nombrar servicios por su tecnología
   - Usar logos en lugar de función

5. **Diagramas estáticos**
   - No actualizar cuando el sistema cambia
   - Diagramas que no reflejan la realidad

## Herramientas Recomendadas

### Para C1, C2, C3

**Opción 1: Draw.io** (Recomendado para nosotros)

- ✅ Librería corporativa
- ✅ Templates
- ✅ Fácil de usar
- ✅ Versionable (XML)

**Opción 2: Structurizr**

- ✅ Diagrams as Code
- ✅ Basado en C4
- ✅ Muy profesional
- ⚠️ Requiere aprendizaje de DSL

**Opción 3: PlantUML / Mermaid**

- ✅ Text-based
- ✅ Fácil de versionar
- ⚠️ Limitado visualmente

## Workflow Recomendado

### Para un Nuevo Sistema

1. **Empezar con C1**
   - Identificar actores
   - Identificar sistemas externos
   - Definir responsabilidades principales

2. **Expandir a C2**
   - Identificar containers (APIs, DBs, etc.)
   - Definir integraciones
   - Documentar protocolos

3. **C3 solo si es necesario**
   - Para servicios complejos
   - Cuando el equipo lo necesite

4. **Complementar**
   - Agregar deployment diagram
   - Agregar sequence diagrams para flujos críticos

### Para Actualizar un Sistema Existente

1. Revisar C1: ¿Hay nuevos sistemas externos o actores?
2. Revisar C2: ¿Hay nuevos servicios, DBs, o integraciones?
3. Revisar C3: ¿Hay refactoring significativo?
4. Actualizar deployment si la infra cambió

## Validación de Diagramas C4

### Checklist C1

- [ ] Muestra el sistema como caja negra
- [ ] Incluye todos los actores principales
- [ ] Incluye sistemas externos que interactúan
- [ ] NO muestra detalles internos
- [ ] NO muestra tecnologías específicas
- [ ] Las flechas están etiquetadas

### Checklist C2

- [ ] Muestra todos los containers principales
- [ ] Incluye databases, message buses, storage
- [ ] Las integraciones están claras
- [ ] NO muestra componentes internos
- [ ] Los colores siguen el estándar
- [ ] Las flechas indican protocolos

### Checklist C3

- [ ] Se enfoca en UN solo container
- [ ] Muestra componentes con responsabilidades claras
- [ ] NO muestra clases o código
- [ ] Los nombres son descriptivos
- [ ] Sigue convenciones de nomenclatura

## Integración con SDLC

### En el Código

Cada repositorio debe tener:

```
/docs
  /architecture
    - context.md (descripción + link al diagrama)
    - containers.md
    - components.md (si aplica)
```

### En PRs

Pull Request checklist:

- [ ] ¿El cambio afecta el C2? → Actualizar
- [ ] ¿Hay nuevas integraciones? → Actualizar C2
- [ ] ¿Cambió la arquitectura interna? → Actualizar C3

## Próximos Pasos

1. Leer las guías detalladas de cada nivel:
   - [Best Practices](./best-practices.md)

## Referencias

- Sitio oficial C4 Model: <https://c4model.com>
