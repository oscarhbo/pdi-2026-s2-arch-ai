# Registro de decisión

<a id="ADR-0001"></a>
## ADR-0001 — Relaciones con procedencia y publicación por contribución

Estado: Propuesto  
Fecha-revisada: 2026-11-17  
Responsable: Óscar Hernández Barrón  
Sustituye: No aplica  
Sustituido-por: No aplica

Concreto la representación de relaciones y la elegibilidad de publicación con independencia del almacenamiento, concretado posteriormente en ADR-0003. Este ID pertenece a ArchFlow; no renumera ni sustituye el ADR-001 del PDI.

Requisitos: [FR-002](../docs/requirements.md#FR-002), [FR-003](../docs/requirements.md#FR-003), [NFR-001](../docs/requirements.md#NFR-001), [NFR-002](../docs/requirements.md#NFR-002). Elementos: [análisis](../docs/sad.md#EL-001), [catálogo](../docs/sad.md#EL-002) y [consulta](../docs/sad.md#EL-003). Fuentes: [criterios vigentes](../README.md#SRC-008), [inspección](../revision-parser-y-dags.md), [recorrido propuesto](../docs/recorrido-sintetico.md).

### Contexto y problema

El extractor previo enumera tablas, columnas proyectadas y textos, pero no conserva todas las relaciones que necesita el análisis de impacto. Hay varios productores de un destino y diferencias entre dependencia de valor, influencia sobre filas y orden de tareas. Publicar una salida vacía/incompleta puede retirar conocimiento correcto.

El reemplazo unitario por DAG y la conservación del último resultado completo son criterios vigentes. Esta propuesta concreta cómo representarlos y qué bloquea la publicación, sin atribuir aceptación nueva.

### Criterios de decisión

Evidencia por tramo; distinguir valores/condiciones; preservar contribuciones ajenas; soportar STG, vistas y ventanas; no inventar relaciones ante contexto ausente; implementación proporcional al prototipo. No se asignan ponderaciones ni costos sin medición.

### Alternativas consideradas

| Alternativa | Ventaja | Riesgo/costo | Evaluación |
|---|---|---|---|
| Conservar solo inventario y reglas en texto | Reutiliza más directamente la salida actual | No permite reconstruir caminos de columna de forma fiable ni retirar aportes por procedencia | Insuficiente para el alcance |
| Grafo simple columna→columna para todo | Facilita recorrido uniforme | Confunde selección de filas y ventanas con flujo de valor; pierde semántica de operaciones | No recomendado como único modelo |
| Objetos, operaciones, relaciones tipadas y aportes por DAG/revisión | Explica motivos, evidencia y publicación independiente del motor físico | Más contratos y casos por mantener | Recomendado para evaluar |
| Publicar automáticamente cualquier salida parcial | Expone novedades antes | Relaciones ausentes pueden interpretarse como retiradas y ocultar impacto | Contradice la regla vigente; no recomendado |

### Decisión propuesta

Represento el valor entre columnas; las condiciones con rol y ámbito sobre operación o expresión; las lecturas/escrituras entre operación y objeto; y la orquestación como relación separada. Cada afirmación conserva aportes por DAG, versión de análisis, sentencia y evidencia. La consulta compone caminos sin reetiquetar una condición como valor.

Un resultado es publicable solo cuando sus invariantes son válidas y el análisis es completo para un perfil de cobertura previamente identificado. Diagnósticos de identidad ambigua, estructura no resuelta, destino posicional sin esquema o patrón relevante no soportado lo vuelven parcial. Un fallo interno que impide un resultado consistente lo vuelve fallido. Ninguno reemplaza el vigente completo.

Un valor simbólico que no altera identidad, estructura ni resolución de relaciones puede mantenerse como advertencia visible; esto acredita linaje estructural, no resultados de ejecución. La clasificación se determina por el uso del parámetro, no solo por su nombre.

Guardar resultado preparado y activar contribución mediante comparación de la revisión esperada. Repetir la misma identidad de análisis es idempotente. Conservar la referencia de revisión consultada para explicar cada reporte. Un conflicto de publicación devuelve conflicto y no sobrescribe. El mecanismo transaccional, historial y retención se concretan en [ADR-0003](ADR-0003-sqlite-y-revisiones.md#ADR-0003) y el [diseño técnico](../docs/diseno-tecnico.md).

### Consecuencias y trade-offs

Se puede consultar por motivo y retirar un aporte sin borrar los demás. La aplicación debe gestionar identidad, ámbitos, invalidación y evidencia de expresiones; no basta adaptar nombres de campos del JSON previo. Los resultados parciales quedan disponibles para diagnóstico, separados del catálogo vigente. Eso retrasa incorporación de novedades incompletas a cambio de evitar pérdidas silenciosas.

No se promete que conservar el último completo mantenga validez frente a esquemas externos cambiados: mostrar revisión requerida y contexto empleado sigue siendo obligatorio. Límites de recorrido o caminos incompletos deben llegar hasta la explicación.

### Validación y revisión

Validaciones previstas: [VAL-002](../docs/validation.md#VAL-002) y [VAL-003](../docs/validation.md#VAL-003); resultados No ejecutado. Revisar con el corpus y ante cambios de patrones, almacenamiento o modelo de identidad. Responsable: Óscar. Aceptación y riesgo residual pendientes.

### Historial y aprobación

16/11/2026: propuesta preparada con el recorrido sintético. Sin aprobación formal de este ADR ni selección implícita de base de grafos.
