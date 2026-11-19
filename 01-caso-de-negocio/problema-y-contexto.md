# Problema y contexto — Lean Business Case

Lectura 1 de 5 · [Índice del caso de negocio](README.md)

[← Portada del repositorio](../README.md) · [Siguiente: alcance y exclusiones →](alcance-y-exclusiones.md)

**Estado: Completado para H1-01. Versión 1.0, 15/11/2026.** Caso de negocio con usuarios, problema, valor, alcance funcional y entrega del MVP definidos. Referencia: [PDI vigente](../00-pdi/Objetivos_PDI_2026.md). Los beneficios se comprobarán durante la evaluación; el cierre documental no acredita implementación ni resultados comerciales.

Revisión editorial: **19/11/2026**. Se conserva el cierre y la versión de H1-01; esta revisión mejora la justificación de negocio y distingue su alcance original de la evolución posterior de arquitectura.

## Resumen del caso de negocio

El proyecto transforma los artefactos de los pipelines en conocimiento trazable sobre sus dependencias. Ingenieros y analistas podrán consultar el origen y los consumidores de tablas y columnas, explorar caminos upstream y downstream y orientar investigaciones o cambios con referencias al código.

La oportunidad es reutilizar el conocimiento obtenido al analizar cada DAG, en vez de reconstruirlo en cada investigación. El valor depende de que ese conocimiento pueda mantenerse vigente y de que la herramienta declare sus límites. Una lista rápida pero incompleta, obsoleta o sin evidencia puede aumentar el riesgo de una decisión equivocada.

La iniciativa del PDI plantea un prototipo acotado con artefactos sintéticos para comprobar si ese conocimiento reutilizable mejora la investigación. En el cierre de H1-01 del 15/11, un reporte era suficiente y la web era opcional. Posteriormente, [SRC-019](../02-arquitectura/README.md#SRC-019) incorporó la web como requisito de la propuesta de arquitectura. Esta evolución debe incluirse en la estimación, sin reescribir el cierre original ni dar por aprobados los ADR. La evaluación posterior determinará si merece la pena extender la solución a un piloto.

La justificación actual es suficiente para desarrollar y revisar el diseño: existe una necesidad concreta, usuarios identificados y una forma acotada de contrastar el valor. Todavía no demuestra rentabilidad ni viabilidad de ejecución en la capacidad disponible. H1-02 ya cuenta con una propuesta en revisión; el siguiente paso es revisar ese diseño y completar la estimación H1-03, no iniciar otro diseño desde cero ni autorizar construcción o gasto mediante este documento.

El documento se complementa con [alcance funcional](alcance-y-exclusiones.md), [beneficios y criterios de decisión](beneficios-y-criterios-de-exito.md), [esfuerzo y costo](costos-y-esfuerzo.md) y [propuesta inicial de piloto](propuesta-de-piloto.md). Cada uno mantiene un contenido distinto para evitar duplicar acuerdos.

## Problema principal y usuario

En una plataforma con numerosos procesos, nuevas tablas y DAGs se acumulan sin una visión accesible de quién produce cada dato y quién lo consume. Consultar una tabla no permite conocer por sí mismo qué proceso la alimenta ni la cadena de transformaciones previa. Si surge un problema, reconstruir esa cadena exige recorrer código, configuraciones y repositorios, saltando de un proceso a otro.

La búsqueda puede ocupar varias horas o una jornada y aun así dejar dependencias sin identificar. Esta es una referencia cualitativa de la experiencia de trabajo, no una medición formal ni un promedio. El resultado depende del acceso a los repositorios, de la destreza para buscar y del conocimiento de las personas. Cuando ese conocimiento está incompleto o desactualizado, aumenta el riesgo de omisiones y conclusiones equivocadas.

El problema crece con la cantidad de procesos y la profundidad de las cadenas. Repetir la búsqueda manual es costoso y difícil de mantener consistente. En el flujo actual no hay una vista de linaje disponible que resuelva esta necesidad; esto no implica que no existan herramientas de linaje en el mercado ni constituye una comparación de productos.

## Objetivos de la iniciativa

1. Hacer visibles las dependencias de tablas y columnas que podrían omitirse al investigar código disperso.
2. Reducir la reconstrucción repetida de origen y consumidores mediante conocimiento reutilizable y vigente.
3. Facilitar investigaciones reproducibles sin depender exclusivamente de la memoria individual.
4. Demostrar en el PDI capacidad de análisis de negocio, diseño y evaluación de una solución de IA, distinguiendo utilidad comprobada de expectativas comerciales.

Los tres primeros objetivos mantienen el orden de valor definido para H1-01. Se contrastarán en H2 con diez cambios y en H3 con cinco comparaciones manuales; esas cantidades son compromisos de evaluación, no metas de ahorro o precisión.

## Usuarios y situaciones de uso

| Usuario | Situación | Ayuda que recibe |
| --- | --- | --- |
| Ingeniero o analista | Necesita entender una tabla o columna sin proponer un cambio. | Origen, productores, consumidores y caminos de linaje. |
| Ingeniero o analista | Investiga un problema conocido en un DAG. | Dependencias upstream para orientar la búsqueda y downstream para localizar consumidores que requieren revisión. |
| Ingeniero que modifica un proceso | Renombra, elimina o cambia el tipo de una columna. | Dependencias directas y transitivas, evidencia y posibles caminos de afectación. |
| Persona que se incorpora o retoma un proceso | No conoce el código o su documentación no está actualizada. | Un punto de entrada verificable para investigar sin depender exclusivamente de la memoria del equipo. |

El diagnóstico se limita al linaje. No consulta logs ni estados de ejecución y no determina automáticamente la causa de un fallo. Los consumidores identificados son objetos y procesos técnicos; no se infieren personas responsables sin información explícita.

## Investigación actual y cambio propuesto

Proceso actual, sin mediciones formales:

1. Recibir una consulta de origen, una investigación de un problema o una solicitud de cambio; identificar DAG, tabla o columna.
2. Buscar referencias en DAGs, configuraciones y SQL; seguir alias, tablas intermedias y consumidores.
3. Consultar documentación o conocimiento de otros desarrolladores cuando falte contexto.
4. Preparar una lista de candidatos y verificaciones, con el riesgo de dejar supuestos sin documentar.
5. Repetir o actualizar parte de esa investigación en cambios posteriores.

Proceso propuesto: incorporar artefactos identificados por versión, consultar sus relaciones y revisar un reporte sustentado. El ingeniero conserva la responsabilidad de validar el cambio; el sistema le entrega una base de investigación reutilizable. La actualización del conocimiento pasa a formar parte del trabajo, por lo que debe medirse y no tratarse como gratuita.

| Problema actual | Consecuencia posible | Evidencia que falta recoger |
| --- | --- | --- |
| Dependencias dispersas entre archivos y personas. | Repetición de investigación y coordinación. | Tiempo activo, archivos revisados y fuentes consultadas por caso. |
| Origen o transformación de una columna poco visible. | Omisiones o revisión excesiva de consumidores. | Comparación con relaciones conocidas y falsas alarmas. |
| Documentación que puede quedar desactualizada. | Uso de relaciones que ya no corresponden al código. | Pruebas de modificación/baja y versión visible en el reporte. |
| Dependencia del conocimiento individual. | Dificultad para explicar o reproducir una investigación. | Capacidad de volver desde una conclusión a su evidencia. |

No se atribuyen incidentes, pérdidas económicas ni retrasos reales a estas situaciones sin mediciones.

## Entorno de referencia declarado

Data Lake en GCP: datos Parquet en GCS, tablas principalmente externas con metadatos en Hive Metastore; procesamiento con Spark SQL sobre Dataproc; orquestación mediante Airflow. Un framework utiliza YAML con SQL embebido y una configuración de orden de tareas. Los procesos pueden ingerir archivos o bases de datos, leer tablas, transformar mediante STG y alimentar targets.

Esto describe el entorno de origen, no una arquitectura de despliegue aprobada para el prototipo. No necesitamos aprovisionarlo para definir el caso. Las convenciones del framework, su parametrización y la identificación de targets deben verificarse con ejemplos; no asumir que la última tarea siempre escribe un único target.

## Escenario sintético de trabajo

| DAG conceptual | Consume | Produce |
| --- | --- | --- |
| DAG 1: ventas | raw.ventas y raw.clientes | curated.ventas, posiblemente mediante STG. |
| DAG 2: indicadores | curated.ventas | mart.indicadores. |
| DAG 3: reportes | curated.ventas | mart.reporte_comercial. |

Pregunta de impacto: «Necesito eliminar raw.ventas.precio, ¿qué podría afectar?». En el ejemplo, curated.ventas.total se calcula a partir de precio y cantidad. La dependencia del valor se distingue de las columnas usadas para JOIN, filtros o agrupación, que pueden influir en qué filas participan en el resultado.

Los DAGs 2 y 3 son consumidores de la tabla; eso no demuestra que sus columnas dependan de total. Habrá que resolver sus consultas antes de afirmar una propagación por columna. Una dependencia respaldada por código tampoco demuestra un fallo en ejecución. El mapeo de posiciones de un INSERT a columnas destino requiere información suficiente sobre su esquema; no se deduce únicamente del alias proyectado.

Los nombres anteriores son ilustrativos. No hay DAGs, YAML, esquemas ni SQL del corpus implementados en el repositorio.

### Recorrido ilustrativo de la decisión

El ingeniero plantea eliminar `raw.ventas.precio`. El reporte debería indicar que, en los artefactos analizados, esa columna participa en el cálculo de `curated.ventas.total`, mostrar los pasos intermedios disponibles y enlazar el fragmento que lo demuestra. Si la referencia permanece después de eliminar la columna, la transformación requiere revisión; no se afirma haber ejecutado ni observado una rotura.

Como extensión sintética propuesta, DAG 2 calcularía un indicador a partir de `total`, mientras que DAG 3 proyectaría únicamente identificadores y región, sin utilizar `total` en expresiones ni condiciones. Con esas definiciones verificadas, DAG 2 tendría una dependencia de valor transitiva; DAG 3 sería consumidor de la tabla, pero no tendría esa dependencia de valor en el corpus. El reporte no debe equiparar ambos resultados. Tampoco puede certificar que DAG 3 seguirá ejecutándose: aún podría depender de la ejecución satisfactoria del productor.

Si falta el YAML de DAG 2, el resultado debe decir que su impacto por columna es desconocido. Si mañana se incorpora ese DAG, la consulta debe aprovechar el conocimiento ampliado. Si cambia, las relaciones antiguas no deben presentarse como actuales. Esta secuencia muestra por qué el mantenimiento del linaje es parte del caso de negocio.

## Dos capacidades relacionadas

**A. Descubrimiento incremental:** analizar los artefactos asociados a un DAG, extraer SQL y descubrir relaciones entre tablas y columnas, conservando STG y evidencia. Incorporar progresivamente ese conocimiento y considerar cambios o bajas de DAGs para no conservar relaciones obsoletas.

**B. Consulta de linaje y análisis de impacto:** consultar origen y consumidores sin exigir un cambio como entrada; recorrer relaciones upstream y downstream para orientar investigaciones. Ante un cambio, explicar candidatos afectados, evidencia, límites y verificaciones recomendadas. La cobertura depende de los DAGs y versiones analizados; ausencia de relación registrada no implica ausencia de dependencia.

El linaje de tablas y columnas tiene prioridad 1 desde el primer prototipo. Spark SQL es el dialecto de referencia. El PDI distingue desarrollar el parser en C11 y revisar el existente en Plan de Carrera C23. La inspección previa ya está [documentada](../00-pdi/fuentes/arquitectura/revision-parser-y-dags.md); no equivale a validar el futuro prototipo. La cobertura y la adaptación propuestas se revisan en [arquitectura](../02-arquitectura/docs/revision-propuesta.md), sin convertirlas aquí en decisiones aceptadas. El [alcance funcional](alcance-y-exclusiones.md) conserva la referencia del cierre de H1-01.

## Aporte esperado de la IA

El valor principal se apoya en relaciones trazables. La IA las traduce en explicaciones de las dependencias, posibles afectaciones y caminos de revisión, indicando qué falta conocer. Eso será útil solo si facilita entender el reporte sin inventar relaciones ni ocultar incertidumbre.

La evaluación de la explicación se realiza sobre los mismos resultados estructurados: comprobar citas, afirmaciones y claridad para responder qué revisar y por qué. Si no añade utilidad, reducir su función y documentarlo; el requisito de incorporar IA no justifica convertirla en autoridad sobre el linaje. Agentes, RAG y conversación no son decisiones tomadas.

## Alternativas de negocio

Comparación conceptual para este caso, sin evaluación de productos comerciales ni afirmaciones sobre capacidades de proveedores.

| Alternativa | Ventaja potencial | Costo o límite a comprobar | Cuándo tendría sentido |
| --- | --- | --- | --- |
| Mantener investigación manual | Poco trabajo inicial y flexibilidad ante casos excepcionales. | Esfuerzo recurrente, reproducibilidad y omisiones. | Cambios poco frecuentes o artefactos muy variables. |
| Documentar dependencias con un proceso manual estructurado | Hace explícitas relaciones y evidencia con menor desarrollo de software. | Disciplina de actualización y costo de revisión. | Corpus pequeño y estable; también sirve como comparación del valor incremental de automatizar. |
| Prototipo de descubrimiento incremental con explicación IA | Reutilización de conocimiento y consulta entre DAGs. | Desarrollo, mantenimiento, cobertura e incertidumbre. | Patrones repetidos y suficiente reutilización para compensar el costo inicial. |
| Evaluar capacidades existentes de gobierno/linaje | Podría evitar construir funciones ya disponibles en la organización. | Inventario, acceso, cobertura de artefactos y esfuerzo de integración desconocidos. | Si existen herramientas accesibles con cobertura adecuada; no se ha comprobado. |

La opción elegida es el prototipo de descubrimiento incremental con explicación IA. Frente a una búsqueda aislada, permite conservar lo descubierto y consultarlo de nuevo; frente a documentación manual, vincula las relaciones con los artefactos que las sustentan y permite mantener sus versiones. Su ventaja depende de comprobar cobertura, actualización y esfuerzo total. No se descarta integrar herramientas existentes posteriormente, pero su disponibilidad y adecuación no están verificadas.

## Beneficios esperados e indicadores de éxito

El orden de prioridad es: identificar dependencias omitidas, reducir tiempo de investigación y reducir dependencia del conocimiento individual.

Los recorridos de origen y consumidores descritos en las situaciones de uso buscan dar un punto de partida para investigar, compartir contexto y evitar búsquedas repetidas. Su valor depende de conservar la evidencia y declarar cuándo faltan artefactos; no basta con mostrar una cadena de relaciones.

Estas ventajas son expectativas de valor, no ahorros ni incidentes evitados ya demostrados. El reporte no sustituye permisos de acceso ni descubre código que no se le proporciona.

| Valor por contrastar | Indicador y evidencia prevista | Límite de interpretación |
| --- | --- | --- |
| Menos dependencias omitidas (B-02). | Relaciones esperadas, omitidas y falsas en los diez cambios, por tabla y columna. | Un corpus sintético no acredita cobertura empresarial. |
| Menor esfuerzo recurrente (B-01). | Tiempo activo en cinco comparaciones manuales; incorporación, actualización y revisión separados. | Una consulta rápida no demuestra ahorro neto. |
| Conocimiento vigente (B-03). | Relaciones conservadas, actualizadas o retiradas tras cambios y bajas. | La vigencia depende de los artefactos y versiones proporcionados. |
| Explicación útil y reproducible (B-04/B-05). | Correspondencia entre afirmaciones y evidencia, claridad e información adicional requerida. | La autoevaluación no equivale a validación independiente de usuarios. |

Las hipótesis B-01 a B-05 y los criterios propuestos CA-01 a CA-06 se mantienen en [beneficios y criterios de éxito](beneficios-y-criterios-de-exito.md). No hay línea base medida ni meta de ahorro aceptada. Se podrá recomendar ampliar, acotar y repetir o no extender todavía según trazabilidad, calidad y esfuerzo total, incluso si los resultados son desfavorables.

## Esfuerzo, costos y viabilidad a alto nivel

El PDI registra 4 h/semana de práctica y 2 h/semana de revisión para el primer objetivo; no confirma una capacidad global para los posteriores. USD 20 es un presupuesto API propuesto, no autorización de consumo ni costo total. No hay estimación cerrada de construcción, tarifa interna ni retorno económico calculado.

El esfuerzo incluye delimitar patrones, adaptar el análisis, incorporar artefactos, mantener relaciones, explicar resultados, evaluar y presentar. La web incorporada después de H1-01 también debe estimarse. La inspección disponible aporta una base para H1-03; falta convertir sus hallazgos en horas por trabajo y contrastarlas con la disponibilidad restante.

La comparación debe incluir preparación inicial, mantenimiento y revisión de resultados, además del tiempo de consulta. El [modelo de costos y esfuerzo](costos-y-esfuerzo.md) conserva el desglose y el ejemplo aritmético, que no es un pronóstico. La viabilidad permanece por confirmar: el ejercicio sintético permite evaluar un subconjunto, pero no garantiza que todo el alcance quepa en el tiempo disponible.

## Entrega mínima y condiciones de utilidad

El MVP entrega un reporte con el objeto consultado, origen y consumidores de tablas y columnas, caminos entre DAGs y pasos intermedios relevantes. Cada relación permite localizar DAG, archivo, fragmento SQL y versión. Se incluyen artefactos analizados, información incompleta y referencias no resueltas.

La explicación IA complementa ese núcleo con posibles afectaciones y recomendaciones sustentadas. No encontrar una dependencia no equivale a asegurar que un objeto no será afectado fuera de la cobertura analizada. La utilidad de negocio mínima es proporcionar un punto de partida trazable para investigar. El reporte describe la entrega original de H1-01; el alcance posterior incorpora web obligatoria, como se precisa en el resumen.

## Supuestos y riesgos del caso

| Supuesto | Si resulta falso | Respuesta propuesta |
| --- | --- | --- |
| Los artefactos disponibles contienen suficiente información para el subconjunto elegido. | Relaciones ambiguas o imposibles de resolver. | Pedir esquemas/contexto o declarar no resuelto; reducir patrones, conservando prioridad de columnas. |
| El conocimiento se reutilizará en más de una consulta. | La preparación podría costar más que investigar manualmente. | Medir costo inicial y recurrente; no defender ahorro si no existe. |
| Se podrán identificar versiones y actualizaciones por DAG. | El catálogo puede inducir decisiones con evidencia obsoleta. | Evaluar altas/cambios/bajas antes de recomendar extensión. |
| El subconjunto cabe en la capacidad individual. | Retraso del PDI o entrega superficial. | Estimar después de inspeccionar parser; ajustar patrones sin ocultar fechas ni degradar prioridades unilateralmente. |
| Un corpus sintético sirve para probar mecánica y límites. | Resultados poco transferibles al entorno real. | Reportar esa limitación; exigir validación adicional antes de extrapolar beneficios. |

Los riesgos técnicos detallados permanecen en [riesgos y limitaciones](../02-arquitectura/docs/sad.md#11-riesgos-y-deuda-técnica). El riesgo comercial principal es confundir una demostración de relaciones conocidas con una garantía de cobertura empresarial.

## Preguntas abiertas y continuidad

Hipótesis: facilitar la investigación, reducir omisiones, mantener documentación trazable y mejorar la planificación de cambios. Mantener los compromisos de diez cambios conocidos y cinco comparaciones manuales sin prometer ahorro ni cobertura universal.

- ¿Qué patrones y esfuerzo caben en la disponibilidad restante, incluida la web? Resolver en H1-03 utilizando la inspección y el diseño propuesto.
- ¿Cuál es el tiempo y frecuencia reales de investigación y actualización? Medir antes de extrapolar ahorro o retorno comercial.
- ¿La explicación IA aporta utilidad suficiente frente a revisar el mismo reporte sin ella? Contrastar evidencia, claridad y esfuerzo de revisión.
- ¿Existen herramientas accesibles que cubran los artefactos del caso? Su disponibilidad e integración no están verificadas; conservar esa alternativa para una eventual extensión.

Los documentos de apoyo fechados el 28/10 y el 15/11 conservan pendientes históricos sobre inspección y web opcional. Para el estado posterior prevalecen la [ficha de arquitectura](../02-arquitectura/README.md) y su [propuesta en revisión](../02-arquitectura/docs/revision-propuesta.md). Esta distinción no acepta sus ADR ni modifica los compromisos originales.

Este escenario reemplaza como hilo principal el borrador del 25/10 sobre ventas.total y vistas diarias/mensuales. El PDI original sigue incluyendo tablas, vistas y scripts; explicitar su correspondencia con el corpus de DAGs sin eliminar vistas silenciosamente.

## Cierre del caso de negocio

H1-01 queda completado: problema, usuarios, situaciones de uso, prioridades de valor, alternativas y límites funcionales definidos. El MVP se justifica por hacer visible y reutilizable un conocimiento hoy disperso y costoso de reconstruir, empezando por reducir las dependencias que pasan inadvertidas.

El cierre no exige demostrar todavía ahorro ni retorno económico. Los [beneficios](beneficios-y-criterios-de-exito.md) se contrastarán con resultados; el [modelo de costos](costos-y-esfuerzo.md) incluye construcción, actualización y verificación para no confundir una consulta rápida con ahorro neto. H1-02 mantiene la definición técnica de cobertura y arquitectura; H1-03 mantiene la estimación y capacidad. Las fechas del PDI no cambian.

---

[← Portada del repositorio](../README.md) · [Índice del caso de negocio](README.md) · [Siguiente: alcance y exclusiones →](alcance-y-exclusiones.md)
