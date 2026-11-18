# Diseño técnico del MVP local

Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17  
Estado: Propuesta para revisión  
Versión: 1.2

Concreto las decisiones técnicas conforme al criterio definido en [SRC-018](../README.md#SRC-018). Esta es la base seleccionada para implementar; no acredita capacidades probadas ni aceptación de puertas. El [SAD](sad.md) mantiene estrategia y riesgos; este documento mantiene los contratos técnicos. Revisar ante cambios de cobertura, dependencias, esquema, proveedor o resultados.

## Aplicación y entrada

Una aplicación Python modular expuesta por FastAPI a una SPA React local, con entorno virtual y dependencias fijadas en el primer build reproducible. La web sustituye CLI como entrada del usuario. Requiere servidor local, sin Docker, Airflow ni Spark. [Experiencia web](experiencia-web.md) fija transporte, trabajos y pantallas. Los módulos corresponden a EL-001 a EL-004 del SAD. Reutilizo SQLGlot para AST SQL detrás de un adaptador propio; extraigo Python con AST estático y YAML con carga segura. No importo los DAGs. Sustituyo las heurísticas defectuosas del extractor, conservando sus convenciones útiles como referencia.

El paquete cargado desde el selector de carpeta se completa desde la web y declara `dag_id`, namespace, archivo de configuración, inventario de archivos y sus hashes, parámetros, esquemas ordenados y perfil `spark-static-v1`. No infiero la asociación por orden de directorio ni regex del nombre. Una ruta debe permanecer dentro del paquete autorizado incluso después de resolver enlaces; cualquier referencia externa requiere entrada explícita. El shell se conserva como texto: solo se admiten asignaciones literales reconocidas, sin evaluar comandos.

Fijo el dialecto de análisis como Spark de SQLGlot; no declaro compatibilidad certificada con una versión de Spark productiva desconocida. Conservo SQL original y localización antes de normalizar. Parseo todas las sentencias con errores visibles. No uso recuperación silenciosa ni regex que borre SET dentro de literales. La resolución de scopes, columnas, esquemas y destinos se valida por separado: el AST y la función de lineage de la biblioteca no son el contrato completo del sistema.

Los parámetros se sustituyen por tokens y con procedencia, nunca mediante reemplazos globales sucesivos. Primero se resuelve un nombre exacto; una convención legado de prefijo y sufijo solo se permite si el manifiesto declara esa regla y la resolución es única. No se transforma silenciosamente `$xyz` usando `$x`. Parámetro en nombre de tabla, columna o estructura sin resolver bloquea publicación; escalar de filtro puede quedar simbólico si no afecta resolución estructural.

## Cobertura seleccionada

La [matriz del recorrido](recorrido-sintetico.md) es el inventario de familias y esperados. Selecciono proyección/alias/aritmética, constantes/NULL, CASE/IF, INNER y LEFT JOIN con ON, WHERE, SUM/COUNT y GROUP BY, subconsulta derivada no correlacionada, UNION ALL, CREATE con esquema explícito, INSERT INTO/OVERWRITE con mapeo conocido y vista SELECT estática.

Ventanas: SUM, COUNT y ROW_NUMBER con PARTITION BY y ORDER BY; para agregados admito marco ROWS explícito con límites constantes, CURRENT ROW y UNBOUNDED PRECEDING/FOLLOWING cuando la combinación sea válida. ROW_NUMBER conserva partición y orden; no requiere inventar argumento de valor ni marco. En esta primera cobertura, ventanas agregadas con marco implícito, RANGE/GROUPS, funciones no enumeradas o reglas de marco no resueltas producen diagnóstico de patrón no soportado. No se promete interpretar por omisión la semántica del framework o del motor.

CTE, SELECT *, subconsultas correlacionadas, SQL dinámico y funciones no enumeradas quedan fuera de `spark-static-v1`; COUNT(*) sí está incluido como cardinalidad. Es una delimitación inicial revisable, no una afirmación de incapacidad de SQLGlot. Encontrar un patrón fuera de cobertura dentro del paquete genera resultado parcial, no éxito por exclusión retrospectiva.

El mapeo de INSERT usa lista destino explícita o esquema ordenado aportado. Distingue particiones estáticas y dinámicas; una partición estática no consume posición de SELECT, una dinámica sí cuando el dialecto/contexto lo establezca. Ambigüedad bloquea el mapeo. Las transformaciones aritméticas conservan dependencias; no calculan compatibilidad exacta de tipos. En fixtures usaré DECIMAL(18,2), INTEGER, STRING y BOOLEAN como declaraciones sintéticas, sin inferir tipos empresariales.

## Contratos e identidad

Serialización JSON UTF-8 con `schema_version: 1`; campos obligatorios del SAD, arrays vacíos explícitos y null solo para campos declarados opcionales. Identidades y referencias son cadenas; ordinales enteros desde 1; fechas UTC ISO 8601. `status` admite complete/partial/failed. Un resultado complete exige referencias consistentes, evidencias localizables y ausencia de diagnósticos bloqueantes. El contrato se validará antes de persistir/publicar.

`analysis_id` es SHA-256 del JSON canónico de manifiesto ordenado por ruta relativa, hashes de contenido, parámetros con rol, esquemas y reglas de nombres, perfil, versión del contrato y revisión del analizador/dependencias. Excluye fecha, ruta absoluta y orden de descubrimiento. No contiene secretos. `dag_id` es declarado y estable; mover un paquete no crea otro DAG.

`object_id` deriva de plataforma, catálogo, esquema, nombre y scope declarados. El manifiesto fija sensibilidad a mayúsculas; se conserva texto original. Nunca se fusionan nombres sin contexto. Una columna deriva de object_id y nombre normalizado; su ordinal pertenece a una revisión de esquema. Un cambio de nombre crea otra identidad, sin inferir equivalencias. Intermedios usan analysis_id, sentencia y scope. Operaciones, evidencias y relaciones derivan del análisis y su localizador/tipo/extremos; dos DAGs pueden aportar la misma relación lógica conservando ambos aportes.

Roles de condición: JOIN, FILTER, GROUP, WINDOW_PARTITION, WINDOW_ORDER y EXPRESSION. Lectura/escritura y orden declarado de tareas tienen tipos distintos. COUNT(*) y ROW_NUMBER registran dependencia de filas; no crean columna ficticia. CASE/IF asocia condición a su expresión destino. El marco de ventana se conserva como metadato y evidencia.

| Diagnóstico | Efecto | Publicación |
|---|---|---|
| INPUT_MISSING, AMBIGUOUS_REFERENCE, SCHEMA_MISSING, PARAMETER_STRUCTURAL, SQL_UNSUPPORTED, SQL_PARSE_ERROR | partial; referencias afectadas sin resolver | No |
| CONTRACT_INVALID, INTERNAL_ERROR | failed; resultado inconsistente o no disponible | No |
| SYMBOLIC_SCALAR | warning; valor de filtro desconocido, estructura resuelta | Sí, si no existe otra causa bloqueante |
| ORCHESTRATION_CONFLICT | warning; conservar ambas declaraciones, precedencia desconocida | Solo si tareas y SQL están íntegramente asociados; si la asociación depende del conflicto, partial |
| EXTERNAL_LINEAGE_BOUNDARY | warning; esquema conocido pero productor fuera del catálogo | Sí; origen más allá de esa frontera desconocido |

Una causa no enumerada bloquea hasta clasificarla. Los diagnósticos conservan fase, afectados, evidencia y efecto; el usuario no puede convertir un partial en complete ocultando mensajes.

## SQLite, publicación y vigencia

Selecciono un archivo SQLite en disco local, un escritor y lectores breves; journal DELETE, foreign_keys activado por conexión y transacciones explícitas. No ubicarlo en unidad de red o carpeta sincronizada. No necesito una base de grafos para recorrer relaciones tipadas del corpus inicial.

| Entidad lógica | Clave y contenido | Restricción |
|---|---|---|
| analyses | analysis_id; manifiesto/contexto/versiones/estado | Resultado inmutable |
| artifacts, evidence | hash y evidence_id; archivo/localización/fragmento | Evidencia pertenece al análisis |
| objects, schemas, operations, assertions | Identidades, versiones, tipos y relaciones del análisis | Extremos resueltos y procedencia; no mezclar esquema de otra revisión |
| catalog_revisions | revision_id entero, padre, fecha y operación | Revisión inmutable después de activar |
| revision_members | revision_id, dag_id, analysis_id | Una contribución activa por DAG en cada revisión |
| catalog_head | única fila; revision_id | Cambio transaccional del puntero vigente |
| attempts, context_dependencies, invalidations | Intentos y contexto usado; estado de vigencia | Separados de relaciones verificadas |
| report_runs | revisión consultada, consulta, hashes de salidas y configuración | No reescribe hechos del catálogo |

Se prepara el análisis fuera de la transacción de activación. Publicar abre BEGIN IMMEDIATE, comprueba revisión global y contribución previa esperadas, elegibilidad y contexto; crea revisión con mapa completo de miembros, reemplaza solo el DAG solicitado y activa catalog_head en el mismo COMMIT. Un error hace ROLLBACK. Si hubo otra publicación, devuelve conflicto para revaluar; no reintenta sobrescribiendo. Repetir la misma operación/analysis_id ya vigente devuelve la revisión existente. La baja crea otra revisión sin el miembro; no borra aportes ni objetos físicos ajenos.

La consulta fija revision_id y lee sus miembros en una transacción breve; obtiene un resultado autocontenido antes de llamar IA. Una nueva publicación no cambia el reporte en preparación. Las revisiones de catálogo son snapshots de aplicación, no uso de la API C de snapshots de SQLite.

Conservo todas las revisiones durante el MVP, sin limpieza automática. Copia de seguridad mediante API de backup de SQLite con aplicación sin publicar, incluyendo manifiesto y contenidos de paquetes/resultados referenciados en el workspace web; restauración a archivo nuevo y comprobación de integridad antes de activarlo. No copiar solo un archivo abierto ignorando journal. VAL-003 probará fallo, reapertura y restauración.

Los esquemas/parámetros aportados tienen hash; registro cuáles consume cada análisis. Una nueva revisión de contexto marca afectados y dependientes transitivos como revisión requerida. Un cambio solo de dependencia de valor recompone caminos, sin fingir cambio de esquema. Una baja de productor mantiene lecturas ajenas con frontera visible. Los reportes muestran contexto usado, invalidaciones y último intento: conservar el último completo no demuestra vigencia actual. No existe vigilancia automática de fuentes externas; los cambios no cargados son desconocidos.

## Recorrido e impacto

Recorro en anchura la revisión elegida con estados `(nodo, motivo)`, visitados y predecesores. Valor propaga por valor; condición de expresión afecta su destino; JOIN/FILTER/GROUP y cardinalidad propagan influencia sobre filas a operaciones consumidoras, conservando el motivo. Consumo de tabla y orquestación se reportan aparte, sin convertirlos en dependencias de todas las columnas. Upstream invierte las relaciones conservando su tipo.

Defaults de protección seleccionados: 64 saltos, 50 000 relaciones examinadas y 1 000 candidatos por consulta. Son configurables y no son objetivos de rendimiento ni volumen confirmado. Al alcanzar cualquier límite, `truncated=true`, motivo y frontera; nunca concluir ausencia de impacto. Conservo subgrafo de evidencia y un camino testigo por candidato/motivo, sin enumerar infinitos caminos cíclicos. Reporto alternativas existentes en ese subgrafo sin prometer enumeración exhaustiva.

Renombrado/eliminación de tabla o columna identifican usos sintácticos y efectos transitivos candidatos. Adición y modificación de columna evalúan esquema hipotético y compatibilidad por revisar según [contrato web](experiencia-web.md), sin mutar catálogo. Escrituras sin lista destino conservan dependencia explícita de esquema, orden y aridad. Cambio de tipo devuelve usos y expresiones a revisar; no certifica coerciones ni fallos en Spark. Una consulta ambigua se rechaza con candidatos cualificados. La ausencia de resultado se limita a revisión, cobertura y fronteras conocidas.

## Reporte e IA

JSON es el resultado canónico; HTML autocontenido es su representación legible, sin servidor ni recursos externos, con contenido escapado. Incluye consulta, revisión, objetos y motivos, caminos/evidencia, cobertura, diagnósticos, vigencia y explicación opcional. No ejecuta SQL, scripts o HTML provenientes de entradas. Rutas y fragmentos empresariales no entran al corpus del prototipo.

Selecciono OpenAI Responses API detrás de una interfaz de proveedor, con snapshot `gpt-5.4-mini-2026-03-17` como candidato inicial reproducible. La selección no acredita calidad ni autoriza consumo. Modo predeterminado offline: reporte determinista completo; IA solo al habilitar credencial y presupuesto autorizado. Solo se envían resultados/evidencias sintéticos permitidos, sin archivos completos, herramientas, ejecución ni acceso de escritura al catálogo.

La salida estructurada contiene explanation, evidence_refs, recommendations y limitations. Valido esquema, referencias pertenecientes al resultado y estados refusal/incomplete antes de mostrarla. Los hechos y caminos se renderizan desde JSON determinista; la prosa generada se etiqueta explicación no verificada hasta revisión de respaldo. Una cita existente no prueba que la frase sea correcta. Recomendaciones se mantienen como propuestas. Rechazo, salida inválida, falta de respaldo o fallo conserva reporte sin explicación validada.

Defaults propuestos de operación: timeout 60 s, entrada máxima 12 000 tokens y salida máxima 2 000, una llamada y como máximo un reintento ante fallo transitorio sujeto a presupuesto; nunca reintentar por contenido rechazado. Si no cabe contexto suficiente, no truncar evidencia silenciosamente: emitir reporte determinista y explicación no disponible. Registrar modelo/snapshot, prompt, hashes de entrada/salida, tokens, costo estimado y error. Credencial fuera de archivos y logs. USD 20 sigue como techo propuesto, no autorizado. No se usa RAG, agentes ni búsqueda vectorial en este MVP.

## Secuencia futura de implementación — fuera de esta entrega

Esta secuencia orienta H1-03/H2/H3; no es una lista de pendientes para cerrar la propuesta H1-02. [SRC-020](../README.md#SRC-020) limita la actividad actual a arquitectura y revisión.

1. Congelar fixtures sintéticos y esperados del recorrido y diez cambios; fijar entorno/dependencias reproducibles.
2. Implementar adaptador y contrato; VAL-001 y VAL-002 por patrón, incluyendo ventanas.
3. Implementar SQLite/publicación/baja/backup; VAL-003 con fallos inducidos.
4. Implementar recorridos y JSON/HTML; VAL-004 sobre diez cambios y VAL-007 sobre cambios de esquema. Construir SPA/API, carga y trabajos, y verificar VAL-006.
5. Integrar explicación con doble de prueba; VAL-005. Evaluar modelo real solo con consumo autorizado.
6. Ejecutar cinco comparaciones manuales del PDI con casos congelados y registrar tiempo, corrección y límites, sin sustituirlas por pruebas unitarias.

No estimo horas disponibles ni prometo fechas sin capacidad real: esa estimación corresponde a H1-03. Para revisar H1-02 ya quedan elegidos componentes, contratos, cobertura, tecnología, mecanismos, alternativas y validaciones. La propuesta queda lista para revisión formal según [criterios de entrega](revision-propuesta.md#criterio-de-entrega-y-aceptación). La revisión visual pertenece a esa revisión; fixtures, builds y pruebas no son condición de entrega documental. Las puertas formales se evalúan aparte, sin declarar terminado H2 ni aprobaciones.
