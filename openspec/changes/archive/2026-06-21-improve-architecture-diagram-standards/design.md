# Design: Improve Architecture Diagram Standards

## Overview

Documentation-only change to `tlm-doc-diagram-standards`. Edits `STANDARDS.md` in place to fix inconsistencies, expand the component catalog, document two previously-unwritten conventions (tab naming, CI/CD workflow), and adds two `reference/` docs (cheat-sheet, contribution-guide). No code, no tests; verification via the 11-point manual checklist (STANDARDS.md §7).

Scope boundary: out of scope are new library elements, automated validators, real PNG examples, and semver/CHANGELOG (per proposal). The `drawio-library/TLM - Librería C4.xml` is **not** modified.

---

## Architecture Decisions

| # | Decision | Choice | Alternatives | Rationale |
|---|----------|--------|--------------|-----------|
| 1 | Where to put tab convention | New `§15 Convención de Pestañas` at end of STANDARDS.md (after Deployment) | Inline in §2; new `§2.5` | Tab convention is cross-cutting (applies to C1/C2/C3/Deployment) and references §11 (CI/CD); putting it last keeps topical sections together. Numbering §2.5 breaks existing TOC anchors. |
| 2 | Section 11 rewrite scope | Full rewrite (replace §11 entirely) | Append-only | The current §11 mixes tool setup with ad-hoc automation tips; spec requires a precise contract (trigger, permissions, behavior) that reads better as one cohesive section. |
| 3 | Component list granularity | One row per library title (34 entries) | Group "generic + specific" rows into one | README claims "30 componentes + 3 boundaries + 1 leyenda"; aligning Section 2 to that count eliminates the existing 36-vs-46 ambiguity. |
| 4 | ALB color resolution | ALB/NLB → `#E1D5E7` (App, not System) | Keep blue `#B2CEFF`; add "managed" exception | ALB is the API Gateway shape (hexagon, App category). Per §1 "External" rule, color overrides only when **third-party**. ALB is conceptually a managed gateway, semantically App. Treats ALB consistently with Kong (also an API Gateway). |
| 5 | Deprecated state rule | Document as opt-in visual convention, no library element | Add new library element | Proposal explicit on this; avoids XML edit + library version churn. Marked optional so teams are not forced to use it. |
| 6 | Cheat-sheet vs reference split | Cheat-sheet is **operational** (what you do), contribution-guide is **governance** (how you change) | Single mega-doc | Single-file search beats multi-file lookup for daily work; governance needs its own artifact because cadence differs (cheat-sheet changes per release, contribution-guide changes per process change). |

---

## Data Flow

The change does **not** alter runtime data flow. The only "flow" introduced is the documentation flow for the CI/CD workflow documented in §11:

```
Developer edits .drawio on feature branch
        │
        ▼
Opens PR ──► pull_request event with paths: ['**.drawio']
        │           │
        │           └─► shared workflow `tlm-org/diagram-export-workflow` runs
        │                       │
        │                       ├─► For each tab matching ^[Nombre] - (Context|Container|Component|Deployment|Sequence|Integration|Data Flow|Network|Infrastructure)$
        │                       │       └─► export PNG → commit to PR branch
        │                       └─► Tabs not matching pattern → silently skipped
        ▼
Reviewer opens PR → sees .drawio + .png diff → manual checklist item unchecked
```

---

## File Changes

| File | Action | Description |
|------|--------|-------------|
| `STANDARDS.md` | Modify | 11 distinct edits across §2, §3, §4, §5, §7, §8, §9, §11, §13; new §15 |
| `reference/cheat-sheet.md` | Create | One-page quick reference (~100 lines) |
| `reference/contribution-guide.md` | Create | Ownership + proposal process (~150 lines) |
| `reference/validation-criteria.md` | Modify | Add tab-convention row and CI/CD PNG-update row |
| `README.md` | Modify | Update "Mejoras y Contribuciones" section to link contribution-guide |
| `drawio-library/TLM - Librería C4.xml` | **No change** | Out of scope per proposal |

---

## STANDARDS.md Edits — Section-by-Section

### §2 Componentes Estándar (lines 28–81)

**Audit result**: library has 30 components; current section lists 28 + 1 generic "Boundary". Add these rows:

| Section | Add | Shape | Color | Notes |
|---------|-----|-------|-------|-------|
| App (morado) | **Aplicación** | Rectángulo redondeado | `#E1D5E7` | c4Type=`App`, c4Technology=`ej. Tecnología` (generic, no specific icon). Distinct from "Aplicación Web" (browser shape) |
| Store (coral) | **Almacenamiento** | Cilindro vertical | `#F8CECC` | c4Type=`Store`, c4Technology=`ej. Tecnología` (generic) |
| Boundaries (new subsection) | **Agrupador de Sistema** | Rect, dashed border, fillColor none | strokeColor `#666666` | c4Type=`SystemScopeBoundary` |
| Boundaries | **Agrupador de Aplicación** | Rect, dashed border, fillColor none | strokeColor `#666666` | c4Type=`ContainerScopeBoundary` |
| Boundaries | **Agrupador** | Rect, solid border, fillColor none | strokeColor `#666666` | c4Type=`Boundary` (generic grouping) |
| Boundaries | **Leyenda** | Meta-element | n/a | Title="Leyenda"; explains the diagram's color/shape key |

Replace the current single "Boundary" row with the 4 boundary rows above.

Update intro line: "46 elementos totales — 30 componentes + 12 flechas + 3 boundaries + 1 leyenda" (matches README).

### §3 Flechas y Conectores (lines 84–124)

- Line 107 (Evento row): change `<Nombre evento>` → `<Nombre evento (formato §9)>` and add example `<evt.payment.invoice.created>`.
- Add note after the table: "Los nombres de eventos siguen la nomenclatura de §9 — prefijo `tipo.dominio.entidad.acción`."
- Rename "REST/HTTP" row to "HTTPS" (matches library title) — purely nomenclature fix.

### §4 Modelo C4 (lines 127–215)

In each of §4 C1 / C2 / C3 "Excluir" bullets, replace the current "Preferiblemente no más de N" wording with:

> **Límite recomendado**: N elementos. Exceder requiere división en múltiples diagramas.
> **Tope absoluto**: 25 elementos por diagrama (ver §8). Excepciones documentadas en el README del diagrama.

### §5 Reglas Visuales — Estados Visuales (line 310)

Augment the existing `| **Deprecado** | ...` row with a paragraph after the table:

> **Deprecado (opcional)**: No existe elemento en la librería para este estado. Se representa con un componente estándar (cualquier categoría) con borde discontinuo + estereotipo `<<Deprecated>>` en la línea 2 entre corchetes, p. ej. `[App: .NET 8] <<Deprecated>>`. Es optativo — los equipos que no lo necesiten pueden omitirlo.

### §7 Checklist de Validación (lines 390–402)

Add item 12 after item 11:

> 12. [ ] **Convención de pestañas** — Si el archivo `.drawio` tiene pestañas exportables, sus nombres siguen el patrón `[Nombre del Sistema] - [Tipo]` (ver §15). Tabs `borrador`, `WIP`, etc. deben existir solo como pestañas no exportables.

### §8 Prohibiciones (lines 406–414)

Replace item 1 with the two-tier wording:

> 1. **Diagramas con más de 25 elementos** — Tope absoluto. Exceder requiere justificación documentada en el README del diagrama y aprobación de un revisor de Architecture. Límites recomendados por nivel: C1 ≤ 10, C2 ≤ 20, C3 ≤ 12. Deployment y diagramas de integración pueden acercarse al tope si documentan infraestructura real.

### §9 Nomenclatura (lines 418–475)

- **SSR Web row (line 436)**: keep `[App: Next.js 14]` but add a note below the table: "El campo tecnología captura el stack real del equipo (Next.js, Angular SSR, Nuxt, Remix). El framework SSR ya implica React/Vue subyacente; no duplicar."
- **Kafka eventos (line 456)**: add cross-reference: "Las flechas de evento deben usar el nombre completo del topic (ver §3 ejemplo `evt.payment.invoice.created`)."

### §11 Herramientas (lines 510–555) — full rewrite

New §11 structure (see "New Sections" below).

### §13 Deployment (lines 570–626)

- Line 580 (Load Balancer row): change color `#B2CEFF` → `#E1D5E7` and update "Uso" to: "Balanceador de carga (ALB, NLB) — **categoría App** porque es un API Gateway managed".
- Line 589 (Aplicación list): change `AWS ALB / NLB (gestionado) → **azul**` → `AWS ALB / NLB (gestionado) → **morado** (es API Gateway managed, no infraestructura de red)`.

---

## New Sections

### New §15 Convención de Pestañas (insert after §14)

Content outline:

1. **Propósito**: que el workflow de CI/CD (ver §11) pueda exportar automáticamente solo las pestañas que representan diagramas reales, ignorando borradores y notas.
2. **Patrón**: `^([^ ].*) - (Context|Container|Component|Deployment|Sequence|Integration|Data Flow|Network|Infrastructure)$`
   - `[Nombre del Sistema]`: cualquier string no vacío que no empiece con espacio
   - ` - `: separador literal (espacio-guión-espacio)
   - `[Tipo]`: enum exacto (9 valores)
3. **Ejemplos correctos**: ✅ `Payment System - Container`, `Identity - C2`, `Real-Time Order Processing - Context`
4. **Ejemplos incorrectos**: ❌ `borrador`, `WIP`, `Payment - Wireframe` (tipo no permitido), ` - Container` (nombre vacío), `Payment_Container` (separador incorrecto)
5. **Comportamiento del workflow**: pestañas que no matchean → ignoradas silenciosamente, sin warning ni error.

### Rewrite §11 Herramientas (full replace)

New structure:

1. **Herramienta oficial**: Draw.io (unchanged)
2. **Herramientas autorizadas** (3 bullets, drop Structurizr DSL):
   - Sequence: PlantUML, Mermaid
   - Cloud diagrams: AWS Architecture Icons, Azure Icons
3. **Workflow de exportación automática** (new — see ci-cd-export-workflow spec):
   - Repositorio compartido: `tlm-org/diagram-export-workflow` (path placeholder; replace with actual org/repo at implementation time)
   - Trigger: `on: pull_request` con `paths: ['**.drawio']`
   - Permissions: `contents: write`
   - Behavior: por cada tab que matchea §15, exporta PNG al mismo branch del PR; tabs no-matching se ignoran silenciosamente; no commits si el PNG es byte-identical al actual; nunca pushea a `main`.
   - Adoption: opt-in por repo (cada equipo referencia el workflow en su `.github/workflows/`).
4. **Checklist PR actualizado**: nueva línea — "Si se modificó un `.drawio`, ¿el workflow generó/actualizó el `.png` en este PR?"

---

## New Reference Files

### `reference/cheat-sheet.md` (~100 lines)

Sections:
- **Paleta**: 7-row table (System/App/Store/Component/Person/External + hex)
- **Shapes**: 6-row icon → meaning table
- **Límites**: C1 ≤ 10 / C2 ≤ 20 / C3 ≤ 12 / Tope absoluto 25
- **Convención de pestañas**: regex visual `Nombre - Tipo` con 3 ✅ / 3 ❌
- **Checklist ultra-corto** (5 items): título+metadata, colores, shapes, `[Tipo:Tecnología]`, flechas etiquetadas
- Footer: "Ver STANDARDS.md para detalle"

### `reference/contribution-guide.md` (~150 lines)

Sections:
- **Ownership**: Architecture Team es source-of-truth. Repo principal: `tlm-doc-diagram-standards`.
- **Cómo proponer un cambio**:
  1. Abrir PR contra `main` con la modificación a STANDARDS.md / cheat-sheet / library
  2. Incluir en la descripción del PR: problema observado, cambio propuesto, impacto en diagramas existentes
  3. Para cambios en la librería (XML): describir el componente nuevo (tipo, shape, color, uso) y adjuntar screenshot
  4. Para cambios de color/shape: discussion thread en `#architecture` antes del PR
- **Approval**: 1 reviewer del Architecture Team.
- **Versioning policy**: **sin semver en este repo**. Cambios se trackean por fecha en el header de STANDARDS.md (`Última actualización`). PRs significativos se mencionan en el PR description. (Decisión explícita del usuario — propuesta lo confirma.)
- **Cómo agregar un componente a la librería**: pasos XML (abrir `TLM - Librería C4.xml` en Draw.io → File → Open Library → Edit → guardar; o editar el JSON directamente siguiendo el formato existente).
- **Cómo actualizar colores/shapes**: requires prior `#architecture` discussion; nunca unilateral.
- **PR template snippet**: bloque copy-paste con los bullets del proceso.

---

## Component List — Confirmed Library Audit

Library has exactly 46 titles (verified via grep on `"title":` lines). Breakdown:

| Group | Count | Documented in §2 | Missing in §2 |
|-------|-------|------------------|---------------|
| Actores (Person, External Person) | 2 | 2 | 0 |
| Sistemas (System, External System) | 2 | 2 | 0 |
| Apps (Web, App genérica, Mobile, API, Microservice, Worker, Batch, CDC, Gateway) | 9 | 8 | **1: Aplicación** |
| Stores (Storage genérico, RDB, NoSQL, Cache, Event Bus, Queue, Object Storage) | 7 | 6 | **1: Almacenamiento** |
| C3 Components | 10 | 10 | 0 |
| Boundaries (Sistema, Aplicación, Agrupador) | 3 | 1 (generic "Boundary") | **2: split boundary into 3 rows** |
| Leyenda | 1 | 0 | **1: Leyenda** |
| Connectors (covered in §3) | 12 | 12 | 0 |
| **Total** | **46** | **41** | **5 (net new) + 2 (boundary split) = 6 row changes** |

Note: the proposal says "~10 missing" — actual gap is **5 new rows + 1 row split into 3**. The discrepancy is because connectors were double-counted in the proposal's "~36" baseline. The design adds what the library actually contains, not an invented 10th component.

---

## Migration Considerations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Existing diagrams reference old ALB color | Low | None — color was only in §13 example text, no diagram asset encodes the rule | No diagram-level fix needed; future diagrams use new color |
| Existing `.drawio` files with old tab names | Medium | Tabs won't export via the workflow | Tab convention is forward-looking; existing tabs are grandfathered. PR reviewer checks the new §7 item 12 for new/renamed tabs only |
| Tab "Identity - C2" (uses short alias) — does it match? | Edge case | Would fail regex | The spec explicitly enumerates the 9 allowed `Tipo` values; "C2" is not in the enum. The design flags this in the "Incorrectos" examples. Recommend a migration note in contribution-guide |
| Structurizr DSL users (if any) | Low | They keep using it; just no longer listed as "authorized" | Not a removal enforcement, only a documentation change |
| CI/CD workflow adoption | Medium | Workflow requires a new external repo `tlm-org/diagram-export-workflow` | Path is placeholder in design; final repo URL set at implementation time. Teams opt-in — no retroactive change required |

---

## Verification Approach

Run the **existing 11-point checklist** (STANDARDS.md §7) on the updated STANDARDS.md, plus the **new item 12** (tab convention). Additional manual checks:

1. `grep -c '"title":' drawio-library/TLM - Librería C4.xml` → should return 46
2. Section 2 row count: 2 (Actores) + 2 (Sistemas) + 9 (Apps) + 7 (Stores) + 10 (C3) + 4 (Boundaries+Leyenda) = **34 rows**
3. Section 11 contains the words "pull_request" and "paths:" and "tlm-org/diagram-export-workflow"
4. Section 13 line 580 shows `#E1D5E7` for Load Balancer
5. Section 3 Evento row example contains `evt.payment.invoice.created`
6. `reference/cheat-sheet.md` and `reference/contribution-guide.md` exist and link to STANDARDS.md
7. Tab convention §15 regex matches all 9 `Tipo` values; `borrador` and ` - Container` do NOT match

No automated tests; strict_tdd = false per project context.

---

## Open Questions

- [ ] Actual repo path for the shared workflow (`tlm-org/diagram-export-workflow` is a placeholder) — confirm with Architecture Team before implementation
- [ ] Whether the work item should bump `Última actualización` header on STANDARDS.md — proposed yes (this is a substantive edit)