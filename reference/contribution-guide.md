# Guía de Contribución — Estándares de Diagramas

Guía para proponer cambios a los estándares de diagramas de arquitectura.

---

## Ownership

**Architecture Team** es el source-of-truth para los estándares de diagramas.

**Repositorio principal**: [`tlm-doc-diagram-standards`](https://github.com/tlm-org/tlm-doc-diagram-standards)

Los cambios son bienvenidos — este documento explica cómo proponer y aprobar cambios.

---

## Cómo Proponer un Cambio

### Paso 1: Abrir PR contra `main`

Crear un Pull Request con la modificación propuesta:

- Cambios a `STANDARDS.md` para reglas/nomenclatura
- Cambios a `reference/cheat-sheet.md` para la referencia rápida
- Cambios a `drawio-library/TLM - Librería C4.xml` para componentes nuevos o modificados

### Paso 2: Descripción del PR

Incluir en la descripción del PR:

1. **Problema observado** — ¿Qué inconsistencia, ambigüedad o gap existe en el estándar actual?
2. **Cambio propuesto** — ¿Qué se propone cambiar/addicionar/eliminar?
3. **Impacto** — ¿Cómo afectan los cambios a los diagramas existentes?

### Paso 3: Para cambios en la librería (XML)

Al proponer un componente nuevo o modificar color/shape de uno existente:

1. Describir el componente: tipo, shape, color, uso previsto
2. Adjuntar screenshot de ejemplo
3. Justificar por qué pertenece a la librería oficial vs. uso ad-hoc

### Paso 4: Para cambios de color/shape

Los cambios de color o shape **requieren discusión previa** en `#architecture` antes de abrir el PR. No abrir PRs unilateralmente para estos cambios.

---

## Proceso de Approval

1. **1 reviewer** del Architecture Team debe aprobar el PR
2. El reviewer verifica: consistencia con la paleta, no se introducen nuevos anti-patrones, impacto en diagramas existentes
3. Si el cambio es significativo (nueva sección, cambio de regla), el reviewer puede pedir que se mencione en el PR description del change log

---

## Versioning Policy

**No se usa semver en este repositorio.**

Los cambios se trackean por:

- **Fecha** en el header `Última actualización` de `STANDARDS.md` (bump manual en PRs significativos)
- **PR description** para cambios grandes
- **Issue tracker** para discusión previa

No hay release tags ni changelog formal. La fecha de `STANDARDS.md` es la versión implícita.

---

## Cómo Agregar un Componente a la Librería

### Opción A: Draw.io GUI

1. Abrir `TLM - Librería C4.xml` en Draw.io (File → Open Library)
2. File → Edit Library
3. Agregar el nuevo componente con: shape, color, iconos, metadata
4. Guardar el archivo XML

### Opción B: Editar XML directamente

El archivo `drawio-library/TLM - Librería C4.xml` contiene componentes en formato JSON/Draw.io. Seguir el formato existente:

```json
{
  "c4Name": "Nombre del componente",
  "c4Type": "App",
  "c4Technology": "ej. Node.js",
  "c4Description": "Descripción breve",
  "shape": "...",
  "fillColor": "#E1D5E7"
}
```

### Campos obligatorios

- `c4Name`: Nombre que aparece en la librería
- `c4Type`: `Person`, `System`, `App`, `Store`, `Component`
- `c4Technology`: Ejemplo de tecnología (o `n/a` si no aplica)
- `c4Description`: Descripción de uso

---

## PR Template Snippet

Copiar y pegar en la descripción del PR:

```markdown
## Problema observado

[Descripción del problema o gap en el estándar actual]

## Cambio propuesto

[Descripción concreta del cambio]

## Impacto

[¿Cómo afecta a los diagramas existentes? ¿Requiere actualización de diagramas anteriores?]
```

---

## Mantenedores

- **Slack**: `#architecture`
- **Email**: <architecture@company.com>

Para preguntas: abrir issue o preguntar en `#architecture` antes de crear PR.
