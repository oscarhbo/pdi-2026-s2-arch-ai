# Aplicación del aprendizaje al proyecto

**Versión 1.0 · 15/11/2026 · H1-05: aplicación al diseño documentada.**

Este documento establece cómo se aplican las notas de estudio al analizador de linaje e impacto. Es la ubicación única de estas aplicaciones; la arquitectura las incorpora por referencia. Define el comportamiento de diseño, no resultados de pruebas ejecutadas.

| Recurso | Aplicación al diseño | Evidencia |
| --- | --- | --- |
| [A Philosophy of Software Design](libros/a-philosophy-of-software-design.md) | Módulos profundos, contratos, actualización y separación de responsabilidades. | APSD-01 a APSD-05 de este documento. |
| [AI Engineering](libros/ai-engineering.md) | Evaluación, contexto con evidencia, regresión y operación. | AIE-01 a AIE-05 de este documento. |
| [TOGAF Standard, 10th Edition](cursos/togaf-standard-10th-edition.md) | Formación complementaria; aplicaciones recogidas en las notas del curso. | Curso completado según confirmación de Óscar; no sustituye las lecturas del PDI. |

Architecting AI Software Systems corresponde a formación continua durante el hito 2, conforme a la aclaración de Óscar del 15/11/2026. No condiciona el cierre de H1-05. Se conserva el Excel original; su referencia bibliográfica precisa sigue por completar.

Las reglas siguientes son aplicaciones de ingeniería al caso del PDI basadas en las notas disponibles, no recetas textuales atribuidas a los autores. Véase el fundamento en [ADR-002](../02-arquitectura/decisiones/ADR-002-aplicacion-lecturas-al-diseno.md).

<a id="aplicaciones-software-design"></a>
## A Philosophy of Software Design

<a id="apsd-01"></a>
### APSD-01 — Contrato estable y ocultamiento de información

**Conceptos:** [módulos profundos](libros/a-philosophy-of-software-design.md#tema-4), [ocultamiento de información](libros/a-philosophy-of-software-design.md#tema-5) y [contratos antes del código](libros/a-philosophy-of-software-design.md#tema-15). Relación con RF-01 y RF-02.

El análisis recibe artefactos identificados por DAG y versión, configuración, SQL extraído y contexto disponible de esquemas y parámetros. Devuelve un modelo del dominio independiente de los nodos internos del parser:

| Información | Contenido y significado |
| --- | --- |
| Identidad | DAG, versión de artefactos, analizador y contexto utilizado. |
| Relaciones | Origen, destino, granularidad tabla/columna, tipo valor/condición y pasos STG. |
| Evidencia | Identificador de referencia, archivo, ubicación, fragmento y versión. |
| Cobertura | Artefactos y patrones analizados, referencias no resueltas y diagnósticos. |
| Sustento | Verificado, Inferido o Desconocido; no equivale a ejecutar el cambio. |
| Resultado de operación | Completo dentro de la cobertura declarada, parcial o fallido. |

Analizar no publica en el catálogo. El consumidor interpreta el resultado sin manipular el AST ni conocer las convenciones internas del extractor. Un patrón no soportado o un esquema ausente produce un diagnóstico, no un éxito vacío.

**Ejemplo sintético:** para total = precio × cantidad se conservan ambos orígenes y su evidencia. Si no se puede resolver un comodín, se declara qué información falta.

**Trade-off:** el adaptador al modelo propio tiene costo, pero evita propagar cambios del parser a todos los consumidores. Ocultar implementación no significa ocultar evidencia.

<a id="apsd-02"></a>
### APSD-02 — Encapsular actualización y errores

**Conceptos:** [concentrar complejidad](libros/a-philosophy-of-software-design.md#tema-8), [simplificar errores desde el contrato](libros/a-philosophy-of-software-design.md#tema-10) y [comparar diseños](libros/a-philosophy-of-software-design.md#tema-11). Relación con RF-03.

La publicación reemplaza la contribución vigente de un DAG como una unidad después de preparar y comprobar su nuevo análisis. Conserva la procedencia de otros DAGs y no expone una mezcla intermedia de versiones.

- Repetir el mismo análisis con iguales artefactos y contexto no duplica relaciones.
- Un análisis parcial o fallido no reemplaza el último resultado completo; conserva diagnósticos y comunica la antigüedad del resultado disponible.
- Si no existe resultado previo, se declara esa ausencia.
- La baja requiere una operación explícita; un archivo ausente no equivale a retirada.
- La publicación comprueba la versión vigente que esperaba reemplazar. Si cambió durante el análisis, no sobrescribe silenciosamente el resultado más reciente.
- Esquemas, parámetros y versión del analizador forman parte de la identidad del análisis; cambios relevantes obligan a revisar la vigencia de resultados dependientes.

**Ejemplo sintético:** A deja de aportar una relación y B la mantiene. Actualizar A retira solo su procedencia. Un fallo conserva la versión anterior de A con una advertencia visible.

**Alternativas y trade-off:** reemplazar por contribución facilita retirar relaciones obsoletas. Actualizar diferencias puede reducir escrituras, pero aumenta la complejidad de comparación y recuperación. Se utiliza reemplazo por DAG; mecanismo de almacenamiento y retención histórica se detallan en H1-02.

<a id="apsd-03"></a>
### APSD-03 — Separar análisis, consulta y explicación

**Conceptos:** [abstracciones por capa](libros/a-philosophy-of-software-design.md#tema-7) y [responsabilidades coherentes](libros/a-philosophy-of-software-design.md#tema-9). Relación con RF-04 y RF-05.

El análisis descubre relaciones; el catálogo conserva aportes y versiones; la consulta recorre dependencias; la IA explica el resultado. La consulta entrega cambio solicitado, versión consultada, caminos, evidencia y límites. La explicación no modifica el catálogo ni eleva inferencias a relaciones verificadas.

Son módulos lógicos, no una obligación de desplegar servicios independientes. Cada frontera aísla una razón de cambio: extracción, resolución, almacenamiento, recorrido o generación de texto.

**Ejemplo sintético:** B consume total y C solo identificadores de la misma tabla. El reporte distingue dependencia de valor y consumo de tabla; no garantiza que C siga ejecutándose si falla el productor.

**Trade-off:** mantener resultado estructurado y explicación añade trabajo, pero permite comprobar impacto sin llamadas IA y cambiar el generador sin alterar relaciones.

<a id="apsd-04"></a>
### APSD-04 — Nombres, invariantes y documentación semántica

**Conceptos:** [nombres](libros/a-philosophy-of-software-design.md#tema-14), [consistencia](libros/a-philosophy-of-software-design.md#tema-17) y [comportamiento evidente](libros/a-philosophy-of-software-design.md#tema-18).

Cada contrato documenta significado, precondiciones, efectos secundarios y errores. Se diferencia relación de valor, dependencia de condición, consumidor de tabla y candidato de impacto. Verificado significa respaldado por evidencia del análisis, no rotura comprobada.

Las identidades incorporan ámbito: dos STG con igual nombre no se fusionan sin evidencia. El orden de tareas no demuestra flujo de datos ni identifica por sí solo el target. Cada camino conserva evidencia por tramo.

**Trade-off:** mantener un vocabulario preciso requiere disciplina, pero evita interpretaciones incompatibles entre componentes.

<a id="apsd-05"></a>
### APSD-05 — Evolución y rendimiento guiados por evidencia

**Conceptos:** [diseño estratégico](libros/a-philosophy-of-software-design.md#tema-3), [evolución](libros/a-philosophy-of-software-design.md#tema-16) y [rendimiento](libros/a-philosophy-of-software-design.md#tema-20).

La inspección del parser precede a reutilizarlo, adaptarlo o reemplazarlo. Se conservan tablas y columnas y se acotan patrones, sin diseñar una solución universal para dialectos fuera del PDI. Se miden extracción, resolución, publicación, consulta y explicación antes de añadir caché, paralelismo o infraestructura.

**Trade-off:** invertir en contratos reduce acoplamiento futuro; no justifica construir funcionalidades sin necesidad presente.

<a id="aplicaciones-ai-engineering"></a>
## AI Engineering

<a id="aie-01"></a>
### AIE-01 — Evaluar componentes y tarea completa

**Conceptos:** [metodología](libros/ai-engineering.md#tema-3), [evaluación integral](libros/ai-engineering.md#tema-4) y [datos de evaluación](libros/ai-engineering.md#tema-8).

Antes de ejecutar se fijan artefactos sintéticos, relaciones esperadas y criterios de explicación. Se evalúan por separado:

| Dimensión | Comprobación |
| --- | --- |
| Linaje e impacto | Esperado frente a obtenido; omisiones y falsos positivos por tablas, columnas y condiciones. |
| Mantenimiento | Repetición, modificación, baja, fallo, versiones y aportes ajenos. |
| Fidelidad | Afirmaciones respaldadas por evidencia y conservación de límites. |
| Utilidad | Claridad sobre qué revisar y por qué; esfuerzo de consulta y verificación. |

La rúbrica de explicación comprueba exactitud, pertinencia de citas, incertidumbre y claridad. Una cita existente pero irrelevante no es sustento. Un formato válido no demuestra veracidad. La evaluación asistida por IA no constituye la única referencia de corrección.

**Trade-off:** preparar esperados y segmentar resultados exige trabajo, pero evita que una redacción convincente oculte omisiones. Se conservan los diez cambios de H2 y las cinco comparaciones de H3.

<a id="aie-02"></a>
### AIE-02 — Contexto pertinente con evidencia

**Conceptos:** [construcción del contexto](libros/ai-engineering.md#tema-5) y [recuperación y herramientas](libros/ai-engineering.md#tema-6).

La explicación recibe cambio solicitado, versión consultada, caminos relevantes, identificadores y fragmentos de evidencia, cobertura y referencias no resueltas. Se conservan los tramos y límites necesarios para sostener la conclusión.

La salida separa hechos, inferencias identificadas y verificaciones recomendadas. Si falta evidencia declara qué no puede determinar. Los comentarios y textos de los artefactos son datos, no instrucciones autorizadas. El componente de explicación no dispone de operaciones de escritura en el catálogo.

**Ejemplo sintético:** sin YAML de B, su nombre no demuestra que use total. La explicación comunica qué falta conocer.

**Trade-off:** acotar contexto reduce ruido y consumo, pero una selección excesiva elimina soporte. Si no cabe la evidencia necesaria, se comunica cobertura parcial, sin ocultarla.

<a id="aie-03"></a>
### AIE-03 — Convertir errores confirmados en regresiones

**Conceptos:** [calidad de datos](libros/ai-engineering.md#tema-8) y [feedback](libros/ai-engineering.md#tema-10).

Cada error confirmado conserva entrada, versión, esperado, obtenido, causa, corrección y resultado de repetición. Se clasifica por extracción, resolución, actualización, consulta, contexto o explicación para corregir el componente responsable.

Cambios de prompt, modelo o parser se comparan sobre casos versionados. Se separan ejemplos de ajuste y evaluación y se declara su eventual reutilización. El feedback se contrasta antes de convertirlo en referencia esperada.

**Trade-off:** mantener regresiones cuesta esfuerzo, pero permite detectar deterioros. Modificar el esperado para que coincida con una salida incorrecta no corrige el sistema.

<a id="aie-04"></a>
### AIE-04 — Complejidad proporcional a la tarea

**Conceptos:** [aplicaciones con IA](libros/ai-engineering.md#tema-1), [agentes y herramientas](libros/ai-engineering.md#tema-6) y [adaptación](libros/ai-engineering.md#tema-7).

El diseño utiliza un flujo explícito de análisis, publicación, consulta y explicación. La IA interpreta evidencia; el componente de linaje mantiene su propia evaluación. No se incorpora autonomía, recuperación vectorial o entrenamiento por el solo hecho de aparecer en la formación.

Una ampliación identifica primero una carencia observada y se compara con la base en calidad, esfuerzo y costo. Proveedor y modelo se eligen según tarea y restricciones, sin quedar seleccionados por este documento.

**Trade-off:** el flujo explícito limita flexibilidad, pero facilita reproducibilidad y control de fallos en un prototipo individual.

<a id="aie-05"></a>
### AIE-05 — Observabilidad, costo y respuesta ante fallos

**Conceptos:** [inferencia](libros/ai-engineering.md#tema-9) y [operación](libros/ai-engineering.md#tema-10).

Cada ejecución identifica artefactos, analizador, catálogo y, cuando aplique, modelo, configuración y prompt. Registra tiempos por etapa, errores, reintentos y consumo disponible. El costo se calcula con precios verificados al elegir proveedor; USD 20 sigue siendo presupuesto propuesto, no autorización de gasto.

Si falla la IA o no se valida el respaldo de su explicación, el reporte conserva el resultado estructurado e indica que el texto no está disponible o validado. Esto no sustituye demostrar el aporte de IA en el prototipo.

**Trade-off:** observar y comprobar añade esfuerzo, pero distingue costo de generación, costo de verificación y calidad. Se utilizan datos sintéticos y no se registran secretos.

## Recorrido integrado y evidencia de aplicación

A calcula total desde precio y cantidad, B consume total y C solo identificadores. APSD-01 conserva relaciones y evidencia; APSD-02 publica contribuciones sin duplicar ni borrar aportes ajenos; APSD-03 separa consulta y texto. AIE-02 aporta evidencia pertinente; AIE-01 evalúa relaciones y afirmaciones por separado; AIE-03 conserva errores confirmados como regresiones.

Si el reanálisis de B falla, permanece la versión anterior con advertencia. Si falta evidencia se expresa incertidumbre. Si falla la generación sigue accesible el resultado estructurado. Este recorrido define comportamiento esperado; las pruebas ejecutables corresponden a H2.

H1-05 queda cubierto por las notas de ambos libros y su aplicación a estos contratos, reglas y criterios. H1-02 mantiene abiertos la cobertura SQL verificada, el adaptador real del parser, el esquema físico, las tecnologías y el detalle restante de arquitectura; H1-03 conserva la estimación de esfuerzo.
