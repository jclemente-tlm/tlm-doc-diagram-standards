# Ejemplos de diagramas — escenarios ficticios

> **EJEMPLOS FICTICIOS — no representan la arquitectura actual de la empresa.** Los procesos se inspiran en actividades reconocibles de carga y rampa. Los sistemas propios de los proyectos, sus flujos, integraciones y despliegues son supuestos didácticos. Identidad, API Gateway y observabilidad son plataformas corporativas existentes.

El [plan de proyectos de ejemplo](./PROJECT-PLAN.md) explica la selección, el alcance y las convenciones de estos tres proyectos. Cada uno cuenta un proceso completo y contiene C1, C2, C3 y Deployment.

| Caso | Requerimiento | Diagramas |
| --- | --- | --- |
| [Atención de un vuelo en tierra](./atencion-vuelo-tierra/README.md) | Solicitud, planificación, ejecución, incidencias y cierre. | C1, C2, C3 y Deployment |
| [Recepción y entrega de carga de importación](./recepcion-entrega-carga-importacion/README.md) | Documentos, recepción, procesamiento, consulta y entrega. | C1, C2, C3 y Deployment |
| [Seguimiento de carga refrigerada](./seguimiento-carga-refrigerada/README.md) | Registro, mediciones, desviaciones, avisos y entrega con historial. | C1, C2, C3 y Deployment |

| Nivel | Elementos de la librería que se muestran |
| --- | --- |
| C1 | Person, External Person, System, External System y Simple Relationship. |
| C2 | System Scope Boundary, Web App, Application, Database, Object Storage, Queue or Topic y Relationship; el ejemplo de importación incluye Legend. |
| C3 | Container Scope Boundary, Component y relaciones con sistemas o stores externos al contenedor. |
| Deployment | Application y stores concretos con relaciones y protocolos de despliegue ilustrativo. |

En los flujos de acceso, la persona utiliza el portal; el navegador sigue la redirección OIDC hacia el Sistema de Gestión de Identidad y Accesos. Las APIs reciben las solicitudes a través del API Gateway.

Los C1 se centran en actores y proceso; los C2 muestran las tres plataformas corporativas compartidas y los Deployment muestran además el Servicio de Configuración (Parameter Store) y la Gestión de Secretos (Secrets Manager). Estos últimos se representan como fuentes de configuración al iniciar los workloads, no como pasos de cada operación de negocio.

En los tres proyectos, «Sistema de Gestión de Identidad y Accesos» y «API Gateway» son nombres funcionales de servicios compartidos; **Keycloak** y **Kong OSS** indican sus tecnologías. Los flujos y los servicios propios de los proyectos son ficticios; las plataformas corporativas compartidas existen. La Plataforma de Observabilidad aparece como sistema corporativo compartido, fuera del límite de cada proyecto; sus componentes internos no se descomponen aquí.

[Identity System - Container](./identity-container.drawio) es un ejemplo técnico anterior e independiente de estos tres casos. También está marcado como ficticio dentro del lienzo.

El [stack tecnológico vigente](../TECH-STACK.md) es la referencia para sistemas reales. Los ejemplos principales usan tecnologías adoptadas del catálogo y no constituyen evidencia de una topología real.

Los elementos y relaciones de los doce diagramas usan las formas y los campos editables de la [librería C4 corporativa](../drawio-library/TLM%20-%20Librería%20C4.xml). Los archivos `.drawio` son editables. Para publicar imágenes PNG, exportar cada archivo desde Draw.io y guardar el PNG junto al original, siguiendo [STANDARDS.md §12](../STANDARDS.md#-12-herramientas).
