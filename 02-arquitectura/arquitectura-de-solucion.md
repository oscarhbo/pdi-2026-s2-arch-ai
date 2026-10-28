# Arquitectura de solución

**Estado: En curso.** Modelo conceptual incorporado el 28/10/2026; no es diseño aprobado ni implementación. [Alcance](../01-caso-de-negocio/alcance-y-exclusiones.md) y [prioridades confirmadas](decisiones/ADR-001-prioridades-de-linaje.md).

## Flujos conceptuales propuestos

Descubrimiento: artefactos del DAG + configuración + YAML → extracción de SQL y contexto → parser existente y resolución de linaje → relaciones de tablas/columnas con evidencia → conocimiento acumulado.

Impacto: solicitud de cambio → consulta del conocimiento disponible → recorrido de dependencias → explicación asistida por IA → reporte con evidencia, cobertura, incertidumbres y verificaciones recomendadas.

Son responsabilidades lógicas; no implican servicios separados, framework de agentes ni motor de persistencia seleccionado.

| Responsabilidad propuesta | Información necesaria | Pregunta de diseño |
| --- | --- | --- |
| Interpretar artefactos | DAG, configuración, YAML, parámetros y versión. | ¿Cómo identificar archivos, tareas y orden sin ejecutar código arbitrario ni inventar convenciones? |
| Extraer y resolver linaje | SQL, esquemas cuando sean necesarios, ámbito de STG y orden. | ¿Qué devuelve el parser existente y qué resolución adicional necesita? |
| Registrar conocimiento | Identidades de objetos/columnas, relaciones tipadas y procedencia. | ¿Cómo conservar aportes de varios DAGs sin mezclar versiones? |
| Actualizar conocimiento | DAG añadido, modificado o retirado; éxito o fallo del análisis. | ¿Cuándo reemplazar relaciones y cómo evitar obsoletas o borrado de aportes ajenos? |
| Consultar impacto | Cambio, relaciones, cobertura y versión consultada. | ¿Qué propagación se justifica por columnas y cuál solo produce candidatos por tabla? |
| Explicar | Resultados verificables y límites. | ¿Cómo impedir que una sugerencia de IA se presente como relación verificada? |

## Información a modelar antes de elegir almacenamiento

DAG/tarea y versión de artefactos; tablas y columnas con identidad y ámbito; lecturas/escrituras; relaciones de valor y de condiciones; pasos STG; evidencia localizable (archivo, ubicación y fragmento); estado Verificado/Inferido/Desconocido; fecha y resultado del análisis; cobertura y referencias no resueltas. Es una lista de necesidades, no un esquema de datos aceptado.

Preguntas del catálogo: productor de una tabla, tablas consumidas por un DAG, origen de una columna, consumidores de una columna, dependencias directas/transitivas y procedencia de cada relación. La herramienta puede declarar qué artefactos no analizó o qué referencias quedaron abiertas; no puede enumerar con certeza dependencias que nunca recibió.

## Límites por resolver

No asumir que orden de tareas equivale a flujo de datos. Conservar las STG necesarias para reconstruir relaciones, aunque se proponga una vista resumida hacia targets. No asignar columnas destino por alias sin validar esquema/mapeo. Distinguir lectura de tabla, dependencia de columna y efecto de JOIN/filtro. La identificación del target y la semántica de escrituras requieren ejemplos.

La separación de análisis verificable y explicación IA es propuesta: evaluar sus contratos y validación. La IA podría explicar, resumir, recomendar verificaciones o proponer inferencias clasificadas, pero no es la única fuente de verdad. GCP describe el entorno de referencia; despliegue local o nube todavía no seleccionado.

Completar después: contratos, manejo de fallos, seguridad, actualización incremental, consultas, diagramas, costo, operación y trazabilidad a pruebas. Comparar [alternativas](alternativas.md), mantener [riesgos](riesgos-y-limitaciones.md) y registrar decisiones en [ADR](decisiones/README.md).
