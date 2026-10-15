# Proyecto ficticio: seguimiento de carga refrigerada

> **EJEMPLO FICTICIO.** El proyecto, sus reglas, integraciones y despliegue son hipotéticos. Las plataformas corporativas de identidad, API Gateway y observabilidad sí existen; su participación concreta en este proyecto es ilustrativa.

## Requerimiento

El operador registra una carga que requiere una condición de temperatura. Un servicio de mediciones ficticio envía lecturas. El sistema guarda el historial, detecta desviaciones, avisa al supervisor y deja constancia de la atención. El cliente puede consultar el seguimiento y recibe la carga con su historial.

## Actores y límite

El **Sistema de Seguimiento de Carga Refrigerada** es el sistema bajo estudio. Participan el operador, el supervisor y el cliente. El Servicio de Mediciones y el Proveedor de Notificaciones son terceros ficticios. El Sistema de Gestión de Identidad y Accesos, el API Gateway y la Plataforma de Observabilidad se muestran como sistemas corporativos compartidos, fuera del límite del proyecto. **Keycloak y Kong OSS son tecnologías**, no nombres funcionales.

## Flujo y reglas ficticias

1. El operador registra la carga y el rango de temperatura requerido.
2. El proveedor ficticio envía mediciones a la API, que las coloca en Amazon SQS. Un procesador consume cada lectura, valida identificador, hora y unidad, y guarda en PostgreSQL las lecturas válidas antes de compararlas con el rango vigente.
3. Si detecta una desviación, el procesador registra la alerta y publica un evento en Amazon SNS. La suscripción entrega el evento a una cola SQS de avisos.
4. Un segundo procesador consume la cola de avisos, solicita el envío al proveedor externo y registra el resultado. El supervisor revisa y resuelve la desviación en el portal.
5. Al entregar la carga, el operador registra el cierre y el cliente puede consultar el historial de mediciones e incidencias.

El ejemplo supone manejo de reintentos y consumo idempotente para mensajes que puedan recibirse más de una vez; sus políticas exactas se definirían en un proyecto real. No modela sensores, instalaciones de frío ni límites de temperatura reales.

La autenticación se inicia desde el portal mediante redirección del navegador al Sistema de Gestión de Identidad y Accesos; los diagramas no representan una llamada directa de la persona a esa plataforma.

## Qué enseña cada diagrama

| Nivel | Archivo | Foco |
| --- | --- | --- |
| C1 | [Contexto](./context.drawio) | Actores, mediciones externas, alertas y cierre del seguimiento. |
| C2 | [Contenedores](./container.drawio) | Portal, API, procesadores, PostgreSQL, SQS, SNS y terceros. |
| C3 | [Componentes](./component.drawio) | Componentes internos del **Procesador de Mediciones**. |
| Deployment | [Despliegue](./deployment.drawio) | Workloads ECS Fargate y dependencias en una topología **ilustrativa**. |

El Deployment representa el Servicio de Configuración (AWS Parameter Store) y la Gestión de Secretos (AWS Secrets Manager) como capacidades disponibles y fuentes de configuración y secretos al iniciar los workloads. Los tres workloads envían logs, métricas y trazas a la Plataforma de Observabilidad, representada como caja negra; sus herramientas internas no aparecen aquí. El alojamiento de PostgreSQL, redes, regiones, réplicas y endpoints no se especifica.
