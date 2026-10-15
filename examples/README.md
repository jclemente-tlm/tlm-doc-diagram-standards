# Ejemplos de diagramas — escenarios ficticios

> **EJEMPLOS FICTICIOS — no representan la arquitectura actual de la empresa.** Los procesos se inspiran en actividades reconocibles de carga y rampa. Los nombres de sistemas, flujos, tecnologías y despliegues son supuestos didácticos.

| Caso | Requerimiento | Diagramas |
| --- | --- | --- |
| [Recepción de carga](./cargo-reception/README.md) | Registrar llegada, peso y ubicación de carga. | C1, C2, C3 y Deployment |
| [Tareas de rampa](./ramp-tasks/README.md) | Asignar una tarea y registrar su avance. | C1, C2, C3 y Deployment |
| [Avisos operativos](./operational-alerts/README.md) | Preparar, enviar y registrar avisos. | C1, C2, C3 y Deployment |

En los tres casos, Kong y Keycloak son **supuestos del ejercicio** para mostrar acceso a APIs mediante JWT. Los diagramas no muestran comunicación directa entre el API Gateway y el servicio de identidad. El mecanismo de validación del token queda fuera del alcance; cada API conserva su propia autorización de negocio. Los mensajes de cola no se dirigen por el gateway.

[Identity System - Container](./identity-container.drawio) es un ejemplo técnico anterior e independiente de estos tres casos. También está marcado como ficticio dentro del lienzo.

Los archivos `.drawio` son editables. Para publicar imágenes PNG, exportar cada archivo desde Draw.io y guardar el PNG junto al original, siguiendo [STANDARDS.md §12](../STANDARDS.md#-12-herramientas).
