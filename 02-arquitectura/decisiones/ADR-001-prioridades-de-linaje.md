# ADR-001 · Linaje incremental de tablas y columnas como prioridad

Fecha: 28/10/2026. **Estado: Aceptada como prioridad de alcance por Óscar**, según [contexto transferido, secciones 7 y 15](../../00-pdi/fuentes/contexto-transferido-2026-10-28.md). Este registro documenta una decisión comunicada por el usuario, no una arquitectura aprobada por inferencia.

## Contexto

El borrador inicial dejaba abierta la granularidad. El nuevo contexto sitúa la falta de trazabilidad entre DAGs como problema principal y prioriza tablas y columnas desde el primer prototipo.

## Alternativas

- Solo relaciones entre tablas: alcance más acotado, insuficiente para la prioridad de columnas comunicada.
- Tablas y columnas con patrones delimitados: prioridad elegida por el usuario.
- Cobertura SQL universal desde el inicio: no comprometida; no hay evidencia de viabilidad.

## Decisión

Orientar el prototipo al descubrimiento incremental y al análisis de impacto entre DAGs, incluyendo linaje de tablas y columnas como prioridad 1. Tomar Spark SQL embebido en YAML como referencia y examinar el parser existente antes de proponer reemplazarlo. Delimitar patrones soportados después de revisar artefactos.

## Consecuencias

El esfuerzo se controla acotando patrones y escenarios; no se reduce unilateralmente a tablas. Se requiere distinguir linaje de valores, dependencias de condiciones y relaciones no resueltas, y diseñar el tratamiento de STG. Persistencia, ejecución manual, tecnologías y arquitectura IA siguen pendientes. Las fechas y entregables originales del PDI no cambian.
