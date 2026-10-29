# Aplicación del aprendizaje al proyecto

Propósito: relacionar el aprendizaje con decisiones, cambios y pruebas. Mantener las notas de estudio en su carpeta de [aprendizaje](README.md). Documentar aquí las aplicaciones concretas al proyecto y enlazar los conceptos que las sustentan, sin duplicar el contenido del libro.

| Recurso / nota | Concepto aprendido | Aplicación al proyecto | Enlace a evidencia | Estado |
| --- | --- | --- | --- | --- |
| [A Philosophy of Software Design](libros/a-philosophy-of-software-design.md) | Módulos profundos, ocultamiento de información, contratos y diseño estratégico. | [Tres aplicaciones propuestas](#aplicaciones-software-design). | Notas de estudio disponibles; sin decisión aceptada ni implementación verificada de estas propuestas. | Estudio documentado; aplicación propuesta. |
| [TOGAF Standard, 10th Edition](cursos/togaf-standard-10th-edition.md) | Baseline/Target, brechas, requisitos y gobierno. | [Propuestas para el analizador de linaje](cursos/togaf-standard-10th-edition.md#aplicacion). | Sin implementación ni resultado verificado. | Propuesta de aplicación; curso completado según confirmación del usuario. |

No confundir una intención de aplicación con una modificación verificada. Las constancias se enlazan cuando existan; no se exige completar cursos enteros por inferencia.

<a id="aplicaciones-software-design"></a>
## A Philosophy of Software Design: Aplicaciones al proyecto

**Fecha de revisión:** 29/10/2026. **Estado:** propuestas para evaluar; ninguna aceptada o implementada por este registro.

### Validación de las notas y selección

La revisión de las notas frente a la [arquitectura conceptual](../02-arquitectura/arquitectura-de-solucion.md) y el [alcance propuesto](../01-caso-de-negocio/alcance-y-exclusiones.md) permite concretar tres aplicaciones pertinentes. Es una revisión de coherencia y utilidad para el proyecto, no una comprobación íntegra del texto del libro ni una validación del parser, cuyo código no se ha inspeccionado.

Conviene conservar estos matices al aplicar lo estudiado:

- **Profundidad no equivale a una función que haga todo.** La interfaz debe simplificar una responsabilidad coherente sin mezclar descubrimiento, persistencia y explicación por comodidad.
- **Ocultar implementación no significa ocultar evidencia.** Podemos encapsular nodos internos del parser, pero el consumidor necesita conocer procedencia, cobertura y referencias no resueltas.
- **Eliminar errores por diseño no significa ignorar fallos.** Repetir una actualización válida puede ser una operación normal; una extracción fallida nunca debe interpretarse como ausencia de dependencias.
- **Separar abstracciones no obliga a crear microservicios.** Las fronteras pueden ser módulos dentro del mismo proceso; cada separación debe justificar el conocimiento que aísla.

Las propuestas siguientes son aplicaciones propias al PDI. Idempotencia, publicación de versiones y controles sobre IA se usan como decisiones de ingeniería por evaluar, no como recetas textuales atribuidas al autor.

<a id="apsd-01"></a>
### APSD-01 — Definir un contrato estable para el análisis de linaje

**Conceptos:** [módulos profundos](libros/a-philosophy-of-software-design.md#tema-4), [ocultamiento de información](libros/a-philosophy-of-software-design.md#tema-5) y [contratos antes de implementar](libros/a-philosophy-of-software-design.md#tema-15). Relación con RF-01 y RF-02 del alcance propuesto.

**Problema que buscamos evitar.** Si cada consumidor debe entender nodos del parser, aliases y convenciones de YAML para consultar una relación, un cambio interno puede propagarse al catálogo, a la consulta y al reporte.

**Aplicación propuesta.** Definir una operación de análisis con entradas y salidas explícitas. Recibiría los artefactos identificados por versión y el contexto disponible; devolvería relaciones de tablas y columnas, pasos STG relevantes, evidencia localizable y diagnósticos. El contrato indicaría que analizar no publica por sí mismo una nueva versión del catálogo.

El módulo ocultaría la representación interna del parser y sus recorridos. Mantendría visibles las distinciones que necesitamos para interpretar el resultado: relaciones de valor o condición, referencias no resueltas, cobertura y sustento Verificado/Inferido/Desconocido. Estos últimos describen el sustento de una relación, no el éxito global del análisis ni un fallo ejecutado.

**Ejemplo sintético.** Si una columna de salida se calcula con precio y cantidad, el resultado debe permitir rastrear ambas entradas y su evidencia. Si falta el esquema para resolver una proyección, debemos comunicar la limitación; una lista vacía de columnas no basta para afirmar que no existen dependencias.

**Trade-off.** Un modelo de salida propio requiere mantener una adaptación desde el parser. Se justifica si reduce el acoplamiento; no conviene construir un modelo universal para dialectos y casos fuera de alcance.

**Cómo validar la propuesta.** Preparar ejemplos sintéticos de proyección, paso STG y referencia no resuelta, dentro de la cobertura que se acuerde. Verificar que un consumidor pueda interpretar las salidas sin acceder a nodos del parser y que un cambio de representación interna compatible no obligue a modificarlo. Comprobar por separado que cada relación conserve evidencia.

**Evidencia esperada:** contrato documentado, ejemplos de entradas/salidas y pruebas de comportamiento cuando exista implementación. No hay resultados de estas comprobaciones todavía.

<a id="apsd-02"></a>
### APSD-02 — Encapsular la actualización del conocimiento por DAG

**Conceptos:** [asumir complejidad dentro del módulo](libros/a-philosophy-of-software-design.md#tema-8), [simplificar errores desde el contrato](libros/a-philosophy-of-software-design.md#tema-10) y [comparar diseños](libros/a-philosophy-of-software-design.md#tema-11). Relación con RF-03.

**Problema que buscamos evitar.** Reanalizar un DAG puede duplicar relaciones, conservar relaciones que ya no existen o eliminar aportes de otros DAGs. Si cada invocador resuelve estas situaciones por su cuenta, la política se dispersa.

**Aplicación propuesta.** Concentrar la política de actualización en una responsabilidad que reciba el resultado de análisis y su versión. Comparar dos alternativas antes de elegir:

| Alternativa | Ventaja posible | Costo o riesgo |
| --- | --- | --- |
| Reemplazar el conjunto de aportes del DAG al publicar una versión válida. | Facilita razonar sobre qué relaciones pertenecen a la versión vigente. | Requiere conservar procedencia y publicar el cambio sin exponer un estado intermedio inconsistente. |
| Aplicar diferencias entre versiones. | Puede reducir el trabajo de actualización cuando el conjunto es grande. | Añade comparación, identidad estable y más casos de recuperación y borrado. |

Para el primer diseño conviene explorar el reemplazo por contribución del DAG por su facilidad de razonamiento; sigue siendo una recomendación, no una decisión aceptada. No supone recalcular todos los DAGs de la plataforma.

**Reglas candidatas del contrato.** Reprocesar la misma versión con el mismo contexto de análisis no duplicaría relaciones. Una nueva publicación retiraría los aportes anteriores de ese DAG y conservaría los de los demás. Un análisis fallido no borraría silenciosamente el último resultado válido: se registraría el fallo y se haría visible que el resultado conservado corresponde a una versión anterior. La baja explícita de un DAG tendría una operación distinta de un fallo de lectura.

La identidad del análisis debe contemplar cambios relevantes de esquemas, parámetros y versión del analizador; el contenido del DAG por sí solo puede ser insuficiente. La política ante resultados parciales y publicaciones fuera de orden debe quedar explícita antes de implementar.

**Ejemplo sintético.** El DAG A deja de producir una relación en su versión nueva, mientras B mantiene un aporte equivalente. Al actualizar A, debemos retirar solo su procedencia anterior y conservar la de B.

**Trade-off.** Centralizar esta responsabilidad simplifica a los consumidores, pero exige definir versiones, límites de publicación y recuperación. No selecciona una base de datos ni obliga a utilizar infraestructura distribuida.

**Cómo validar la propuesta.** Diseñar casos para repetición del mismo análisis, sustitución de una relación, contribuciones compartidas, fallo de análisis, baja explícita y llegada tardía de una versión anterior. Comprobar ausencia de duplicados, preservación de aportes ajenos y visibilidad de la versión consultada. Son verificaciones previstas, no pruebas ejecutadas ni sustitutos de los diez cambios comprometidos en el PDI.

**Evidencia esperada:** comparación de alternativas, contrato de actualización y pruebas del estado antes/después cuando exista implementación.

<a id="apsd-03"></a>
### APSD-03 — Separar los resultados de impacto de su explicación con IA

**Conceptos:** [abstracciones distintas por capa](libros/a-philosophy-of-software-design.md#tema-7), [responsabilidades coherentes](libros/a-philosophy-of-software-design.md#tema-9) y [comportamiento predecible](libros/a-philosophy-of-software-design.md#tema-18). Relación con RF-04 y RF-05.

**Problema que buscamos evitar.** Si descubrir dependencias, consultar impacto y redactar una explicación son una sola operación sin fronteras claras, resulta difícil distinguir un hallazgo sustentado de una inferencia de IA y comprobar qué cambió cuando varía el modelo.

**Aplicación propuesta.** La consulta produciría un resultado estructurado con cambio solicitado, versión consultada, caminos de dependencia, evidencia y límites. La explicación recibiría ese resultado para describirlo y sugerir verificaciones. No modificaría el catálogo ni elevaría una inferencia a relación verificada.

Ambas responsabilidades pueden vivir en el mismo proceso. La separación se justifica por sus contratos y razones de cambio, no por el número de clases o servicios.

**Ejemplo sintético.** A produce una tabla con la columna total; B consume total y C solo utiliza un identificador de esa tabla. Una dependencia de tabla no basta para afirmar impacto de valor en todas las columnas de C. La explicación debe conservar lo que demuestra el camino de columnas y señalar las condiciones o referencias que el análisis no haya resuelto.

**Trade-off.** Mantener un resultado estructurado y comprobar la explicación añade trabajo, pero permite probar la consulta sin invocar IA y cambiar la generación de texto sin cambiar las relaciones descubiertas. La separación por sí sola no impide afirmaciones incorrectas: hay que comprobar correspondencia entre afirmaciones, referencias y límites.

**Cómo validar la propuesta.** Usar un resultado sintético conocido y revisar que toda afirmación presentada como verificada tenga sustento, que las inferencias permanezcan identificadas y que el catálogo no cambie al generar texto. Ante fallo de la IA, conservar accesible el resultado estructurado y señalar que la explicación no se generó. Ese comportamiento no elimina el compromiso del PDI de demostrar el aporte de IA.

**Evidencia esperada:** contrato consulta-explicación, casos de contradicción o evidencia insuficiente y registro de revisión de explicaciones cuando exista implementación. No hay ejecuciones ni métricas acreditadas.

### Orden de trabajo sugerido

Empezar por APSD-01, porque el resultado del análisis condiciona las otras dos aplicaciones. Después concretar APSD-02 para definir qué versión y cobertura consultamos; finalmente delimitar APSD-03 sobre ese conocimiento. Podemos contrastar contratos con ejemplos sintéticos antes de disponer del parser, pero su integración requiere inspeccionar las capacidades reales.

La siguiente acción propuesta es redactar entradas, salidas e invariantes de APSD-01 usando un DAG sintético representativo. Si se acepta una decisión arquitectónica, registrar su fundamento y consecuencias en un ADR. Este documento conserva las aplicaciones como propuestas hasta entonces.
