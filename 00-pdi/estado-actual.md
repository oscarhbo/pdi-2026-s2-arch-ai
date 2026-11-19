# Estado actual

Última actualización: **18/11/2026**, zona America/Mexico_City. Sesión más reciente: separación de evidencias y documentación principal de arquitectura.

## Base vigente para retomar

La [propuesta v1.0](../02-arquitectura/docs/revision-propuesta.md) queda integrada como borrador para revisión en este PDI. Conservo `02-arquitectura/README.md`, `docs/`, `adr/` y `diagrams/` como fuentes canónicas, conforme a [AGENTS.md](../AGENTS.md). La copia en ArchFlow queda como antecedente de transferencia.

- Alcance vigente de la propuesta: web local obligatoria, carga manual de carpeta/DAG, linaje de tablas/columnas/STG/vistas y ventanas delimitadas, catálogo por revisiones, escenarios de impacto y descarga de resultados.
- Base técnica propuesta: React/TypeScript y FastAPI, adaptador SQLGlot, SQLite, JSON/HTML y explicación IA opcional. Cinco ADR Propuesto; no se aceptan al trasladarlos.
- Parser y dos DAGs de referencia ya inspeccionados en modo de consulta; sin acceso al framework como restricción conocida. La evidencia está en [revisión](fuentes/arquitectura/revision-parser-y-dags.md).
- H1-02 y H1-03 conservan En curso. La elaboración del borrador está preparada para observaciones; no se declara aprobado ni Completado. H1-01, H1-04 y H1-05 mantienen sus cierres originales.
- Próxima acción: revisar y ajustar alcance/decisiones en el PDI, sin construir ni prototipar. Estimación en H1-03, pruebas de implementación en H2 y comparaciones en H3.
- Fechas originales sin cambios; la integración del 17/11 no equivale a reprogramación ni cumplimiento en fecha del hito del 15/11.

La [integración](../02-arquitectura/docs/validation-log.md#procedencia-transferencia) registra mapa, procedencia y verificaciones. Las entradas siguientes reflejan las fechas indicadas; sus pendientes y tecnologías no seleccionadas describen el estado anterior, no reemplazan esta base vigente.

## Avance comprobado

- 18/11/2026: reubicados los manifiestos de entrega/fuentes y el informe del parser en [fuentes de arquitectura](fuentes/arquitectura/README.md). Se mantienen accesibles para trazabilidad y adaptación futura; ya no ocupan la raíz de arquitectura. Ajustadas referencias y conservados hashes originales, estados y fechas de compromisos. Próxima acción: revisar la propuesta canónica y estimar H1-03.

- 18/11/2026: retirados el insumo de arranque ya consumido y la guía separada de integración. Consolidadas procedencia, equivalencias y verificaciones de transferencia en el log canónico; conservados los manifiestos de hashes y la inspección del parser por aportar evidencia propia. Referencias actualizadas, sin cambiar estados, fechas de compromisos ni decisiones. Próxima acción: continuar la revisión de la propuesta y la estimación H1-03.

- 17/11/2026, depuración: retirados tres borradores de arquitectura y dos de evaluación, y eliminado el índice anterior duplicado de decisiones. Criterios útiles consolidados en el plan canónico y enlaces ajustados. Se conservan ADR, revisión simulada, fuentes y entregables con propósito propio. Sin cambios de estados, fechas o compromisos; próxima acción: revisar el borrador integrado.

- 17/11/2026: integrada la propuesta 1.0 de ArchFlow como fuente canónica de arquitectura en el PDI. Conservados documentos previos y ADR originales; ajustadas navegación, reglas de edición y referencias de evaluación. Sin cambios a objetivos/Excel, estados de compromisos, aprobaciones o código del producto.

## Historial previo a la integración

- 15/11/2026: revisado ArchFlow 0.2.0 mediante lectura de metodología, guías, plantillas y código del inicializador/comprobador. Preparado el insumo de arranque, posteriormente consolidado en la ficha y el [registro de transferencia](../02-arquitectura/docs/validation-log.md#procedencia-transferencia), con fuentes, decisiones vigentes, vacíos técnicos e instrucción de arranque. Próximo paso: iniciar la ficha con ese contexto y localizar parser y ejemplos para concretar cobertura. H1-02/H1-03 siguen En curso; no se creó un proyecto en ArchFlow, no se ejecutaron sus scripts ni se modificó su repositorio. La integración de salidas está documentada como propuesta, no realizada.

- 15/11/2026: H1-01 Completado. [Caso v1.0](../01-caso-de-negocio/problema-y-contexto.md) con usuarios de ingeniería y análisis, consulta de origen/consumidores y diagnóstico limitado a linaje; reporte con DAG, archivo, SQL y cobertura. Prioridades: dependencias omitidas, tiempo y menor dependencia del conocimiento individual. Interfaz web opcional; sin logs ni estados de ejecución. Beneficios documentados como expectativas, no mediciones. H1-02 y H1-03 siguen En curso.

- 15/11/2026: H1-05 Completado. Documentadas [diez aplicaciones de las lecturas](../05-aprendizaje/aplicacion-al-proyecto.md), integradas en arquitectura y ADR-002: contratos, publicación por DAG, separación de responsabilidades, evaluación, contexto, regresión y operación. Versión final redactada por instrucción de Óscar. Architecting AI Software Systems continúa como formación de H2 conforme a su aclaración. Sin implementación ni pruebas ejecutadas; H1-02 permanece En curso.

- 15/11/2026: H1-04 Completado. Óscar confirma sus respuestas de la [revisión simulada](../02-arquitectura/decisiones/revision-simulada-hito-1.md) y decide conservar las observaciones de IA para considerarlas conforme avance el proyecto. Registrado el cierre v1.0; las recomendaciones no se convierten en decisiones técnicas aceptadas ni en condiciones adicionales de cierre. Los otros entregables mantienen sus estados.

- 10/11/2026: documentado [AI Engineering](../05-aprendizaje/libros/ai-engineering.md) en un único archivo desde la transferencia de estudio: diez temas, ejemplos, preguntas, guía de decisiones, glosario y caso de linaje. Aplicaciones AIE-01 a AIE-03 propuestas, enlazadas desde aprendizaje. H1-05 conserva estado En curso; no se acreditan lectura íntegra, evaluación aprobada ni implementación por generar las notas.

- 06/11/2026: preparada la [revisión simulada del hito 1](../02-arquitectura/decisiones/revision-simulada-hito-1.md), con 16 preguntas de la sesión simulada entre la IA y Óscar.

- 29/10/2026: revisada la coherencia de las notas del libro con el alcance y la arquitectura conceptual. Documentadas [APSD-01 a APSD-03](../05-aprendizaje/aplicacion-al-proyecto.md#aplicaciones-software-design), con ejemplos sintéticos, trade-offs y validaciones previstas. Son propuestas; no se inspeccionó el parser ni se ejecutaron pruebas. Las anotaciones del usuario en el libro se conservaron.

- 28/10/2026: consolidadas las [notas de A Philosophy of Software Design](../05-aprendizaje/libros/a-philosophy-of-software-design.md) en un único archivo con 22 temas, resumen, ejemplos, criterios de revisión y glosario. Fuente: transferencia de estudio aportada; primeros tres temas reconstruidos temáticamente en esa fuente. H1-05 pasa a En curso. Las aplicaciones a linaje son propuestas, no decisiones aceptadas ni resultados.

- 28/10/2026: incorporado a la [ficha de TOGAF](../05-aprendizaje/cursos/togaf-standard-10th-edition.md) el enlace al certificado de finalización de Udemy aportado por Óscar. Se conserva una única ubicación para el enlace externo. La herramienta web no pudo abrirlo; no se atribuyen datos adicionales del certificado ni validación independiente. No equivale a certificación profesional de TOGAF.

- 28/10/2026: creadas las [notas de TOGAF Standard, 10th Edition](../05-aprendizaje/cursos/togaf-standard-10th-edition.md), asociadas al curso de Udemy “Learn about the TOGAF 10.0 Standard and prepare for the certification”. Curso completo confirmado por Óscar; prevalece sobre el alcance parcial de la transferencia. Notas organizadas por temas, sin referencias web, con resumen, ejemplos y repaso rápido. No se registra certificación ni aplicación verificada al prototipo.

- 28/10/2026: creado el prompt local en `00-pdi/auxiliar/prompt-documentar-cursos-y-libros.md`, con la carpeta auxiliar excluida mediante .gitignore. Óscar confirmó un solo archivo por curso o libro, notas versionadas y entrada basada en títulos/contexto. Ajustados los índices de aprendizaje a esa organización. El prompt distingue fuentes consultadas, complementos y aplicaciones propuestas; no acredita formación. Próximo paso de este flujo: indicar el primer curso o libro y su contexto.

- Añadida una ruta de lectura desde la portada hasta el [índice del caso de negocio](../01-caso-de-negocio/README.md), con cinco pasos y enlaces anterior/siguiente al principio y al final de cada documento. La ruta termina en arquitectura. Este ajuste de navegación no cambia compromisos ni estados de entregables.

- Desarrollado el [caso de negocio v0.1](../01-caso-de-negocio/problema-y-contexto.md): resumen, usuarios, proceso actual/propuesto, recorrido ilustrativo, alternativas, papel de IA y riesgos. H1-01 pasa a En revisión; no significa aceptado ni completado.
- Elaborados alcance funcional RF-01 a RF-05, hipótesis B-01 a B-04, criterios propuestos CA-01 a CA-06, modelo de esfuerzo/costo y propuesta inicial de piloto. H3-02 pasa a En curso. No hay precios cotizados, horas medidas ni tecnologías nuevas aceptadas.

- Sincronizado [Objetivos_PDI_2026.md](Objetivos_PDI_2026.md) con las dos hojas del plan en el Excel vigente: diez celdas con cambios de contenido respecto a la transcripción anterior, resumen y anexo actualizados. E3 y E14 conservan su redacción previa. C11 indica desarrollar el parser; C23 del Plan de Carrera conserva revisar el existente. La elección de reutilizar/adaptar/reemplazar requiere aclaración, no se deduce de esta sincronización.

- Previamente se preparó una versión revisada del Excel con 12 celdas ajustadas; la copia de outputs y el registro histórico antes/después ya no están disponibles en sus rutas anteriores. Después Óscar actualizó el Excel de fuentes, que ahora es el archivo vigente. No se atribuyen avances ni aprobación externa.

- Estructura documental creada el 25/10; fuentes originales conservadas. [Fuentes y aclaraciones](fuentes/README.md).
- En la sesión de incorporación se leyó el contexto transferido y se revisaron los documentos del repositorio. Su archivo histórico ya no está disponible en fuentes. Óscar autorizó adaptar documentación manteniendo estructura, AGENTS.md y compromisos.
- Actualizados [caso](../01-caso-de-negocio/problema-y-contexto.md), [alcance](../01-caso-de-negocio/alcance-y-exclusiones.md), [arquitectura conceptual](../02-arquitectura/docs/sad.md), evaluación y plantillas relacionadas. Esto no completa los entregables.
- Registrada la prioridad comunicada por el usuario en [ADR-001](../02-arquitectura/decisiones/ADR-001-prioridades-de-linaje.md): descubrimiento incremental y linaje de tablas/columnas, ambos prioridad 1.
- Git local sin commits. No hay implementación ni parser entregado en este repositorio; su existencia fue declarada por el usuario, sin inspección de capacidades.

## Contexto anterior para retomar — hasta el 15/11/2026

El problema principal es la falta de trazabilidad entre procesos, tablas y columnas. Dos capacidades: descubrir linaje por DAG y acumular conocimiento; después consultar impacto entre DAGs. Entorno de referencia declarado: GCP, GCS/Parquet, Hive Metastore, Spark/Dataproc y Airflow con Spark SQL embebido en YAML. No implica desplegar ese stack.

Caso: DAG 1 lee raw.ventas/raw.clientes y produce curated.ventas, con posibles STG; DAGs 2/3 consumen esa tabla. Eliminar raw.ventas.precio podría afectar el cálculo de total. La propagación a columnas de los consumidores necesita evidencia específica; consumo de tabla no demuestra impacto en todas sus columnas.

## Trabajo en curso registrado antes de la integración

Caso v1.0 Completado para H1-01 y alcance funcional definido; costos, propuesta de piloto, planificación y detalle de arquitectura En curso. [Entregables](seguimiento-compromisos.md). Tablas y columnas ya son prioridad confirmada; versión y patrones Spark SQL siguen abiertos. TOGAF está documentado y su finalización fue confirmada por Óscar. A Philosophy of Software Design dispone de notas del recorrido de estudio aportado; H1-05 está Completado por notas y aplicación documental al diseño. No hay pruebas, métricas ni grabaciones acreditadas, ni constancias de los cursos comprometidos en el Excel. H1-04 está Completado: conversación, respuestas, reflexión y tratamiento de las observaciones cerrados por Óscar. Los criterios técnicos propuestos siguen siendo parte del borrador, no acuerdos ni resultados.

## Decisiones pendientes registradas antes de la integración

Cobertura exacta del MVP y de vistas del PDI; convenciones de DAG/YAML; esquemas y parámetros; capacidades del parser; modelo de relaciones de valor/condición; ámbito y tratamiento de STG; identificación de targets; catálogo/persistencia; versionado, actualización y baja de DAGs; consultas de impacto; aporte específico de IA; interfaz y activación del análisis; tecnologías; criterios y metas de calidad; estimación de esfuerzo y presupuesto API de USD 20 propuesto, sin gasto autorizado.

El catálogo físico y arranque manual siguen por concretar. La separación análisis/IA y las reglas de actualización se definen en las aplicaciones de las lecturas y ADR-002. Verificado/Inferido/Desconocido distingue sustento de una relación; no demuestra ejecución ni ausencia de impacto fuera del catálogo.

## Bloqueos y dependencias registrados antes de la integración

Faltan código/ruta del parser y ejemplos representativos de DAG, configuración y YAML. Esto impide verificar reutilización y convenciones, pero permite continuar la delimitación funcional. No reemplazar el parser ni inventar estructura del framework. No hay dependencia de revisión humana externa para cerrar el proyecto. Fechas ambiguas del Excel y referencia bibliográfica siguen en [fuentes](fuentes/README.md).

## Próximas acciones registradas antes de la integración

Continuar H1-02 con un ejemplo sintético que concrete los contratos ya documentados en APSD-01 y AIE-02. Localizar e inspeccionar el parser para fijar cobertura y adaptador; preparar los esperados de AIE-01 para las pruebas de H2.

Continuar H1-02 concretando contratos y política de actualización/evidencia. Considerar las observaciones de la [simulación H1-04 ya cerrada](../02-arquitectura/decisiones/revision-simulada-hito-1.md) cuando sean pertinentes y registrar las decisiones técnicas que se adopten. Las respuestas de Óscar se conservan; no hay tarea pendiente de reformularlas.

Las aplicaciones de las lecturas están documentadas en su versión vigente. El trabajo siguiente concreta el mecanismo de persistencia, los esquemas y la cobertura SQL sin reabrir H1-05.

Para aprovechar el aprendizaje documentado, contrastar las propuestas de aplicación de TOGAF con el caso de negocio y registrar únicamente las decisiones expresamente aceptadas. La documentación del curso queda terminada.

Continuar H1-02 con parser y artefactos representativos para fijar patrones Spark SQL, vistas y STG, y definir el adaptador y la persistencia. Concretar el protocolo técnico CA-01 a CA-06 antes de medir. En H1-03, estimar esfuerzo y capacidad. H1-01, H1-04 y H1-05 están completados; no necesitan reabrirse para realizar ese trabajo.

## Verificación documental previa

Caso de negocio v0.1: verificados 67 enlaces relativos de los ocho documentos modificados, sin destinos faltantes ni tablas con columnas inconsistentes. Criterios, corpus adicional y ejemplo aritmético identificados como propuestas/ilustración. Las fechas de los tres hitos se mantienen; no se atribuyeron mediciones ni ejecución del prototipo.

Sincronización posterior del Excel: comprobadas las 112 celdas no vacías de las dos hojas transcritas, sin diferencias de contenido respecto al anexo. El hash del Excel vigente no cambió durante esta tarea. Las verificaciones que siguen corresponden a la revisión documental anterior.

Revisados 114 enlaces relativos sin destinos faltantes y tablas Markdown sin diferencias de columnas. Los 16 IDs, entregables y fechas originales/revisadas del seguimiento permanecen iguales. Comprobado byte a byte que AGENTS.md, .gitignore, objetivos y Excel no cambiaron. Sin nuevos archivos de código, SQL o YAML; sin commits ni remotos.

## Cierre y continuidad

Fechas originales: 15/11/2026, 20/12/2026 y 31/01/2027; ninguna vencida al 28/10. Faltan 18 días para el hito de diseño. Sin reprogramaciones; no asumir viabilidad de la ampliación sin revisar capacidad. Las ventanas intermedias del [plan](plan-de-trabajo.md) siguen siendo propuestas.

La revisión propuesta del Excel se guardó por separado y posteriormente Óscar actualizó el archivo de fuentes y se sincronizó su transcripción. Esta sesión desarrolla documentos de negocio y actualiza seguimiento: H1-01 En revisión y H3-02 En curso. Se conservan las fechas registradas, el Excel y los objetivos. No se programó el prototipo, crearon commits, publicaron archivos ni autorizaron gastos.

### Cierre de documentación TOGAF — 28/10/2026

Notas consolidadas en un único archivo y enlazadas desde aprendizaje, cursos y aplicación al proyecto. La base fue la transferencia aportada, con la confirmación posterior de curso completo. La sección de gobierno se desarrolló como síntesis conceptual complementaria; no se inspeccionaron lecciones privadas de Udemy. Se contrastó la estructura general con información pública oficial; el acceso al texto completo del estándar requirió autenticación y no permitió una verificación exhaustiva. Por instrucción del usuario, las notas no incluyen referencias web. No se modificaron objetivos ni compromisos, ni se realizaron commits o publicaciones.

Verificación de esta sesión: los enlaces nuevos y las anclas internas de las notas resuelven; el documento no contiene enlaces web ni estados pendientes. Las 16 filas de compromisos se conservaron sin cambios, al igual que AGENTS.md, .gitignore, objetivos, prompt base y transferencia. Se detectó un enlace histórico ya existente cuyo destino no está disponible: `fuentes/revision-excel-2026-10-28.md` en el seguimiento; no se alteró ese registro.

### Cierre de notas de A Philosophy of Software Design — 28/10/2026

Conservada la transferencia original y elaborado un documento de consulta con voz de apuntes personales. Los ejemplos de seguros, datos e IA se distinguen de las ideas atribuidas al libro; el código es ilustrativo, no ejecutado ni verificado como API de un proveedor. No se infiere edición ni fecha de finalización. Índices de libros, aprendizaje y aplicación actualizados. H1-05 refleja avance documental parcial, sin completar el conjunto de lecturas o acreditar aplicación. Sin cambios a objetivos, fechas, commits o publicaciones.

Verificación documental del libro: 22 temas, 27 enlaces internos válidos, tablas y bloques de código sin inconsistencias estructurales. Comprobados los 7 enlaces nuevos al libro desde índices y registros. Únicamente H1-05 cambió en la tabla de compromisos; fechas conservadas. Fuente, prompt, AGENTS.md, .gitignore y objetivos permanecen sin cambios.

### Verificación de aplicaciones del libro — 29/10/2026

Comprobados 16 enlaces relativos e internos del registro de aplicaciones, sin destinos faltantes. Tres propuestas documentadas. Las notas del libro permanecen idénticas al inicio de esta revisión, incluidas las anotaciones del usuario. Las 16 filas de compromisos no cambiaron. La revisión comprueba coherencia documental y pertinencia al proyecto; no certifica fidelidad íntegra a una edición del libro ni ejecución técnica. Sin commits ni publicaciones.

### Cierre de notas de AI Engineering — 10/11/2026

Notas consolidadas en una única ubicación y enlazadas desde libros, aprendizaje, aplicación y seguimiento. La fuente principal es la transferencia de diez días; se distingue la ruta de estudio del índice oficial no contrastado, y los ejemplos y propuestas de resultados comprobados. H1-05 amplía su evidencia sin cambiar de estado; no se modifican objetivos ni fechas. Próxima acción: preparar el caso y la rúbrica de AIE-01 y contrastar el contexto de AIE-02 con el contrato del análisis. Sin commits, publicaciones ni gastos.

Verificación documental: diez temas y 33 enlaces de las notas y sus nuevas referencias comprobados, sin destinos faltantes; tablas sin inconsistencias de columnas y ejemplos aritméticos revisados. Los 16 compromisos conservan estado y fechas; solo se amplía la evidencia de H1-05. Fuente, prompt base, AGENTS.md, objetivos, notas de A Philosophy of Software Design y revisión simulada permanecen idénticos a su contenido al inicio de esta sesión. No se ejecutaron pruebas del prototipo ni se certificó fidelidad literal al libro original.


