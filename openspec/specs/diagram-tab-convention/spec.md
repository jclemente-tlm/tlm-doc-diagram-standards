# Delta Spec: diagram-tab-convention

## ADDED Requirements

### Requirement: Tab naming format

Draw.io tab names that target the export workflow MUST follow the pattern `[Nombre del Sistema] - [Tipo]`, where `Tipo` MUST be one of: `Context`, `Container`, `Component`, `Deployment`, `Sequence`, `Integration`, `Data Flow`, `Network`, `Infrastructure`. The system SHALL identify a tab as exportable when the name matches this pattern.

#### Scenario: Canonical tab name is exportable

- GIVEN a Draw.io file contains a tab named `Payment System - Container`
- WHEN the export workflow evaluates the tab name against the pattern
- THEN the tab is identified as exportable
- AND the workflow proceeds to render a PNG for that tab

#### Scenario: All allowed `Tipo` values are recognized

- GIVEN a Draw.io file contains tabs named `X - Context`, `X - Container`, `X - Component`, `X - Deployment`, `X - Sequence`, `X - Integration`, `X - Data Flow`, `X - Network`, `X - Infrastructure`
- WHEN the export workflow evaluates the tab names
- THEN every tab is identified as exportable

#### Scenario: Unknown `Tipo` value is not exportable

- GIVEN a Draw.io file contains a tab named `Payment - Wireframe`
- WHEN the export workflow evaluates the tab name
- THEN the tab is NOT identified as exportable
- AND the tab is silently skipped

#### Scenario: C4 level alias is not a valid Tipo

- GIVEN a Draw.io file contains a tab named `Payment System - C2`
- WHEN the export workflow evaluates the tab name
- THEN the tab is NOT identified as exportable
- AND the tab is silently skipped
- BECAUSE C4 level aliases (`C1`, `C2`, `C3`) are not in the allowed `Tipo` enum — only the full type names (`Context`, `Container`, `Component`, etc.) are valid

### Requirement: Non-matching tabs are ignored

Tabs whose names do not match the `[Nombre del Sistema] - [Tipo]` pattern SHALL be silently ignored by the export workflow. The workflow MUST NOT raise an error, MUST NOT emit a warning, and MUST NOT generate a PNG for a non-matching tab.

#### Scenario: Draft tab is silently ignored

- GIVEN a Draw.io file contains a tab named `borrador`
- WHEN the export workflow evaluates the tab name
- THEN the tab is silently ignored
- AND no PNG is generated for that tab
- AND no error is raised

#### Scenario: Mixed valid and invalid tabs

- GIVEN a Draw.io file contains a tab named `borrador` and another tab named `Identity - Container`
- WHEN the export workflow evaluates all tabs
- THEN exactly one PNG is generated for `Identity - Container`
- AND no output is produced for `borrador`
- AND the workflow exits successfully

### Requirement: Pattern allows flexible system names

The `[Nombre del Sistema]` portion of the pattern SHALL accept any non-empty string. The system SHALL NOT restrict the system name to a fixed vocabulary, predefined list, or character class beyond requiring at least one non-whitespace character.

#### Scenario: Diverse system names are accepted

- GIVEN a Draw.io file contains tabs named `Identity - Container`, `Payment System - Container`, `HR Worker - Component`
- WHEN the export workflow evaluates the tab names
- THEN all three tabs are identified as exportable

#### Scenario: System name with multiple words is accepted

- GIVEN a Draw.io file contains a tab named `Real-Time Order Processing - Context`
- WHEN the export workflow evaluates the tab name
- THEN the tab is identified as exportable

#### Scenario: Empty system name is rejected

- GIVEN a Draw.io file contains a tab named ` - Container`
- WHEN the export workflow evaluates the tab name
- THEN the tab is NOT identified as exportable
- AND the tab is silently ignored
