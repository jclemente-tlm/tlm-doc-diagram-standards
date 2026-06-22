# Tasks: Improve Architecture Diagram Standards

## Review Workload Forecast

| Field | Value |
|-------|-------|
| Estimated changed lines | ~410 (design estimated ~465) |
| 400-line budget risk | Low |
| Chained PRs recommended | No |
| Suggested split | Single PR |
| Delivery strategy | ask-always |
| Chain strategy | pending |

Decision needed before apply: No
Chained PRs recommended: No
Chain strategy: pending
400-line budget risk: Low

### Suggested Work Units

| Unit | Goal | Likely PR | Notes |
|------|------|-----------|-------|
| 1 | All changes | PR 1 | Single PR; all groups combined fit under 800-line review budget |

---

## Phase 1: Core STANDARDS.md Clarifications (no behavior change)

- [x] 1.1 **§4 + §8 element limits** — In STANDARDS.md C1/C2/C3 "Excluir" bullets, replace "Preferiblemente no más de N" wording with two-tier: "**Límite recomendado**: N elementos" + "**Tope absoluto**: 25 elementos (ver §8)". In §8 item 1, replace with full two-tier wording including C1≤10, C2≤20, C3≤12.
  - Files: `STANDARDS.md`
  - Spec coverage: None (clarification only)
  - Est. lines: ~10

- [x] 1.2 **§3 Kafka arrow format** — Line 107: change `<Nombre evento>` → `<Nombre evento (formato §9)>`; add example `<evt.payment.invoice.created>`. Add note after table: "Los nombres de eventos siguen la nomenclatura de §9 — prefijo `tipo.dominio.entidad.acción`." Rename "REST/HTTP" row to "HTTPS".
  - Files: `STANDARDS.md`
  - Spec coverage: None (existing convention clarification)
  - Est. lines: ~5

- [x] 1.3 **§9 SSR Web example** — Line 436 SSR Web row: keep `[App: Next.js 14]`; add note below table: "El campo tecnología captura el stack real del equipo (Next.js, Angular SSR, Nuxt, Remix). El framework SSR ya implica React/Vue subyacente; no duplicar."
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: ~3

- [x] 1.4 **§5 Deprecated state rule** — After line 310 "Deprecado" table row, add paragraph: "**Deprecado (opcional)**: No existe elemento en la librería para este estado. Se representa con un componente estándar con borde discontinuo + estereotipo `<<Deprecated>>` en la línea 2 entre corchetes, p. ej. `[App: .NET 8] <<Deprecated>>`. Es optativo."
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: ~4

- [x] 1.5 **§13 ALB color fix** — Line 580: change Load Balancer color `#B2CEFF` → `#E1D5E7` and "Uso" to "Balanceador de carga (ALB, NLB) — **categoría App** porque es un API Gateway managed". Line 589: change `AWS ALB / NLB → **azul**` → `AWS ALB / NLB → **morado** (es API Gateway managed)`.
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: ~4

- [x] 1.6 **§11 Structurizr removal** — Remove "Diagrams-as-Code: Structurizr DSL" from §11 authorized tools (full rewrite in Group 4, but removal is part of same task).
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: ~1

- [x] 1.7 **Verification: grep checks** — Run grep checks on STANDARDS.md: verify `#E1D5E7` at LoadBalancer, `evt.payment.invoice.created` in Evento row, two-tier wording in §4 and §8.
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: 0

---

## Phase 2: Component Catalog Expansion (§2)

- [x] 2.1 **Split "Boundary" into 3 rows** — In §2 Sistemas table, replace single "Boundary" row with three rows: **Agrupador de Sistema** (dashed border, `c4Type=SystemScopeBoundary`), **Agrupador de Aplicación** (dashed border, `c4Type=ContainerScopeBoundary`), **Agrupador** (solid border, `c4Type=Boundary`). All use strokeColor `#666666`, fillColor none.
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: ~12

- [x] 2.2 **Add "Aplicación" App row** — In §2 App table, add row: **Aplicación** | Rectángulo redondeado | monitor | `[App]` | c4Technology=`ej. Tecnología` (generic, no specific icon). Distinct from "Web Application".
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: ~5

- [x] 2.3 **Add "Almacenamiento" Store row** — In §2 Store table, add row: **Almacenamiento** | Cilindro vertical | database | `[Store]` | c4Technology=`ej. Tecnología` (generic).
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: ~5

- [x] 2.4 **Add "Leyenda" row** — In §2 (or as separate subsection), add row: **Leyenda** | Meta-element | n/a | n/a | Title="Leyenda"; explains the diagram's color/shape key.
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: ~4

- [x] 2.5 **Update §2 intro line** — Change "30 componentes + 12 flechas + 3 boundaries + 1 leyenda" → confirm totals = 34 rows (2 Actores + 2 Sistemas + 9 Apps + 7 Stores + 10 C3 + 4 Boundaries+Leyenda). Update intro text to match.
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: ~2

- [x] 2.6 **Verification** — `grep -c '"title":' drawio-library/TLM\ -\ Librería\ C4.xml` → 46. Section 2 row count = 34. Header intro matches 46 total.
  - Files: `STANDARDS.md`
  - Spec coverage: None
  - Est. lines: 0

---

## Phase 3: New §15 Tab Convention

- [x] 3.1 **Add new §15 after §14** — Insert new section "§15 Convención de Pestañas" with: (1) Propósito, (2) regex pattern `^([^ ].*) - (Context|Container|Component|Deployment|Sequence|Integration|Data Flow|Network|Infrastructure)$`, (3) 3 correct examples, (4) 3 incorrect examples (borrador, ` - Container`, `Payment_Container`), (5) workflow behavior — non-matching tabs silently ignored.
  - Files: `STANDARDS.md`
  - Spec coverage: `diagram-tab-convention` spec scenarios 1–10
  - Est. lines: ~25

- [x] 3.2 **Add §7 checklist item 12** — Add after item 11: "12. [ ] **Convención de pestañas** — Si el archivo `.drawio` tiene pestañas exportables, sus nombres siguen el patrón `[Nombre del Sistema] - [Tipo]` (ver §15). Tabs `borrador`, `WIP`, etc. deben existir solo como pestañas no exportables."
  - Files: `STANDARDS.md`
  - Spec coverage: `diagram-tab-convention` spec (checklist enforcement)
  - Est. lines: ~3

- [x] 3.3 **Verification** — Manually test regex against canonical examples (all 9 `Tipo` values) and against `borrador`, ` - Container`, `Payment_Container`. §7 item 12 present.
  - Files: `STANDARDS.md`
  - Spec coverage: `diagram-tab-convention` scenarios 3, 5, 9
  - Est. lines: 0

---

## Phase 4: Rewrite §11 CI/CD Workflow

- [x] 4.1 **Full §11 rewrite** — Replace entire §11 with: (1) Draw.io as official tool (unchanged), (2) authorized tools — Sequence (PlantUML, Mermaid), Cloud (AWS/Azure Icons), no Structurizr, (3) new CI/CD workflow section: shared repo placeholder `tlm-org/diagram-export-workflow`, trigger `pull_request` with `paths: ['**.drawio']`, permissions `contents: write`, behavior per spec, (4) PR checklist item.
  - Files: `STANDARDS.md`
  - Spec coverage: `ci-cd-export-workflow` spec all scenarios
  - Est. lines: ~45 (replaces ~45)

- [x] 4.2 **Verification** — §11 contains "pull_request", "paths:", and "tlm-org/diagram-export-workflow" (or confirmed actual repo path). Checklist item present.
  - Files: `STANDARDS.md`
  - Spec coverage: `ci-cd-export-workflow` scenarios 1, 6, 8
  - Est. lines: 0

---

## Phase 5: New Reference Files

- [x] 5.1 **Create `reference/cheat-sheet.md`** — One-page quick reference (~100 lines): Paleta (7-row table), Shapes (6-row table), Límites (C1≤10/C2≤20/C3≤12/absolute 25), Convención pestañas (regex + 3✅/3❌), Ultra-short checklist (5 items). Footer links to STANDARDS.md.
  - Files: `reference/cheat-sheet.md` (new)
  - Spec coverage: None
  - Est. lines: ~100

- [x] 5.2 **Create `reference/contribution-guide.md`** — ~150 lines: Ownership (Architecture Team, `tlm-doc-diagram-standards` repo), Cómo proponer (PR against main, description template), Approval (1 reviewer from Architecture Team), Versioning (no semver, date-based), Cómo agregar componente (XML steps), Cómo cambiar color/shape (requires `#architecture` discussion first), PR template snippet.
  - Files: `reference/contribution-guide.md` (new)
  - Spec coverage: None
  - Est. lines: ~150

- [x] 5.3 **Verification** — Both files exist, contain expected sections, link to STANDARDS.md.
  - Files: `reference/cheat-sheet.md`, `reference/contribution-guide.md`
  - Spec coverage: None
  - Est. lines: 0

---

## Phase 6: Cross-link Updates

- [x] 6.1 **Update `reference/validation-criteria.md`** — Add two new checklist rows: (1) tab convention row (from §15), (2) CI/CD PNG update row (from §11).
  - Files: `reference/validation-criteria.md`
  - Spec coverage: Both specs (checklist integration)
  - Est. lines: ~8

- [x] 6.2 **Update `README.md`** — In "Mejoras y Contribuciones" section, add link to `reference/contribution-guide.md`.
  - Files: `README.md`
  - Spec coverage: None
  - Est. lines: ~3

- [x] 6.3 **Verification** — Links in both files resolve to existing targets.
  - Files: `reference/validation-criteria.md`, `README.md`
  - Est. lines: 0

---

## Phase 7: Header Bump + Final Verification

- [x] 7.1 **Bump `Última actualización` date** — Update line 3 of STANDARDS.md to today's date (2026-06-21 or current).
  - Files: `STANDARDS.md`
  - Est. lines: 1

- [x] 7.2 **Run full 12-point checklist** — Execute all 12 checklist items (§7) against updated STANDARDS.md to confirm self-consistency.
  - Files: `STANDARDS.md`
  - Spec coverage: All modified sections
  - Est. lines: 0

- [x] 7.3 **Run all design verification checks** — Execute grep checks per design.md §Verification: library title count=46, §2 row count=34, §11 contains required strings, §13 ALB color correct, Evento row has `evt.payment.invoice.created`.
  - Files: `STANDARDS.md`, `drawio-library/TLM - Librería C4.xml`
  - Spec coverage: Both specs
  - Est. lines: 0

---

## Summary

| Phase | Tasks | Focus |
|-------|-------|-------|
| Phase 1 | 7 | Core STANDARDS.md clarifications |
| Phase 2 | 6 | Component catalog expansion |
| Phase 3 | 3 | Tab convention |
| Phase 4 | 2 | CI/CD workflow rewrite |
| Phase 5 | 3 | New reference files |
| Phase 6 | 3 | Cross-link updates |
| Phase 7 | 3 | Header bump + final verification |
| **Total** | **27** | |

### Implementation Order
Phase 1 (clarifications) → Phase 2 (§2 catalog) → Phase 3 (§15 tab) → Phase 4 (§11 rewrite) → Phase 5 (new files) → Phase 6 (cross-links) → Phase 7 (header + final checks). Groups 1 and 2 are independent; §11 rewrite (Group 4) should follow Groups 1+2 since it references §15 which follows Group 3.

### Review Workload Forecast
```yaml
review_workload_forecast:
  total_estimated_lines: 410
  per_group:
    - group: "Group 1 — Core clarifications (§4, §3, §9, §5, §13, §11 removal)"
      lines: 35
    - group: "Group 2 — §2 component catalog expansion"
      lines: 30
    - group: "Group 3 — §15 Tab Convention + §7 item 12"
      lines: 28
    - group: "Group 4 — §11 CI/CD rewrite"
      lines: 45
    - group: "Group 5 — cheat-sheet.md"
      lines: 100
    - group: "Group 6 — contribution-guide.md"
      lines: 150
    - group: "Group 7 — Cross-links (validation-criteria, README)"
      lines: 11
    - group: "Group 8 — Header bump"
      lines: 1
  chained_prs_recommended: false
  four_hundred_line_budget_risk: low
  decision_needed_before_apply: false
  notes: "Single PR feasible. All groups combined (~410 lines) fit well under the 800-line review budget. The delivery_strategy is ask-always but chained PRs are not warranted — no decision needed."
```

---

## Archived

**Date**: 2026-06-21
**Change**: improve-architecture-diagram-standards
**Phases completed**: 7 (all)
**Tasks completed**: 27/27
**Verification**: 11/11 proposal criteria PASS, 19/19 spec scenarios PASS, 12/12 checklist PASS, 9/9 grep checks PASS
**Critical issues**: 0
**Archive status**: complete
