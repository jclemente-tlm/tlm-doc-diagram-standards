# Mejores Prácticas de Diagramas de Arquitectura

## Principios Fundamentales

### 1. Propósito Sobre Completitud

> **Los diagramas NO son inventarios técnicos exhaustivos. Son herramientas de comunicación.**

#### ✅ Hacer

- Incluir solo lo necesario para entender el propósito del diagrama
- Omitir detalles que no aportan valor
- Enfocarse en el mensaje principal

#### ❌ Evitar

- Intentar mostrar TODO en un diagrama
- Agregar elementos "por si acaso"
- Crear diagramas que nadie entiende

---

### 2. Consistencia Visual

> **La gente debe reconocer elementos SIN leer texto.**

#### ✅ Hacer

- Usar SIEMPRE los mismos colores para los mismos tipos
- Usar SIEMPRE los mismos shapes para las mismas categorías
- Seguir SIEMPRE las mismas convenciones de layout

#### ❌ Evitar

- Cambiar colores arbitrariamente
- Mezclar estilos visuales
- Crear "excepciones" sin justificación

---

### 3. Actualización Regular

> **Un diagrama desactualizado es peor que no tener diagrama.**

#### ✅ Hacer

- Actualizar diagramas cuando el sistema cambia
- Incluir actualización en Definition of Done
- Versionar diagramas en Git
- Asignar ownership claro

#### ❌ Evitar

- "Olvidar" actualizar diagramas
- Diagramas sin fecha de última actualización
- Diagramas huérfanos sin owner

---

## Mejores Prácticas por Aspecto

### Layout y Composición

#### Flujo Visual

**Regla**: El flujo debe ser natural y predecible.

✅ **Mejores prácticas**:

- **Flujo horizontal**: Izquierda → Derecha (request flow)
- **Flujo vertical**: Arriba → Abajo (jerarquía)
- **Usuarios**: Siempre arriba o izquierda
- **Stores**: Siempre abajo
- **Externos**: Siempre fuera del boundary principal

```
Usuarios/Actores (arriba)
    ↓
Gateways/APIs (borde)
    ↓
Servicios (centro)
    ↓
Stores (abajo)
```

#### Espaciado

**Regla**: Dar espacio para respirar.

✅ **Mejores prácticas**:

- Mínimo 20px entre elementos
- Mínimo 40px entre grupos
- Margins consistentes
- No apiñar elementos

❌ **Evitar**:

- Elementos pegados
- Flechas que se cruzan innecesariamente
- Texto que se solapa

#### Alineación

**Regla**: Alinear elementos del mismo tipo.

✅ **Mejores prácticas**:

- Servicios del mismo nivel: misma altura
- Bases de datos: alineadas con su servicio
- Grids imaginarios

---

### Nomenclatura

#### Nombres Descriptivos

**Regla**: El nombre debe ser autoexplicativo.

✅ **Correcto**:

```
Identity Service
Notification API
Payment Processor
Order Management System
```

❌ **Incorrecto**:

```
Service 1
API
MS
Sistema
```

#### Evitar Abreviaciones

**Regla**: Solo usar abreviaciones universalmente aceptadas.

✅ **Aceptado**:

- API, DB, UI, CDN, IAM

❌ **Evitar**:

- svc, proc, mgr, ctrl, repo, notif

#### Singular vs Plural

**Regla**: Usar singular por defecto.

✅ **Correcto**:

- `User Service` (no `Users Service`)
- `order-db` (no `orders-db`)
- `Product API` (no `Products API`)

---

### Flechas y Relaciones

#### Siempre Etiquetar

**Regla**: Toda flecha debe tener etiqueta.

✅ **Correcto**:

```
[Service A] --HTTPS/REST--> [Service B]
[Service A] --Kafka/Event--> [Event Bus]
[Service A] --SQL--> [Database]
```

❌ **Incorrecto**:

```
[Service A] --> [Service B]  (sin etiqueta)
```

#### Protocolo + Propósito

**Regla**: Indicar protocolo Y propósito cuando sea necesario.

✅ **Correcto**:

```
--HTTPS: "Autentica"-->
--Kafka: "OrderCreated event"-->
--SQL: "Lee usuarios"-->
```

#### Sincronía Clara

**Regla**: Usar estilo de línea para indicar sync/async.

✅ **Correcto**:

- Línea sólida → Sync (HTTP, gRPC)
- Línea dashed → Async (Events, messages)
- Línea dotted → Batch, scheduled

---

### Colores e Íconos

#### No Depender Solo de Color

**Regla**: Los diagramas deben ser entendibles en blanco y negro.

✅ **Hacer**:

- Color + Shape + Label + Icon
- Usar shapes distintivos
- Bordes claramente diferenciados

❌ **Evitar**:

- Depender solo de color
- Ignorar personas con daltonismo

#### Tecnología como Metadata

**Regla**: La tecnología es secundaria.

✅ **Correcto**:

```
┌─────────────────────┐
│ Notification API    │
│ .NET 8 | REST       │
└─────────────────────┘
```

❌ **Incorrecto**:

```
┌─────────────────────┐
│ .NET API            │
│ REST                │
└─────────────────────┘
```

#### Íconos Apropiados al Nivel

**Regla**: Íconos genéricos en C1/C2, tecnológicos en Deployment.

✅ **C1/C2**:

- 🌐 Globe para APIs
- ⚙️ Gear para workers
- 🗄️ Database para stores

✅ **Deployment**:

- AWS icons oficiales
- Docker logo
- ECS/Fargate logos

---

### Boundaries y Agrupación

#### Usar Boundaries con Propósito

**Regla**: Los boundaries deben agrupar elementos relacionados.

✅ **Usar para**:

- Sistemas completos
- Dominios de negocio
- Países/Tenants
- AWS Accounts/VPCs

❌ **NO usar para**:

- Agrupar elementos arbitrariamente
- "Decoración"

#### Etiquetar Boundaries

**Regla**: Toda boundary debe estar claramente etiquetada.

✅ **Formato**:

```
[Sistema] Identity Platform
[Dominio] Notificaciones
[País] Peru
[AWS Account] Production
```

---

### Nivel de Detalle

#### Regla del Goldilocks

**Regla**: Ni muy detallado, ni muy abstracto. Justo lo necesario.

✅ **Correcto**:

- C1: 5-10 elementos
- C2: 10-20 elementos
- C3: 5-12 elementos

❌ **Evitar**:

- C2 con 50 microservicios
- C1 con bases de datos
- C3 con clases

#### Dividir Cuando Sea Necesario

**Regla**: Si el diagrama no cabe cómodamente, dividir.

✅ **Estrategias de división**:

- Por dominio de negocio
- Por país/tenant
- Por bounded context
- Por subsistema

---

## Mejores Prácticas por Escenario

### Microservicios

#### Mostrar Patrones, No Todo

**Problema**: 50 microservicios no caben en un diagrama legible.

✅ **Solución**:

```
Opción 1: Agrupar por dominio
[Order Domain]
  - Order Service
  - Order-DB
  - Order Worker

[Payment Domain]
  - Payment Service
  - Payment-DB

Opción 2: Diagrama por dominio
- order-services.drawio
- payment-services.drawio
- catalog-services.drawio

Opción 3: Mostrar patrón representativo
"Patrón típico de microservicio"
[API] → [Service] → [DB]
              ↓
         [Event Bus]
```

---

### Event-Driven Architecture

#### Clarity en Eventos

✅ **Mejores prácticas**:

1. **Etiquetar eventos**:

```
[Order Service] --"OrderCreated"--> [Event Bus]
[Event Bus] --"OrderCreated"--> [Email Worker]
```

2. **Usar líneas dashed**:

```
[Service] -------> [Event Bus] (dashed = async)
```

3. **Indicar topics**:

```
[Event Bus: "order.created"]
```

4. **Mostrar patrones**:

```
Producer → Topic → Consumer(s)
```

---

### CDC (Change Data Capture)

#### Representación Clara

✅ **Mejores prácticas**:

```
[Oracle DB] --CDC (Debezium)--> [Kafka] --"user.changes"--> [Service]
```

1. Etiquetar explícitamente como "CDC"
2. Indicar tecnología (Debezium, etc.)
3. Línea dashed (es async)
4. Mostrar topic name

---

### Multi-Tenant / Multi-País

#### Strategies de Representación

**Opción 1: Boundaries**

```
┌─[Peru]──────────────┐
│ [Services]          │
└─────────────────────┘

┌─[Chile]─────────────┐
│ [Services]          │
└─────────────────────┘
```

**Opción 2: Diagramas Separados**

```
- architecture-peru.drawio
- architecture-chile.drawio
+ architecture-common.drawio (componentes compartidos)
```

**Opción 3: Anotaciones**

```
[Identity Service]
(Peru, Chile, Colombia)
```

---

### Integraciones con Sistemas Legacy

#### Claridad en Integración

✅ **Mejores prácticas**:

1. **Marcar como externo**:

```
[SAP ERP] (Shape: Hexágono, Color: Azul claro)
```

2. **Indicar protocolo claramente**:

```
[Our System] --SOAP/XML--> [SAP]
[Our System] --RFC--> [SAP]
[Our System] --REST--> [External API]
```

3. **Mostrar adapter pattern**:

```
[Service] → [SAP Adapter] → [SAP ERP]
```

---

## Anti-Patrones Comunes

### 1. El "Diagrama Spaghetti"

**Problema**: Todo conectado con todo.

❌ **Síntomas**:

- Flechas cruzadas por todos lados
- Imposible seguir un flujo
- Más de 30 elementos

✅ **Solución**:

- Dividir en múltiples diagramas
- Agrupar por dominio
- Mostrar solo relaciones principales
- Usar sequence diagrams para flujos complejos

---

### 2. El "Diagrama PowerPoint"

**Problema**: Más logos que arquitectura.

❌ **Síntomas**:

- Logos gigantes de vendors
- Íconos coloridos dominando
- Parece slide de marketing

✅ **Solución**:

- Íconos pequeños y consistentes
- Tecnología como metadata
- Foco en función, no en vendor

---

### 3. El "Diagrama Frankenstein"

**Problema**: Mezcla múltiples niveles.

❌ **Síntomas**:

- Clases y sistemas externos en el mismo diagrama
- C1 con componentes internos
- C2 con métodos de código

✅ **Solución**:

- Respetar niveles C4
- Un propósito por diagrama
- Dividir en múltiples diagramas

---

### 4. El "Diagrama Fantasma"

**Problema**: Desactualizado y olvidado.

❌ **Síntomas**:

- Sin fecha de actualización
- Menciona servicios que ya no existen
- Nadie sabe quién lo mantiene

✅ **Solución**:

- Asignar owner
- Incluir en PR checklist
- Agregar fecha de actualización
- Versionar en Git

---

### 5. El "Diagrama Minimalista Extremo"

**Problema**: Demasiado abstracto para ser útil.

❌ **Síntomas**:

- Cajas sin nombres específicos
- "Sistema A", "Sistema B"
- Flechas sin etiquetar

✅ **Solución**:

- Nombres específicos y descriptivos
- Etiquetar todas las relaciones
- Agregar contexto suficiente

---

## Workflow Recomendado

### Crear un Nuevo Diagrama

1. **Definir propósito**:
   - ¿Qué pregunta responde?
   - ¿Quién es la audiencia?

2. **Elegir nivel C4 apropiado**

3. **Sketch inicial**:
   - Whiteboard o papel
   - Validar con equipo

4. **Crear versión digital**:
   - Usar plantilla corporativa
   - Aplicar convenciones visuales

5. **Revisar**:
   - Checklist de validación
   - Feedback del equipo

6. **Publicar y versionar**:
   - Commit a Git
   - Documentar en README

---

### Actualizar un Diagrama Existente

1. **Identificar cambio**:
   - ¿Nuevo servicio?
   - ¿Nueva integración?
   - ¿Refactoring?

2. **Determinar impacto**:
   - ¿Afecta C1?
   - ¿Afecta C2?
   - ¿Afecta C3?

3. **Actualizar diagrama**:
   - Mantener consistencia visual
   - Actualizar fecha

4. **Revisar en PR**:
   - Incluir diagrama actualizado
   - Validar cambios

---

## Checklist General de Calidad

### Para TODO Diagrama

- [ ] Tiene un título claro
- [ ] Tiene fecha de última actualización
- [ ] Tiene owner/responsable identificado
- [ ] Sigue convenciones de color estándar
- [ ] Usa shapes apropiados
- [ ] Todas las flechas están etiquetadas
- [ ] Nomenclatura consistente
- [ ] Layout lógico y claro
- [ ] Nivel de detalle apropiado
- [ ] Es entendible en blanco y negro
- [ ] Está versionado en Git
- [ ] Está vinculado desde documentación

---

## Herramientas y Técnicas

### Collaborative Creation

#### Whiteboard Sessions

✅ **Usar para**:

- Workshops iniciales
- Brainstorming de arquitectura
- Event storming
- Validación rápida

**Herramientas**: Miro, Mural, FigJam, whiteboard físico

#### Digital Diagrams

✅ **Usar para**:

- Documentación formal
- Versionamiento
- Precisión
- Distribución

**Herramientas**: Draw.io, Structurizr, Lucidchart

---

### Diagrams as Code

#### Cuándo Usar

✅ **Ventajas**:

- Versionamiento fácil
- Diff friendly
- Automation friendly
- Consistencia forzada

✅ **Usar para**:

- Equipos técnicos
- Diagramas que cambian frecuentemente
- CI/CD integration

#### Herramientas

- **Structurizr DSL**: C4-native, muy completo
- **PlantUML**: Versátil, ampliamente soportado
- **Mermaid**: Simple, soportado en GitHub/GitLab

---

### Automation

#### Validación Automática

```bash
# Pre-commit hook
- Validar que diagramas existen
- Validar fecha de actualización
- Validar nomenclatura (script)
```

#### Generación Automática

```bash
# Desde código
- Dependency graphs
- Database schemas (ERD)
- API documentation diagrams
```

---

## Tips por Audiencia

### Para Stakeholders de Negocio

✅ **Enfatizar**:

- Propósito de negocio
- Integraciones clave
- Actores/usuarios
- Flujos principales

❌ **Evitar**:

- Detalles técnicos
- Tecnologías específicas
- Componentes internos

**Diagrama ideal**: C1 (Context)

---

### Para Arquitectos

✅ **Enfatizar**:

- Patrones arquitectónicos
- Integraciones
- Decisiones de diseño
- Trade-offs

**Diagramas ideales**: C1, C2, Deployment

---

### Para Developers

✅ **Enfatizar**:

- Estructura de código
- APIs y contratos
- Flujos de datos
- Responsabilidades

**Diagramas ideales**: C2, C3, Sequence

---

### Para DevOps/SRE

✅ **Enfatizar**:

- Infraestructura
- Networking
- Escalabilidad
- Alta disponibilidad

**Diagramas ideales**: Deployment, Infrastructure

---

## Métricas de Calidad

### Indicadores de un Buen Diagrama

1. **Clarity**: ¿Se entiende sin explicación?
2. **Accuracy**: ¿Refleja la realidad?
3. **Relevance**: ¿Es útil para su propósito?
4. **Maintainability**: ¿Es fácil de actualizar?
5. **Consistency**: ¿Sigue los estándares?

### Red Flags

- ❌ Nadie sabe quién lo creó
- ❌ Nadie lo ha actualizado en 6+ meses
- ❌ No está versionado
- ❌ Contradice otros diagramas
- ❌ Usa colores/shapes no estándar

---

## Referencias

- [C4 Overview](./c4-overview.md)
- [Validation Criteria](../validation-criteria.md)
