# PDI 2026 S2 · Arquitectura e IA

Repositorio de Óscar Hernández Barrón, Technical Specialist en Bluetab México, para demostrar capacidades de Technical Architect y avanzar hacia AI Solutions Architect. El trabajo desarrolla Accountability y Acción Comercial mediante una solución de descubrimiento incremental de linaje y análisis de impacto de cambios asistido por IA en plataformas de datos.

**Estado al 17/11/2026:** propuesta de arquitectura v1.0 integrada como borrador para revisión, con organización ArchFlow en `02-arquitectura/`. Incluye web local, stack seleccionado, contratos, decisiones, vistas y validaciones previstas. La inspección del parser y los ejemplos queda documentada. H1-02/H1-03 conservan estado En curso; no hay implementación, pruebas funcionales ni aceptación automática de ADR. Fechas originales: 15/11/2026, 20/12/2026 y 31/01/2027.

## Empieza aquí

Si visitas el repositorio por primera vez, comienza por la **[guía del caso de negocio](01-caso-de-negocio/README.md)**. Te llevará del problema a la propuesta de piloto en cinco pasos. Cada documento incluye enlaces para volver al índice y continuar con el siguiente.

Para recorrer el proyecto completo, sigue este orden. Algunas secciones aún contienen propuestas o plantillas; consulta el estado indicado en cada documento.

| Paso | Sección | Qué encontrarás |
| --- | --- | --- |
| 1 | [Caso de negocio](01-caso-de-negocio/README.md) | Problema, alcance, beneficios, esfuerzo y propuesta de piloto. |
| 2 | [Arquitectura](02-arquitectura/README.md) | Propuesta v1.0, SAD, requisitos, ADR, vistas y validación con estructura ArchFlow. |
| 3 | [Solución](03-solucion/README.md) | Organización del futuro prototipo; todavía sin implementación. |
| 4 | [Evaluación](02-arquitectura/docs/validation.md) | Cómo se propone comprobar el linaje, el impacto y el valor. |
| 5 | [Aprendizaje](05-aprendizaje/README.md) | Recursos previstos y cómo se documentará su aplicación. |
| 6 | [Presentación](06-presentacion/presentacion-ejecutiva.md) | Preparación del relato ejecutivo, la demo y el ensayo. |

Para entender el marco de desarrollo profesional, consulta los [objetivos del PDI](00-pdi/Objetivos_PDI_2026.md) y sus [fuentes](00-pdi/fuentes/README.md).

## Retomar o colaborar en el trabajo

Leer [instrucciones](AGENTS.md), [objetivos](00-pdi/Objetivos_PDI_2026.md), [estado actual](00-pdi/estado-actual.md) y [seguimiento](00-pdi/seguimiento-compromisos.md). La próxima tarea es revisar y ajustar la [propuesta de arquitectura](02-arquitectura/docs/revision-propuesta.md) directamente en el PDI. La arquitectura se modifica en docs/, adr/ y diagrams/; los borradores sustituidos se retiraron y los registros originales de decisiones y revisión se conservan por su evidencia propia. Construcción y evaluación pertenecen a etapas posteriores.

## Organización de arquitectura

Adopto la estructura documental de ArchFlow dentro de [02-arquitectura](02-arquitectura/README.md). El [registro de integración](02-arquitectura/docs/validation-log.md#procedencia-transferencia) identifica la entrega fuente, sus hashes, destinos e historial conservado. El caso de negocio, objetivos, aprendizaje y presentación mantienen sus ubicaciones; no son copias del SAD.

## Accesos de consulta

- [Fuentes y aclaraciones](00-pdi/fuentes/README.md) y [plan de trabajo](00-pdi/plan-de-trabajo.md).
- [Índice del caso de negocio](01-caso-de-negocio/README.md).
- [Arquitectura](02-arquitectura/README.md).
- [Solución futura](03-solucion/README.md).
- [Plan de evaluación](02-arquitectura/docs/validation.md).
- [Aprendizaje](05-aprendizaje/README.md).
- [Presentación](06-presentacion/presentacion-ejecutiva.md), [demo](06-presentacion/guion-demo.md) y [recursos externos](06-presentacion/recursos-externos.md).

El Excel final es la autoridad sobre los compromisos; el Markdown organiza su contexto. Una plantilla no acredita un entregable terminado. Las revisiones humanas son opcionales; las simulaciones con IA se registran como tales.
