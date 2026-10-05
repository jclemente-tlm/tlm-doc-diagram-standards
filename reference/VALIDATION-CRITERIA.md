# Criterios de Validación de Diagramas

Checklists esenciales para validación rápida durante creación, PR review y auditorías.

---

## Validación por Nivel C4

### C1 - Context Diagram

- [ ] Título claro: `[Nombre del Sistema] - Context Diagram`
- [ ] Sistema principal como una sola caja
- [ ] Actores internos (verde) y externos (gris) identificados
- [ ] Sistemas externos representados
- [ ] Todas las flechas etiquetadas
- [ ] Sin detalles internos (no DBs, microservices, componentes)
- [ ] Máximo 10 elementos

### C2 - Container Diagram

- [ ] Título claro: `[Nombre del Sistema] - Container Diagram`
- [ ] Todos los containers: APIs, servicios, workers, frontends
- [ ] Todos los stores: DBs, message buses, queues, storage
- [ ] Separación visual: Apps (morado), Stores (coral), Externos (gris)
- [ ] Shapes correctos: Cilindro=DB, Folder=storage, gateway identificado por nombre
- [ ] Protocolos en todas las conexiones (HTTPS, Kafka, SQL)
- [ ] Líneas: Sólida (sync), Dashed (async), Dotted (batch)
- [ ] Máximo 20 elementos

### C3 - Component Diagram

- [ ] Título claro: `[Nombre del Container] - Component Diagram`
- [ ] Scope: UN solo container
- [ ] Componentes con responsabilidades claras
- [ ] Todos los componentes en amarillo (`#FFF2CC`)
- [ ] Formato: `[Función] [Tipo]` (ej: "User Controller")
- [ ] Flujo claro: arriba→abajo (request→response)
- [ ] Máximo 12 componentes

### Deployment Diagram

- [ ] Infraestructura: VPCs, subnets, clusters
- [ ] Containers mapeados desde C2
- [ ] Networking: Load balancers, DNS
- [ ] Security: Security groups, IAM
- [ ] Cloud services con íconos oficiales
- [ ] Ambientes separados (Prod, Staging, Dev)

---

## Validación Transversal

### Metadata

- [ ] Título: `[Sistema] - [Tipo de Diagrama]`
- [ ] Versión: vX.Y
- [ ] Fecha: YYYY-MM-DD
- [ ] Owner/Autor

### Colores

| Elemento | Color |
|----------|-------|
| System (interno) | `#B2CEFF` |
| App | `#E1D5E7` |
| Store | `#F8CECC` |
| Component (C3) | `#FFF2CC` |
| Person (interno) | `#D5E8D4` |
| External | `#DFDFDF` |

### Shapes

- [ ] Rectángulo redondeado: Apps, APIs, servicios
- [ ] Cilindro vertical: DBs, cache
- [ ] Cilindro horizontal: Message buses, colas
- [ ] Folder: Object storage
- [ ] API Gateway: usar `Application` y nombrarlo como gateway cuando deba verse en C2
- [ ] Actor shape: Usuarios

### Flechas

- [ ] Sólida: Sync (HTTP, gRPC)
- [ ] Dashed: Async (Events, messages)
- [ ] Dotted: Batch
- [ ] Todas etiquetadas: Protocolo + propósito
- [ ] Dirección clara

### Nomenclatura

- [ ] Descriptivos: No "Sistema A", "DB 1", "API"
- [ ] Inglés para elementos técnicos
- [ ] Singular: "User Service" no "Users Service"
- [ ] Sin abreviaciones: No "svc", "proc", "mgr"
- [ ] Formato: `dominio-db`, `tipo.dominio.entidad.accion` (Kafka)
- [ ] **Pestañas exportables**: nombres siguen `[Nombre] - [Tipo]` (ver [§16](../STANDARDS.md#-16-convención-de-pestañas))

---

## Checklist PR Review

Copiar en el PR:

```markdown
## Architecture Diagram Checklist

- [ ] C1 / C2/ C3/ Deployment actualizado(s) según cambio
- [ ] Colores según paleta corporativa
- [ ] Shapes correctos por tipo
- [ ] Nomenclatura consistente
- [ ] Elementos en orden nombre → categoría/tecnología → descripción, con forma y color según la librería
- [ ] Metadata actualizada (versión, fecha, owner)
- [ ] Todas las flechas etiquetadas
- [ ] Límites de elementos respetados (C1≤10, C2≤20, C3≤12)
- [ ] Si se modificó un `.drawio`, ¿se generó/actualizó el `.png` en este PR (manualmente hasta disponer del workflow)?
- [ ] Diagramas en Git (`.drawio`)
```

---

## Referencias

- [STANDARDS.md](../STANDARDS.md) - Documento principal de estándares
- [Best Practices](./c4-best-practices.md) - Principios y anti-patrones