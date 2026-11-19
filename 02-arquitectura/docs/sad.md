# Arquitectura de solución — Linaje e impacto asistido por IA

Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17  
Estado: Propuesta lista para revisión  
Versión: 1.0

En este documento concreto el diseño lógico de H1-02 con las doce secciones arc42. Mantengo los criterios vigentes del PDI; los contratos detallados de este documento son PROPUESTOS. Fuentes: [ficha](../README.md), [inspección](../../00-pdi/fuentes/arquitectura/revision-parser-y-dags.md) y [requisitos](requirements.md).

## Control documental

Entrega documental de H1-02 conforme a [SRC-020](../README.md#SRC-020). [Guía de revisión](revision-propuesta.md): decisiones, alcance y criterio de aceptación. Esta versión no requiere construir ni prototipar el sistema. VAL-001 a VAL-007 describen comprobaciones de implementación en H2; VAL-008 revisa la propuesta actual. Las puertas y los ADR mantienen su aprobación pendiente.

Organizo la propuesta con las plantillas de ArchFlow 0.2.0. La falta de acceso a `workflow_framework` está confirmada en [SRC-016](../README.md#SRC-016): continúo sin exigir ese código como entrada. El comportamiento interno del framework no se considera conocido. Revisión formal y puertas pendientes en el [log](validation-log.md). Cambios en artefactos, contratos, cobertura, versiones o pruebas disparan revisión.

## 1. Introducción y objetivos

Ingenieros y analistas necesitan reconstruir origen y consumidores, identificar dependencias omitidas y orientar cambios. La prioridad es trazabilidad de tablas y columnas, seguida de reducción de esfuerzo y dependencia del conocimiento individual. Stakeholders y alcance inicial: [ficha](../README.md#stakeholders-y-preocupaciones).

<a id="OBJ-001"></a>
## OBJ-001 — Investigación trazable y reutilizable

Resultado buscado: consultar origen, consumidores y candidatos de impacto con caminos, evidencia y cobertura explícita. Responsable: Óscar Hernández Barrón. Fuente: [caso y alcance](../README.md#SRC-004). No se compromete ahorro cuantitativo ni garantía de ausencia universal de impacto. Diez cambios y cinco comparaciones mantienen su significado original.

## 2. Restricciones

Datos sintéticos para construir y evaluar el prototipo; artefactos empresariales únicamente como referencia autorizada. No ejecutar DAGs, shell, SQL ni código del framework para descubrir relaciones. No hay gasto autorizado, despliegue contratado ni acceso requerido a metastore. PDI y repositorios de origen se conservan sin cambios.

El framework no está disponible. El DDL final y las versiones del entorno previo tampoco están en las entradas consultadas; esa ausencia limita comprobaciones concretas, pero no impide definir contratos. No reconstruyo esquema final por alias ni atribuyo las versiones del equipo actual al parser original.

## 3. Contexto y alcance

El usuario proporciona un paquete de archivos de un DAG y contexto opcional de esquemas/parámetros. El analizador prepara un resultado; una operación separada publica una contribución. La acción web «Analizar y guardar» solicita automáticamente publicación solo si el resultado es completo y sin conflicto; siempre conserva diagnósticos. El usuario consulta linaje o propone un cambio; recibe resultado estructurado y explicación cuando esté disponible y respaldada.

Entradas externas: archivos Python, YAML y shell como texto, SQL embebido, esquemas aportados y parámetros declarados. Salidas: diagnósticos de análisis y reporte. El proveedor IA seleccionado es OpenAI Responses API, con activación opcional; no participa en descubrir hechos ni tiene acceso de escritura al catálogo.

SAMS es referencia de convenciones: Python y YAML coinciden en la secuencia, aunque el orden no demuestra flujo de datos. DCP sirve como contraste de discrepancias. Vistas permanecen en el alcance del PDI aunque estos ejemplos no aporten su definición; se necesita un caso sintético explícito. Web obligatoria por SRC-019; automatización de repositorios y ejecución de pipelines siguen fuera del mínimo.

## 4. Estrategia de solución

Mantengo extracción/análisis, publicación, consulta y explicación separados lógicamente. Propongo un adaptador de archivos que reconozca el subconjunto observado sin importar el framework. Si una construcción no puede interpretarse estáticamente, devuelve un diagnóstico, no la ejecuta ni inventa su significado.

Selecciono SPA React/TypeScript y API FastAPI local sobre servicios Python, SQLGlot, SQLite y reportes JSON/HTML. [Experiencia web](experiencia-web.md) define pantallas, carga y trabajos. Las alternativas y consecuencias se registran en ADR-0002 a ADR-0004. El [diseño técnico](diseno-tecnico.md) fija cobertura y mecanismos; su selección no acredita pruebas ni aprobación de puertas. No se requieren microservicios.

La independencia respecto del framework permite avanzar y limita el acoplamiento; su costo es no acreditar semántica de ejecución ni resolver contexto que solo existe en tiempo de ejecución. El contrato de entrada hará visible ese límite.

## 5. Vista de bloques de construcción

Estos bloques son responsabilidades lógicas propuestas. Ejecución local y carga manual confirmadas en [SRC-017](../README.md#SRC-017); no exigen procesos independientes. SPA/API local y [vistas C4](../diagrams/README.md) seleccionadas; renderizado visual pendiente.

<a id="EL-001"></a>
## EL-001 — Ingesta y análisis estático

Lee únicamente el paquete autorizado, descubre asociaciones explícitas, conserva SQL original, resuelve contexto disponible y produce relaciones con evidencia. SQLGlot queda detrás de un adaptador propio que aprovecha las convenciones del parser previo; su salida no se publica directamente. No ejecuta artefactos ni escribe en el catálogo.

<a id="EL-002"></a>
## EL-002 — Catálogo y publicación

Valida el contrato, conserva contribuciones por DAG y revisiones; publica un reemplazo como unidad y mantiene diagnósticos de intentos parciales/fallidos. Atiende lectura de una revisión consistente y baja explícita. SQLite conserva revisiones inmutables y todo el historial durante el MVP; activación transaccional según ADR-0003.

<a id="EL-003"></a>
## EL-003 — Consulta de linaje e impacto

Recorre relaciones de una revisión del catálogo; separa valor, condición, consumo de tabla y orquestación declarada. Devuelve caminos y cobertura. No propaga automáticamente un cambio de columna a todas las columnas de un consumidor.

<a id="EL-004"></a>
## EL-004 — Reporte y explicación

Construye el reporte desde el resultado estructurado; entrega a IA un contexto acotado con evidencias. Separa hechos, inferencias y recomendaciones; comprueba referencias y conserva el resultado estructurado si falla la explicación. No modifica relaciones publicadas. JSON canónico y HTML autocontenido; explicación opcional con el modelo fijado en ADR-0004.

### Contratos propuestos

Los nombres siguientes son conceptuales; aún no son una API implementada ni obligan a un lenguaje o formato de transporte.

| Contrato | Campos mínimos | Precondición, efecto y error |
|---|---|---|
| Paquete de análisis | Identidad declarada del DAG; manifiesto de rutas y hashes; referencia a configuración; esquemas con procedencia; parámetros con origen; perfil de cobertura; versión del adaptador | Las rutas deben pertenecer al paquete autorizado. Archivo ausente o asociación ambigua produce diagnóstico. No se selecciona otro archivo por semejanza silenciosa |
| Resultado de análisis | Identidad del análisis; entradas/contexto versionados; objetos; relaciones; evidencias; diagnósticos; cobertura; estado completo/parcial/fallido | No publica. Cada afirmación verificable necesita evidencia; el resultado conserva las referencias pendientes |
| Solicitud de publicación | DAG; resultado completo; revisión de contribución esperada; identidad de operación | Reemplazo indivisible de aportes propios. Repetición no duplica. Conflicto de revisión no sobrescribe un resultado posterior |
| Solicitud de baja | DAG; revisión esperada; motivo | Operación explícita. Retira aportes vigentes del DAG, conserva consumidores/aportes ajenos; no borra datos físicos |
| Consulta | Tabla/columna identificada; dirección upstream/downstream; tipo de consulta; cambio opcional y revisión de catálogo | Cambio admite operaciones tipadas de tabla/columna, incluida adición, eliminación, renombrado y modificación; ver experiencia web. Identidad ambigua exige desambiguación; no se resuelve por nombre corto |
| Resultado de consulta | Objeto/cambio; revisión; candidatos; caminos; motivo; evidencias; vigencia; límites y truncamiento | Vacío con cobertura incompleta se comunica como desconocido en ese ámbito, nunca como impacto universalmente ausente |
| Contexto y salida de explicación | Resultado de consulta; evidencias permitidas; versión de prompt/configuración; afirmaciones y referencias; recomendaciones y estado de verificación | Referencia inexistente o insuficiente invalida la afirmación como hecho. Fallo conserva resultado estructurado |

### Modelo lógico de relaciones

Un objeto tiene clase (tabla, vista, columna o intermedio), identidad cualificada y ámbito. Los identificadores técnicos deben distinguir fuente/plataforma/catálogo/esquema cuando esos datos existan; una parte desconocida no se inventa. La columna pertenece a un objeto y la escritura puede incluir posición y partición. Una STG con DDL físico no se trata automáticamente como temporal de sesión.

Cada relación conserva origen, destino, tipo, DAG/tarea/sentencia que la aporta, expresión cuando aplique y referencias de evidencia. Una relación agregada puede tener varios aportes; retirar uno no elimina los demás. La evidencia identifica archivo, hash, localización, fragmento original y sustituciones relevantes. El texto reserializado por el parser no reemplaza el SQL original.

Distingo dependencias de valor, condición de selección/unión/agrupación/ventana, lectura/escritura de objeto y orden de tareas. En un CASE o IF conservo condiciones y valores que determinan el resultado; las constantes y NULL no originan dependencias ficticias. Un UNION alinea ramas por posición; un INSERT necesita mapeo al esquema destino. Las reglas se concretan en el [diseño técnico](diseno-tecnico.md) y sus esperados en el recorrido sintético.

## 6. Vista de ejecución

### Invariantes y campos del contrato de análisis

Refinamiento PROPUESTO de los contratos anteriores, independiente de serialización y almacenamiento:

| Registro | Campos obligatorios | Invariante |
|---|---|---|
| Análisis | analysis_id, dag_id, input_manifest, context_revision, analyzer_revision, coverage_profile, status | La identidad depende de entradas y contexto normalizados y versionados; fecha sola no identifica una revisión |
| Objeto | object_id, kind, namespace, qualified_name, scope | Identidad incompleta/ambigua se conserva como referencia sin resolver, no se fusiona por nombre corto |
| Columna | column_id, object_id, name; ordinal cuando se usa escritura posicional | La posición pertenece al esquema de esa revisión; alias de proyección no prueba identidad del destino |
| Operación | operation_id, dag_id, task_id, statement_id, kind, evidence_refs | Identidad estable dentro del análisis; orden de sentencia no equivale a dependencia entre objetos |
| Relación de valor | relation_id, source_column_id, target_column_id, operation_id, evidence_refs | Ambos extremos y evidencia resueltos; una expresión puede producir varias relaciones |
| Condición | condition_id, source_refs, role, operation_id, expression_target opcional, evidence_refs | Rol JOIN/FILTER/GROUP/WINDOW_PARTITION/WINDOW_ORDER/EXPRESSION; especificar si afecta filas o una expresión |
| Lectura/escritura | operation_id, object_id, role, evidence_refs; write_mode/partition_spec cuando consten | No confundir productor global con único productor ni inferir compatibilidad entre escrituras |
| Evidencia | evidence_id, artifact_id, content_hash, location, original_excerpt, substitutions | La ubicación identifica archivo y sentencia; fragmento reserializado no sustituye al original |
| Diagnóstico | code, phase, affected_refs, message, completeness_effect, evidence_refs disponibles | Cada causa explica qué quedó sin resolver; ausencia de evidencia localizable se declara |
| Aporte publicado | dag_id, analysis_id, contribution_revision, assertions, expected_previous_revision | Las afirmaciones son propiedad de un aporte; deduplicar una relación no borra su procedencia múltiple |

Enumeraciones, tipos, normalización e IDs por hash quedan fijados en el [diseño técnico](diseno-tecnico.md). El esquema de diagnóstico distingue efectos BLOCKING, WARNING e INFO. La matriz de publicabilidad y alternativas están en [ADR-0001](../adr/ADR-0001-relaciones-y-publicacion.md#ADR-0001); sigue Propuesto.

### Análisis sin framework

1. Inventariar archivos y resolver la configuración declarada sin importar Python. Si hay más de una asociación plausible, emitir diagnóstico y conservar candidatos.
2. Extraer tareas, referencias y bloques SQL. Conservar por separado enlaces Python y `parents` YAML; si difieren, marcar conflicto de orquestación. No decidir cuál ejecuta el framework.
3. Resolver parámetros explícitos conservando su procedencia y expresión original. Fechas calculadas por shell quedan simbólicas salvo valores aportados; no ejecutar el shell.
4. Resolver sentencias dentro de la cobertura elegida y construir relaciones. Si falta esquema destino, conservar lecturas y expresiones conocidas, pero dejar pendientes los vínculos de columnas destino que dependan de posiciones.
5. Clasificar el resultado y devolver diagnósticos. Solo un resultado completo dentro de una cobertura previamente declarada es elegible para publicación.

Propongo distinguir contexto ausente que afecta identidad/mapeo de contexto ausente que solo afecta valores de un filtro. En el primer caso, la porción dependiente no puede presentarse como resuelta; en el segundo puede conservarse linaje estructural con la condición simbólica y límite visible. La matriz de publicabilidad queda seleccionada en el [diseño técnico](diseno-tecnico.md); no se cambia cobertura después de observar un fallo para presentarlo como éxito.

### Publicación y recuperación

El catálogo verifica contrato y revisión esperada antes de activar el reemplazo. Un fallo previo a la activación deja visible la contribución anterior completa; un conflicto devuelve un resultado explícito y exige revaluar sobre la nueva revisión. Sin contribución previa, se informa que no existe resultado publicado. Los diagnósticos del intento no se mezclan con relaciones vigentes como si pertenecieran a la misma versión.

### Consulta y explicación

La consulta fija una revisión al empezar y recorre aristas conservando evidencia por tramo. Controla ciclos mediante identidades visitadas; si aplica un límite de recorrido, lo declara y marca resultado incompleto. El diseño técnico fija límites configurables de protección y declara truncamiento; no son mediciones de capacidad. La explicación recibe ese resultado y sus límites; no completa relaciones por plausibilidad. Una referencia incorrecta o una llamada fallida produce reporte estructurado con explicación no disponible/no validada.

### Recorrido sintético propuesto

El [recorrido sintético 0.1](recorrido-sintetico.md) fija esquemas, operaciones A/B/C, 16 relaciones directas de valor y condiciones separadas. Incluye STG, vista, agregación y una variante de ventana con argumento, partición, orden y marco explícitos. Define esperados de impacto y actualización antes de ejecutar el parser. Es especificación del corpus, no fixtures implementados ni prueba ejecutada.

## 7. Vista de despliegue

CONFIRMADO: ejecución en equipo local, carga manual de archivos y generación de reporte, sin levantar Airflow, Spark ni servicios de nube, según [SRC-017](../README.md#SRC-017). Propongo una aplicación modular con directorio de entrada, espacio separado de resultados preparados y catálogo local. No se escribirá en las fuentes. Selecciono React en navegador y FastAPI en loopback, SQLite local y reporte JSON/HTML con explicación opcional de OpenAI; detalle en el [diseño técnico](diseno-tecnico.md). Una llamada IA externa requiere selección y autorización de consumo; ejecución local no presupone modelo local ni conectividad ilimitada.

## 8. Conceptos transversales

- Seguridad: archivos y comentarios son datos, nunca instrucciones; no importar DAGs ni ejecutar shell. Resolver rutas dentro del paquete permitido, declarar referencias externas y no seguirlas automáticamente. No enviar fuentes empresariales a la integración IA del prototipo.
- Identidad y vigencia: huellas de archivos, versión del analizador, perfil de cobertura, esquemas y parámetros forman la identidad del análisis. Cambiar contexto marca contribuciones dependientes como pendientes de revisión; invalidación por dependencias de contexto y hashes, con revisión requerida visible.
- Diagnósticos: código, severidad, fase, objeto/sentencia, evidencia y efecto sobre completitud. Distinguir entrada inválida, referencia ausente, patrón no soportado, identidad ambigua, esquema faltante y fallo interno. No borrar causas mediante excepciones genéricas.
- Observabilidad: tiempos por fase, errores y versiones; consumo y reintentos cuando exista IA. Se observa el analizador, no logs productivos de pipelines. El diseño técnico fija límites de protección, historial y reintento acotado; no se compromete latencia sin medir.
- Escrituras compartidas: conservar DAG productor y evidencia de partición cuando exista. Compartir tabla destino no equivale a compartir la misma contribución ni a demostrar que las ejecuciones son compatibles.

## 9. Decisiones arquitectónicas

Las decisiones originales mantienen sus estados en [SRC-007](../README.md#SRC-007); no se renumeran. La continuidad sin acceso al framework está confirmada en [SRC-016](../README.md#SRC-016). Los contratos y tratamiento de incertidumbre de este SAD son propuestas.

| Decisión | Estado | Alcance |
|---|---|---|
| [ADR-0001](../adr/ADR-0001-relaciones-y-publicacion.md#ADR-0001) | Propuesto | Relaciones tipadas, procedencia y criterios de publicación |
| [ADR-0002](../adr/ADR-0002-adaptador-estatico.md#ADR-0002) | Propuesto | Adaptación con SQLGlot; entrada CLI evolucionada a web por ADR-0005 |
| [ADR-0003](../adr/ADR-0003-sqlite-y-revisiones.md#ADR-0003) | Propuesto | SQLite y activación atómica de revisiones |
| [ADR-0004](../adr/ADR-0004-reporte-e-ia.md#ADR-0004) | Propuesto | JSON/HTML y explicación IA opcional |
| [ADR-0005](../adr/ADR-0005-interfaz-web-y-trabajos.md#ADR-0005) | Propuesto | Interfaz web y trabajos persistidos |

Ventanas y ejecución local/manual confirmadas en SRC-017; el subconjunto detallado de ventanas sigue Propuesto en la matriz. Selecciono estas alternativas como base técnica con el criterio definido en SRC-018; su aceptación formal sigue pendiente. La confirmación de preferencias no aprueba automáticamente este SAD ni el ADR propuesto.

## 10. Requisitos de calidad

La prioridad es evidencia rastreable y publicación consistente. Escenarios y medidas propuestas: [QS-001](requirements.md#QS-001) y [QS-002](requirements.md#QS-002). Métodos en [validaciones](validation.md); resultados y revisión separados en el [log](validation-log.md). Latencia, volumen y costos permanecen sin umbrales inventados.

## 11. Riesgos y deuda técnica

| Riesgo | Causa y consecuencia | Mitigación / contingencia | Dueño y valoración |
|---|---|---|---|
| Semántica de framework no observable | Interpretar metadatos estáticos como comportamiento ejecutado produciría conclusiones falsas | Conservar declaraciones y conflictos; limitar afirmaciones a análisis estático | Óscar; impacto alto propuesto sobre confiabilidad. Probabilidad no cuantificada; riesgo residual a revisar |
| Linaje incompleto de columnas | DDL ausente, subconsultas/ventanas y defectos del adaptador pueden omitir dependencias | Corpus sintético con esperados, matriz por patrón, diagnósticos y publicación controlada | Óscar; impacto alto sobre utilidad, sin aceptación residual |
| Estado obsoleto o mezcla de aportes | Actualización parcial o identidad incorrecta puede contaminar consultas | Reemplazo unitario por DAG, revisión esperada y pruebas de fallo/aportes compartidos | Óscar; transacción SQLite seleccionada, ensayo pendiente |
| Esfuerzo mayor a capacidad | Cobertura técnica y disponibilidad sin cuantificar | Estimar por patrón y contrato tras priorizar; acotar patrones mediante decisión expresa | Óscar; H1-03 pendiente |
| Acceso, cuota o costo de IA | Interrupción de explicaciones o consumo mayor al previsto | Presupuesto y límites antes de consumir; conservar resultados deterministas ante fallo; VAL-005 | Óscar; presupuesto propuesto, consumo sin autorizar |
| Comparación manual sesgada | Familiaridad con el corpus o tiempos incompletos producen beneficios aparentes | Mismo corpus y pregunta, declarar orden y aprendizaje, separar descubrimiento, mantenimiento y consulta; cinco casos no prueban ahorro general | Óscar; protocolo de H3, sin mediciones |
| Explicación convincente sin soporte | Pérdida de evidencia o inferencia no marcada | Comprobar citas y respaldo, comunicar límites y conservar resultado estructurado | Óscar; impacto alto propuesto; prueba H2 |
| Exposición de archivos o acceso local indebido | Carga arbitraria, rutas maliciosas o API accesible fuera del equipo | Selección explícita, validación de rutas/cuotas, loopback, sesión local y CSRF; datos sintéticos para prototipo | Óscar; impacto alto propuesto; controles por verificar en H2 |
| Pérdida o crecimiento de datos locales | Historial sin poda y backup incompleto de SQLite/archivos | Workspace delimitado, límites de entrada, backup con manifiesto; restauración prevista | Óscar; impacto alto propuesto; capacidad y recuperación por medir en H2 |
| Grafo difícil de interpretar | Densidad de nodos y diferencias entre representación y resultado | Vista acotada, límites visibles, tabla y evidencia de la misma revisión | Óscar; impacto medio propuesto; usabilidad por evaluar en H2 |

La inspección ya identificó defectos en el extractor; son hallazgos observados, no riesgos hipotéticos. No se atribuye su corrección a este diseño. Seguimiento: [informe](../../00-pdi/fuentes/arquitectura/revision-parser-y-dags.md) y [log](validation-log.md).

## 12. Glosario

| Término | Significado |
|---|---|
| Aporte | Evidencia y relaciones atribuibles a un DAG y una revisión |
| Completo | Resuelto respecto de una cobertura fijada previamente; no universal |
| Verificado | Sustentado por análisis y evidencia; no prueba de ejecución |
| Condición | Dependencia que interviene en selección, unión, agrupación, ventana o control de una expresión |
| Candidato de impacto | Objeto o proceso a revisar con un motivo; no fallo demostrado |
| Contexto simbólico | Parámetro o expresión cuyo valor no está disponible, conservado sin inventarlo |

## Viabilidad conceptual de la propuesta

Evaluación de diseño, no evidencia de funcionamiento: el stack web conserva los servicios Python del analizador; SQLite reduce operación en uso local; el trabajo persistido desacopla la petición HTTP del análisis; consulta y descarga fijan revisión; la IA queda fuera del camino necesario para obtener hechos. Cada elección tiene alternativa y consecuencia en los ADR. La inspección previa fundamenta adaptar el extractor, pero no demuestra aún soporte del perfil SQL.

Propongo esta solución para una construcción acotada bajo estos límites, sin afirmar capacidad medida, recuperación probada ni calidad IA evaluada. Los escenarios de revisión y sus conclusiones se registran en VAL-008. Si cambia uso local/individual o se exige cobertura universal, se reabre la propuesta; no se escala por inferencia.

## Anexos opcionales

Estimación H1-03: paquetes de trabajo propuestos — adaptador de archivos, resolución de columnas por patrón, publicación, consultas, explicación y evaluación. Frontend, API, carga, trabajos y visualización se añaden por SRC-019. Horas y disponibilidad pendientes. USD 20 continúa como presupuesto API propuesto, no gasto autorizado; el costo se calculará como tokens de entrada y salida por tarifas vigentes del modelo seleccionado, registrando también reintentos; no se autoriza consumo. No se requiere migración productiva para este prototipo. Gobierno: mantengo separadas la propuesta, su revisión y su aprobación. Registraré cualquier aprobación sobre una versión identificada y con su evidencia.
