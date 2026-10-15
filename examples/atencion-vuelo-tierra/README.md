# Proyecto ficticio: atención de un vuelo en tierra

> **EJEMPLO FICTICIO.** El proyecto, sus reglas, integraciones y despliegue son hipotéticos. Las plataformas corporativas de identidad, API Gateway y observabilidad sí existen; su participación concreta en este proyecto es ilustrativa.

## Requerimiento

Una aerolínea solicita la atención de un vuelo. El coordinador consulta la información del vuelo, planifica personal y equipos, y asigna tareas de rampa. El equipo registra avances e incidencias. El coordinador resuelve los pendientes y confirma el cierre de la atención a la aerolínea.

## Actores y límite

El **Sistema de Atención de un Vuelo en Tierra** es el sistema bajo estudio. Participan un representante de aerolínea, un coordinador de operaciones y un equipo de rampa. Un servicio externo ficticio aporta horarios y cambios de vuelo. El Sistema de Gestión de Identidad y Accesos, el API Gateway y la Plataforma de Observabilidad se muestran como sistemas corporativos compartidos, fuera del límite del proyecto. **Keycloak y Kong OSS son tecnologías**, no nombres de esos servicios.

## Flujo y reglas ficticias

1. El representante solicita la atención del vuelo; el coordinador consulta horario y servicios requeridos.
2. El coordinador asigna personal y equipos disponibles a las tareas de rampa. No se puede asignar el mismo recurso a tareas simultáneas incompatibles.
3. El equipo registra inicio, avances y término de cada tarea. Cada cambio conserva responsable y hora.
4. Ante una incidencia, el equipo la registra y el coordinador deja constancia de su resolución.
5. El sistema permite cerrar la atención solo si las tareas obligatorias están terminadas y las incidencias resueltas. El coordinador confirma el cierre a la aerolínea.

La planificación y la confirmación son parte del proceso ficticio; el ejemplo no pretende definir reglas operativas reales. El servicio externo de vuelos también es hipotético.

La autenticación se inicia desde el portal mediante redirección del navegador al Sistema de Gestión de Identidad y Accesos; los diagramas no representan una llamada directa de la persona a esa plataforma.

## Qué enseña cada diagrama

| Nivel | Archivo | Foco |
| --- | --- | --- |
| C1 | [Contexto](./context.drawio) | Proceso completo, actores y sistemas relacionados. |
| C2 | [Contenedores](./container.drawio) | Portal Angular, API ASP.NET Core 10, PostgreSQL y servicios externos. |
| C3 | [Componentes](./component.drawio) | Componentes internos de la **API de Atención de Vuelos** para planificación, tareas, incidencias y cierre. |
| Deployment | [Despliegue](./deployment.drawio) | Tareas ECS Fargate y dependencias en una topología **ilustrativa**. |

El Deployment representa el Servicio de Configuración (AWS Parameter Store) y la Gestión de Secretos (AWS Secrets Manager) como capacidades disponibles y fuentes de configuración y secretos al iniciar el workload; no representa consultas en cada petición. La Plataforma de Observabilidad aparece como caja negra para logs, métricas y trazas. El alojamiento de PostgreSQL, las redes, regiones, réplicas y endpoints quedan sin especificar.
