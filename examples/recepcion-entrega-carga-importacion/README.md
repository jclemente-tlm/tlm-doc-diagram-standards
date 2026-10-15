# Proyecto ficticio: recepción y entrega de carga de importación

> **EJEMPLO FICTICIO.** El proyecto, sus reglas, integraciones y despliegue son hipotéticos. Las plataformas corporativas de identidad, API Gateway y observabilidad sí existen; su participación concreta en este proyecto es ilustrativa.

## Requerimiento

Una aerolínea remite información y documentos de una carga de importación. Un operador registra su llegada, peso y ubicación. El sistema conserva los archivos y procesa los documentos de forma asíncrona. El cliente consulta el estado y solicita el retiro. El operador verifica una autorización ficticia y registra la entrega.

## Actores y límite

El **Sistema de Recepción y Entrega de Carga de Importación** es el sistema bajo estudio. Participan la aerolínea, el operador y el cliente o agente de carga. Un Servicio Externo de Autorización de Retiro, proveedor tercero ficticio, confirma si procede el retiro. El Sistema de Gestión de Identidad y Accesos, el API Gateway y la Plataforma de Observabilidad se muestran como sistemas corporativos compartidos, fuera del límite del proyecto. **Keycloak y Kong OSS son tecnologías** de los dos primeros.

## Flujo y reglas ficticias

1. La aerolínea carga los documentos y el operador registra identificador, peso positivo y ubicación disponible.
2. La API guarda cada archivo en Amazon S3 y publica en Amazon SQS una referencia al archivo; evita insertar el archivo completo en el mensaje.
3. Un procesador .NET consume la cola, lee el documento, extrae y valida los datos necesarios y registra el resultado en PostgreSQL. Los errores de procesamiento quedan visibles para revisión; no se interpreta un mensaje publicado como documento ya procesado.
4. El cliente consulta el estado y solicita el retiro. La API verifica los requisitos de registro y consulta al proveedor externo ficticio de autorización.
5. El operador confirma la entrega; el sistema conserva fecha, responsable e historial. Si la autorización falla, la entrega no se registra.

El proveedor externo de autorización y sus reglas son supuestos del ejercicio. No se modelan procedimientos aduaneros reales, facturación ni una topología de almacenamiento corporativa.

La autenticación se inicia desde el portal mediante redirección del navegador al Sistema de Gestión de Identidad y Accesos; los diagramas no representan una llamada directa de la persona a esa plataforma.

## Qué enseña cada diagrama

| Nivel | Archivo | Foco |
| --- | --- | --- |
| C1 | [Contexto](./context.drawio) | Actores, proceso completo e integración con el tercero ficticio que autoriza el retiro. |
| C2 | [Contenedores](./container.drawio) | Portal, API, worker, PostgreSQL, S3, SQS y proveedor externo ficticio de autorización. |
| C3 | [Componentes](./component.drawio) | Componentes internos de la **API de Carga de Importación**; el worker permanece como otro contenedor de C2. |
| Deployment | [Despliegue](./deployment.drawio) | Workloads ECS Fargate y servicios AWS en una topología **ilustrativa**. |

El Deployment representa el Servicio de Configuración (AWS Parameter Store) y la Gestión de Secretos (AWS Secrets Manager) como capacidades disponibles y fuentes al iniciar los workloads. La Plataforma de Observabilidad es una caja negra que recibe logs, métricas y trazas. El alojamiento de PostgreSQL, redes, regiones, réplicas y endpoints no se especifica.
