# Cheat Sheet — Diagramas de Arquitectura

Referencia rápida para crear y validar diagramas C4. Ver [STANDARDS.md](../STANDARDS.md) para el documento completo.

---

## Paleta de Colores

| Categoría | Color | Código Hex |
|---|---|---|
| **System** | Azul | `#B2CEFF` |
| **App** | Morado claro | `#E1D5E7` |
| **Store** | Coral claro | `#F8CECC` |
| **Component** (C3) | Amarillo claro | `#FFF2CC` |
| **Person** (interno) | Verde claro | `#D5E8D4` |
| **External** | Gris | `#DFDFDF` |
| **Leyenda** | Meta-elemento | n/a |

---

## Shapes (Formas)

| Shape | Uso |
|---|---|
| Rectángulo redondeado | APIs, servicios, workers, apps |
| Cilindro vertical | Bases de datos, cache |
| Cilindro horizontal | Message bus, colas, event bus |
| Folder | Object storage, file storage |
| Hexágono | Load Balancer en Deployment cuando corresponda; no forma básica C4 |
| Actor | Usuarios (Person, External Person) |
| Rect (dashed/solid border) | Boundary (System/Container/Generic) |

---

## Límites de Elementos

| Nivel | Límite recomendado | Tope absoluto |
|---|---|---|
| **C1** | ≤ 10 | 25 |
| **C2** | ≤ 20 | 25 |
| **C3** | ≤ 12 | 25 |

**Tope absoluto**: 25 elementos por diagrama. Exceder requiere justificación documentada en el README del diagrama y aprobación de un revisor de Architecture.

---

## Convención de Pestañas

**Patrón**: `^([^ ].*) - (Context|Container|Component|Deployment|Sequence|Integration|Data Flow|Network|Infrastructure)$`

### Correctos ✅

- `Payment System - Container`
- `Identity - Context`
- `Real-Time Order Processing - Context`

### Incorrectos ❌

- `borrador` — falta el separador ` - ` y el Tipo
- ` - Container` — nombre de sistema vacío
- `Payment_Container` — separador incorrecto (debe ser ` - ` no `_`)
- `Identity - C2` — `C2` no es un Tipo válido (usar `Container`)

---

## Checklist Ultra-Corto (5 items)

1. [ ] **Título + Metadata** — Nombre del sistema + tipo de diagrama + fecha + owner
2. [ ] **Colores** — Según paleta corporativa
3. [ ] **Shapes** — Cilindro vertical = base de datos, horizontal = cola/topic, folder = object storage
4. [ ] **Elementos** — Orden: nombre → categoría/tecnología → descripción; tecnología real para App y Store
5. [ ] **Flechas etiquetadas** — Protocolo + propósito en todas

---

## Links

- [STANDARDS.md](../STANDARDS.md) — Documento principal
- [Validation Criteria](./VALIDATION-CRITERIA.md) — Checklists detallados
