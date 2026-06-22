# Archive Report: improve-architecture-diagram-standards

**Status**: archived
**Date**: 2026-06-21
**Archiver**: sdd-archive
**Project**: tlm-doc-diagram-standards
**Artifact store mode**: openspec

---

## Change Summary

The `improve-architecture-diagram-standards` change completed all 7 SDD phases successfully, delivering a comprehensive set of clarifications, additions, and corrections to the architecture diagram standards documentation. The change operated in **documentation-only mode** — no code, no library XML modifications, no tests — with verification conducted via a 12-point manual checklist embedded in STANDARDS.md §7.

**Phase delivery**: Phase 1 (7 tasks) addressed core STANDARDS.md clarifications including two-tier element limits, Kafka arrow format, SSR Web technology capture, deprecated state rule, AWS ALB color fix (`#E1D5E7` App not System), and Structurizr DSL removal. Phase 2 (6 tasks) expanded the §2 component catalog from ~28 to 34 rows, aligning with the actual 46-element library (30 components + 12 connectors + 3 boundaries + 1 leyenda). Phase 3 (3 tasks) added new §15 tab convention with regex pattern, correct/incorrect examples, and §7 checklist item 12. Phase 4 (2 tasks) rewrote §11 to include the CI/CD PNG export workflow using a shared GitHub Actions repo placeholder. Phase 5 (3 tasks) created `reference/cheat-sheet.md` and `reference/contribution-guide.md`. Phase 6 (3 tasks) updated cross-links in `validation-criteria.md` and `README.md`. Phase 7 (3 tasks) bumped the header date and ran final verification.

**Notable during apply**: A spec issue was discovered where the original `diagram-tab-convention` spec used `C2` as a valid `Tipo` value (scenario 4), conflicting with the regex `^([^ ].*) - (Context|Container|...)$` which would NOT match `Identity - C2`. The spec was corrected to explicitly document that C4 level aliases (`C1`, `C2`, `C3`) are NOT valid `Tipo` values — only the full type names are. A new scenario 4 was added and the incorrectos section in STANDARDS.md §15 was updated to reflect this. Also, the XML library (`drawio-library/TLM - Librería C4.xml`) was unintentionally modified during apply; it was reverted to comply with the scope agreement which explicitly excluded library modifications.

---

## Files Changed

| File | Action | Lines (approx) | Notes |
|------|--------|----------------|-------|
| `STANDARDS.md` | Modified | ~+150 / ~-45 | 11 distinct edits across §2, §3, §4, §5, §7, §8, §9, §11, §13; new §15 |
| `reference/cheat-sheet.md` | Created | 79 | One-page quick reference |
| `reference/contribution-guide.md` | Created | 129 | Ownership + proposal process |
| `reference/validation-criteria.md` | Modified | ~+8 | Added tab-convention row and CI/CD PNG-update row |
| `README.md` | Modified | ~+3 | Updated "Mejoras y Contribuciones" section |
| `openspec/specs/diagram-tab-convention/spec.md` | Created | 78 | Main spec for tab naming convention |
| `openspec/specs/ci-cd-export-workflow/spec.md` | Created | 88 | Main spec for CI/CD export workflow |
| `openspec/changes/archive/2026-06-21-improve-architecture-diagram-standards/` | Moved | — | Full change archived |

**Files explicitly NOT modified** (scope compliance): `drawio-library/TLM - Librería C4.xml`, `STANDARDS.md` (XML library references), any `.drawio` source files.

---

## Specs Promoted

| Capability | Delta Spec | Main Spec | Action |
|------------|-----------|-----------|--------|
| `diagram-tab-convention` | `openspec/changes/.../specs/diagram-tab-convention/spec.md` | `openspec/specs/diagram-tab-convention/spec.md` | Copied as new (new capability) |
| `ci-cd-export-workflow` | `openspec/changes/.../specs/ci-cd-export-workflow/spec.md` | `openspec/specs/ci-cd-export-workflow/spec.md` | Copied as new (new capability) |

Both capabilities are **new** — no existing main specs existed to merge into. The delta specs were copied verbatim as the main spec for each capability. Total requirements promoted: 5 (diagram-tab-convention: 3 requirements, 9 scenarios; ci-cd-export-workflow: 5 requirements, 10 scenarios).

---

## Verification Results

| Layer | Scope | Result |
|-------|-------|--------|
| Proposal success criteria | 11 criteria | **11/11 PASS** |
| Spec scenarios | 19 total (9 + 10) | **19/19 PASS** |
| 12-point manual checklist | STANDARDS.md §7 | **12/12 PASS** |
| design.md grep checks | 9 checks | **9/9 PASS** |

**Critical issues**: 0
**Warnings (non-blocking)**: 0
**Suggestions (non-blocking)**: 3
- §11 YAML indentation nit (cosmetic)
- Scenario count approximations in brief (not quality issues)
- Line number drift in design.md references (cosmetic)

---

## Lessons Learned

### Identity-C2 spec/regex conflict resolved during apply

During apply phase, the team discovered that the original `diagram-tab-convention` spec included a scenario (scenario 4) saying `Payment System - C2` should match as a valid tab name, while simultaneously specifying a regex `^([^ ].*) - (Context|Container|...)$` that explicitly excludes `C2` as a valid `Tipo`. The conflict was resolved by:
1. Updating the spec to state that C4 level aliases (`C1`, `C2`, `C3`) are NOT valid `Tipo` values — only the full type names are
2. Adding explicit incorrectos examples in §15 documenting that `Identity - C2` is rejected
3. Adding scenario 4 to the spec documenting the C4 alias rejection behavior
4. Updating the cheat-sheet to mirror the C4 alias rejection rule

**Takeaway**: Regex patterns and GIVEN/WHEN/THEN scenario text must be kept in sync during spec authoring. A regex-changing PR should always audit scenarios.

### XML library was unintentionally modified during apply and reverted

During Phase 2 edits to STANDARDS.md, the component catalog expansion referenced the XML library file (`drawio-library/TLM - Librería C4.xml`) for title counts. One of the edit operations inadvertently modified the XML file (adding boundary elements that were already present or duplicating entries). The modification was detected during Phase 7 verification and reverted to comply with the scope agreement, which explicitly excluded library modifications.

**Takeaway**: Documentation-only SDD cycles should treat library files as invariant. Any operation that touches library paths should be audited carefully even when the operation is "read only." Consider making library files read-only at the filesystem level for projects that never intend to modify them.

### Element limit ambiguity resolved: two-tier wording

The existing STANDARDS.md used "Preferiblemente no más de N" (preferably no more than N) which was ambiguous — it could be interpreted as a soft recommendation or a hard requirement. The two-tier wording ("Límite recomendado: N" + "Tope absoluto: 25") makes the distinction explicit. The hard cap of 25 was already documented in §8 but not reflected in §4's exclude bullets.

---

## Open Follow-Ups

| Item | Owner | Status |
|------|-------|--------|
| **External repo `tlm-org/diagram-export-workflow` is a placeholder** — the spec and §11 document a shared GitHub Actions workflow repo at `tlm-org/diagram-export-workflow` but this repo has not been created or confirmed. The Architecture Team needs to create or confirm the actual repo path before teams can opt-in to the CI/CD PNG export workflow. | Architecture Team | Pending confirmation |
| **Teams with existing `Identity - C2` style tabs** — the tab convention is forward-looking (grandfathered existing tabs), but a migration note in the contribution-guide would help teams that want to rename their tabs for consistency. Currently absent from contribution-guide (minor gap, not blocking). | Architecture Team | Future SDD cycle |
| **YAML indentation suggestion for §11** — the documentation uses `on: pull_request` + `paths:` on separate lines; a first-time reader might misread this as a nested structure. Consider adding a properly formatted YAML block or clarifying the indentation in a future SDD cycle. | Future SDD | Non-blocking |

---

## Archive Audit Trail

| Artifact | Location | Complete |
|----------|----------|----------|
| proposal.md | `openspec/changes/archive/2026-06-21-improve-architecture-diagram-standards/proposal.md` | ✅ |
| design.md | `openspec/changes/archive/2026-06-21-improve-architecture-diagram-standards/design.md` | ✅ |
| specs/diagram-tab-convention/spec.md | `openspec/specs/diagram-tab-convention/spec.md` | ✅ |
| specs/ci-cd-export-workflow/spec.md | `openspec/specs/ci-cd-export-workflow/spec.md` | ✅ |
| tasks.md | `openspec/changes/archive/2026-06-21-improve-architecture-diagram-standards/tasks.md` | ✅ (27/27 tasks, archived footer added) |
| verify-report.md | `openspec/changes/archive/2026-06-21-improve-architecture-diagram-standards/verify-report.md` | ✅ |
| archive-report.md | `openspec/changes/archive/2026-06-21-improve-architecture-diagram-standards/archive-report.md` | ✅ (this file) |

---

## SDD Cycle Complete

**Phases completed**: init → propose → spec → design → tasks → apply → verify → **archive**

All 27 tasks completed. All 19 spec scenarios verified. All 11 proposal criteria satisfied. Zero critical issues. The change is fully planned, implemented, verified, and archived.
