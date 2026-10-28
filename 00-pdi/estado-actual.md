# Estado actual

Última actualización: **28/10/2026**, zona America/Mexico_City. Sesión más reciente: desarrollo del caso de negocio v0.1.

## Avance comprobado

- Desarrollado el [caso de negocio v0.1](../01-caso-de-negocio/problema-y-contexto.md): resumen, usuarios, proceso actual/propuesto, recorrido ilustrativo, alternativas, papel de IA y riesgos. H1-01 pasa a En revisión; no significa aceptado ni completado.
- Elaborados alcance funcional RF-01 a RF-05, hipótesis B-01 a B-04, criterios propuestos CA-01 a CA-06, modelo de esfuerzo/costo y propuesta inicial de piloto. H3-02 pasa a En curso. No hay precios cotizados, horas medidas ni tecnologías nuevas aceptadas.

- Sincronizado [Objetivos_PDI_2026.md](Objetivos_PDI_2026.md) con las dos hojas del plan en el Excel vigente: diez celdas con cambios de contenido respecto a la transcripción anterior, resumen y anexo actualizados. E3 y E14 conservan su redacción previa. C11 indica desarrollar el parser; C23 del Plan de Carrera conserva revisar el existente. La elección de reutilizar/adaptar/reemplazar requiere aclaración, no se deduce de esta sincronización.

- Previamente se preparó una versión revisada del Excel con 12 celdas ajustadas; la copia de outputs ya no está disponible en esa ruta. Se conserva el [antes/después por celda](fuentes/revision-excel-2026-10-28.md). Después Óscar actualizó el Excel de fuentes, que ahora es el archivo vigente. No se atribuyen avances ni aprobación externa.

- Estructura documental creada el 25/10; fuentes originales conservadas. [Fuentes y aclaraciones](fuentes/README.md).
- Leído el [contexto transferido](fuentes/contexto-transferido-2026-10-28.md) y revisados todos los documentos del repositorio. Óscar autorizó adaptar documentación manteniendo estructura, AGENTS.md y compromisos.
- Actualizados [caso](../01-caso-de-negocio/problema-y-contexto.md), [alcance](../01-caso-de-negocio/alcance-y-exclusiones.md), [arquitectura conceptual](../02-arquitectura/arquitectura-de-solucion.md), evaluación y plantillas relacionadas. Esto no completa los entregables.
- Registrada la prioridad comunicada por el usuario en [ADR-001](../02-arquitectura/decisiones/ADR-001-prioridades-de-linaje.md): descubrimiento incremental y linaje de tablas/columnas, ambos prioridad 1.
- Git local sin commits. No hay implementación ni parser entregado en este repositorio; su existencia fue declarada por el usuario, sin inspección de capacidades.

## Contexto vigente para retomar

El problema principal es la falta de trazabilidad entre procesos, tablas y columnas. Dos capacidades: descubrir linaje por DAG y acumular conocimiento; después consultar impacto entre DAGs. Entorno de referencia declarado: GCP, GCS/Parquet, Hive Metastore, Spark/Dataproc y Airflow con Spark SQL embebido en YAML. No implica desplegar ese stack.

Caso: DAG 1 lee raw.ventas/raw.clientes y produce curated.ventas, con posibles STG; DAGs 2/3 consumen esa tabla. Eliminar raw.ventas.precio podría afectar el cálculo de total. La propagación a columnas de los consumidores necesita evidencia específica; consumo de tabla no demuestra impacto en todas sus columnas.

## Trabajo en curso

Caso v0.1 En revisión; alcance funcional, costos, propuesta de piloto, planificación y arquitectura conceptual En curso. [Entregables](seguimiento-compromisos.md). Tablas y columnas ya son prioridad confirmada; versión y patrones Spark SQL siguen abiertos. No hay pruebas, métricas, cursos, lecturas, grabaciones o revisiones simuladas acreditados. Los criterios propuestos son parte del borrador, no acuerdos ni resultados.

## Decisiones pendientes

Cobertura exacta del MVP y de vistas del PDI; convenciones de DAG/YAML; esquemas y parámetros; capacidades del parser; modelo de relaciones de valor/condición; ámbito y tratamiento de STG; identificación de targets; catálogo/persistencia; versionado, actualización y baja de DAGs; consultas de impacto; aporte específico de IA; interfaz y activación del análisis; tecnologías; criterios y metas de calidad; estimación de esfuerzo y presupuesto API de USD 20 propuesto, sin gasto autorizado.

El flujo conceptual, catálogo persistente, separación análisis/IA y arranque manual siguen siendo propuestas. Verificado/Inferido/Desconocido distingue sustento de una relación; no demuestra ejecución ni ausencia de impacto fuera del catálogo.

## Bloqueos y dependencias

Faltan código/ruta del parser y ejemplos representativos de DAG, configuración y YAML. Esto impide verificar reutilización y convenciones, pero permite continuar la delimitación funcional. No reemplazar el parser ni inventar estructura del framework. No hay dependencia de revisión humana externa para cerrar el proyecto. Fechas ambiguas del Excel y referencia bibliográfica siguen en [fuentes](fuentes/README.md).

## Próxima acción concreta

Contrastar el caso v0.1 y revisar RF-01 a RF-05 y CA-01 a CA-06: utilidad del reporte, evidencia y límites necesarios. Después, con parser y artefactos representativos, fijar patrones Spark SQL/vistas/STG, estimar esfuerzo y resolver reutilización/adaptación/desarrollo. Conservar tablas y columnas; controlar complejidad acotando patrones. Registrar qué criterios se aceptan o ajustan antes de cerrar el alcance. La revisión simulada del hito sigue pendiente; no se ha realizado por el mero hecho de redactar este caso.

## Verificación documental previa

Caso de negocio v0.1: verificados 67 enlaces relativos de los ocho documentos modificados, sin destinos faltantes ni tablas con columnas inconsistentes. Criterios, corpus adicional y ejemplo aritmético identificados como propuestas/ilustración. Las fechas de los tres hitos se mantienen; no se atribuyeron mediciones ni ejecución del prototipo.

Sincronización posterior del Excel: comprobadas las 112 celdas no vacías de las dos hojas transcritas, sin diferencias de contenido respecto al anexo. El hash del Excel vigente no cambió durante esta tarea. Las verificaciones que siguen corresponden a la revisión documental anterior.

Revisados 114 enlaces relativos sin destinos faltantes y tablas Markdown sin diferencias de columnas. Los 16 IDs, entregables y fechas originales/revisadas del seguimiento permanecen iguales. Comprobado byte a byte que AGENTS.md, .gitignore, objetivos y Excel no cambiaron. Sin nuevos archivos de código, SQL o YAML; sin commits ni remotos.

## Cierre y continuidad

Fechas originales: 15/11/2026, 20/12/2026 y 31/01/2027; ninguna vencida al 28/10. Faltan 18 días para el hito de diseño. Sin reprogramaciones; no asumir viabilidad de la ampliación sin revisar capacidad. Las ventanas intermedias del [plan](plan-de-trabajo.md) siguen siendo propuestas.

La revisión propuesta del Excel se guardó por separado y posteriormente Óscar actualizó el archivo de fuentes y se sincronizó su transcripción. Esta sesión desarrolla documentos de negocio y actualiza seguimiento: H1-01 En revisión y H3-02 En curso. Se conservan las fechas registradas, el Excel y los objetivos. No se programó el prototipo, crearon commits, publicaron archivos ni autorizaron gastos.
