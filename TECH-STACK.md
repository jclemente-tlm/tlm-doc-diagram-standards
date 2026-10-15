# Stack tecnológico vigente

**Fuente:** catálogo facilitado por el equipo el 2026-10-15. Este documento registra el estado y alcance declarados; los diagramas ficticios no acreditan despliegues reales.

## Adoptadas

| Tecnología | Alcance registrado | Fundamento |
| --- | --- | --- |
| AWS ECS Fargate | Workloads corporativos desplegados en la plataforma AWS documentada. | ADR-002 |
| Keycloak | Proveedor corporativo de identidad operativo y consumido por varios servicios; el onboarding puede requerir coordinación manual. | ADR-003 |
| AWS Secrets Manager | Secretos administrados en workloads AWS que consumen esta capacidad. | ADR-004 |
| AWS Parameter Store | Configuración de workloads AWS dentro del alcance documentado. | ADR-005 |
| PostgreSQL | Persistencia relacional para los servicios incluidos por la decisión corporativa. | ADR-006 |
| Amazon S3 | Almacenamiento corporativo de objetos en soluciones AWS. | ADR-007 |
| Kong OSS | API Gateway corporativo operativo y consumido por varios servicios; el onboarding puede requerir coordinación manual. | ADR-010 |
| HashiCorp Terraform CLI | Infraestructura declarativa mediante módulos y estado corporativos, con distribución y versión de licenciamiento y soporte compatibles. | ADR-011 |
| GitHub Actions | Repositorios y pipelines dentro de la plataforma GitHub corporativa. | ADR-012 |
| AWS ECR | Imágenes de workloads desplegados en la plataforma AWS. | ADR-013 |
| Grafana, Loki, Mimir, Tempo y OpenTelemetry | Plataforma corporativa de observabilidad adoptada para visualización, logs, métricas y trazas. | ADR-014 |

## En evaluación

| Tecnología | Alcance registrado | Fundamento |
| --- | --- | --- |
| Apache Kafka | Tecnología objetivo para mensajería; faltan owner, canal, ambientes y prueba de consumo soportada. | ADR-008 |
| Debezium | Tecnología objetivo para CDC; faltan owner, procedimiento, ambientes y prueba de incorporación soportada. | ADR-009 |

## Componentes adoptados del stack

Estas tablas son normativas aunque no cada fila tenga un ADR propio. Un ADR se añade cuando sea necesario conservar alternativas, consecuencias o un alcance arquitectónico específico.

### Plataforma

| Tecnología | Uso corporativo |
| --- | --- |
| Amazon EC2 | Cómputo general y ejecución de contenedores cuando la plataforma lo requiera. |
| Application Load Balancer | Balanceo HTTP/HTTPS de workloads AWS. |
| Docker | Empaquetado y ejecución de aplicaciones. |
| Imágenes runtime oficiales mínimas y compatibles | Usar una variante publicada para la versión adoptada, probar el comportamiento y promover por digest. `slim` aplica únicamente cuando el proveedor la publica y satisface el caso. |
| Docker Compose | Entornos locales de desarrollo. |

### Desarrollo

| Tecnología | Uso corporativo |
| --- | --- |
| C# / .NET 10 LTS | Lenguaje y runtime para nuevos desarrollos backend. |
| ASP.NET Core 10 | APIs y servicios web sobre .NET 10 LTS. |
| Angular | Framework frontend principal. |
| Entity Framework Core | ORM principal y migraciones de bases de datos. |
| Dapper | Consultas SQL optimizadas cuando el caso lo requiere. |
| FluentValidation | Validación de modelos. |
| Mapster | Mapeo entre contratos y modelos. |
| Microsoft.Extensions.Diagnostics.HealthChecks | Instrumentación de salud en servicios .NET. |

### Datos

| Tecnología | Uso corporativo |
| --- | --- |
| Oracle 19c | Sistemas existentes y casos justificados dentro de su alcance. |

### Integración y mensajería

| Tecnología | Uso corporativo |
| --- | --- |
| Amazon SQS | Colas para desacoplar procesos y permitir su procesamiento asíncrono. |
| Amazon SNS | Publicación y distribución de mensajes a suscriptores. |

### Observabilidad

| Tecnología | Uso corporativo |
| --- | --- |
| Serilog | Provider recomendado para logs JSON por stdout en aplicaciones .NET cuando se requieran formatters, enrichers, filtros o sinks adicionales; la ruta OTLP principal usa `ILogger<T>` con OpenTelemetry. |
| Grafana Alloy | Recolección y envío de señales a la plataforma corporativa. |

### Ingeniería y DevSecOps

| Tecnología | Uso corporativo |
| --- | --- |
| Git | Control de versiones. |
| GitHub | Plataforma corporativa de repositorios. |
| GitHub Packages | Registro corporativo de paquetes NuGet. |
| Hadolint | Validación de Dockerfiles. |
| Gitleaks | Detección de secretos expuestos. |
| Trivy | Escaneo de vulnerabilidades en imágenes. |
| Checkov | Validación de seguridad para infraestructura declarativa. |
| SonarQube Community | Análisis estático y calidad de código. |
| Semgrep | Análisis estático mediante reglas de seguridad y arquitectura. |
| OWASP Dependency-Check | Identificación de dependencias vulnerables. |

## Uso restringido

| Tecnología | Condición |
| --- | --- |
| React | Casos frontend específicos con evaluación arquitectónica previa. |
| DynamoDB | Casos donde el modelo NoSQL o las capacidades del servicio aportan una ventaja técnica justificada. |

## Tecnologías heredadas

Continúan para mantenimiento y operación de sistemas existentes; no se incorporan a nuevos desarrollos sin una excepción aprobada.

| Tecnología | Alcance |
| --- | --- |
| .NET Framework | Aplicaciones corporativas existentes. |
| ASP.NET MVC / Web API sobre .NET Framework | Aplicaciones legacy existentes. |
| Python | Aplicaciones y procesos existentes. |
| IIS | Hospedaje de aplicaciones .NET Framework existentes en servidores Windows. |
| AWS API Gateway | APIs creadas antes de la adopción de Kong. |
| SQL Server | Aplicaciones existentes on-premise. |

## Tecnologías no adoptadas

| Tecnología | Alternativa o motivo vigente |
| --- | --- |
| AutoMapper | Mapster es la alternativa adoptada. |
| MediatR | CQRS se implementa sin esta dependencia obligatoria. |
| Kubernetes, incluido Amazon EKS | AWS ECS Fargate es la plataforma de orquestación adoptada. |
| MySQL | PostgreSQL es la base de datos relacional adoptada para nuevos desarrollos. |

## Uso en diagramas

| Caso | Criterio para ejemplos nuevos |
| --- | --- |
| APIs y workers | ASP.NET Core 10 para APIs y .NET 10 para workers; ECS Fargate cuando representen nuevos workloads AWS. |
| Interfaces web | Angular como framework principal; React solo con evaluación arquitectónica previa. |
| Persistencia | PostgreSQL para los servicios bajo ADR-006; no asumir RDS u otro alojamiento si no está confirmado. |
| Mensajería | Amazon SQS y Amazon SNS están adoptados. Kafka y Debezium permanecen en evaluación. RabbitMQ no figura en este catálogo; no presentarlo como opción adoptada. |
| Observabilidad | En diagramas de otros proyectos, representar la plataforma corporativa como un sistema externo que recibe logs, métricas y trazas; detallar sus componentes internos solo al documentar esa plataforma. |
| Identidad y gateway | Keycloak y Kong OSS son capacidades corporativas adoptadas; el diagrama debe respetar el alcance real del sistema. |

Los diagramas de nuevas soluciones deben reflejar las tecnologías adoptadas que realmente utiliza el sistema. Las tecnologías en evaluación solo se muestran cuando el diagrama representa explícitamente una propuesta. Las restringidas o heredadas requieren que el alcance del caso quede indicado. Los ejemplos ficticios no constituyen evidencia de adopción ni de topología real.
