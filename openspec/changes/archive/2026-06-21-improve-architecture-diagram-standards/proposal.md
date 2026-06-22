# Proposal: Improve Architecture Diagram Standards

## Intent

Clarify ambiguities, fix inconsistencies, and add missing sections in STANDARDS.md to reduce incorrect diagram interpretations. The document has internal contradictions (element limits, AWS ALB color), undocumented conventions (tab naming), and gaps (CI/CD workflow, contribution process) that cause ongoing confusion during reviews.

## Scope

### In Scope
- Clarify element limits: C1≤10, C2≤20, C3≤12 (recommended); 25 is the absolute hard cap with documented exceptions
- Fix AWS ALB color: `#E1D5E7` (App/morado) not `#B2CEFF` (System/azul) — STANDARDS.md:589
- Unify Kafka event arrow format: enforce `[tipo].[dominio].[entidad].[acción]` prefix (evt./cmd./cdc./dlq.)
- Clarify SSR Web example: Marketing Web = `[App: Next.js 14]` — technology captures actual stack
- Complete Section 2 component list: document all 46 library elements (currently ~36)
- Add tab naming convention: `[Nombre del Sistema] - [Tipo]` for exportable tabs
- Add CI/CD workflow section (Section 11): shared reusable GitHub Actions workflow, PNG auto-export per PR
- Add `reference/cheat-sheet.md`: one-page quick reference
- Add `reference/contribution-guide.md`: ownership, proposal process, approval workflow
- Remove Structurizr DSL from authorized tools (team doesn't use it)
- Document deprecated state rule (optional, no new library element)

### Out of Scope
- Automated validation scripts
- New library elements for deprecated state
- Real PNG examples
- CHANGELOG.md / semver versioning

## Capabilities

> Contract with sdd-spec: this is a documentation-only project; no new capabilities at the code level.

### New Capabilities
- `diagram-tab-convention`: Standardized naming for exportable Draw.io tabs
- `ci-cd-export-workflow`: Shared GitHub Actions workflow for PNG export on PR
- `quick-reference`: One-page cheat sheet with colors, shapes, limits, and ultra-short checklist
- `contribution-process`: Guidelines for proposing changes to the standard

### Modified Capabilities
- None — all changes are documentation corrections and reorganizations within STANDARDS.md

## Approach

1. Edit STANDARDS.md in place: fix colors, clarify limits, unify nomenclature, expand component list
2. Add new sections to STANDARDS.md: tab convention (§2.5), CI/CD workflow (§11)
3. Create `reference/cheat-sheet.md` with compact visual reference
4. Create `reference/contribution-guide.md` with ownership and proposal process
5. No code, no tests — verification via manual 11-point checklist (§7)

## Affected Areas

| Area | Impact | Description |
|------|--------|-------------|
| `STANDARDS.md` | Modified | Clarify limits, fix AWS ALB color, unify Kafka arrows, expand components, add tabs + CI/CD sections |
| `reference/cheat-sheet.md` | New | One-page quick reference (colors, shapes, limits, checklist) |
| `reference/contribution-guide.md` | New | How to propose changes, ownership, approval workflow |
| `drawio-library/TLM - Librería C4.xml` | None | No changes |

## Risks

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Team adopts new tab convention inconsistently | Medium | Add to checklist §7 item 12; show examples in contribution-guide |
| CI/CD workflow conflicts with existing team YAML | Low | Workflow is shared/reusable but teams opt-in; PR checklist item reminds users |
| Breaking existing diagrams that exceed 25-element cap | Low | Hard cap has documented exception process; no retroactive fixes required |

## Rollback Plan

Revert to previous STANDARDS.md version via Git. New reference files (cheat-sheet.md, contribution-guide.md) can be deleted if not needed. No code or library changes to roll back.

## Dependencies

- GitHub Actions runner access for CI/CD section
- Architecture team approval for shared workflow ownership

## Success Criteria

- [ ] STANDARDS.md: AWS ALB color corrected to `#E1D5E7`
- [ ] STANDARDS.md: Element limits clarified with recommended vs. hard-cap distinction
- [ ] STANDARDS.md: Kafka arrow format enforces `[tipo].[dominio].[entidad].[acción]` prefix
- [ ] STANDARDS.md: Section 2 documents all 46 library elements
- [ ] STANDARDS.md: Tab naming convention added with export/ingnore rules
- [ ] STANDARDS.md: CI/CD workflow section documents shared GitHub Actions approach
- [ ] `reference/cheat-sheet.md` created and accessible
- [ ] `reference/contribution-guide.md` created with ownership and proposal process
- [ ] Structurizr DSL removed from authorized tools
- [ ] Deprecated state rule documented without new library element
- [ ] Manual 11-point checklist passes on updated STANDARDS.md
