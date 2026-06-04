# Criterios de Validación de Diagramas

## Propósito

Este documento define los criterios específicos que los diagramas deben cumplir para ser considerados válidos y completos. Úsalo como checklist durante creación, revisión y auditorías.

---

## Validación por Nivel C4

### C1 - Context Diagram

#### ✅ Criterios Obligatorios

- [ ] **Título claro**: Formato "[Nombre del Sistema] - Context Diagram"
- [ ] **Sistema principal**: Representado como una sola caja
- [ ] **Actores identificados**: Todos los usuarios principales (internos y externos)
- [ ] **Sistemas externos**: Todos los sistemas que interactúan están representados
- [ ] **Relaciones etiquetadas**: Todas las flechas tienen etiquetas descriptivas
- [ ] **Colores estándar**: Usuarios internos (verde), externos (gris), sistemas externos (azul)
- [ ] **Sin detalles internos**: NO muestra bases de datos, microservicios, o componentes

#### ✅ Criterios de Calidad

- [ ] **Máximo 10 elementos**: Si tiene más, probablemente tiene demasiado detalle
- [ ] **Layout lógico**: Usuarios arriba/izquierda, externos a los lados
- [ ] **Propósito claro**: Alguien nuevo al proyecto puede entender qué hace el sistema
- [ ] **Verbos de acción**: Las etiquetas usan verbos ("Autentica", "Consulta", "Envía")

#### ❌ Anti-Patrones a Evitar

- [ ] No muestra bases de datos en C1
- [ ] No muestra microservicios individuales
- [ ] No muestra tecnologías específicas
- [ ] No usa nombres genéricos ("Sistema A", "Usuario")
- [ ] No tiene flechas sin etiquetar

---

### C2 - Container Diagram

#### ✅ Criterios Obligatorios

- [ ] **Título claro**: Formato "[Nombre del Sistema] - Container Diagram"
- [ ] **Todos los containers**: APIs, servicios, workers, frontends representados
- [ ] **Todos los stores**: DBs, message buses, queues, object storage representados
- [ ] **Separación visual clara**: Apps (morado), Stores (rojo), Externos (azul)
- [ ] **Shapes correctos**: Cilindros para DBs/buses, rectángulos para apps, folders para storage
- [ ] **Protocolos etiquetados**: Todas las conexiones muestran protocolo (HTTPS, Kafka, SQL, etc.)
- [ ] **Sincronía indicada**: Líneas sólidas (sync), dashed (async), dotted (batch)
- [ ] **Boundaries definidos**: Si hay múltiples sistemas/dominios, usar boundaries

#### ✅ Criterios de Calidad

- [ ] **15-20 elementos máximo**: Si tiene más, considerar dividir por dominio
- [ ] **Layout lógico**: Usuarios arriba, gateways en borde, stores abajo
- [ ] **Nombres descriptivos**: "Identity Service", "notification-db", "Event Bus"
- [ ] **Integraciones claras**: Se entiende cómo fluyen los datos
- [ ] **Consistencia**: Elementos similares tienen tamaño y estilo consistente

#### ✅ Validación Técnica

- [ ] **Databases**: Una por servicio (o justificación de DB compartida)
- [ ] **Message buses**: Etiquetados con tecnología (Kafka, SQS, etc.)
- [ ] **APIs**: Incluyen protocolo (REST, gRPC, GraphQL)
- [ ] **Gateways**: Claramente identificados y posicionados

#### ❌ Anti-Patrones a Evitar

- [ ] No muestra componentes internos (controllers, repositories)
- [ ] No muestra clases o métodos
- [ ] No mezcla niveles (C1 y C3 en un C2)
- [ ] No tiene elementos sin nombre
- [ ] No tiene flechas sin protocolo

---

### C3 - Component Diagram

#### ✅ Criterios Obligatorios

- [ ] **Título claro**: Formato "[Nombre del Container] - Component Diagram"
- [ ] **Scope único**: Se enfoca en UN solo container
- [ ] **Componentes identificados**: Controllers, services, repositories, handlers, adapters
- [ ] **Responsabilidades claras**: Cada componente tiene una responsabilidad principal
- [ ] **Colores estándar**: Todos los componentes en amarillo (`#FFF2CC`)
- [ ] **Nomenclatura consistente**: Formato "[Función] [Tipo]" (ej: "User Controller")

#### ✅ Criterios de Calidad

- [ ] **10-12 componentes máximo**: Si tiene más, probablemente es demasiado detallado
- [ ] **Flujo claro**: De arriba hacia abajo (request → response)
- [ ] **Patrones reconocibles**: Controller → Service → Repository es evidente
- [ ] **Separación de concerns**: UI, lógica de negocio, y acceso a datos están separados

#### ❌ Anti-Patrones a Evitar

- [ ] No muestra clases individuales
- [ ] No muestra métodos o propiedades
- [ ] No mezcla componentes de diferentes containers
- [ ] No usa nombres de clases específicas del código
- [ ] No tiene componentes sin responsabilidad clara

---

## Validación por Tipo de Diagrama

### Deployment Diagram

#### ✅ Criterios Obligatorios

- [ ] **Infraestructura completa**: VPCs, subnets, clusters, nodes representados
- [ ] **Containers mapeados**: Todos los containers del C2 están desplegados
- [ ] **Networking**: Load balancers, DNS, CDN incluidos
- [ ] **Security**: Security groups, firewalls, IAM representados
- [ ] **Cloud services**: RDS, S3, Lambda, etc. con íconos oficiales
- [ ] **Regiones/Zonas**: Multi-AZ o multi-region claramente indicado

#### ✅ Criterios de Calidad

- [ ] **Íconos oficiales**: AWS/Azure/GCP icons oficiales
- [ ] **Alta disponibilidad**: Se muestra redundancia
- [ ] **Escalabilidad**: Auto-scaling groups indicados
- [ ] **Ambientes separados**: Prod, staging, dev en diagramas separados

#### ❌ Anti-Patrones a Evitar

- [ ] No mezcla lógica de negocio con infraestructura
- [ ] No usa íconos genéricos para servicios cloud específicos
- [ ] No omite seguridad o networking

---

### Sequence Diagram

#### ✅ Criterios Obligatorios

- [ ] **Título descriptivo**: Nombre del flujo (ej: "User Authentication Flow")
- [ ] **Actores identificados**: Usuario, sistemas, servicios involucrados
- [ ] **Orden temporal**: De arriba hacia abajo
- [ ] **Mensajes etiquetados**: Cada flecha tiene descripción
- [ ] **Respuestas indicadas**: Request/response claramente diferenciados
- [ ] **Condiciones**: If/else, loops claramente marcados

#### ✅ Criterios de Calidad

- [ ] **Enfoque específico**: Un flujo, no múltiples escenarios
- [ ] **Errores incluidos**: Happy path Y error paths
- [ ] **Timing**: Indicaciones de async cuando relevante

---

### Integration Diagram

#### ✅ Criterios Obligatorios

- [ ] **Sistemas integrados**: Todos los sistemas externos claramente identificados
- [ ] **Protocolos**: REST, SOAP, RFC, batch, CDC, etc. etiquetados
- [ ] **Dirección de datos**: Unidireccional o bidireccional claro
- [ ] **Adaptadores**: Si existen, están representados
- [ ] **Formato de datos**: JSON, XML, CSV, etc. indicado

#### ✅ Criterios de Calidad

- [ ] **Error handling**: Cómo se manejan fallos de integración
- [ ] **Frecuencia**: Real-time, batch, scheduled indicado
- [ ] **Autenticación**: OAuth, API Keys, certificados mencionados

---

## Validación Transversal

### Metadata

#### ✅ Todo Diagrama Debe Tener

- [ ] **Título**: "[Sistema] - [Tipo de Diagrama]"
- [ ] **Versión**: vX.Y
- [ ] **Fecha**: Última actualización (formato YYYY-MM-DD)
- [ ] **Autor**: Persona o equipo responsable
- [ ] **Estado**: Draft | Review | Approved | Deprecated

#### Ejemplo de Metadata

```
Title: Identity Platform - Container Diagram
Version: v2.3
Date: 2024-01-15
Author: Platform Team
Status: Approved
```

---

### Convenciones Visuales

#### ✅ Validación de Colores

- [ ] **External System**: `#DAE8FC` (azul claro)
- [ ] **Container-App**: `#E1D5E7` (morado claro)
- [ ] **Container-Store**: `#F8CECC` (rojo claro)
- [ ] **Component**: `#FFF2CC` (amarillo claro)
- [ ] **Internal User**: `#D5E8D4` (verde claro)
- [ ] **External User**: `#EEEEEE` (gris)
- [ ] **Sin colores personalizados** no documentados

#### ✅ Validación de Shapes

- [ ] **Rectángulo**: Para apps, APIs, servicios
- [ ] **Cilindro vertical**: Para bases de datos
- [ ] **Cilindro horizontal**: Para message buses
- [ ] **Folder**: Para object storage
- [ ] **Shape web**: Para frontends
- [ ] **Hexágono**: Para sistemas externos
- [ ] **Persona**: Para usuarios/actores

#### ✅ Validación de Flechas

- [ ] **Sólida**: Para comunicación síncrona
- [ ] **Dashed**: Para comunicación asíncrona
- [ ] **Dotted**: Para batch/scheduled
- [ ] **Todas etiquetadas**: Protocolo + propósito
- [ ] **Dirección clara**: Unidireccional o bidireccional

---

### Nomenclatura

#### ✅ Validación de Nombres

- [ ] **Descriptivos**: No "Sistema A", "DB 1", "API"
- [ ] **Consistentes**: Misma convención en todo el diagrama
- [ ] **Inglés**: Para elementos técnicos
- [ ] **Singular**: "User Service" no "Users Service"
- [ ] **Sin abreviaciones prohibidas**: No "svc", "proc", "mgr"
- [ ] **Sufijos estándar**: "Service", "API", "Worker", "DB"

#### ✅ Validación de Protocolo/Tecnología

- [ ] **Kafka topics**: Formato `tipo.dominio.entidad.accion`
- [ ] **Bases de datos**: Formato `dominio-db`
- [ ] **APIs**: Incluyen tipo (REST, gRPC, GraphQL)

---

### Layout y Composición

#### ✅ Validación de Layout

- [ ] **Flujo lógico**: Izquierda → derecha o arriba → abajo
- [ ] **Usuarios arriba/izquierda**: Siempre en posición inicial
- [ ] **Stores abajo**: Bases de datos y storage en la parte inferior
- [ ] **Externos fuera**: Sistemas externos fuera del boundary principal
- [ ] **Espaciado consistente**: Mínimo 20px entre elementos
- [ ] **Sin cruces innecesarios**: Flechas no se cruzan sin razón
- [ ] **Alineación**: Elementos del mismo tipo alineados

#### ✅ Validación de Boundaries

- [ ] **Etiquetados**: Todos los boundaries tienen nombre
- [ ] **Propósito claro**: Sistema, dominio, país, AWS account
- [ ] **No redundantes**: No boundaries innecesarios
- [ ] **Formato**: `[Tipo] Nombre` (ej: `[Sistema] Identity Platform`)

---

### Tipografía y Legibilidad

#### ✅ Validación de Texto

- [ ] **Tamaño legible**: Mínimo 9pt
- [ ] **Fuente consistente**: Una fuente sans-serif en todo el diagrama
- [ ] **Contraste**: Texto legible sobre fondo
- [ ] **Sin texto cortado**: Todos los nombres completos visibles
- [ ] **Jerarquía clara**: Títulos principales en bold

---

## Validación de Calidad General

### Comunicación

#### ✅ El Diagrama Debe

- [ ] **Ser autoexplicativo**: Alguien nuevo puede entenderlo sin explicación verbal
- [ ] **Responder preguntas**: ¿Qué hace? ¿Cómo funciona? ¿Cómo se integra?
- [ ] **Ser útil**: Alguien lo usará (onboarding, análisis, revisión)
- [ ] **Estar completo**: No faltan elementos críticos
- [ ] **No tener exceso**: No tiene elementos innecesarios

### Mantenibilidad

#### ✅ El Diagrama Debe

- [ ] **Ser actualizable**: Fácil de modificar cuando el sistema cambie
- [ ] **Estar versionado**: En Git con historial
- [ ] **Tener owner**: Alguien responsable de mantenerlo
- [ ] **Ser accesible**: En ubicación conocida y accesible al equipo
- [ ] **Estar documentado**: Link desde README o wiki

### Precisión

#### ✅ El Diagrama Debe

- [ ] **Reflejar realidad**: Representa el sistema como es (o será)
- [ ] **Estar actualizado**: Última modificación < 3 meses (o < 1 mes para sistemas activos)
- [ ] **Ser consistente**: Con otros diagramas del mismo sistema
- [ ] **No tener errores**: Nombres, protocolos, relaciones son correctos

---

## Checklists por Escenario

### Pull Request Review

```markdown
## Architecture Diagram Checklist

- [ ] Diagramas afectados identificados
- [ ] Diagramas actualizados con cambios
- [ ] Convenciones visuales seguidas
- [ ] Nomenclatura consistente
- [ ] Metadata actualizada (versión, fecha)
- [ ] Validación de criterios completada
- [ ] README actualizado con links a diagramas (si aplica)
```

### Architecture Review Board

```markdown
## ARB Review Checklist

### Diagramas Presentados

- [ ] C1 - Context Diagram
- [ ] C2 - Container Diagram
- [ ] Deployment Diagram (si aplica)
- [ ] Otros diagramas relevantes

### Cumplimiento de Estándares

- [ ] Siguen modelo C4
- [ ] Usan colores corporativos
- [ ] Usan shapes estándar
- [ ] Nomenclatura consistente
- [ ] Metadata completa

### Calidad Arquitectónica

- [ ] Escalabilidad considerada
- [ ] Alta disponibilidad considerada
- [ ] Seguridad considerada
- [ ] Observabilidad considerada
- [ ] Manejo de fallos considerado

### Integraciones

- [ ] Sistemas externos identificados
- [ ] Protocolos especificados
- [ ] Autenticación/autorización definida
- [ ] Formatos de datos especificados

### Documentación

- [ ] ADRs escritos (si aplica)
- [ ] README con contexto
- [ ] Links a diagramas en documentación

### Decisión

- [ ] Aprobado
- [ ] Aprobado con cambios menores
- [ ] Requiere cambios mayores
- [ ] Rechazado

**Comentarios**:
[Feedback del ARB]
```

### Auditoría Trimestral

```markdown
## Architecture Audit Checklist

### Sistema: [Nombre del Sistema]

#### Existencia de Diagramas

- [ ] C1 - Context Diagram existe
- [ ] C2 - Container Diagram existe
- [ ] Deployment Diagram existe
- [ ] C3 - Component Diagrams (si aplica)

#### Actualidad

- [ ] C1 actualizado en últimos 6 meses
- [ ] C2 actualizado en últimos 3 meses
- [ ] Deployment actualizado en últimos 3 meses

#### Ubicación y Accesibilidad

- [ ] Diagramas en repositorio correcto
- [ ] Links desde README funcionales
- [ ] Accesibles al equipo

#### Cumplimiento de Estándares

- [ ] Colores corporativos
- [ ] Shapes estándar
- [ ] Nomenclatura consistente
- [ ] Metadata completa

#### Owner y Mantenimiento

- [ ] Owner identificado
- [ ] Owner activo en la organización
- [ ] Plan de actualización definido

#### Calidad

- [ ] Diagramas son útiles para el equipo
- [ ] Reflejan la realidad del sistema
- [ ] Sin contradicciones entre diagramas

**Score**: \_\_\_ / 20

**Acciones Requeridas**:
[Lista de acciones si hay gaps]

**Deadline**: [Fecha]
```

---

## Herramientas de Validación

### Validación Manual

1. **Abrir diagrama**
2. **Ir sección por sección** de este documento
3. **Marcar cada criterio** ✅ o ❌
4. **Documentar issues** encontrados
5. **Corregir y re-validar**

### Scripts de Validación (Futuros)

```bash
# Pre-commit hook (ejemplo)
./scripts/validate-diagram.sh path/to/diagram.drawio

Validando: identity-platform-containers.drawio
✅ Tiene título
✅ Tiene metadata
✅ Usa colores estándar
❌ Encontradas flechas sin etiquetar (3)
❌ Nombre no sigue convención: "svc-identity"

Resultado: FAILED (2 errors, 0 warnings)
```

---

## Scorecard de Calidad

### Sistema de Puntuación

| Aspecto           | Peso | Puntos Máximos |
| ----------------- | ---- | -------------- |
| **Existencia**    | 20%  | 20             |
| C1 existe         |      | 5              |
| C2 existe         |      | 10             |
| Deployment existe |      | 5              |
| **Cumplimiento**  | 30%  | 30             |
| Colores estándar  |      | 10             |
| Shapes correctos  |      | 10             |
| Nomenclatura      |      | 10             |
| **Actualidad**    | 20%  | 20             |
| <3 meses          |      | 20             |
| 3-6 meses         |      | 10             |
| >6 meses          |      | 0              |
| **Calidad**       | 30%  | 30             |
| Metadata completa |      | 10             |
| Layout lógico     |      | 10             |
| Útil y claro      |      | 10             |
| **TOTAL**         |      | **100**        |

### Interpretación

- **90-100**: Excelente
- **75-89**: Bueno
- **60-74**: Aceptable, requiere mejoras
- **<60**: Insuficiente, requiere acción inmediata

---

## FAQ

### ¿Qué hacer si encuentro un criterio que no puedo cumplir?

1. Documentar la razón
2. Proponer alternativa
3. Discutir con Architecture team
4. Obtener aprobación de excepción (si aplica)

### ¿Todos los criterios son obligatorios?

- **✅ Criterios Obligatorios**: Sí, deben cumplirse
- **✅ Criterios de Calidad**: Altamente recomendado, pero puede haber excepciones justificadas

### ¿Con qué frecuencia debo validar?

- **Creación**: Antes de commit inicial
- **Actualización**: Antes de cada commit
- **PR Review**: En cada PR que afecte arquitectura
- **Auditoría**: Quarterly

---

## Referencias

- [PLAYBOOK.md](./PLAYBOOK.md): Guía maestra
- [Visual Conventions](./conventions/visual-conventions.md): Estándares visuales
- [Naming Conventions](./conventions/naming-conventions.md): Estándares de nomenclatura
- [C4 Guidelines](./c4-guidelines/): Guías de modelo C4
- [Definition of Done](./DEFINITION-OF-DONE.md): DoD de arquitectura
