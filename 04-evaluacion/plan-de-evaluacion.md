# Plan de evaluación

**Estado: Pendiente.** Protocolo propuesto actualizado al 28/10/2026; sin pruebas ejecutadas ni umbrales aceptados.

Compromisos originales: diez cambios conocidos de renombrado, eliminación y modificación de tipos, y cinco comparaciones manuales. La validación del descubrimiento y del mantenimiento incremental aporta condiciones previas; no reemplaza esos diez cambios.

## Capas de evaluación propuestas

| Capa | Verificación esperada | Evidencia por preparar |
| --- | --- | --- |
| Extracción y parser | Ubicación de SQL, patrón soportado y limitación reproducible. | Artefacto/versiones, consulta extraída, salida del parser y resultado esperado independiente. |
| Linaje | Relaciones de tablas y columnas; valores frente a JOIN/filtros/agrupación; recorrido por STG. | Relaciones esperadas/detectadas/omitidas, falsas relaciones y citas. |
| Incrementalidad | Incorporar DAG 1, luego DAGs 2/3; consultar sin redescubrimiento completo. | Versiones de catálogo y aportes por DAG, antes/después. |
| Mantenimiento | Reanalizar sin duplicar, modificar o retirar un DAG, conservar aportes ajenos; tratar fallo parcial. | Estados esperados y obtenidos, relaciones retiradas/conservadas. |
| Impacto | Diez cambios, con propagación directa y transitiva justificada. | Cambio, candidatos, caminos, límites y clasificación. |
| Explicación IA | Fidelidad a resultados, citas válidas y tratamiento de incertidumbre. | Afirmaciones respaldadas, inferencias identificadas y recomendaciones separadas de hechos. |

Son escenarios candidatos del protocolo; la cobertura final se acordará tras revisar parser y ejemplos. No hay un número adicional de pruebas comprometido por esta tabla.

## Preparación y medición

1. Elegir patrones Spark SQL y convenciones verificables del framework; disponer de esquemas sintéticos cuando hagan falta. Registrar versión y composición del corpus y del catálogo.
2. Definir relaciones esperadas antes de observar el prototipo. Separar linaje por tabla, por columna y dependencias de condiciones; no sumar granularidades como una sola métrica.
3. Completar [casos](casos-de-prueba.md), incluyendo controles y límites. Distinguir ausencia comprobada dentro del corpus de información desconocida fuera de él.
4. Acordar metas y unidades. Proponer precisión/exhaustividad por clase de relación, citas respaldadas y cobertura de patrones; denominadores explícitos y N/A cuando no proceda. No transformar datos ausentes en ceros.
5. Ejecutar y guardar entrada, esperado/obtenido, referencias no resueltas, versiones, duración y costo si aplica. Una relación Verificada no acredita fallo de un pipeline ejecutado.
6. Comparar cinco casos con [protocolo manual](comparacion-manual-vs-solucion.md), separando costo de descubrir/mantener conocimiento del costo de consultar impacto.
7. Registrar fallos y correcciones, repetir casos afectados y consolidar [resultados](resultados/README.md) y [conclusiones](conclusiones.md).

Validación individual: declarar familiaridad con el corpus y sesgos del mismo evaluador. La IA puede cuestionar el protocolo, pero no certifica resultados. Diez cambios y cinco comparaciones no demuestran cobertura universal ni ahorro en producción.
