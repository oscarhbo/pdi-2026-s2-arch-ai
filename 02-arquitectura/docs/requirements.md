# Requisitos y trazabilidad — Linaje e impacto asistido por IA

Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17  
Estado: Propuesta para revisión

Formalizo los requisitos significativos a partir del alcance vigente y del análisis disponible. Las formulaciones detalladas y sus criterios son Propuestos; no se atribuye aprobación nueva al trasladar fuentes. Los RF/CA son identificadores externos del PDI, no registros renumerados. Disparadores: cobertura, contratos, corpus, decisiones o resultados nuevos.

<a id="FR-001"></a>
## FR-001 — Analizar artefactos sin ejecutar el framework

Descripción: Recibir el paquete versionado, extraer tareas y SQL, conservar procedencia y emitir diagnósticos de contexto ausente o conflictivo.  
Objetivo: [OBJ-001](sad.md#OBJ-001). Fuente: [SRC-016](../README.md#SRC-016). Correspondencia PDI: RF-01.  
Significancia arquitectónica: Determina la frontera de entrada y evita depender del entorno productivo.  
Prioridad: alta por trazabilidad, utilidad o integridad del catálogo; secuencia de implementación definida en el diseño técnico.  
Estado: Propuesto  
Criterio de aceptación: El ejemplo sintético se analiza sin importar DAGs ni ejecutar shell; SQL, parámetros originales y evidencias quedan localizables. Nombre fuera del patrón antiguo o orden de archivos distinto no omite silenciosamente el DAG.  
Responsable: Óscar Hernández Barrón. Revisar al cambiar entradas, cobertura, esquema o implementación.  
Diseño relacionado: [EL-001](sad.md#EL-001). Validación: [VAL-001](validation.md#VAL-001).

<a id="FR-002"></a>
## FR-002 — Consultar origen y consumidores

Descripción: Consultar relaciones upstream/downstream de tablas y columnas con evidencia por tramo, conservando STG y vistas incluidas en la cobertura.  
Objetivo: [OBJ-001](sad.md#OBJ-001). Fuente: [SRC-004](../README.md#SRC-004). Correspondencia PDI: RF-02.  
Significancia arquitectónica: Exige identidad de objetos, relaciones tipadas y resolución por ámbito.  
Prioridad: alta por trazabilidad, utilidad o integridad del catálogo; secuencia de implementación definida en el diseño técnico.  
Estado: Propuesto  
Criterio de aceptación: El recorrido A/B/C distingue dependencias de valor, condición y consumo de tabla. Las posiciones sin DDL y las identidades ambiguas permanecen no resueltas. Ventanas incluidas por [SRC-017](../README.md#SRC-017); se deben distinguir argumento, partición, orden y marco según la [matriz propuesta](recorrido-sintetico.md). La inclusión está confirmada; variantes y criterios detallados siguen propuestos.
Responsable: Óscar Hernández Barrón. Revisar al cambiar entradas, cobertura, esquema o implementación.  
Diseño relacionado: [EL-001](sad.md#EL-001). Validación: [VAL-002](validation.md#VAL-002).

<a id="FR-003"></a>
## FR-003 — Mantener aportes por DAG

Descripción: Publicar contribuciones completas como unidad, reemplazar solo aportes propios, detectar conflictos de revisión y procesar bajas explícitas.  
Objetivo: [OBJ-001](sad.md#OBJ-001). Fuente: [SRC-008](../README.md#SRC-008). Correspondencia PDI: RF-03.  
Significancia arquitectónica: Condiciona almacenamiento, concurrencia y recuperación.  
Prioridad: alta por trazabilidad, utilidad o integridad del catálogo; secuencia de implementación definida en el diseño técnico.  
Estado: Propuesto  
Criterio de aceptación: Repetir no duplica; actualizar/bajar A no elimina aportes de B; parcial/fallido conserva el anterior con aviso; sin anterior se declara ausencia. Revisión esperada incorrecta impide sobrescritura.  
Responsable: Óscar Hernández Barrón. Revisar al cambiar entradas, cobertura, esquema o implementación.  
Diseño relacionado: [EL-002](sad.md#EL-002). Validación: [VAL-003](validation.md#VAL-003).

<a id="FR-004"></a>
## FR-004 — Investigar cambios con motivos diferenciados

Descripción: Recibir cambios tipados de tabla/columna: eliminar, renombrar, agregar columna o modificar tipo/nulabilidad/default/posición; devolver candidatos directos/transitivos con caminos, revisión y límites. Fuente adicional: [SRC-019](../README.md#SRC-019). Evaluar esquema hipotético sin ejecutar DDL ni alterar el catálogo.  
Objetivo: [OBJ-001](sad.md#OBJ-001). Fuente: [SRC-004](../README.md#SRC-004). Correspondencia PDI: RF-04.  
Significancia arquitectónica: Define semántica del motor de recorrido y contrato de consulta.  
Prioridad: alta por trazabilidad, utilidad o integridad del catálogo; secuencia de implementación definida en el diseño técnico.  
Estado: Propuesto  
Criterio de aceptación: Eliminar precio devuelve el camino documentado hacia total y sus consumidores de valor; C no se marca dependiente de ese valor solo por consumir la tabla. No se afirma compatibilidad de tipos ni rotura en ejecución sin evidencia.  
Responsable: Óscar Hernández Barrón. Revisar al cambiar entradas, cobertura, esquema o implementación.  
Diseño relacionado: [EL-003](sad.md#EL-003). Validación: [VAL-004](validation.md#VAL-004).

<a id="FR-005"></a>
## FR-005 — Explicar sin alterar conocimiento

Descripción: Explicar el resultado estructurado con citas y recomendaciones separadas de hechos; conservarlo cuando falle la explicación.  
Objetivo: [OBJ-001](sad.md#OBJ-001). Fuente: [SRC-008](../README.md#SRC-008). Correspondencia PDI: RF-05.  
Significancia arquitectónica: Aísla integración IA y exige un contrato comprobable de salida.  
Prioridad: alta por trazabilidad, utilidad o integridad del catálogo; secuencia de implementación definida en el diseño técnico.  
Estado: Propuesto  
Criterio de aceptación: Una referencia ausente/irrelevante no sustenta un hecho. La explicación no escribe en el catálogo; un fallo conserva resultado y límites, señalando texto no disponible o no validado.  
Responsable: Óscar Hernández Barrón. Revisar al cambiar entradas, cobertura, esquema o implementación.  
Diseño relacionado: [EL-004](sad.md#EL-004). Validación: [VAL-005](validation.md#VAL-005).

<a id="NFR-001"></a>
## NFR-001 — Trazabilidad y límites visibles

Descripción: Cada relación presentada como verificada y cada tramo de camino conservan evidencia, versión y cobertura; la falta de datos no se presenta como ausencia universal de impacto.  
Objetivo: [OBJ-001](sad.md#OBJ-001). Fuente: [SRC-005](../README.md#SRC-005). Correspondencia PDI: CA-01, CA-03 y CA-05 propuestos.  
Significancia arquitectónica: Condiciona todo el modelo de resultados y la evaluación de explicación.  
Prioridad: alta por trazabilidad, utilidad o integridad del catálogo; secuencia de implementación definida en el diseño técnico.  
Estado: Propuesto  
Criterio de aceptación: Propuesta: ninguna relación verificada del corpus carece de evidencia suficiente; todo caso incompleto preparado comunica su límite. Escenario QS-001.  
Responsable: Óscar Hernández Barrón. Revisar al cambiar entradas, cobertura, esquema o implementación.  
Diseño relacionado: [EL-001](sad.md#EL-001). Validación: [VAL-002](validation.md#VAL-002).

<a id="NFR-002"></a>
## NFR-002 — Consistencia de publicación

Descripción: Las consultas observan una revisión consistente y nunca una mezcla intermedia de contribuciones durante el reemplazo.  
Objetivo: [OBJ-001](sad.md#OBJ-001). Fuente: [SRC-008](../README.md#SRC-008). Correspondencia PDI: CA-04 propuesto y APSD-02 vigente.  
Significancia arquitectónica: Condiciona atomicidad lógica, revisión esperada y recuperación.  
Prioridad: alta por trazabilidad, utilidad o integridad del catálogo; secuencia de implementación definida en el diseño técnico.  
Estado: Propuesto  
Criterio de aceptación: Propuesta: toda lectura del ensayo corresponde al estado anterior o al nuevo completo; ante fallo previo a activar, queda el anterior. Escenario QS-002.  
Responsable: Óscar Hernández Barrón. Revisar al cambiar entradas, cobertura, esquema o implementación.  
Diseño relacionado: [EL-002](sad.md#EL-002). Validación: [VAL-003](validation.md#VAL-003).

<a id="QS-001"></a>
## QS-001 — Entrada incompleta durante una consulta

Requisito: [NFR-001](#NFR-001).  
Fuente-estímulo: usuario con un paquete sintético de DAG.  
Estímulo: falta DDL del destino o un parámetro que determina la identidad de un objeto.  
Entorno: caso congelado antes de medir, sin framework, sobre revisión identificada.  
Elemento: [EL-001](sad.md#EL-001) y [EL-003](sad.md#EL-003).  
Respuesta: conservar lo resoluble y declarar relaciones/posiciones pendientes y su causa; no completar por alias o plausibilidad.  
Medida: cero vínculos no sustentados etiquetados como verificados; todos los límites previstos del caso aparecen en el resultado. Revisar por relación y diagnóstico, no por promedio global.  
Estado-umbrales: PROPUESTO.  
Validación: [VAL-002](validation.md#VAL-002).

<a id="QS-002"></a>
## QS-002 — Fallo o conflicto durante reemplazo

Requisito: [NFR-002](#NFR-002).  
Fuente-estímulo: operación de actualización del usuario o fallo inducido del almacenamiento de prueba.  
Estímulo: reemplazo interrumpido antes de activar o revisión esperada distinta de la vigente.  
Entorno: catálogo sintético con A y B compartiendo una relación; lector concurrente en puntos controlados, mecanismo SQLite seleccionado, implementación pendiente.  
Elemento: [EL-002](sad.md#EL-002).  
Respuesta: conservar estado anterior completo ante fallo; rechazar conflicto sin sobrescritura; no borrar aportes de B.  
Medida: cero estados mixtos observados en el ensayo definido; conjunto de aportes coincide exactamente con el esperado antes/después en cada punto de control. No es un SLA productivo.  
Estado-umbrales: PROPUESTO.  
Validación: [VAL-003](validation.md#VAL-003).

<a id="FR-006"></a>
## FR-006 — Realizar el flujo completo desde la web

Estado: Propuesto  
Responsable: Óscar Hernández Barrón  
Objetivo: [OBJ-001](sad.md#OBJ-001). Fuente confirmada de la necesidad: [SRC-019](../README.md#SRC-019). Criterios detallados propuestos.
Descripción: Seleccionar carpeta/DAG, completar contexto, analizar y guardar, visualizar linaje, formular cambios, consultar resumen y descargar el mismo resultado desde navegador.
Significancia: incorpora SPA, API, carga de archivos y trabajos persistidos. Criterio: recargar una URL conserva trabajo/resultado, un parcial se distingue del publicado, grafo y tabla muestran evidencia, reporte y pantalla usan misma revisión.
Diseño: [experiencia web](experiencia-web.md), [EL-010](../diagrams/README.md#EL-010), [ADR-0005](../adr/ADR-0005-interfaz-web-y-trabajos.md#ADR-0005). Validación: [VAL-006](validation.md#VAL-006).

## Cobertura del diseño

La base seleccionada en el [diseño técnico](diseno-tecnico.md) cubre las responsabilidades, contratos y mecanismos. Cubierto por diseño no significa implementado, probado ni aprobado. VAL-001 a VAL-007 siguen planificadas para H2; la revisión de arquitectura actual se define en [VAL-008](validation.md#VAL-008). No se exigen ejecuciones del producto para entregar esta propuesta.

| Objetivo | Requisito | Escenario | ADR o No aplica con motivo | Elementos / vistas | VAL | Cobertura / brecha |
|---|---|---|---|---|---|---|
| [OBJ-001](sad.md#OBJ-001) | [FR-001](#FR-001) | No aplica: requisito funcional; casos en VAL | [Decisiones del SAD](sad.md#9-decisiones-arquitectónicas) | [EL-001](sad.md#EL-001) | [VAL-001](validation.md#VAL-001) | Cubierto por diseño; implementación y pruebas pendientes |
| [OBJ-001](sad.md#OBJ-001) | [FR-002](#FR-002) | No aplica: requisito funcional; casos en VAL | [Decisiones del SAD](sad.md#9-decisiones-arquitectónicas) | [EL-001](sad.md#EL-001) | [VAL-002](validation.md#VAL-002) | Cubierto por diseño; implementación y pruebas pendientes |
| [OBJ-001](sad.md#OBJ-001) | [FR-003](#FR-003) | No aplica: requisito funcional; casos en VAL | [Decisiones del SAD](sad.md#9-decisiones-arquitectónicas) | [EL-002](sad.md#EL-002) | [VAL-003](validation.md#VAL-003) | Cubierto por diseño; implementación y pruebas pendientes |
| [OBJ-001](sad.md#OBJ-001) | [FR-004](#FR-004) | No aplica: requisito funcional; casos en VAL | [Decisiones del SAD](sad.md#9-decisiones-arquitectónicas) | [EL-003](sad.md#EL-003) | [VAL-004](validation.md#VAL-004) | Cubierto por diseño; implementación y pruebas pendientes |
| [OBJ-001](sad.md#OBJ-001) | [FR-005](#FR-005) | No aplica: requisito funcional; casos en VAL | [Decisiones del SAD](sad.md#9-decisiones-arquitectónicas) | [EL-004](sad.md#EL-004) | [VAL-005](validation.md#VAL-005) | Cubierto por diseño; implementación y pruebas pendientes |
| [OBJ-001](sad.md#OBJ-001) | [NFR-001](#NFR-001) | [QS-001](#QS-001) | [Decisiones del SAD](sad.md#9-decisiones-arquitectónicas) | [EL-001](sad.md#EL-001) | [VAL-002](validation.md#VAL-002) | Cubierto por diseño; implementación y pruebas pendientes |
| [OBJ-001](sad.md#OBJ-001) | [NFR-002](#NFR-002) | [QS-002](#QS-002) | [Decisiones del SAD](sad.md#9-decisiones-arquitectónicas) | [EL-002](sad.md#EL-002) | [VAL-003](validation.md#VAL-003) | Cubierto por diseño; implementación y pruebas pendientes |
| [OBJ-001](sad.md#OBJ-001) | [FR-006](#FR-006) | No aplica: funcional | [ADR-0005](../adr/ADR-0005-interfaz-web-y-trabajos.md#ADR-0005) | [EL-010](../diagrams/README.md#EL-010), [EL-006](../diagrams/README.md#EL-006) | [VAL-006](validation.md#VAL-006) | Cubierto por diseño; pruebas web pendientes |

FR-004 incorpora además [VAL-007](validation.md#VAL-007) y ADR-0005 para ampliación de cambios de esquema.

## Cambios

16/11/2026: primer catálogo desde RF y criterios del PDI; continuidad sin acceso al framework. Posteriormente se confirma inclusión de ventanas y ejecución local/manual, y se prepara [recorrido con esperados](recorrido-sintetico.md) y [ADR de relaciones/publicación](../adr/ADR-0001-relaciones-y-publicacion.md#ADR-0001), todavía Propuesto. Impacta contratos del SAD y validaciones previstas. La cobertura SQL está seleccionada en el diseño técnico; capacidad medida, latencia y aceptación formal de CA siguen pendientes. No se altera el compromiso de diez cambios y cinco comparaciones, ni se afirma cierre de H1-02.
