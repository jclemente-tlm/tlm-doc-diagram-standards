# Estándares de Diagramas de Arquitectura

> **Documentación compacta y práctica** para crear diagramas corporativos de arquitectura

---

## 🚀 START HERE

**Para el 95% de los casos, solo necesitas:**

### 📄 [STANDARDS.md](./STANDARDS.md) - **EL DOCUMENTO ÚNICO**

Todo lo esencial en un solo lugar (~10 minutos de lectura):

1. **Paleta corporativa** - Colores con códigos hex
2. **46 elementos totales** - 30 componentes + 12 flechas + 3 boundaries + 1 leyenda
3. **Reglas por nivel C4** - Qué incluir/excluir en C1, C2, C3
4. **Guía rápida** - Start to finish en bullets
5. **Reglas visuales** - Fuentes, tamaños, shapes, estructura de componentes
6. **Checklist de 11 puntos** - Validación rápida
7. **Prohibiciones** - Anti-patrones y errores comunes
8. **Nomenclatura** - Reglas simples
9. **Diagramas obligatorios** - Qué crear para cada tipo de sistema
10. **Herramientas** - Setup de Draw.io
11. **Proceso de creación** - Flujo de trabajo
12. **Deployment Diagram** - Guía de infraestructura
13. **Tipos de diagramas permitidos** - Qué crear y qué evitar

---

## 🎨 Herramientas Listas para Usar

### 1. Librería Draw.io

📦 **[TLM - Librería C4.xml](./drawio-library/TLM%20-%20Librería%20C4.xml)**

46 elementos pre-configurados:

- 30 componentes (Actores, Sistemas, Apps, Stores, Componentes C3)
- 12 tipos de flechas (HTTPS, HTTP, SOAP, gRPC, Kafka, SQS, CDC, Batch, etc.)
- 3 boundaries (Sistema, Container, Genérico)
- 1 leyenda de colores

**Instalación:**

1. Abrir Draw.io
2. File → Open Library
3. Seleccionar `TLM - Librería C4.xml`
4. Arrastrar y soltar componentes

### 2. Templates

📋 **[/templates/](./templates/)**

Templates listos para usar:

- **[template-c1-context.drawio](./templates/template-c1-context.drawio)** - Context Diagram
- **[template-c2-container.drawio](./templates/template-c2-container.drawio)** - Container Diagram
- **[template-c3-component.drawio](./templates/template-c3-component.drawio)** - Component Diagram

**Uso:**

1. Abrir template en Draw.io
2. Reemplazar elementos placeholder
3. Actualizar metadata (versión, fecha, owner)
4. Guardar en tu repositorio

---

## ⚡ Quick Start

### Primera vez creando un diagrama (30-45 min)

1. **Lee** [STANDARDS.md](./STANDARDS.md) (10 min)
2. **Instala** la librería Draw.io (2 min)
3. **Abre** el template correspondiente (C1/C2/C3)
4. **Arrastra** componentes de la librería
5. **Etiqueta** todas las flechas
6. **Valida** con el checklist de 10 puntos
7. **Guarda** en Git

### Actualizando un diagrama existente (15-30 min)

1. Identifica qué diagrama(s) cambió
2. Mantén convenciones visuales (colores, shapes)
3. Actualiza metadata (fecha, versión)
4. Valida con checklist
5. Commit con el PR relacionado

---

## 📚 Documentación de Referencia

**Solo para casos especiales o consultas profundas:**

La documentación detallada está en [\`/reference/\`](./reference/):

- [Estructura de componentes](./reference/component-structure.md) - Plantillas y ejemplos de 30 componentes
- [Validation Criteria](./reference/VALIDATION-CRITERIA.md) - Criterios detallados por nivel C4
- [Introducción a C4](./reference/c4-guidelines/c4-overview.md) - Qué es C4 y por qué usarlo
- [Best Practices](./reference/c4-guidelines/best-practices.md) - Mejores prácticas y antipatrones

**⚠️ Nota**: La mayoría de los equipos NO necesita leer la documentación de referencia. [STANDARDS.md](./STANDARDS.md) cubre el 95% de los casos de uso.

---

## 🎯 Estructura del Repositorio

```
/tlm-doc-diagram-standards
├── README.md                    # Este archivo - punto de entrada
├── STANDARDS.md                 # ⭐ EL DOCUMENTO ÚNICO - empieza aquí
│
├── /drawio-library              # Librería corporativa Draw.io
│   └── TLM - Librería C4.xml    # 46 elementos pre-configurados
│
├── /templates                   # Templates listos para usar
│   ├── template-c1-context.drawio   # Template C1
│   ├── template-c2-container.drawio # Template C2
│   └── template-c3-component.drawio # Template C3
│
├── /examples                    # Ejemplos de referencia
├── /icons                       # Iconografía estándar
│
└── /reference                   # 📚 Documentación detallada (lectura opcional)
    ├── component-structure.md   # Plantillas de 30 componentes
    ├── VALIDATION-CRITERIA.md   # Criterios de validación detallados
    └── /c4-guidelines
        ├── c4-overview.md       # Introducción a C4
        └── best-practices.md    # Mejores prácticas
```

---

## 🎨 Paleta Corporativa (Copiar/Pegar)

\`\`\`
Container-App (APIs, servicios): #E1D5E7
Container-Store (DBs, queues): #F8CECC
Component (componentes internos): #FFF2CC
Internal System: #9FC2FF
Internal User: #D5E8D4
External System/User: #EEEEEE
\`\`\`

---

## ✅ Checklist Ultra-Rápido (30 segundos)

Antes de commitear:

- [ ] Título + metadata (fecha, versión, owner)
- [ ] Colores según paleta corporativa
- [ ] Shapes correctos (cilindro = DB, folder = storage)
- [ ] Estructura `[Tipo: Tecnología]` en componentes
- [ ] TODAS las flechas etiquetadas
- [ ] Nombres descriptivos (NO "Service 1", "DB")
- [ ] Límites recomendados: C1 ≤10, C2 ≤20, C3 ≤12 elementos

---

## 📞 Contacto y Soporte

- **Slack**: \`#architecture\`
- **Email**: <architecture@company.com>
- **Preguntas**: Revisa primero [STANDARDS.md](./STANDARDS.md), luego pregunta en Slack

---

## 🎓 Para Nuevos en el Equipo

**Orden recomendado de lectura:**

1. **[STANDARDS.md](./STANDARDS.md)** (10 min) - Lectura obligatoria
2. **Instalar librería Draw.io** (2 min)
3. **Crear tu primer diagrama** con templates (30-45 min)
4. **Pedir feedback** en \`#architecture\`

**No necesitas leer nada más.** La documentación en \`/reference/\` es solo para casos especiales.

---

## 📊 Diagramas Obligatorios

| Sistema/Cambio        | C1  | C2  | C3  | Deployment |
| --------------------- | --- | --- | --- | ---------: |
| **Sistema nuevo**     | ✅  | ✅  | ⚠️  |         ✅ |
| **Servicio nuevo**    | -   | ✅  | ⚠️  |          - |
| **Cambio de infra**   | -   | -   | -   |         ✅ |
| **Integración nueva** | ✅  | ✅  | -   |          - |

✅ Obligatorio | ⚠️ Solo si es complejo | - No necesario

Ver detalles en [STANDARDS.md - Sección 10](./STANDARDS.md#-10-diagramas-obligatorios)

---

## 🏗️ Estructura de Componentes

**Formato estándar para todos los componentes:**

```
Nombre del Componente
[Tipo: Tecnología]
Descripción breve de responsabilidades.
```

**Ejemplos:**

```
Identity API
[App: .NET 8 REST API]
Gestiona autenticación y autorización.
```

```
identity-db
[Store: PostgreSQL]
Almacena usuarios, roles y permisos.
```

```
Cliente
[External Person]
Accede a servicios de autogestión.
```

Ver estructura completa en [component-structure.md](./reference/component-structure.md)

---

## 🔥 Prohibiciones

❌ **Diagrama Spaghetti** - Más de 30 elementos, líneas cruzadas
❌ **PowerPoint Architecture** - Colores y formas aleatorias
❌ **Frankenstein** - Mezclar C1 + C2 + Deployment en uno
❌ **Ghost Diagram** - Sin owner, desactualizado hace 2 años
❌ **Minimalista Extremo** - 3 cajas y 0 etiquetas
❌ **C4-Code diagrams** - Usar código fuente como documentación

Ver todas las prohibiciones en [STANDARDS.md - Sección 8](./STANDARDS.md#-8-prohibiciones)

---

## 🚀 Mejoras y Contribuciones

Para sugerir mejoras a los estándares:

1. Crear issue/PR con justificación
2. Discutir en \`#architecture\`
3. Obtener aprobación del Architecture Team
4. Actualizar documentación + librería + templates

---

**Versión**: 1.0 (Final)
**Última actualización**: 2026-06-04
**Mantenedores**: Architecture Team

---

**🎯 Recuerda: [STANDARDS.md](./STANDARDS.md) es tu mejor amigo. Léelo primero.**
