# Plan de proyectos de ejemplo C4

## Propósito y alcance

Crear tres ejemplos de **proyectos con requerimientos concretos** en el contexto de los servicios aeroportuarios de Talma. Los proyectos, procesos, reglas, sistemas propios de cada proyecto, integraciones y despliegues son **ficticios**. Los sistemas corporativos de identidad, API Gateway y observabilidad sí existen; Parameter Store y Secrets Manager son capacidades disponibles. Los diagramas enseñarán a delimitar un sistema y a representarlo en C1, C2, C3 y Deployment; no documentarán una solución existente ni afirmarán que Talma utiliza estos flujos.

Cada proyecto describe un proceso de principio a fin. El sistema bajo estudio se delimitará en la ficha del proyecto antes de dibujar; un proyecto puede involucrar varios sistemas. Los servicios compartidos y proveedores externos se dibujarán fuera de ese límite cuando corresponda.

## Proyectos seleccionados

| Proyecto ficticio | Requerimiento y proceso completo | Sistema bajo estudio propuesto | Cobertura didáctica principal |
| --- | --- | --- | --- |
| Atención de un vuelo en tierra | Recibir una solicitud de atención y los datos del vuelo; planificar personal y equipos; ejecutar y registrar tareas de rampa; resolver incidencias; confirmar el cierre de la atención. | Sistema de Atención de un Vuelo en Tierra. | Varios roles, planificación, estados de tareas, información de vuelos de un tercero, identidad, API Gateway y persistencia. |
| Recepción y entrega de carga de importación | Recibir información y documentos; registrar la llegada y ubicación; procesar documentos; consultar el estado; autorizar y registrar la entrega. | Sistema de Recepción y Entrega de Carga de Importación. | Carga de archivos, almacenamiento de objetos, cola y worker, trazabilidad en base de datos e integración con un tercero ficticio para autorizar el retiro. |
| Seguimiento de carga refrigerada | Registrar la carga y sus condiciones requeridas; recibir mediciones; detectar una desviación; atender y notificar la incidencia; entregar la carga con su historial. | Sistema de Seguimiento de Carga Refrigerada. | Ingesta de eventos, reglas y alertas, procesamiento asíncrono, notificaciones, series de mediciones y observabilidad. |

## Proyectos opcionales para ampliar los ejemplos

Estos candidatos también son **proyectos ficticios** situados en servicios aeroportuarios de Talma. Aún no tienen ficha ni diagramas; el sistema bajo estudio y las integraciones se definirían al elegir cada proyecto.

| Proyecto opcional | Requerimiento y proceso completo | Elementos que podría enseñar |
| --- | --- | --- |
| Atención de irregularidades de carga | Detectar un daño o una diferencia al recibir o mover la carga; registrar evidencias; coordinar la revisión con la aerolínea; resolver el caso; comunicar y conservar el resultado. | Actores internos y externos, carga de evidencias a S3, estados del caso, tareas de revisión, avisos y trazabilidad. |
| Atención de equipaje extraviado | Recibir el reporte del pasajero; registrar características y trayecto; buscar y coordinar la localización; informar avances; entregar el equipaje o cerrar el caso con su resultado. | Pasajero y agentes, consulta a un sistema externo de equipajes, seguimiento de casos, notificaciones y control de acceso. |
| Atención de mantenimiento en línea | Recibir una observación técnica; revisar antecedentes y bitácora; asignar personal; registrar diagnóstico y trabajo realizado; verificar el resultado y cerrar la atención. | Roles con permisos diferentes, documentos y bitácora, asignación de tareas, integración con un sistema técnico externo y auditoría de cambios. |

Los avisos o notificaciones son capacidades dentro de estos procesos, no necesariamente un proyecto independiente. La selección de tecnologías y componentes concretos se haría a partir del requerimiento de cada proyecto, sin asumir que estos flujos existen hoy.

## Convenciones para los diagramas

- **Nombres:** mantener el nombre del proyecto en títulos y carpetas; nombrar el sistema bajo estudio como «Sistema de» + nombre del proyecto. Repetirlo sin variantes en C1, el límite de C2 y las fichas.
- **Nombre funcional primero.** Usar «Sistema de Gestión de Identidad y Accesos» y «API Gateway» como nombres de sistemas o servicios. Indicar **Keycloak** y **Kong OSS** como tecnologías, por ejemplo en la etiqueta tecnológica de C2 o Deployment. No usar «Keycloak» o «Kong» como nombre del sistema.
- **C1:** personas, sistema bajo estudio y sistemas relacionados con el proceso de negocio. Las plataformas técnicas compartidas se muestran en C2 y Deployment.
- **C2:** aplicaciones, workers, almacenes y colas del proyecto; identidad, API Gateway y observabilidad se representan como sistemas corporativos compartidos en los tres ejemplos.
- **C3:** componentes internos de un único contenedor elegido por su aporte al requerimiento; no descomponer toda la solución ni convertir servicios externos en componentes internos.
- **Deployment:** despliegue hipotético de los contenedores del sistema bajo estudio y dependencias relevantes, sin afirmar regiones, redes, endpoints ni topologías corporativas reales.
- **Observabilidad:** representar «Plataforma de Observabilidad» como un sistema corporativo compartido fuera del límite del proyecto y mostrar el envío de logs, métricas y trazas cuando corresponda. Grafana, Loki, Mimir, Tempo y Alloy se detallarán solo en diagramas cuyo sistema bajo estudio sea esa plataforma.
- **Configuración y secretos:** representar el Servicio de Configuración (AWS Systems Manager Parameter Store) y la Gestión de Secretos (AWS Secrets Manager) en los Deployment de los tres ejemplos; evitar fingir que cada petición de negocio los consulta.
- **Mensajería:** Amazon SQS y Amazon SNS están adoptados. Usar SQS para colas y consumidores asíncronos, y SNS para publicación y distribución cuando el requerimiento lo pida. Kafka y Debezium permanecen en evaluación.
- **Tecnologías:** escogerlas del [stack vigente](../TECH-STACK.md) según el caso. Que una tecnología esté adoptada no implica que todos los proyectos deban usarla.

## Nombres canónicos de los ejemplos

| Proyecto | Sistema C1 / límite C2 | Portal C2 | API C2 / límite C3 o worker C3 | Base de datos |
| --- | --- | --- | --- | --- |
| Atención de un vuelo en tierra | Sistema de Atención de un Vuelo en Tierra | Portal de Atención de Vuelos | API de Atención de Vuelos | Base de Atención de Vuelos |
| Recepción y entrega de carga de importación | Sistema de Recepción y Entrega de Carga de Importación | Portal de Carga de Importación | API de Carga de Importación | Base de Carga de Importación |
| Seguimiento de carga refrigerada | Sistema de Seguimiento de Carga Refrigerada | Portal de Carga Refrigerada | API de Carga Refrigerada; C3 del Procesador de Mediciones | Base de Carga Refrigerada |

Los componentes C3 y los almacenes especializados mantienen nombres por responsabilidad, por ejemplo «Validador de Lecturas», «Cola de Documentos» y «Tema de Desviaciones». Los actores conservan el mismo nombre entre los niveles donde aparecen. Deployment se centra en instancias y dependencias, sin agrupar actores distintos en una sola figura.

## Trabajo para cada proyecto

1. Redactar una ficha breve con objetivo, actores, disparador, flujo principal, cierre, reglas, excepciones, sistema bajo estudio y supuestos ficticios.
2. Definir qué capacidades e integraciones son necesarias para ese requerimiento; evitar añadir componentes solo para completar una lista tecnológica.
3. Dibujar C1 y comprobar que el límite y las relaciones explican el proceso sin conocer detalles técnicos.
4. Dibujar C2 y C3 con responsabilidades coherentes, tecnología en su campo correspondiente y relaciones etiquetadas.
5. Dibujar Deployment como topología ilustrativa coherente con C2; incluir configuración, secretos y telemetría cuando sean relevantes.
6. Revisar los cuatro diagramas contra la ficha: mismo alcance, nombres consistentes, proceso completo y aviso visible de que es ficticio.

## Estado de implementación

Los tres proyectos ya tienen ficha y diagramas C1, C2, C3 y Deployment en [el índice](./README.md). Las carpetas `atencion-vuelo-tierra`, `recepcion-entrega-carga-importacion` y `seguimiento-carga-refrigerada` corresponden a los tres proyectos seleccionados. Los diagramas siguen siendo ejemplos ficticios y editables.
