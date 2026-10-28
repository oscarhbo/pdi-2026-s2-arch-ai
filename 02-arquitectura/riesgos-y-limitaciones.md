# Riesgos y limitaciones

**Estado: En curso.** Registro inicial de riesgos previstos; no son fallos observados del prototipo.

| Riesgo | Consecuencia | Mitigación propuesta / verificación |
| --- | --- | --- |
| Tiempo disponible insuficiente (FODA). | Incumplimiento de hitos. | Estimar tareas, revisar semanalmente y reducir alcance propuesto sin ocultar fechas. |
| Alias, SQL dinámico y referencias externas. | Dependencias omitidas o ambiguas. | Definir cobertura; reportar no resueltas y evaluar límites con casos conocidos. |
| Confundir dependencia con rotura. | Falsas alarmas y pérdida de confianza. | Separar candidato a revisión de fallo demostrado. |
| Explicación IA sin soporte. | Conclusiones incorrectas. | Citas verificables y evaluación de afirmaciones; no usar el texto como prueba de relación. |
| Cobertura de columnas demasiado amplia. | Complejidad incompatible con capacidad. | Mantener prioridad 1 de tablas y columnas; acotar patrones a partir del parser y declarar límites. |
| Catálogo incompleto u obsoleto. | Falsa sensación de seguridad. | Mostrar DAGs/versiones analizados y probar actualización/baja sin relaciones obsoletas. |
| Convenciones del framework desconocidas. | Extracción u orden incorrectos; target mal identificado. | Revisar ejemplos antes de fijar adaptador; no asumir que última tarea equivale a target. |
| Esquemas o parámetros ausentes. | Mapeo de columnas ambiguo, especialmente SELECT * e INSERT posicional. | Declarar no resuelto; no inventar metadatos. |
| STG, JOIN y filtros tratados como simples proyecciones. | Pérdida de linaje o propagación excesiva. | Separar relaciones de valor/condición y preservar pasos y ámbito de intermedios. |
| Parser existente aún no compartido. | Estimaciones de reutilización sin fundamento. | Inspeccionar antes de proponer reemplazo; continuar delimitación conceptual mientras se obtiene. |
| Acceso, cuota o costo de IA. | Evaluación interrumpida. | Estimar consumo, validar presupuesto y definir comportamiento de fallo antes de gastar. |
| Comparación manual sesgada. | Beneficio aparente no defendible. | Congelar protocolo, registrar familiaridad y tiempos; reconocer muestra pequeña. |

Responsable de seguimiento: Óscar. Revisar en el diseño y en cada retrospectiva; añadir probabilidad, impacto, acción y evidencia cuando se valoren. Ver [seguimiento](../00-pdi/seguimiento-compromisos.md).
