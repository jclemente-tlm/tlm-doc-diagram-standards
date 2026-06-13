# Mejores Prácticas de Diagramas de Arquitectura

Principios fundamentales, anti-patrones y recomendaciones para crear diagramas efectivos.

---

## Principios Fundamentales

### 1. Propósito Sobre Completitud

> Los diagramas NO son inventarios técnicos exhaustivos. Son herramientas de comunicación.

- Incluir solo lo necesario para entender el propósito
- Omitir detalles que no aportan valor
- Enfocarse en el mensaje principal

### 2. Consistencia Visual

> La gente debe reconocer elementos SIN leer texto.

- Usar SIEMPRE los mismos colores para los mismos tipos
- Usar SIEMPRE los mismos shapes para las mismas categorías
- Seguir SIEMPRE las mismas convenciones de layout

### 3. Actualización Regular

> Un diagrama desactualizado es peor que no tener diagrama.

- Actualizar diagramas cuando el sistema cambia
- Incluir actualización en Definition of Done
- Asignar ownership claro

---

## Anti-Patrones

### 1. Diagrama Spaghetti

**Problema**: Todo conectado con todo, flechas cruzadas, >30 elementos.

**Solución**:
- Dividir en múltiples diagramas
- Agrupar por dominio
- Mostrar solo relaciones principales

### 2. Diagrama PowerPoint

**Problema**: Logos gigantes de vendors, más marketing que arquitectura.

**Solución**:
- Íconos pequeños y consistentes
- Tecnología como metadata, no como identidad
- Foco en función, no en vendor

### 3. Diagrama Frankenstein

**Problema**: Mezcla múltiples niveles C4 en un solo diagrama.

**Solución**:
- Respetar niveles C4 (uno por diagrama)
- Un propósito por diagrama
- Dividir en múltiples diagramas

### 4. Diagrama Fantasma

**Problema**: Desactualizado, sin fecha, sin owner.

**Solución**:
- Asignar owner
- Incluir fecha de actualización
- Versionar en Git

### 5. Diagrama Minimalista Extremo

**Problema**: Cajas sin nombres específicos, flechas sin etiquetar.

**Solución**:
- Nombres específicos y descriptivos
- Etiquetar todas las relaciones
- Agregar contexto suficiente

---

## Mejores Prácticas por Escenario

### Microservicios

50 microservicios no caben en un diagrama legible.

**Estrategias**:
- Agrupar por dominio con boundaries
- Crear un diagrama por dominio
- Mostrar patrón representativo (ej: API → Service → DB)

### Event-Driven Architecture

**Clarity en Eventos**:
- Etiquetar eventos: `[Order Service] --"OrderCreated"--> [Event Bus]`
- Usar líneas dashed (async)
- Indicar topic name

### CDC (Change Data Capture)

```
[Oracle DB] --CDC (Debezium)--> [Kafka] --"user.changes"--> [Service]
```

1. Etiquetar explícitamente como "CDC"
2. Indicar tecnología (Debezium, etc.)
3. Línea dashed (es async)
4. Mostrar topic name

### Multi-Tenant / Multi-País

**Opciones**:
- Boundaries por país/tenant
- Diagramas separados con componentes comunes compartidos
- Anotaciones: `[Identity Service] (Peru, Chile, Colombia)`

### Integraciones con Sistemas Legacy

**Clarity en Integración**:
- Marcar como externo con color/forma apropiada
- Indicar protocolo claramente: SOAP/XML, RFC, REST
- Mostrar adapter pattern: `[Service] → [SAP Adapter] → [SAP ERP]`

---

## Tips por Audiencia

### Stakeholders de Negocio

**Enfatizar**: Propósito de negocio, integraciones clave, actores, flujos principales
**Evitar**: Detalles técnicos, tecnologías específicas, componentes internos
**Diagrama ideal**: C1 (Context)

### Arquitectos

**Enfatizar**: Patrones arquitectónicos, integraciones, decisiones de diseño, trade-offs
**Diagramas ideales**: C1, C2, Deployment

### Developers

**Enfatizar**: Estructura de código, APIs y contratos, flujos de datos, responsabilidades
**Diagramas ideales**: C2, C3, Sequence

### DevOps/SRE

**Enfatizar**: Infraestructura, networking, escalabilidad, alta disponibilidad
**Diagramas ideales**: Deployment, Infrastructure

---

## Métricas de Calidad

### Indicadores de un Buen Diagrama

1. **Clarity**: ¿Se entiende sin explicación?
2. **Accuracy**: ¿Refleja la realidad?
3. **Relevance**: ¿Es útil para su propósito?
4. **Maintainability**: ¿Es fácil de actualizar?
5. **Consistency**: ¿Sigue los estándares?

### Red Flags

- Nadie sabe quién lo creó
- Nadie lo ha actualizado en 6+ meses
- No está versionado
- Contradice otros diagramas
- Usa colores/shapes no estándar

---

## Referencias

- [Validation Criteria](./validation-criteria.md) - Checklists para validación