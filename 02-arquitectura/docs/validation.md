# Definición de validaciones — Linaje e impacto asistido por IA

Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17

Defino los métodos y criterios de éxito antes de ejecutar las validaciones. Estados Planificada y criterios PROPUESTOS: no son pruebas ya realizadas ni aceptación de los CA del PDI. Disparadores: cambios de contrato, cobertura, parser, datos, almacenamiento o modelo.

## Separación por etapa

La [propuesta 1.0](revision-propuesta.md) se entrega para revisión documental, conforme a SRC-020. Esta clasificación aplica a todos los registros de este archivo:

| Validación | Etapa y propósito | ¿Condición para entregar la propuesta? |
|---|---|---|
| VAL-001 a VAL-007 | H2: comprobar implementación, contratos ejecutables y comportamiento | No; permanecen Planificada y No ejecutado |
| VAL-008 | H1-02: coherencia, cobertura de diseño, alternativas y escenarios documentales | Sí, dentro de su alcance textual; no aprueba puertas |
| Cinco comparaciones previstas | H3: evaluar resultados y esfuerzo manual/asistido | No |

La revisión formal de contenido y legibilidad de diagramas forma parte de aceptar el entregable, no de construir el producto. Las puertas se aplican según evidencia necesaria para la decisión concreta: no reclasifico pruebas fallidas como conformes ni doy por aprobada una excepción sin evidencia. Esta propuesta no exige PoC; si la revisión identifica una incertidumbre crítica, se registra la condición específica antes de aprobar la decisión afectada.

## Estrategia

El [recorrido sintético 0.1](recorrido-sintetico.md) fija esperados de diseño para el núcleo A/B/C y la variante de ventanas. Está preparado como especificación; los fixtures y ejecuciones aún no existen. Incluir ventanas y operar localmente está confirmado en [SRC-017](../README.md#SRC-017). No se interpreta la inclusión como soporte universal del dialecto.

Priorizar extracción sin ejecución, corrección de relaciones, actualización y fidelidad de explicación. Mantener evidencia por componente y tarea completa. Las inspecciones previas viven en el [informe](../revision-parser-y-dags.md); no se presentan como ejecución retrospectiva de estas validaciones. Las pruebas usarán corpus sintético creado y versionado, sin reconstruir ni ejecutar el framework empresarial.

Pruebas de carga/costo quedan pendientes de corpus ejecutable y mediciones; entorno y proveedor ya seleccionados en el diseño técnico. No se requieren pruebas de infraestructura productiva para el alcance actual. Las cinco comparaciones manuales de H3 mantienen su protocolo en [comparación manual/asistida](../../04-evaluacion/comparacion-manual-vs-solucion.md); no se realizan en esta ronda.

<a id="VAL-001"></a>
## VAL-001 — Ingesta estática y diagnósticos

Estado: Planificada  
Qué: Probar descubrimiento y extracción sin dependencias de ejecución.  
Por-qué: Evitar omisiones por nombres, orden de archivos o importación de código.  
Referencias: FR-001; [EL-001](sad.md#EL-001); [requisitos](requirements.md). Decisiones: [índice del SAD](sad.md#9-decisiones-arquitectónicas).  
Método: Crear variantes sintéticas de nombre y orden; referencias ausentes, comillas y SQL con literales que contienen SET. Incluir un artefacto centinela que solo produciría un efecto si se ejecutara. Comparar SQL original, referencias y diagnósticos con esperados.  
Condiciones: corpus y esperados congelados antes de medir, implementación identificada, versiones registradas y ejecución aislada. Corpus/implementación aún pendientes.  
Éxito: PROPUESTO. Misma entrada semántica produce inventario equivalente; no se ejecuta el centinela; no se alteran literales; cada referencia ausente produce diagnóstico localizable.  
Responsable: Óscar Hernández Barrón  
Cuándo: durante H2, antes de declarar validada la cobertura afectada; no se fijan fechas nuevas.  
Ejecuciones: No ejecutado. Consultar [log](validation-log.md).

<a id="VAL-002"></a>
## VAL-002 — Linaje y evidencia con contexto incompleto

Estado: Planificada  
Qué: Comparar relaciones por patrón y el tratamiento de DDL/parámetros ausentes.  
Por-qué: Evitar inventar vínculos posicionales o confundir falta de contexto con ausencia de dependencia.  
Referencias: FR-002 y NFR-001; QS-001; [EL-001](sad.md#EL-001); [requisitos](requirements.md). Decisiones: [índice del SAD](sad.md#9-decisiones-arquitectónicas).  
Método: Materializar el recorrido A/B/C con STG y vista, sus 16 relaciones directas de valor y condiciones separadas. Repetir retirando DDL destino, un parámetro de identidad y una fecha usada solo como valor de filtro. Añadir la variante de SUM con partición/orden/marco, COUNT(*) y ROW_NUMBER con esperados independientes; distinguir argumento, cardinalidad y condiciones de ventana. Evaluar alias, subconsulta, agrupación y UNION en casos separados. Variantes seleccionadas en el diseño técnico; esperados a congelar con los fixtures.
Condiciones: corpus y esperados congelados antes de medir, implementación identificada, versiones registradas y ejecución aislada. Corpus/implementación aún pendientes.  
Éxito: PROPUESTO. Las relaciones declaradas verificadas tienen soporte; pendientes corresponden a ausencias introducidas. Para un patrón incluido, esperado y obtenido coinciden por clase o se registra fallo, sin compensarlo con otros patrones. No se declara soporte de todos los candidatos por definición.  
Responsable: Óscar Hernández Barrón  
Cuándo: durante H2, antes de declarar validada la cobertura afectada; no se fijan fechas nuevas.  
Ejecuciones: No ejecutado. Consultar [log](validation-log.md).

<a id="VAL-003"></a>
## VAL-003 — Actualización, baja y fallos

Estado: Planificada  
Qué: Probar reemplazo por DAG, repetición, fallo, revisión esperada y baja.  
Por-qué: Proteger la integridad y vigencia del conocimiento compartido.  
Referencias: FR-003 y NFR-002; QS-002; [EL-002](sad.md#EL-002); [requisitos](requirements.md). Decisiones: [índice del SAD](sad.md#9-decisiones-arquitectónicas).  
Método: Preparar aportes A/B compartidos. Repetir A, retirar una relación solo de A, actualizar, bajar A y simular análisis parcial/fallido. Intercalar lecturas y fallo antes de activar. Publicar con revisión obsoleta. Comparar conjuntos exactos de aportes y revisiones.  
Condiciones: corpus y esperados congelados antes de medir, implementación identificada, versiones registradas y ejecución aislada. Corpus/implementación aún pendientes.  
Éxito: PROPUESTO. No hay duplicados ni borrado de aportes ajenos. No se observan estados mixtos en los puntos ensayados. Fallo conserva anterior completo con aviso; conflicto no sobrescribe. Sin anterior se declara ausencia.  
Responsable: Óscar Hernández Barrón  
Cuándo: durante H2, antes de declarar validada la cobertura afectada; no se fijan fechas nuevas.  
Ejecuciones: No ejecutado. Consultar [log](validation-log.md).

<a id="VAL-004"></a>
## VAL-004 — Candidatos de impacto y caminos

Estado: Planificada  
Qué: Evaluar recorridos y motivos ante cambios.  
Por-qué: Evitar propagación excesiva de columna a todo consumidor y preservar compromisos del PDI.  
Referencias: FR-004; [EL-003](sad.md#EL-003); [requisitos](requirements.md). Decisiones: [índice del SAD](sad.md#9-decisiones-arquitectónicas).  
Método: Usar los diez cambios definidos más abajo, sin sustituirlos por pruebas de componentes. Incluir consumidores de valor y solo de tabla, ciclos y corte de recorrido; registrar por caso esperado/obtenido y límites.  
Condiciones: corpus y esperados congelados antes de medir, implementación identificada, versiones registradas y ejecución aislada. Corpus/implementación aún pendientes.  
Éxito: PROPUESTO. Cada candidato explica un camino sustentado y motivo; límites de recorrido son visibles. Registrar falsos positivos/negativos por caso. Para el corpus cerrado seleccionado, conjuntos de candidatos y motivos deben coincidir con esperados, sin falsos positivos/negativos; cualquier diferencia se registra como fallo. Este criterio no afirma cobertura universal.  
Responsable: Óscar Hernández Barrón  
Cuándo: durante H2, antes de declarar validada la cobertura afectada; no se fijan fechas nuevas.  
Ejecuciones: No ejecutado. Consultar [log](validation-log.md).

<a id="VAL-005"></a>
## VAL-005 — Explicación con evidencia y degradación

Estado: Planificada  
Qué: Evaluar respaldo, aislamiento y fallback.  
Por-qué: Evitar que texto convincente agregue hechos o que un fallo elimine evidencia.  
Referencias: FR-005; [EL-004](sad.md#EL-004); [requisitos](requirements.md). Decisiones: [índice del SAD](sad.md#9-decisiones-arquitectónicas).  
Método: Usar resultados sintéticos fijos; generar variantes con cita inexistente, cita irrelevante, instrucción maliciosa en comentario y fallo del proveedor mediante doble de prueba. Revisar afirmaciones y comparar catálogo antes/después. Prueba real del modelo seleccionado después de autorizar consumo y disponer del entorno.  
Condiciones: corpus y esperados congelados antes de medir, implementación identificada, versiones registradas y ejecución aislada. Corpus/implementación aún pendientes.  
Éxito: PROPUESTO. No se aceptan afirmaciones no sustentadas como hechos; el catálogo no cambia. Resultado estructurado disponible ante fallo. Evaluar claridad por separado; un doble no acredita calidad de un modelo real.  
Responsable: Óscar Hernández Barrón  
Cuándo: durante H2, antes de declarar validada la cobertura afectada; no se fijan fechas nuevas.  
Ejecuciones: No ejecutado. Consultar [log](validation-log.md).



## Diez cambios de impacto seleccionados

Base: [recorrido A/B/C](recorrido-sintetico.md); b2 se publica solo en los casos 9 y 10. Cambios hipotéticos aislados sobre la misma base, sin ejecución de pipelines. En renombrados se evalúa el nombre anterior y el propuesto; no se modifica el catálogo.

| Caso | Cambio | Esperado principal |
|---|---|---|
| 1 | Eliminar raw.ventas.precio | Valor hacia total en STG, curated, vista y resumen; C solo consumidor de tabla |
| 2 | Renombrar raw.ventas.cantidad a unidades | Mismo camino de valor de total; identificar referencia sintáctica pendiente de actualizar |
| 3 | Cambiar tipo raw.clientes.activo de booleano a texto | Filtro en a1 e influencia de filas hacia B/C; sin afirmar compatibilidad |
| 4 | Eliminar raw.clientes.region | Valor hacia region de STG, curated y C; no hacia total de B |
| 5 | Renombrar curated.ventas.total a importe | Usos en vista y resumen; C sin dependencia de ese valor |
| 6 | Cambiar tipo curated.v_importes.total de decimal a texto | SUM y total_ventas de B como candidatos; no fallo demostrado |
| 7 | Eliminar curated.ventas.cliente_id | Valor en vista, resumen y C; GROUP de B como motivo diferenciado |
| 8 | Renombrar curated.ventas a ventas_v2 | Vista y C como consumidores directos; B transitivo vía vista; no reescribir fuentes |
| 9 | Cambiar tipo curated.ventas.venta_id con b2 | Proyección de C/b2 y orden de ventana de acumulado; motivos separados |
| 10 | Eliminar curated.ventas.total con b2 | Vista, resumen y argumento de acumulado; no valor de ROW_NUMBER ni de C |

Al crear fixtures, expandir estos esperados a conjuntos completos de IDs y evidencias antes de correr la solución. La medida es exactitud por caso/motivo, no promedio que oculte un patrón fallido. Ciclos, truncamiento, ambigüedad y contexto ausente son pruebas adicionales, no reemplazan estos diez cambios.

Las cinco comparaciones manuales usarán los casos 1, 3, 5, 8 y 9: mismo paquete/revisión/pregunta, solución esperada congelada y registro de tiempo, errores, omisiones y ayuda recibida. Alternar orden manual/asistido y declarar aprendizaje del único evaluador; no prometer significancia estadística ni ahorro antes de medir. Aplicar el [protocolo de comparación](../../04-evaluacion/comparacion-manual-vs-solucion.md) y los criterios de registro y medición de este plan.


## Registro y medición de las ejecuciones

Conservo los criterios útiles del protocolo inicial del PDI en este plan único. Antes de medir, fijo corpus, esquemas, versiones y resultados esperados independientes de la salida del sistema. Distingo ausencia comprobada dentro del corpus de información desconocida fuera de él.

Registro por caso entrada, esperado, obtenido, evidencia, referencias no resueltas, versiones, duración y consumo cuando aplique. Separo linaje de tablas, columnas y condiciones; indico denominadores y N/A cuando no proceda una medida, sin convertir datos ausentes en ceros. Registro fallos y correcciones y repito los casos afectados. Una relación verificada no demuestra un fallo de ejecución de un pipeline.

Guardo la evidencia y el reporte en [resultados](../../04-evaluacion/resultados/README.md), las cinco comparaciones y su control de sesgos en [comparación manual/asistida](../../04-evaluacion/comparacion-manual-vs-solucion.md), y la interpretación en [conclusiones](../../04-evaluacion/conclusiones.md). Enlazo cada ejecución desde [validation-log.md](validation-log.md). La IA puede cuestionar el protocolo, pero no certifica resultados; diez cambios y cinco comparaciones no demuestran cobertura universal ni ahorro en producción.

## Ensayos específicos de la tecnología seleccionada

VAL-003 incluye fallo antes y después de COMMIT, reapertura del proceso, conflicto entre dos escritores, lector con revisión fijada, invalidación por esquema y backup/restauración. Comparar mapas de miembros, aportes y evidencia; comprobar que el reporte antiguo sigue reproducible y el cambio de contexto es visible.

VAL-005 incluye modo offline, timeout, reintento limitado, presupuesto agotado, salida incompleta/rechazada, cita inexistente e irrelevante y contexto mayor al límite. Revisar respaldo de la prosa por separado del esquema JSON. No se considera validación de un modelo real la prueba con doble.




<a id="VAL-006"></a>
## VAL-006 — Flujo web, trabajos y equivalencia de resultados

Estado: Planificada  
Responsable: Óscar Hernández Barrón  
Referencias: [FR-006](requirements.md#FR-006), [ADR-0005](../adr/ADR-0005-interfaz-web-y-trabajos.md#ADR-0005), [EL-010](../diagrams/README.md#EL-010).
Qué: verificar carga, persistencia, navegación y representación.
Por-qué: evitar omisiones silenciosas y doble publicación.
Método: cargar carpeta sintética y contexto, analizar, recargar navegador durante trabajo, explorar columnas/evidencia, formular cambio, ver resumen y descargar. Comparar result_id, revisión y relaciones de pantalla/tabla/JSON/HTML. Probar doble clic/idempotencia, carga interrumpida, traversal, cuotas, conflicto, parcial, caída y recuperación antes/después de COMMIT. Confirmar que una evaluación de impacto no cambia catálogo.
Condiciones: implementación y fixtures identificados, navegador/viewport registrados; API, UI y proceso de trabajo locales. Revisar build TypeScript, pruebas de contratos y recorrido en navegador.
Éxito: un trabajo lógico por operación idempotente; publicación completa o anterior conservado; sin discrepancias de revisión entre vistas/descarga; error accionable; recarga recupera estado. Grafo acotado indica límites y tabla permite acceder al resultado disponible. Tres temas, teclado/foco, zoom 200%, 1280/768 px y movimiento reducido revisados; contraste de texto normal al menos 4.5:1 como objetivo propuesto.
Cuándo: H2, antes de declarar flujo web operativo.
Ejecuciones: No ejecutado.

<a id="VAL-007"></a>
## VAL-007 — Cambios de esquema ampliados

Estado: Planificada  
Responsable: Óscar Hernández Barrón  
Referencias: [FR-004](requirements.md#FR-004), [ADR-0005](../adr/ADR-0005-interfaz-web-y-trabajos.md#ADR-0005), [EL-003](sad.md#EL-003).
Qué: comprobar eliminación de tabla, adición y modificación de columnas.
Por-qué: evitar dependencias inventadas y ejecución de DDL durante una evaluación.
Método: eliminar curated.ventas (vista, C y B transitivo como candidatos); agregar columna al final y en medio con INSERT posicional frente a lista destino explícita; modificar nulabilidad/default; agrupar renombrado y cambio de tipo; rechazar cambio de columna ya eliminada. Incluir consumidor con SELECT * como frontera conocida fuera de cobertura. Conservar esquema antes/después y motivos por change_id.
Condiciones: fixtures y conjuntos esperados congelados antes de ejecutar; sin framework ni datos productivos.
Éxito: ninguna adición crea valor ficticio; escritura por aridad/posición marcada para revisar; tabla eliminada incluye lectores/escritores conocidos; cobertura desconocida permanece visible; catálogo intacto tras evaluación; escenario contradictorio rechazado.
Cuándo: H2, junto con VAL-004. Los diez cambios originales siguen siendo diez; estos casos amplían cobertura y no reducen el compromiso.
Ejecuciones: No ejecutado.




<a id="VAL-008"></a>
## VAL-008 — Revisión documental de la propuesta de arquitectura

Estado: Lista  
Responsable: Óscar Hernández Barrón. Herramienta de apoyo documental: ArchFlow.
Qué: comprobar alcance, consistencia y trazabilidad de la propuesta 1.0, sin ejecutar el sistema.
Por-qué: permitir revisión y ajustes de arquitectura sin confundir diseño seleccionado con capacidad demostrada.
Referencias: [OBJ-001](sad.md#OBJ-001), FR-001 a FR-006, NFR-001 y NFR-002, ADR-0001 a ADR-0005; [requisitos](requirements.md), [SAD](sad.md), [vistas](../diagrams/README.md).
Método: comprobar enlaces/IDs, contrastar responsabilidades y decisiones, recorrer escenarios de la tabla siguiente, clasificar incertidumbres y separar actividades H1-02/H1-03/H2/H3. Revisar que no se atribuyan pruebas ni aprobaciones inexistentes.
Condiciones: fuentes documentales identificadas por versión; insumos existentes autorizados, sin cambios a PDI ni código del producto.
Éxito: no hay contradicciones textuales conocidas que impidan entender el diseño; cada escenario tiene respuesta, componente y límite; todas las decisiones principales tienen alternativa; las pruebas futuras y el estado de aprobación se distinguen. Legibilidad/renderizado se registran aparte, no se presumen.
Cuándo: preparación de esta entrega y después de ajustes que cambien el diseño.
Ejecuciones: revisión textual en [log](validation-log.md#revision-documental-propuesta-1-0). Aceptación humana pendiente.

### Escenarios de revisión de diseño

| Escenario | Respuesta que debe estar definida | Fuente del diseño | Comprobación posterior |
|---|---|---|---|
| Falta DDL destino | Conservar diagnóstico, mapeo no resuelto y parcial no publicable | EL-001; diseño técnico, publicabilidad | VAL-002 |
| Dos publicaciones compiten | Revisiones esperadas y activación atómica; conflicto no sobrescribe | EL-002; ADR-0003 | VAL-003 |
| Se recarga navegador o reinicia servicio | job_id persistido; consulta recupera estado; interrupción visible y reconciliación por operation_id | EL-006/EL-010; experiencia web | VAL-006 |
| Se agrega columna a tabla con escritura posicional | Escenario hipotético, candidato por aridad/orden; no inventar valor ni ejecutar DDL | EL-003; experiencia web | VAL-007 |
| Falla IA o cita no respalda explicación | Hechos deterministas conservados, explicación no validada/no disponible | EL-004; ADR-0004 | VAL-005 |
| Se alcanza límite de consulta o grafo | Truncamiento/frontera explícitos, tabla sobre mismo resultado; no ausencia universal | EL-003/EL-010; diseño técnico y experiencia web | VAL-004, VAL-006 |
| Paquete contiene rutas no permitidas | Rechazo antes de lectura/publicación, causa visible y workspace delimitado | EL-001/EL-006; experiencia web | VAL-001, VAL-006 |

Estas filas son recorridos razonados sobre documentos. No representan una simulación ejecutada ni resultados del futuro software.
