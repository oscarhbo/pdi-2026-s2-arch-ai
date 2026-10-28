# Beneficios y criterios de éxito

Lectura 3 de 5 · [Índice del caso de negocio](README.md)

[← Anterior: alcance y exclusiones](alcance-y-exclusiones.md) · [Siguiente: costos y esfuerzo →](costos-y-esfuerzo.md)

**Estado: En revisión. Versión 0.1, 28/10/2026.** Criterios propuestos para evaluar el [caso de negocio](problema-y-contexto.md). No son métricas medidas ni umbrales aceptados. No se modifican los indicadores vacíos del Excel.

| Hipótesis / criterio propuesto | Cómo comprobarlo | Meta |
| --- | --- | --- |
| Descubrir linaje trazable de tablas y columnas. | Comparar relaciones conocidas con detectadas, separando valores y condiciones; incluir STG. | Por acordar. |
| Mantener conocimiento vigente entre DAGs. | Probar alta, reanálisis, modificación/baja y cobertura de versiones. | Por acordar. |
| Detectar candidatos relevantes sin omitir dependencias conocidas. | Comparar esperado/obtenido en diez cambios; falsos positivos y negativos por caso. | Por acordar. |
| Explicaciones sustentadas. | Revisar que las citas existan y respalden la afirmación; registrar afirmaciones sin soporte. | Por acordar. |
| Comunicar incertidumbre. | Verificar que las referencias no resueltas se declaren y no aparezcan como ausencia de impacto. | Por acordar. |
| Reducir esfuerzo de revisión. | Comparar tiempo y corrección en cinco casos; separar construcción/mantenimiento del catálogo de consulta de impacto. | Por acordar; beneficio hipotético. |

Diez cambios y cinco comparaciones son compromisos originales, no umbrales de precisión. La evaluación también debe reportar fallos y límites aunque no confirme las hipótesis. Vincular mediciones al [plan](../04-evaluacion/plan-de-evaluacion.md).

## Hipótesis medibles

| ID | Hipótesis | Medición propuesta | Qué la debilitaría |
| --- | --- | --- | --- |
| B-01 | El conocimiento reutilizable reduce trabajo recurrente. | Tiempo activo por caso, preparación inicial, actualización y verificación del reporte por separado. | Consultas más rápidas cuyo ahorro no compense preparación y mantenimiento. |
| B-02 | Las relaciones y citas permiten investigar con menos omisiones. | Relaciones esperadas, omitidas y falsas; precisión y exhaustividad separadas por tabla, columna y condición. | Omisiones ocultas o falsas alarmas que aumenten el trabajo de revisión. |
| B-03 | El conocimiento sigue siendo útil al cambiar un DAG. | Reanálisis, modificación y baja: relaciones correctas conservadas, retiradas o actualizadas. | Relaciones antiguas mostradas como vigentes o pérdida de aportes de otro DAG. |
| B-04 | La explicación IA ayuda a interpretar sin alterar los hechos. | Correspondencia entre afirmación y evidencia; respuesta comprensible a qué revisar, por qué y con qué límite. | Afirmaciones sin soporte o tiempo de corrección mayor que la ayuda obtenida. |

Una relación correcta aporta evidencia técnica; no demuestra por sí sola un beneficio económico. La evaluación de claridad realizada por Óscar se registrará como autoevaluación, con sus límites, no como validación de usuarios independientes.

## Criterios de aceptación propuestos para el corpus acotado

Estos criterios son candidatos a acuerdo antes de ejecutar pruebas. Aplican únicamente a patrones declarados y a un resultado esperado fijado previamente; no prometen un porcentaje de cobertura sobre la plataforma empresarial.

| ID | Condición propuesta | Cómo se comprobaría |
| --- | --- | --- |
| CA-01 | Toda relación presentada como verificada permite localizar artefacto, versión y evidencia que la respalda. | Revisar las relaciones emitidas en los diez cambios y registrar cualquier cita inexistente o insuficiente. |
| CA-02 | Para patrones incluidos, las relaciones esperadas de tablas y columnas coinciden con las obtenidas; cualquier desviación impide declarar ese patrón validado. | Reportar falsos positivos/negativos por clase, sin compensar una omisión con aciertos en otra. |
| CA-03 | Entradas incompletas y patrones fuera de cobertura producen límites explícitos, nunca una garantía de ausencia de impacto. | Casos de YAML faltante, referencia no resuelta o contexto insuficiente. |
| CA-04 | Reanalizar no duplica relaciones; modificar o retirar un DAG actualiza su contribución sin borrar relaciones ajenas. | Comparar conocimiento esperado antes/después y las versiones consultadas. |
| CA-05 | La explicación distingue relaciones verificadas, inferencias, desconocidos y recomendaciones. | Cero afirmaciones sin soporte presentadas como hechos en las salidas evaluadas. Las inferencias se identifican aparte. |
| CA-06 | Se documentan los diez cambios y cinco comparaciones con entradas, versiones, resultados y límites. | Evidencia reproducible, incluso si el resultado es desfavorable. |

La coincidencia completa en un corpus pequeño es una propuesta exigente para validar el subconjunto, no una estimación de precisión futura. Si no se alcanza, registrar fallos, corregir o proponer una reducción explícita de patrones; no reclasificarlos después de medir para presentar éxito artificial.

## Decisión sobre valor y continuidad

- **Continuar hacia una ampliación:** criterios técnicos acordados satisfechos y al menos un beneficio defendible con evidencia, sin ocultar el costo de mantenimiento. El ahorro neto se calcula con [costos y esfuerzo](costos-y-esfuerzo.md).
- **Acotar y repetir:** utilidad potencial, pero fallos concentrados en patrones o mantenimiento corregibles dentro de la capacidad; conservar resultados anteriores y justificar la revisión.
- **No recomendar extensión todavía:** no hay trazabilidad suficiente, persiste obsolescencia, o el costo total supera la alternativa manual sin una mejora de calidad que justifique evaluarlo de nuevo.

No se fija un porcentaje arbitrario de ahorro. Informar diferencias absolutas por caso, mediana y dispersión; con cinco casos no se generaliza a todo el negocio. Una mejora de calidad con mayor esfuerzo puede justificar un análisis posterior, pero no se reportará como ahorro de tiempo.

## Información pendiente

Tiempo y frecuencia reales de investigaciones, costo interno por hora, volumen de DAGs, variación de patrones y costo de actualización. No impiden definir el ejercicio sintético; sí limitan cualquier cálculo de retorno comercial y una promesa de precio o ahorro.

---

[← Anterior: alcance y exclusiones](alcance-y-exclusiones.md) · [Índice del caso de negocio](README.md) · [Siguiente: costos y esfuerzo →](costos-y-esfuerzo.md)
