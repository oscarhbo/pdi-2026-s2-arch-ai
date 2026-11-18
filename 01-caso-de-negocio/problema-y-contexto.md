# Problema y contexto

Lectura 1 de 5 · [Índice del caso de negocio](README.md)

[← Portada del repositorio](../README.md) · [Siguiente: alcance y exclusiones →](alcance-y-exclusiones.md)

**Estado: Completado para H1-01. Versión 1.0, 15/11/2026.** Caso de negocio con usuarios, problema, valor, alcance funcional y entrega del MVP definidos. Referencia: [PDI vigente](../00-pdi/Objetivos_PDI_2026.md). Los beneficios se comprobarán durante la evaluación; el cierre documental no acredita implementación ni resultados comerciales.

## Resumen del caso de negocio

El proyecto transforma los artefactos de los pipelines en conocimiento trazable sobre sus dependencias. Ingenieros y analistas podrán consultar el origen y los consumidores de tablas y columnas, explorar caminos upstream y downstream y orientar investigaciones o cambios con referencias al código.

La oportunidad es reutilizar el conocimiento obtenido al analizar cada DAG, en vez de reconstruirlo en cada investigación. El valor depende de que ese conocimiento pueda mantenerse vigente y de que la herramienta declare sus límites. Una lista rápida pero incompleta, obsoleta o sin evidencia puede aumentar el riesgo de una decisión equivocada.

La decisión es construir un MVP acotado con artefactos sintéticos y un reporte como entrega suficiente. La interfaz web con visualización y descarga del reporte es una ampliación opcional si hay tiempo, sin condicionar el MVP. La evaluación posterior determinará si merece la pena extender la solución a un piloto.

El documento se complementa con [alcance funcional](alcance-y-exclusiones.md), [beneficios y criterios de decisión](beneficios-y-criterios-de-exito.md), [esfuerzo y costo](costos-y-esfuerzo.md) y [propuesta inicial de piloto](propuesta-de-piloto.md). Cada uno mantiene un contenido distinto para evitar duplicar acuerdos.

## Problema principal y usuario

En una plataforma con numerosos procesos, nuevas tablas y DAGs se acumulan sin una visión accesible de quién produce cada dato y quién lo consume. Consultar una tabla no permite conocer por sí mismo qué proceso la alimenta ni la cadena de transformaciones previa. Si surge un problema, reconstruir esa cadena exige recorrer código, configuraciones y repositorios, saltando de un proceso a otro.

La búsqueda puede ocupar varias horas o una jornada y aun así dejar dependencias sin identificar. Esta es una referencia cualitativa de la experiencia de trabajo, no una medición formal ni un promedio. El resultado depende del acceso a los repositorios, de la destreza para buscar y del conocimiento de las personas. Cuando ese conocimiento está incompleto o desactualizado, aumenta el riesgo de omisiones y conclusiones equivocadas.

El problema crece con la cantidad de procesos y la profundidad de las cadenas. Repetir la búsqueda manual es costoso y difícil de mantener consistente. En el flujo actual no hay una vista de linaje disponible que resuelva esta necesidad; esto no implica que no existan herramientas de linaje en el mercado ni constituye una comparación de productos.

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

El linaje de tablas y columnas tiene prioridad 1 desde el primer prototipo. Spark SQL es el dialecto de referencia; versión y patrones soportados pendientes. El PDI vigente dice desarrollar el parser en C11 y revisar el existente en Plan de Carrera C23: definir reutilización, adaptación o reemplazo requiere inspección y una decisión explícita. Ver [alcance vigente](alcance-y-exclusiones.md).

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

## Ventajas y consecuencias para una plataforma con muchos procesos

El orden de prioridad es: identificar dependencias omitidas, reducir tiempo de investigación y reducir dependencia del conocimiento individual.

| Situación | Consecuencia posible si continúa | Ventaja buscada |
| --- | --- | --- |
| Procesos que se crean sin una visión compartida de sus relaciones. | Dependencias ocultas y dificultad para evaluar cambios. | Hacer consultables origen y consumidores dentro de la cobertura disponible. |
| Cadenas que atraviesan varios DAGs y pasos intermedios. | Búsquedas prolongadas y caminos que quedan sin recorrer. | Seguir relaciones directas y transitivas conservando STG y evidencia. |
| Una tabla se utiliza sin conocer qué la alimenta. | Investigar en componentes equivocados o no encontrar dónde comenzar. | Localizar productores y caminos upstream como punto de partida. |
| Un proceso presenta problemas. | Dificultad para identificar consumidores que conviene revisar. | Mostrar caminos downstream, sin afirmar que el fallo se propagó en ejecución. |
| Conocimiento concentrado en pocas personas. | Esperas, dificultades de incorporación y pérdida de contexto con cambios de equipo. | Conservar conocimiento verificable y reutilizable por ingenieros y analistas. |
| Documentación manual o memoria desactualizada. | Decisiones apoyadas en relaciones antiguas o incompletas. | Asociar relaciones con versiones y mostrar límites de vigencia. |
| Una investigación se repite. | Duplicación de búsqueda y coordinación. | Reutilizar el linaje acumulado y enfocar la revisión en lo relevante. |
| Faltan repositorios o artefactos. | Confundir una vista parcial con ausencia de dependencias. | Señalar referencias no resueltas y qué información falta. |

Estas ventajas son expectativas de valor, no ahorros ni incidentes evitados ya demostrados. El reporte no sustituye permisos de acceso ni descubre código que no se le proporciona.

## Entrega mínima y condiciones de utilidad

El MVP entrega un reporte con el objeto consultado, origen y consumidores de tablas y columnas, caminos entre DAGs y pasos intermedios relevantes. Cada relación permite localizar DAG, archivo, fragmento SQL y versión. Se incluyen artefactos analizados, información incompleta y referencias no resueltas.

La explicación IA complementa ese núcleo con posibles afectaciones y recomendaciones sustentadas. No encontrar una dependencia no equivale a asegurar que un objeto no será afectado fuera de la cobertura analizada. La utilidad mínima es proporcionar un punto de partida trazable para investigar, sin exigir una interfaz web.

## Supuestos y riesgos del caso

| Supuesto | Si resulta falso | Respuesta propuesta |
| --- | --- | --- |
| Los artefactos disponibles contienen suficiente información para el subconjunto elegido. | Relaciones ambiguas o imposibles de resolver. | Pedir esquemas/contexto o declarar no resuelto; reducir patrones, conservando prioridad de columnas. |
| El conocimiento se reutilizará en más de una consulta. | La preparación podría costar más que investigar manualmente. | Medir costo inicial y recurrente; no defender ahorro si no existe. |
| Se podrán identificar versiones y actualizaciones por DAG. | El catálogo puede inducir decisiones con evidencia obsoleta. | Evaluar altas/cambios/bajas antes de recomendar extensión. |
| El subconjunto cabe en la capacidad individual. | Retraso del PDI o entrega superficial. | Estimar después de inspeccionar parser; ajustar patrones sin ocultar fechas ni degradar prioridades unilateralmente. |
| Un corpus sintético sirve para probar mecánica y límites. | Resultados poco transferibles al entorno real. | Reportar esa limitación; exigir validación adicional antes de extrapolar beneficios. |

Los riesgos técnicos detallados permanecen en [riesgos y limitaciones](../02-arquitectura/docs/sad.md#11-riesgos-y-deuda-técnica). El riesgo comercial principal es confundir una demostración de relaciones conocidas con una garantía de cobertura empresarial.

## Valor por validar y continuidad

Hipótesis: facilitar la investigación, reducir omisiones, mantener documentación trazable y mejorar la planificación de cambios. Mantener los compromisos de diez cambios conocidos y cinco comparaciones manuales sin prometer ahorro ni cobertura universal.

Este escenario reemplaza como hilo principal el borrador del 25/10 sobre ventas.total y vistas diarias/mensuales. El PDI original sigue incluyendo tablas, vistas y scripts; explicitar su correspondencia con el corpus de DAGs sin eliminar vistas silenciosamente.

## Cierre del caso de negocio

H1-01 queda completado: problema, usuarios, situaciones de uso, prioridades de valor, alternativas y límites funcionales definidos. El MVP se justifica por hacer visible y reutilizable un conocimiento hoy disperso y costoso de reconstruir, empezando por reducir las dependencias que pasan inadvertidas.

El cierre no exige demostrar todavía ahorro ni retorno económico. Los [beneficios](beneficios-y-criterios-de-exito.md) se contrastarán con resultados; el [modelo de costos](costos-y-esfuerzo.md) incluye construcción, actualización y verificación para no confundir una consulta rápida con ahorro neto. H1-02 mantiene la definición técnica de cobertura y arquitectura; H1-03 mantiene la estimación y capacidad. Las fechas del PDI no cambian.

---

[← Portada del repositorio](../README.md) · [Índice del caso de negocio](README.md) · [Siguiente: alcance y exclusiones →](alcance-y-exclusiones.md)
