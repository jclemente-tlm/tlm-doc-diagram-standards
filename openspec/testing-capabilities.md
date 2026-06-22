# Testing Capabilities — tlm-doc-diagram-standards

**Project type**: documentation/standards repository
**Test runner**: none
**Strict TDD**: false

## Summary

This is a documentation-only project. There is no code to test. The repository contains:

- `STANDARDS.md` — main standards document (~645 lines)
- `README.md` — quick start guide (~230 lines)
- `drawio-library/TLM - Librería C4.xml` — corporate Draw.io component library
- `reference/` — detailed reference docs (validation criteria, best practices)

## Implications for SDD

Since there is no code:
- SDD `verify` phase cannot run automated tests
- Verification relies on manual review against checklist in STANDARDS.md
- Change proposals should include diagram validation against the 11-point checklist
- No build/CI pipeline for this repository

## Manual Verification Checklist (from STANDARDS.md §7)

1. [ ] Título claro — Nombre del sistema + tipo de diagrama
2. [ ] Metadata — Fecha, versión, owner
3. [ ] Colores correctos — Según tabla corporativa
4. [ ] Shapes correctos — Cilindro para DB, folder para storage
5. [ ] Estructura de componentes — `[Tipo: Tecnología]` en todos los elementos
6. [ ] TODAS las flechas etiquetadas — Protocolo + propósito
7. [ ] Nombres descriptivos — NO "Service 1", "API", "DB"
8. [ ] Nivel correcto — C1 sin internos, C2 sin componentes
9. [ ] Límite de elementos — Cumple los límites por nivel (C1≤10, C2≤20, C3≤12)
10. [ ] Layout limpio — Sin cruces innecesarios, flujo claro
11. [ ] Actualizado — Refleja el estado real del sistema
