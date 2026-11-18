# Recorrido sintético y cobertura inicial

Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17  
Estado: Propuesto para revisar antes de implementar  
Versión: 0.1

Defino un ejemplo con resultados esperados independientes del parser para concretar el modelo y preparar la evaluación. No contiene SQL ejecutado ni datos empresariales. Es una especificación del corpus por construir, no un corpus ejecutable ni una prueba aprobada. Deriva de [FR-002](requirements.md#FR-002), [FR-004](requirements.md#FR-004) y las [preferencias confirmadas](../README.md#SRC-017). Revisar ante cambios de semántica, esquemas o cobertura.

## Cobertura inicial

Ventanas incluidas y ejecución local/manual confirmadas. Propongo la granularidad de soporte en esta matriz; incluir una familia no significa soportar todas sus variantes.

| Familia | Subconjunto propuesto para el MVP | Caso esperado / límite |
|---|---|---|
| Proyección, alias y cálculo | Referencias cualificadas, renombrado de salida, constantes, NULL y aritmética | Conservar todos los orígenes; constantes no crean orígenes ficticios |
| CASE/IF | Condición y ramas con expresiones dentro del subconjunto | Condición controla la expresión; sus columnas no se pierden aunque no aporten el valor seleccionado |
| JOIN y filtros | INNER/LEFT con ON explícito; WHERE booleano | Separar flujo de valor de influencia sobre filas; modalidad de unión conservada |
| Agregación | SUM y COUNT con GROUP BY explícito | Conservar valor agregado, claves y dependencia de pertenencia/cardinalidad de filas |
| Subconsultas | SELECT derivado con alias y alcance resoluble | Resolver alias por ámbito; correlación y ambigüedades fuera del subconjunto generan diagnóstico |
| UNION ALL | Ramas con mismo número de posiciones y contexto disponible | Cada posición destino recoge orígenes de cada rama; no basta recopilar tablas |
| STG y escrituras | CREATE con DDL, INSERT INTO/OVERWRITE con mapeo conocido | Conservar orden de sentencias y semántica declarada; no simular datos escritos |
| Vista | Definición SELECT estática, nombre y contexto disponibles | Conservar la vista como objeto y recorrer su definición; una vista externa sin definición queda sin resolver |
| Ventanas | Propuesta inicial: SUM, COUNT y ROW_NUMBER; PARTITION BY, ORDER BY y marco ROWS explícito cuando aplique | Separar argumento, partición, orden y marco; no prometer RANGE, funciones arbitrarias ni todas las reglas de tipos |
| Parámetros | Sustituciones declaradas y valores escalares simbólicos con rol conocido | Identidad o estructura SQL dependiente de parámetro ausente bloquea resolución; valor de filtro ausente puede conservarse como límite |
| Fuera del perfil inicial | CTE, SELECT * y variantes no enumeradas | No se asume soporte; si aparecen y afectan el análisis, resultado parcial con motivo |
| SQL dinámico | Fuera del perfil inicial seleccionado | No ejecutar plantillas/código para obtenerlo; registrar el bloque no resuelto |

La cobertura seleccionada y las reglas de ventana están fijadas en el [diseño técnico](diseno-tecnico.md). La compatibilidad funcional requiere pruebas antes de declararse demostrada. Esta matriz define necesidades del diseño, no capacidades verificadas de SQLGlot ni del extractor previo.

## Esquemas de trabajo

Todos pertenecen al namespace sintético `demo`. Los nombres cortos siguientes son únicos dentro de ese namespace. No se aplican al esquema real del DAG SAMS.

| Objeto | Columnas ordenadas y tipos propuestos | Origen del esquema |
|---|---|---|
| raw.ventas | venta_id entero, cliente_id entero, precio decimal, cantidad entero | Definición del corpus |
| raw.clientes | cliente_id entero, region texto, activo booleano | Definición del corpus |
| stg.ventas | venta_id entero, cliente_id entero, total decimal, region texto | DDL sintético aportado por A |
| curated.ventas | venta_id entero, cliente_id entero, total decimal, region texto | DDL sintético aportado por A |
| curated.v_importes | cliente_id entero, total decimal | Proyección explícita de la vista de A |
| mart.resumen | cliente_id entero, total_ventas decimal | DDL sintético aportado por B |
| mart.identificadores | venta_id entero, cliente_id entero, region texto | DDL sintético aportado por C |
| mart.acumulados | venta_id entero, cliente_id entero, acumulado decimal | DDL sintético para la variante de ventanas de B |

El diseño técnico selecciona DECIMAL(18,2) para fixtures; el comportamiento numérico en ejecución no se simula. El análisis de impacto identifica candidatos ante cambio de tipo; no prueba compatibilidad ni valores numéricos resultantes.

## Operaciones declaradas

La notación describe transformaciones; no representa sintaxis ejecutable de un framework.

| Operación | DAG | Transformación |
|---|---|---|
| a1 | A | Unir raw.ventas v con raw.clientes c por v.cliente_id=c.cliente_id; filtrar c.activo. Escribir stg.ventas: v.venta_id, v.cliente_id, v.precio × v.cantidad, c.region |
| a2 | A | Copiar las cuatro columnas de stg.ventas a curated.ventas con mapeo explícito |
| a3 | A | Definir curated.v_importes como proyección de cliente_id y total desde curated.ventas |
| b1 | B | Leer la vista; agrupar por cliente_id y escribir cliente_id, SUM(total) en mart.resumen |
| c1 | C | Leer curated.ventas y escribir venta_id, cliente_id, region en mart.identificadores; no usar total en proyecciones ni condiciones |
| b2 | B, variante adicional | Leer curated.ventas; calcular SUM(total) por cliente_id, ordenada por venta_id, con marco ROWS desde inicio de partición hasta fila actual; escribir venta_id, cliente_id y acumulado |

La variante b2 se evalúa aparte y añade aportes; no modifica silenciosamente el esperado del recorrido base. No se necesita ejecutar A para analizar B: B consume definiciones y contexto explícitos. La ausencia de A en el catálogo limita el linaje upstream, no autoriza inventarlo.

## Esperado base: relaciones directas de valor

Esta lista contiene 16 relaciones directas. No mezcla relaciones de condición, lectura/escritura ni caminos transitivos. Cada una deberá tener evidencia de la operación indicada; las localizaciones de archivo se asignarán al crear los fixtures.

| Operación | Origen | Destino |
|---|---|---|
| a1 | raw.ventas.venta_id | stg.ventas.venta_id |
| a1 | raw.ventas.cliente_id | stg.ventas.cliente_id |
| a1 | raw.ventas.precio | stg.ventas.total |
| a1 | raw.ventas.cantidad | stg.ventas.total |
| a1 | raw.clientes.region | stg.ventas.region |
| a2 | stg.ventas.venta_id | curated.ventas.venta_id |
| a2 | stg.ventas.cliente_id | curated.ventas.cliente_id |
| a2 | stg.ventas.total | curated.ventas.total |
| a2 | stg.ventas.region | curated.ventas.region |
| a3 | curated.ventas.cliente_id | curated.v_importes.cliente_id |
| a3 | curated.ventas.total | curated.v_importes.total |
| b1 | curated.v_importes.cliente_id | mart.resumen.cliente_id |
| b1 | curated.v_importes.total | mart.resumen.total_ventas |
| c1 | curated.ventas.venta_id | mart.identificadores.venta_id |
| c1 | curated.ventas.cliente_id | mart.identificadores.cliente_id |
| c1 | curated.ventas.region | mart.identificadores.region |

## Condiciones y relaciones de operación

| Operación | Entradas de condición | Ámbito del efecto |
|---|---|---|
| a1 | raw.ventas.cliente_id y raw.clientes.cliente_id: JOIN; raw.clientes.activo: FILTER | Filas que alimentan stg.ventas. No se presentan como valores de todas sus columnas |
| b1 | curated.v_importes.cliente_id: GROUP | Composición de grupos que produce mart.resumen |
| b2 | curated.ventas.cliente_id: WINDOW_PARTITION; venta_id: WINDOW_ORDER | Miembros y orden de ventana del resultado acumulado; ROWS fija el marco |

Cada operación registra qué objetos lee y escribe. La influencia de filas se propaga a operaciones consumidoras como candidato por condición/cardinalidad, manteniendo su motivo; no se transforma en arista de valor. CASE/IF se representará a nivel de expresión de salida, para no atribuir su condición indiscriminadamente a todas las proyecciones.

Para b2 se esperan tres relaciones de valor: venta_id→venta_id, cliente_id→cliente_id y total→acumulado, más las dos condiciones de ventana indicadas. Una variante ROW_NUMBER no tiene columna argumento de valor: depende de filas, partición y orden; no debe inventarse una dependencia de valor de total. Una variante COUNT(*) depende de cardinalidad, no de una columna llamada `*`.

## Consultas esperadas

| Consulta/cambio | Resultado esperado |
|---|---|
| Origen de mart.resumen.total_ventas | SUM en b1; vista a3; curated a2; cálculo de precio y cantidad en a1; evidencia por tramo |
| Eliminar raw.ventas.precio | Camino de valor hasta stg.total, curated.total, vista.total y mart.resumen.total_ventas; si b2 está publicada, también mart.acumulados.acumulado |
| La misma eliminación respecto de C | C es consumidor de curated.ventas, sin camino de valor desde precio en este corpus. No garantizar que C se ejecute si el productor falla |
| Cambiar raw.clientes.activo | Candidatos por filtro/pertenencia de filas en a1 y consumidores posteriores; no inventar cambio de valor de region |
| Cambiar el tipo de curated.ventas.venta_id con b2 | Candidato por orden de ventana y por proyecciones que lo usan; compatibilidad exacta no determinada |
| Sin relaciones en una revisión incompleta | Resultado acotado al conocimiento publicado, con lista de ausencias/diagnósticos conocidos; nunca ausencia universal de impacto |

## Variantes de mantenimiento y contexto ausente

1. Publicar A, después B y C: ampliar conocimiento sin analizar todos los DAGs en cada consulta.
2. Repetir A con iguales entradas/contexto: conservar una contribución lógica, sin duplicados.
3. A v2 cambia el cálculo de total para no usar precio: retirar solo ese aporte. Las escrituras o relaciones aportadas por otros DAGs se conservan. El cambio de dependencia semántica se refleja al consultar la nueva revisión.
4. Fallar el reanálisis de B: conservar su contribución anterior completa y mostrar intento fallido/vigencia. Si cambió un esquema del que B depende, marcar además revisión requerida; conservarlo no garantiza que siga siendo válido.
5. Retirar A explícitamente: conservar lecturas de B/C y marcar productor/definición no disponible donde corresponda. No borrar objetos físicos.
6. Omitir DDL de curated.ventas en un INSERT posicional: mantener lo que se puede resolver, diagnóstico de mapeo destino y resultado parcial no publicable. No convertir alias en esquema implícito.
7. Añadir filtro con fecha simbólica: conservar dependencia de la columna filtrada y parámetro desconocido. Publicabilidad solo si identidad, estructura y relaciones pueden resolverse sin ese valor; no afirmar qué filas pasarán.
8. Encontrar un patrón no soportado en un bloque relevante: resultado parcial, sin sustituir la contribución completa anterior. No cambiar el perfil de cobertura después del fallo para declararlo completo.

## Qué demuestra este recorrido y qué falta

Fija esperados de diseño y distingue valor, condición, catálogo y ejecución. No valida todavía el parser. Las 16 relaciones no son una meta de cobertura universal ni sustituyen los diez cambios del PDI. Pendientes: fixtures ejecutables de análisis con DDL completo, casos por familia, fixtures por tipos y fijación de versiones del adaptador seleccionado en [ADR-0002](../adr/ADR-0002-adaptador-estatico.md#ADR-0002). Las cinco comparaciones manuales siguen separadas.
