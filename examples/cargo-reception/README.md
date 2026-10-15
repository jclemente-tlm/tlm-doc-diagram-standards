# Recepción de carga — ejemplo ficticio

> **EJEMPLO FICTICIO — no representa la arquitectura actual de la empresa.** Nombres, flujos, tecnologías, proveedores y despliegue son supuestos didácticos.

## Requerimiento

Un operador registra la llegada de una carga, comprueba su peso y asigna una ubicación. El sistema conserva el registro y solicita un aviso de recepción.

## Reglas del ejemplo

- El peso debe ser positivo y la ubicación debe estar disponible.
- Solo el personal autorizado puede registrar recepciones.
- Cada recepción conserva fecha, operador e identificador de carga.

## Alcance

No incluye facturación, aduanas, aerolíneas ni despacho.

## Acceso e integraciones compartidas

- Keycloak **ilustrativo** autentica usuarios y clientes y emite JWT.
- El API Gateway **ilustrativo** usa Kong para recibir las llamadas HTTP con JWT y dirigirlas a la API. No se representa una comunicación entre Kong y Keycloak. El mecanismo de validación del token queda fuera del alcance del ejemplo; cada API verifica los permisos de negocio que le corresponden.
- Los mensajes internos que pasan por una cola no atraviesan Kong. El acceso directo al proveedor externo se representa únicamente cuando el flujo lo requiere.
- Versiones y ubicaciones de despliegue son hipotéticas. El diagrama Deployment usa una región y dos zonas de ejemplo; no afirma que exista esa infraestructura.

## Diagramas

| Nivel | Archivo | Qué muestra |
| --- | --- | --- |
| C1 | [Context](./context.drawio) | Actores, sistema como caja negra y sistemas relacionados. |
| C2 | [Container](./container.drawio) | Aplicaciones, stores y relaciones con protocolos. |
| C3 | [Component](./component.drawio) | Componentes de **Cargo API** únicamente. |
| Deployment | [Deployment](./deployment.drawio) | Topología de producción ilustrativa. |

Todos los diagramas son editables en Draw.io y llevan el aviso de ficción dentro del lienzo para que siga visible al exportarlos.
