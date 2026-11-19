# Linaje de datos y análisis de impacto asistido por IA

Tipo: Proyecto  
Perfil: Estándar  
ArchFlow: 0.2.0  
Responsable: Óscar Hernández Barrón  
Estado: Propuesta 1.0 lista para revisión  
Fecha-revisada: 2026-11-17

En esta ficha conservo el contexto, las fuentes y el progreso de H1-02. Desde el 17/11/2026, `02-arquitectura/` de este PDI es la ubicación canónica de la propuesta y conserva la estructura documental de ArchFlow 0.2.0. La entrega elaborada en ArchFlow queda como antecedente de transferencia. Esta ficha no acredita aprobación arquitectónica ni ejecución del prototipo.

[Portada del PDI](../README.md) · [Objetivos](../00-pdi/Objetivos_PDI_2026.md) · [Estado actual](../00-pdi/estado-actual.md) · [Procedencia de la transferencia](docs/validation-log.md#procedencia-transferencia)

Las restricciones de solo lectura de SRC-012 a SRC-020 describen la elaboración original. La integración autorizada en SRC-021 permite actualizar la documentación del PDI; no cambia las restricciones sobre datos productivos, gasto, ejecución o publicación.

Los manifiestos de procedencia y el informe de inspección se conservan como [evidencias de referencia](../00-pdi/fuentes/arquitectura/README.md), fuera de la ruta principal de lectura del diseño.

## Entrada para revisar la propuesta

La [propuesta consolidada 1.0](docs/revision-propuesta.md) resume decisiones, alcance, límites y orden de lectura. Esta entrega cierra la preparación documental para revisión y ajustes; no acredita aprobación ni construcción. Las pruebas del producto quedan planificadas en H2.

## Selección y contexto

- **CONFIRMADO — Perfil:** Selecciono el perfil Estándar. Es adecuado para un sistema nuevo con contratos de análisis, persistencia, actualización, consulta e integración IA. Mantengo la profundidad proporcional a un prototipo individual.
- **CONFIRMADO — Problema:** ingenieros y analistas reconstruyen origen y consumidores buscando entre repositorios y código. Pueden omitir dependencias y dependen del acceso, la experiencia y la memoria del equipo.
- **CONFIRMADO — Resultado esperado:** un reporte trazable de linaje e impacto que permita reutilizar conocimiento y orientar qué revisar. Priorizo encontrar dependencias omitidas, después reducir tiempo y finalmente reducir dependencia del conocimiento individual. Los beneficios siguen por medir.
- **CONFIRMADO — Incluido:** DAGs, configuración, YAML con Spark SQL y esquemas proporcionados; tablas y columnas como prioridad 1, pasos STG pertinentes, vistas y scripts dentro de la cobertura seleccionada en el diseño técnico. Consultas upstream/downstream y candidatos de impacto ante eliminación/renombrado de tablas y adición, eliminación, renombrado o modificación de columnas. Interfaz web y descargas obligatorias por SRC-019. El reporte conserva caminos, DAG, archivo, SQL de soporte, versiones, cobertura y referencias no resueltas.
- **CONFIRMADO — Excluido:** logs y estados de ejecución de pipelines, diagnóstico automático de causa raíz, cambios productivos, gestión de permisos y garantía de cobertura universal. La observabilidad interna del analizador sí forma parte del diseño vigente.
- **CONFIRMADO — Restricciones:** datos sintéticos; sin gasto autorizado, infraestructura, publicaciones, remotos ni commits. USD 20 es presupuesto API propuesto. La actualización documental del PDI está autorizada por SRC-021; no modifica objetivos ni el Excel fuente.
- **Entorno de referencia:** GCP, GCS/Parquet, Hive Metastore, Spark/Dataproc y Airflow. **CONFIRMADO:** prototipo local y carga manual (SRC-017); la referencia no exige desplegar ese stack.
- **Audiencia e idioma:** elaboración y revisión individual de arquitectura; usuarios objetivo de ingeniería y análisis; documentación en español.
- **Fuentes y herramientas autorizadas:** lectura de los documentos enumerados en el contexto y elaboración documental en este proyecto. El 16/11 se autoriza además revisar el analizador y los DAGs de referencia de SRC-013 a SRC-015 en modo de consulta. Estos artefactos empresariales no se presentan como corpus sintético; el prototipo mantiene datos sintéticos. No implica autorización para ejecutar pipelines, consultar datos productivos ni enviar artefactos a un proveedor de IA futuro.
- **Disparadores de revisión:** nuevos ejemplos, inspección del parser, cambios de cobertura, esquemas, contratos, dependencias, capacidad, riesgos o resultados de evaluación.

Procedencia: [contexto](#SRC-001), [caso y alcance](#SRC-004), [criterios](#SRC-005) y [solicitud de inicio](#SRC-012).

## Stakeholders y preocupaciones

| Persona o rol | Preocupación | Autoridad / participación | Fuente |
|---|---|---|---|
| Óscar Hernández Barrón | Diseño implementable, aprendizaje aplicado, esfuerzo viable y evidencia del PDI | Responsable del proyecto y de las decisiones; revisión individual | SRC-002, SRC-003, SRC-012 |
| Ingenieros de datos | Localizar dependencias, distinguir valores y condiciones, evaluar cambios con evidencia | Usuarios objetivo; sin participación real acreditada en esta sesión | SRC-004 |
| Analistas | Comprender origen, consumidores y límites para orientar investigaciones | Usuarios objetivo; sin validación independiente acreditada | SRC-004 |

La revisión simulada sirve como material de consideración; no representa aprobación de un cliente ni impone revisión externa para completar el PDI.

## Base vigente y asuntos abiertos

| Tema | Estado de la fuente | Base que conservo / trabajo pendiente |
|---|---|---|
| Prioridad de alcance | ADR-001 del PDI: aceptada como prioridad de alcance | Tablas y columnas, descubrimiento incremental y revisión del parser antes de plantear reemplazo. No equivale a aprobar toda la arquitectura. |
| Aplicaciones de lecturas | ADR-002 del PDI: incorporada a la versión documental vigente | Conservo los diez criterios APSD/AIE. No atribuyo aceptación individual nueva a cada criterio. |
| Entrega funcional | ADR-003 del PDI: aceptada en su alcance original | El PDI conserva reporte suficiente y web opcional; SRC-019 amplía aquí el alcance a web obligatoria, sin modificar retroactivamente el ADR original. |
| Análisis | Diseño vigente, APSD-01 | Resultado independiente de los detalles internos del parser, con identidad, relaciones, evidencia, cobertura, sustento y resultado completo/parcial/fallido. Analizar no publica. Esquema detallado seleccionado en [diseño técnico](docs/diseno-tecnico.md). |
| Publicación y actualización | Diseño vigente, APSD-02 | Reemplazo unitario por DAG, repetición sin duplicados y conservación de aportes ajenos. Parcial/fallido conserva el último resultado completo con vigencia visible. Baja explícita y control de versión esperada al publicar. SQLite, revisiones inmutables y activación transaccional seleccionados en ADR-0003. |
| Consulta y explicación | Diseño vigente, APSD-03 y AIE-02 | La consulta entrega caminos, versiones, evidencia y límites. La IA no modifica el catálogo ni convierte inferencias en hechos; si falla o su respaldo no se valida, permanece el resultado estructurado. |
| Semántica y evidencia | Diseño vigente, APSD-04 | Distingo valor, condición, consumo de tabla y candidato de impacto. Verificado significa respaldado por análisis, no fallo ejecutado. No fusiono STG solo por nombre ni infiero flujo de datos por orden de tareas. |
| Evaluación y evolución | Diseño vigente, APSD-05 y AIE-01 a AIE-05 | Esperados previos, evaluación por componente y tarea, contexto trazable, regresiones de errores confirmados y registro de versiones, errores, tiempos y consumo. Pruebas aún no ejecutadas. |
| Tecnologías | Base seleccionada; pruebas pendientes | SPA React/TypeScript, FastAPI local, SQLGlot, SQLite, JSON/HTML y OpenAI opcional. Alternativas en ADR-0002 a ADR-0004; no requieren RAG, agentes, microservicios ni base de grafos. |
| Cobertura y aceptación | Base seleccionada; revisión pendiente | Perfil spark-static-v1, identidad, ventanas y diagnósticos definidos en el diseño técnico. CA-01 a CA-06 del PDI conservan estado propuesto; no son resultados ni se aceptan retroactivamente. |

Procedencia: [diseño y alternativas](#SRC-006), [decisiones originales](#SRC-007), [aplicaciones](#SRC-008), [simulación](#SRC-009) y [evaluación](#SRC-010).

H1-01, H1-04 y H1-05 están completados en el estado vigente del PDI. H1-02 y H1-03 siguen En curso. Mantengo diez cambios de H2 y cinco comparaciones de H3; no los sustituyo por pruebas del parser. Las fechas originales son 15/11/2026, 20/12/2026 y 31/01/2027. Las ventanas y capacidades históricas no demuestran disponibilidad restante.

## Fuentes autorizadas

Raíz de las fuentes del PDI: `C:/Oscar/Repositorios/pdi-2026-s2-arch-ai`, este repositorio. Las rutas siguientes son relativas a esa raíz y se conservan como localizadores externos, sin copiar los originales ni introducir enlaces locales fuera de la raíz validable de ArchFlow. Lectura realizada el 15/11/2026. La solicitud de inicio autorizó consultar el contexto y las fuentes enumeradas sin modificar el PDI. SRC-021 autoriza posteriormente esta integración documental. Clasificación: documentación de trabajo del PDI, sin declaración de carácter público; se utiliza únicamente lo pertinente al caso. Los ejemplos previstos son sintéticos.

## SRC-001 — Contexto de transferencia

<a id="SRC-001"></a>
Fecha documental original: 15/11/2026. Insumo de arranque utilizado para elaborar la arquitectura. Su contexto útil está consolidado en esta ficha, el caso de negocio y las fuentes SRC-002 a SRC-012. La instrucción de iniciar el proyecto y el mapa de traslado inicial dejaron de aplicar tras la integración; conservo este ID como procedencia histórica, no como requisito vigente.

## SRC-002 — Estado y seguimiento

<a id="SRC-002"></a>
Ubicaciones: `00-pdi/estado-actual.md` y `00-pdi/seguimiento-compromisos.md`. Última actualización declarada: 15/11/2026. Uso: estados actuales y compromisos. Los cierres posteriores explícitos prevalecen sobre propuestas históricas; las referencias a días restantes en octubre no describen la fecha actual.

## SRC-003 — Objetivos y aclaraciones

<a id="SRC-003"></a>
Ubicaciones: `00-pdi/Objetivos_PDI_2026.md` y `00-pdi/fuentes/README.md`. Transcripción sincronizada el 28/10/2026; aclaración de formación del 15/11/2026. Uso: compromisos, fechas y restricciones. No se cargó el Excel ni sus datos auxiliares. Se conserva la diferencia entre revisar el parser existente y desarrollar el parser, sin inferir sustitución.

## SRC-004 — Caso y alcance

<a id="SRC-004"></a>
Ubicaciones: `01-caso-de-negocio/problema-y-contexto.md` y `01-caso-de-negocio/alcance-y-exclusiones.md`. Caso v1.0 y alcance funcional al 15/11/2026. Uso: problema, usuarios, RF-01 a RF-05, reporte y exclusiones. RF son identificadores del PDI; aún no se duplican como requisitos ArchFlow.

## SRC-005 — Beneficios y criterios

<a id="SRC-005"></a>
Ubicación: `01-caso-de-negocio/beneficios-y-criterios-de-exito.md`. Versión 1.0, 15/11/2026. Uso: prioridades, hipótesis y CA-01 a CA-06 propuestos. No acredita mediciones.

## SRC-006 — Arquitectura, alternativas y riesgos

<a id="SRC-006"></a>
Fuentes históricas: `arquitectura-de-solucion.md`, `alternativas.md` y `riesgos-y-limitaciones.md`, retiradas el 17/11/2026 al consolidar el diseño en el [SAD](docs/sad.md) y los [ADR](adr/README.md). Arquitectura conceptual del 28/10 con criterios incorporados el 15/11/2026; alternativas pendientes y riesgos iniciales. Uso: base lógica, opciones y limitaciones. Las alternativas históricas se interpretan junto con las reglas posteriores de actualización por DAG.

## SRC-007 — Decisiones del PDI

<a id="SRC-007"></a>
Ubicaciones: [ADR-001 original](decisiones/ADR-001-prioridades-de-linaje.md), [ADR-002 original](decisiones/ADR-002-aplicacion-lecturas-al-diseno.md) y [ADR-003 original](decisiones/ADR-003-alcance-funcional-del-caso.md). Fechas: 28/10/2026, 15/11/2026 y 15/11/2026. Uso: conservar sus distintos estados y alcance; no renumerarlos ni convertirlos en ADR aceptados nuevos de ArchFlow.

## SRC-008 — Diez aplicaciones del aprendizaje

<a id="SRC-008"></a>
Ubicación: `05-aprendizaje/aplicacion-al-proyecto.md`. Versión 1.0, 15/11/2026. Uso: contratos, invariantes y trade-offs APSD-01 a APSD-05 y AIE-01 a AIE-05. Es la definición de origen; no se requiere cargar todas las notas de libros para este inicio.

## SRC-009 — Revisión simulada

<a id="SRC-009"></a>
Ubicación: `02-arquitectura/decisiones/revision-simulada-hito-1.md`. Ejercicio del 06/11, cierre v1.0 del 15/11/2026. Uso: respuestas finales y observaciones para considerar, sin aceptación de recomendaciones en bloque. Se conserva el cierre de H1-04.

## SRC-010 — Evaluación prevista

<a id="SRC-010"></a>
Fuentes históricas: `04-evaluacion/plan-de-evaluacion.md` y `04-evaluacion/casos-de-prueba.md`, retiradas el 17/11/2026 tras consolidar los criterios útiles en el [plan canónico](docs/validation.md). Los diez cambios y los esperados están allí y en el [recorrido sintético](docs/recorrido-sintetico.md); el protocolo de cinco comparaciones se mantiene en [evaluación](../04-evaluacion/comparacion-manual-vs-solucion.md). Su estado original era Pendiente; no acredita ejecuciones.

## SRC-011 — Plan, costos y esfuerzo

<a id="SRC-011"></a>
Ubicaciones: `00-pdi/plan-de-trabajo.md` y `01-caso-de-negocio/costos-y-esfuerzo.md`. Borradores del 28/10/2026. Uso: dependencias y estructura de estimación. Horas históricas y ejemplo aritmético de ahorro no son capacidad disponible ni estimaciones del prototipo.

## SRC-012 — Solicitud de inicio

<a id="SRC-012"></a>
Procedencia: conversación del 15/11/2026. Uso autorizado: iniciar `linaje-impacto-ia-pdi` con perfil Estándar, consultar fuentes del PDI sin modificarlas, redactar con voz propia y resolver huecos mediante preguntas breves. No se atribuyen aprobaciones adicionales.

## SRC-013 — Analizador existente

<a id="SRC-013"></a>
Ubicación externa: `C:/Oscar/Walmart/dag_analyzer`. Lectura autorizada por la conversación del 16/11/2026. Uso: revisar código, documentación y resultados guardados para determinar capacidades y brechas. Repositorio conservado sin cambios; no se ejecuta su exportador sobre las fuentes. Evidencia y límites en la [revisión](../00-pdi/fuentes/arquitectura/revision-parser-y-dags.md).

## SRC-014 — Primer DAG de referencia

<a id="SRC-014"></a>
Ubicación externa: `C:/Oscar/Walmart/Dags_CRM/ecomm_ords_hdr_trans_dcp`. Lectura autorizada el 16/11/2026. Uso: convenciones y contraste con salida histórica; no constituye corpus sintético. Conservado sin cambios. Sus inconsistencias se documentan sin inferir cuál representación ejecuta el framework.

## SRC-015 — DAG principal de referencia

<a id="SRC-015"></a>
Ubicación externa: `C:/Oscar/Walmart/Dags_CRM/sams_ords_hdr_trans`. Aportado el 16/11/2026 como otro ejemplo ante la pregunta sobre diferencias del primero. Uso: ejemplo principal de estructura, parámetros y SQL. No implica que se haya contestado la regla general de precedencia ni autorizado ejecución. Conservado sin cambios; corpus sintético del prototipo aún pendiente.

## SRC-016 — Continuidad sin acceso al framework

<a id="SRC-016"></a>
Procedencia: conversación del 16/11/2026. Confirmo que no tengo acceso al framework y solicito continuar con los archivos disponibles. Uso: retirar ese acceso como prerrequisito del diseño y conservar semántica no observable como límite. No implica autorizar ejecución de pipelines, inventar esquemas ni aceptar automáticamente decisiones propuestas.

## SRC-017 — Cobertura de ventanas y ejecución local

<a id="SRC-017"></a>
Procedencia: respuestas de la conversación del 16/11/2026. Confirmo incluir ventanas en el primer MVP y ejecutar el prototipo en mi equipo local, con carga manual de archivos y generación de reporte, sin levantar Airflow, Spark ni servicios de nube. Uso: refinar cobertura y despliegue. No selecciona biblioteca, motor de almacenamiento, modelo IA ni todas las variantes de ventanas; tampoco autoriza gasto.

## SRC-018 — Consolidación de decisiones técnicas

<a id="SRC-018"></a>
Procedencia: conversación del 16/11/2026. Defino el criterio para consolidar la base técnica de H1-02: seleccionar las alternativas más adecuadas con el contexto disponible. Mantengo fuera de este alcance el gasto, la infraestructura y las modificaciones al PDI. Los resultados de pruebas y la aprobación de puertas requieren evidencia independiente. Las fuentes técnicas oficiales consultadas constan en ADR-0002 a ADR-0004.

## SRC-019 — Web y referencia tecnológica

<a id="SRC-019"></a>
Procedencia: conversación del 16/11/2026. Requiero flujo completo desde web: seleccionar carpeta/DAG, analizar y guardar, explorar linaje, evaluar cambios de tablas/columnas, mostrar resumen y descargar reporte. Incluye eliminar tablas y agregar/quitar/modificar columnas. Referencia leída: `C:/Oscar/Repositorios/osutils-web/ARQUITECTURA-Y-LOOK-AND-FEEL.md`, revisión 16/11/2026, solo consulta. Reutilizo capas y estilo pertinentes, no Oracle ni experimentos. Esta petición prevalece sobre web opcional y CLI anteriores; conserva ejecución local y restricciones de datos/gasto.

## SRC-020 — Cierre de propuesta sin construcción

<a id="SRC-020"></a>
Procedencia: conversación del 16/11/2026. Confirmo que el trabajo actual es arquitectura y solicito ajustar el conjunto para recibir una propuesta cerrada para revisión y ajustes. No construir ni prototipar el sistema. Separar diseño y revisión documental de implementación y validaciones futuras; no interpretar este encargo como aceptación formal de ADR o puertas.

## SRC-021 — Integración canónica de ArchFlow en el PDI

<a id="SRC-021"></a>
Procedencia: conversación del 17/11/2026. Decido que la arquitectura del PDI adopte los documentos y la organización de ArchFlow, en lugar de repartirlos en la estructura anterior. Incluye ajustar AGENTS.md para conservar la nueva organización. La entrega fuente es la propuesta 1.0, con revisión editorial del 17/11/2026; sus huellas y equivalencias se conservan en [integración](docs/validation-log.md#procedencia-transferencia). La integración no aprueba ADR ni declara H1-02 Completado.

## Entregables aplicables
 
La ejecución local/manual y la inclusión de ventanas están confirmadas en [SRC-017](#SRC-017). El detalle de cobertura y contratos continúa como propuesta revisable.

| Entregable | Aplica y motivo | Ruta cuando exista | Responsable | Estado |
|---|---|---|---|---|
| Ficha | Contexto y reanudación de H1-02 | README.md | Óscar Hernández Barrón | Borrador |
| Revisión del analizador y fuentes | Evidencia de capacidades, brechas y convenciones para decidir reutilización | [Revisión](../00-pdi/fuentes/arquitectura/revision-parser-y-dags.md) y [huellas](../00-pdi/fuentes/arquitectura/fuentes-inspeccionadas.json) | Óscar Hernández Barrón | Borrador de análisis |
| SAD | Estrategia, bloques y contratos lógicos sin framework | [docs/sad.md](docs/sad.md) | Óscar Hernández Barrón | Propuesta 1.0 para revisión |
| Requisitos | Correspondencia con RF/CA y escenarios de calidad | [docs/requirements.md](docs/requirements.md) | Óscar Hernández Barrón | Propuestos |
| Definición de validación | Métodos y criterios antes de ejecutar | [docs/validation.md](docs/validation.md) | Óscar Hernández Barrón | Planificada |
| Log de validación | Revisiones, evidencia y aprobaciones separadas | [docs/validation-log.md](docs/validation-log.md) | Óscar Hernández Barrón | Iniciado; pruebas del prototipo no ejecutadas |
| Recorrido sintético | Esquemas, relaciones esperadas y cobertura propuesta antes de implementar | [docs/recorrido-sintetico.md](docs/recorrido-sintetico.md) | Óscar Hernández Barrón | Propuesto; fixtures no creados |
| ADR | Alternativas y decisiones de relaciones, adaptador, catálogo y reporte | [Índice en SAD](docs/sad.md#9-decisiones-arquitectónicas) | Óscar Hernández Barrón | Cinco ADR propuestos |
| Vistas | Contexto, contenedores, componentes y publicación | [diagrams/README.md](diagrams/README.md) | Óscar Hernández Barrón | Vistas 1.2; revisión visual en la revisión formal |
| Diseño técnico | Contratos, cobertura, persistencia, consulta y reporte | [docs/diseno-tecnico.md](docs/diseno-tecnico.md) | Óscar Hernández Barrón | Base seleccionada 1.2 |
| Experiencia web | Flujos, API, carga y trabajos | [docs/experiencia-web.md](docs/experiencia-web.md) | Óscar Hernández Barrón | Propuesta 1.1 |
| Guía de revisión | Síntesis, límites y criterio de entrega | [docs/revision-propuesta.md](docs/revision-propuesta.md) | Óscar Hernández Barrón | Propuesta 1.0 lista para revisar |
| Insumos para H1-03 | Esfuerzo y dependencias derivados de cobertura y diseño | Aún no creados | Óscar Hernández Barrón | Pendiente |
| Presentación y exportaciones | Condicionales; no necesarias para iniciar la ficha | No creadas | Óscar Hernández Barrón | Pendiente de necesidad |

La carpeta `02-arquitectura/` reúne la arquitectura vigente del proyecto. Su carpeta `docs/` mantiene el núcleo de diseño Estándar desde las plantillas comunes. No se crean ADR, diagramas ni registros de ejecuciones ficticias para simular avance.

## Preguntas y supuestos

| Pregunta o supuesto | Tipo | Impacto | Responsable / fecha | Respuesta y evidencia |
|---|---|---|---|---|
| P1. Ubicación del parser y sus salidas | CONFIRMADO; inspección inicial realizada | Adaptador, reutilización y cobertura | Óscar / 16/11/2026 | SRC-013. Código y JSON histórico revisados; no equivalen a linaje completo ni ejecución reproducida. Versiones pendientes en P7. |
| P2. Ejemplos representativos | CONFIRMADO como fuentes de referencia; corpus sintético PENDIENTE | Identidad, extracción, STG, vistas, targets y patrones | Óscar / 16/11/2026 | SRC-014 y SRC-015. SAMS pasa a ser el ejemplo principal; DCP queda como contraste. Hay DDL de STG, pero falta el DDL final y los esquemas completos de origen. |
| P3. Entorno, volumen y capacidad | CONFIRMADO: local/manual; volumen y capacidad PENDIENTES | Despliegue, escenarios cuantitativos y estimación H1-03 | Óscar / 16/11/2026 | SRC-017 confirma equipo local, carga manual y reporte sin levantar Airflow/Spark/nube. No asignar volumen ni horas por defecto. |
| P4. Criterios y cobertura | CONFIRMADO: incluir ventanas; subconjunto seleccionado, revisión de CA pendiente | Requisitos, validaciones y cierre de diseño | Óscar / 16/11/2026 | SRC-017. Matriz y variantes propuestas en el recorrido sintético; CA-01 a CA-06 no quedan aceptados por esa respuesta. Diez cambios y cinco comparaciones ya son compromisos. |
| P5. Acceso al framework y tratamiento de su semántica | CONFIRMADO: sin acceso; continuar con lo disponible | Precedencia Python/YAML, fechas y extracción estática | Óscar / 16/11/2026 | SRC-016. No volver a exigir su código para avanzar. Analizar declaraciones estáticas, conservar conflictos y parámetros dinámicos no resueltos; no atribuir comportamiento interno. |
| P6. DDL/esquema ordenado de la tabla final | PENDIENTE; no bloquea diseño lógico | Columnas destino, particiones y escritura posicional | Óscar / antes de validar mapeos | No aparece en las carpetas de ejemplo. Continuar declarando mapeos no resueltos; preparar esquema propio explícito para el corpus sintético, sin atribuirlo al entorno real. |
| P7. Entorno/versiones del analizador | PENDIENTE; no bloquea contratos | Reproducibilidad y pruebas SQL | Óscar / antes de pruebas funcionales | Sin manifiesto de dependencias. Definir un entorno de evaluación identificado cuando corresponda; no afirmar que reproduce el original. No se instalaron paquetes. |

La [revisión del analizador](../00-pdi/fuentes/arquitectura/revision-parser-y-dags.md) identifica capacidades observadas, defectos comprobados de forma aislada y resultados históricos. La adaptación con SQLGlot ya está seleccionada explícitamente en ADR-0002; disponer del código no equivale a acreditar cobertura.

## Relación con implementación

Repositorio del analizador: `C:/Oscar/Walmart/dag_analyzer`; módulos en `analyzer_code/`. HEAD observado: `9b6f0f22867a19da40db969e18e265723267ac56`; no se afirma igualdad del árbol de trabajo con ese commit. El [manifiesto](../00-pdi/fuentes/arquitectura/fuentes-inspeccionadas.json) conserva hashes del contenido inventariado. Es implementación previa de extracción, no del prototipo completo de H2. El PDI conserva la versión documental canónica; los repositorios del parser y los DAGs permanecen sin cambios.

La transferencia al PDI queda registrada en [integración](docs/validation-log.md#procedencia-transferencia). Conservo IDs de ArchFlow y ADR históricos de tres dígitos como registros distintos; las revisiones futuras se realizan aquí, sin mantener una segunda arquitectura editable.

## Progreso de trabajo

- Actividad actual: propuesta 1.0 integrada en el PDI con estructura ArchFlow; lista para revisión y ajustes aquí.
- Última actualización: 2026-11-17, America/Mexico_City.
- Base: SAD 1.0, diseño técnico 1.2, experiencia web 1.1, vistas 1.2, requisitos y cinco ADR propuestos. La [guía de revisión](docs/revision-propuesta.md) es el punto de entrada.
- Último avance: alcance web reconciliado, límites y riesgos explícitos, revisión documental separada de pruebas del producto. Sin construcción ni prototipado.
- Próximo paso: revisaré el contenido y los diagramas e incorporaré observaciones sobre esta versión. La entrega de la propuesta no inicia H2.
- Asuntos posteriores: horas/capacidad en H1-03; fixtures, dependencias y VAL-001 a VAL-007 en H2; comparaciones en H3. P6/P7 siguen como límites del contexto real, no como tareas exigidas para recibir esta propuesta.
- Validación actual: VAL-008 define revisión de arquitectura; resultados y comprobación estructural en el [log](docs/validation-log.md). No equivalen a demostración funcional.
- Puertas: pendientes de evaluación formal; no se aprueban automáticamente ni se trasladan todas las pruebas H2 a G3/G4. Presentación/exportaciones no solicitadas. La integración no declara aprobaciones ni cierre de H1-02; el seguimiento conserva sus estados.
