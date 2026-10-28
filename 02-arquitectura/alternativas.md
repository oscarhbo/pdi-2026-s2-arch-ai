# Alternativas

**Estado: Pendiente.** Matriz de exploración actualizada al 28/10/2026; sin comparación técnica concluida. Tablas y columnas ya son prioridad 1, no alternativas excluyentes.

| Decisión pendiente | Alternativas por explorar | Criterio / evidencia necesaria |
| --- | --- | --- |
| Reutilización del parser | Reutilizar / adaptar / reemplazar solo con justificación. | Inspeccionar primero código y comportamiento; cobertura por patrón, esfuerzo y trazabilidad. |
| Cobertura de columnas | Subconjunto acotado de patrones / ampliación progresiva. | Casos representativos, esquemas disponibles y limitaciones verificadas. |
| Modelo de relaciones | Relaciones por pasos conservando STG / vista derivada fuente-target. | Mantener procedencia y distinguir valores de condiciones; no eliminar intermedios necesarios. |
| Catálogo | Persistencia local estructurada / relacional / grafos. | Consultas requeridas, actualización por DAG, versiones y volumen de prueba. |
| Actualización | Reemplazar aportes de un DAG / conservar versiones y activar una. | Bajas, idempotencia, fallos parciales y aportes compartidos; costo de implementación. |
| Activación del descubrimiento | Manual / automatizada. | Necesidad del MVP frente al esfuerzo de integración. |
| Papel de IA | Explicación de evidencia / inferencias adicionales clasificadas. | Utilidad, fidelidad, costo y validación independiente. |
| Flujo y contexto IA | Flujo explícito / agentes; evidencia directa / recuperación adicional. | Necesidad demostrada; no introducir RAG o agentes por defecto. |
| Ejecución | Local / nube; un proceso / servicios. | Reproducibilidad y viabilidad individual; entorno GCP de origen no obliga a desplegar allí. |

Registrar fuentes técnicas vigentes al comparar capacidades específicas de tecnologías. Esta matriz recoge preguntas derivadas del contexto aportado; no atribuye capacidades a librerías no inspeccionadas ni selecciona componentes por aparecer en un curso.
