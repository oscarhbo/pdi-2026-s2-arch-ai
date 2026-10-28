# Problema y contexto

Lectura 1 de 5 · [Índice del caso de negocio](README.md)

[← Portada del repositorio](../README.md) · [Siguiente: alcance y exclusiones →](alcance-y-exclusiones.md)

**Estado: En revisión. Versión 0.1, 28/10/2026.** Caso de negocio desarrollado para H1-01. Listo para contrastar sus supuestos; no implica aceptación del MVP ni beneficios comprobados. Fuentes: [PDI vigente](../00-pdi/Objetivos_PDI_2026.md) y contexto aportado por Óscar en la sesión del 28/10 (su archivo histórico ya no está disponible en el repositorio). Entorno declarado, aún sin contrastar con artefactos. No representa un encargo comercial de un cliente real.

## Resumen del caso de negocio

Proponemos evaluar una herramienta que transforme los artefactos de los pipelines en conocimiento trazable sobre sus dependencias. Un ingeniero podrá investigar el origen de un dato y consultar qué procesos o columnas requieren revisión ante un cambio, con referencias al código que sustenta la respuesta.

La oportunidad es reutilizar el conocimiento obtenido al analizar cada DAG, en vez de reconstruirlo en cada investigación. El valor depende de que ese conocimiento pueda mantenerse vigente y de que la herramienta declare sus límites. Una lista rápida pero incompleta, obsoleta o sin evidencia puede aumentar el riesgo de una decisión equivocada.

La recomendación de este borrador es avanzar con una demostración acotada, individual y sintética que permita comprobar utilidad y costo de mantenimiento. La decisión que buscamos sustentar es si merece la pena extenderla a un piloto posterior; no justificar todavía una plataforma empresarial de gobierno ni aprobar cambios productivos.

El documento se complementa con [alcance funcional](alcance-y-exclusiones.md), [beneficios y criterios de decisión](beneficios-y-criterios-de-exito.md), [esfuerzo y costo](costos-y-esfuerzo.md) y [propuesta inicial de piloto](propuesta-de-piloto.md). Cada uno mantiene un contenido distinto para evitar duplicar acuerdos.

## Problema principal y usuario

Un ingeniero de datos necesita descubrir de dónde proceden tablas y columnas, qué procesos las generan y quién las consume. Trabaja en un entorno con múltiples pipelines mantenidos por equipos distribuidos, por ejemplo entre México e India, donde el conocimiento de dependencias está disperso entre desarrolladores, DAGs y configuraciones.

La falta de trazabilidad obliga a investigar manualmente numerosos artefactos. Dificulta planificar cambios y aumenta la dependencia del conocimiento individual. El análisis de impacto es una capacidad derivada de descubrir y mantener ese linaje: para consultar consumidores entre DAGs, primero hay que conocer sus relaciones.

No se han medido tiempos de investigación, incidentes ni ahorros. Los permisos forman parte del contexto de gobierno, pero no se ha acordado incluir su descubrimiento o gestión en el prototipo.

## Personas y decisiones que necesitan tomar

Los siguientes roles describen usuarios potenciales del caso simulado; no son participantes identificados ni entrevistados.

| Rol | Situación y necesidad | Decisión que la solución ayudaría a fundamentar |
| --- | --- | --- |
| Ingeniero de datos, usuario principal | Recibe una solicitud para renombrar, eliminar o cambiar el tipo de una columna; debe investigar consumidores. | Qué artefactos revisar, qué pruebas preparar y qué información falta antes de integrar el cambio. |
| Responsable de un pipeline, usuario secundario | Incorpora o modifica un DAG y necesita conocer sus entradas, salidas y dependencias. | Qué conocimiento registrar o actualizar para que otros puedan investigar el proceso. |
| Líder técnico o responsable de plataforma, patrocinador hipotético | Necesita valorar esfuerzo, límites y riesgo de una modificación o de adoptar la herramienta. | Si la evidencia es suficiente para continuar la revisión y si el piloto merece inversión adicional. |
| Consumidor de datos, beneficiario indirecto | Depende de resultados producidos por pipelines posteriores. | Qué validaciones funcionales necesita cuando un cambio puede alterar esos resultados. |

El prototipo identificará consumidores técnicos dentro de los artefactos analizados. No promete identificar personas, equipos propietarios ni consumidores externos cuya información no haya sido proporcionada.

## Investigación actual y cambio propuesto

Proceso actual reconstruido a partir del relato, todavía sin observación ni tiempos medidos:

1. Recibir el cambio y precisar objeto, columna y operación.
2. Buscar referencias en DAGs, configuraciones y SQL; seguir alias, tablas intermedias y consumidores.
3. Consultar documentación o conocimiento de otros desarrolladores cuando falte contexto.
4. Preparar una lista de candidatos y verificaciones, con el riesgo de dejar supuestos sin documentar.
5. Repetir o actualizar parte de esa investigación en cambios posteriores.

Proceso propuesto: incorporar artefactos identificados por versión, consultar sus relaciones y revisar un reporte sustentado. El ingeniero conserva la responsabilidad de validar el cambio; el sistema le entrega una base de investigación reutilizable. La actualización del conocimiento pasa a formar parte del trabajo, por lo que debe medirse y no tratarse como gratuita.

| Problema observado en el relato | Consecuencia posible | Evidencia que falta recoger |
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

**B. Análisis de impacto:** consultar el conocimiento disponible ante un cambio, recorrer relaciones directas e indirectas y explicar candidatos afectados, evidencia, límites y verificaciones recomendadas. La cobertura depende de los DAGs y versiones analizados; ausencia de relación registrada no implica ausencia de dependencia.

El linaje de tablas y columnas tiene prioridad 1 desde el primer prototipo. Spark SQL es el dialecto de referencia; versión y patrones soportados pendientes. El PDI vigente dice desarrollar el parser en C11 y revisar el existente en Plan de Carrera C23: definir reutilización, adaptación o reemplazo requiere inspección y una decisión explícita. Ver [alcance vigente](alcance-y-exclusiones.md).

## Aporte esperado de la IA

El valor principal se apoya en relaciones trazables. La IA podría traducirlas en una explicación del cambio, ordenar las verificaciones recomendadas y expresar qué falta conocer. Eso será útil solo si facilita entender el reporte sin inventar relaciones ni ocultar incertidumbre.

Proponemos evaluar la explicación sobre los mismos resultados estructurados: comprobar citas, afirmaciones y claridad para responder qué revisar y por qué. Si no añade utilidad, reducir su función y documentarlo; el requisito de incorporar IA no justifica convertirla en autoridad sobre el linaje. Agentes, RAG y conversación no son decisiones tomadas.

## Alternativas de negocio

Comparación conceptual para este caso, sin evaluación de productos comerciales ni afirmaciones sobre capacidades de proveedores.

| Alternativa | Ventaja potencial | Costo o límite a comprobar | Cuándo tendría sentido |
| --- | --- | --- | --- |
| Mantener investigación manual | Poco trabajo inicial y flexibilidad ante casos excepcionales. | Esfuerzo recurrente, reproducibilidad y omisiones. | Cambios poco frecuentes o artefactos muy variables. |
| Documentar dependencias con un proceso manual estructurado | Hace explícitas relaciones y evidencia con menor desarrollo de software. | Disciplina de actualización y costo de revisión. | Corpus pequeño y estable; también sirve como comparación del valor incremental de automatizar. |
| Prototipo de descubrimiento incremental con explicación IA | Reutilización de conocimiento y consulta entre DAGs. | Desarrollo, mantenimiento, cobertura e incertidumbre. | Patrones repetidos y suficiente reutilización para compensar el costo inicial. |
| Evaluar capacidades existentes de gobierno/linaje | Podría evitar construir funciones ya disponibles en la organización. | Inventario, acceso, cobertura de artefactos y esfuerzo de integración desconocidos. | Si existen herramientas accesibles con cobertura adecuada; no se ha comprobado. |

Recomendación propuesta: comprobar la tercera opción con un corpus acotado y una comparación manual reproducible, manteniendo abierta la posibilidad de integrar una capacidad existente después. No presentar el prototipo como sustituto de herramientas empresariales sin haberlas evaluado.

## Supuestos y riesgos del caso

| Supuesto | Si resulta falso | Respuesta propuesta |
| --- | --- | --- |
| Los artefactos disponibles contienen suficiente información para el subconjunto elegido. | Relaciones ambiguas o imposibles de resolver. | Pedir esquemas/contexto o declarar no resuelto; reducir patrones, conservando prioridad de columnas. |
| El conocimiento se reutilizará en más de una consulta. | La preparación podría costar más que investigar manualmente. | Medir costo inicial y recurrente; no defender ahorro si no existe. |
| Se podrán identificar versiones y actualizaciones por DAG. | El catálogo puede inducir decisiones con evidencia obsoleta. | Evaluar altas/cambios/bajas antes de recomendar extensión. |
| El subconjunto cabe en la capacidad individual. | Retraso del PDI o entrega superficial. | Estimar después de inspeccionar parser; ajustar patrones sin ocultar fechas ni degradar prioridades unilateralmente. |
| Un corpus sintético sirve para probar mecánica y límites. | Resultados poco transferibles al entorno real. | Reportar esa limitación; exigir validación adicional antes de extrapolar beneficios. |

Los riesgos técnicos detallados permanecen en [riesgos y limitaciones](../02-arquitectura/riesgos-y-limitaciones.md). El riesgo comercial principal es confundir una demostración de relaciones conocidas con una garantía de cobertura empresarial.

## Valor por validar y continuidad

Hipótesis: facilitar la investigación, reducir omisiones, mantener documentación trazable y mejorar la planificación de cambios. Mantener los compromisos de diez cambios conocidos y cinco comparaciones manuales sin prometer ahorro ni cobertura universal.

Este escenario reemplaza como hilo principal el borrador del 25/10 sobre ventas.total y vistas diarias/mensuales. El PDI original sigue incluyendo tablas, vistas y scripts; explicitar su correspondencia con el corpus de DAGs sin eliminar vistas silenciosamente.

## Decisión que debe habilitar esta fase

Este borrador permite revisar si el problema, usuario, recorrido y evidencia propuesta justifican continuar al diseño. La recomendación está condicionada a delimitar patrones, disponibilidad de artefactos y capacidad. Los [criterios propuestos](beneficios-y-criterios-de-exito.md) permiten decidir continuar, acotar o detener la extensión sin declarar éxito por haber construido una demo.

El hito de diseño sigue al 15/11/2026. Se ha desarrollado el caso documental; su aprobación, la arquitectura y el resultado del piloto son estados diferentes. Siguiente tarea: revisar los criterios propuestos y, con el parser y artefactos disponibles, concretar cobertura y estimación. No se requiere validación de un cliente real para cerrar el ejercicio del PDI.

---

[← Portada del repositorio](../README.md) · [Índice del caso de negocio](README.md) · [Siguiente: alcance y exclusiones →](alcance-y-exclusiones.md)
