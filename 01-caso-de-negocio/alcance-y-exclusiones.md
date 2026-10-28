# Alcance y exclusiones

**Estado: En curso.** Refinamiento del 28/10/2026. Fuente: [contexto transferido](../00-pdi/fuentes/contexto-transferido-2026-10-28.md). El MVP definitivo sigue abierto. Los [compromisos originales](../00-pdi/Objetivos_PDI_2026.md) mantienen fechas, diez cambios y cinco comparaciones manuales.

## Prioridades confirmadas por el usuario

| Aspecto | Definición vigente | Pendiente |
| --- | --- | --- |
| Problema | Descubrimiento incremental del linaje y análisis de impacto derivado. | Recorrido funcional mínimo y límites del MVP. |
| Entradas de referencia | DAGs, configuración de orden y YAML con Spark SQL embebido. | Ejemplos y convenciones del framework; versión SQL y parametrización. |
| Granularidad | Tablas y columnas, ambas prioridad 1 desde el primer prototipo. | Patrones cubiertos y modelo de relaciones; no volver a plantear tablas frente a columnas como elección excluyente. |
| Intermedios y targets | STG relevantes para reconstruir linaje; target final de especial interés. | Identidad, ámbito, escritura y reglas para identificar destinos; no asumir target por posición de tarea. |
| Incrementalidad | Incorporar conocimiento entre DAGs sin reanalizar toda la plataforma por consulta. | Actualización, baja, versiones, relaciones obsoletas y persistencia. |
| Parser | Existe según el usuario; revisar antes de construir otro. | Código/ruta, capacidades verificadas y posibles adaptaciones. |
| IA | Incorporarla con aporte justificable; no usarla como única fuente de verdad. | Funciones, proveedor, modelo, costo y validación de inferencias. |
| Viabilidad | Desarrollo y validación individual. | Estimación de esfuerzo y criterios de aceptación. |

## Propuestas aún no aceptadas como diseño

Catálogo persistente; separación entre análisis verificable y generación de explicaciones; descubrimiento invocado manualmente; formato del reporte y representación de incertidumbre. El uso de artefactos sintéticos ya es una regla del repositorio y del escenario de prueba original; lo pendiente es la composición concreta del corpus de DAGs, no autorizar datos reales.

Para comunicar resultados se adopta como criterio de diseño la distinción del contexto: **Verificado** (relación respaldada por análisis y evidencia), **Inferido** (plausible, sin demostración completa), **Desconocido** (información insuficiente). Su representación y validación se diseñarán; Verificado no equivale a impacto o fallo ejecutado.

## Cobertura SQL por explorar

Proyecciones, alias, aritmética, funciones, JOIN, WHERE, GROUP BY, subconsultas, CTE, SELECT *, temporales, SQL dinámico y parámetros son candidatos de evaluación, no requisitos aprobados en bloque. INSERT INTO, INSERT OVERWRITE y CREATE TABLE son operaciones mencionadas en el contexto, no capacidades verificadas del parser.

Después de inspeccionar parser y ejemplos, registrar por patrón: evidencia de soporte, limitación, adaptación estimada, decisión incluir/excluir y comportamiento cuando no se resuelva. Conservar linaje de valores separado de dependencias de condiciones. Precisar disponibilidad de esquemas, especialmente para SELECT * y asignación de columnas destino.

## Relación con el PDI y límites

El PDI compromete un dialecto, tablas, vistas y scripts. Spark SQL concreta el dialecto de referencia y YAML contextualiza la extracción de scripts; la cobertura de vistas todavía debe quedar explícita. No eliminar ese elemento por centrarse ahora en DAGs. Los diez cambios siguen incluyendo renombrar, eliminar y modificar tipos; no se sustituyen por diez pruebas del parser.

Evoluciones sin compromiso automático para el MVP: integración Git/CI/CD, disparo automático, interfaz web, asistente conversacional, visualización interactiva, servicios GCP reales, múltiples dialectos, permisos y gobierno empresarial. No se requiere una plataforma empresarial real para demostrar el concepto. Análisis estático local y ejecución manual son opciones por evaluar, no decisiones de implementación aceptadas.

No se han aprobado base de grafos, agentes, RAG, microservicios ni despliegue en nube. Tampoco hay autorización de gasto. Las decisiones importantes se registran en [ADR](../02-arquitectura/decisiones/README.md).

## Recorrido funcional mínimo propuesto · 28/10/2026

La prioridad de tablas y columnas está confirmada. La siguiente descomposición concreta el caso para revisar el MVP; no aprueba todavía formatos internos ni patrones SQL.

| ID | Necesidad del usuario | Entrada | Salida observable propuesta |
| --- | --- | --- | --- |
| RF-01 | Incorporar conocimiento de un proceso. | Artefactos sintéticos de un DAG identificados por versión, configuración, YAML y contexto disponible. | Inventario de tareas/objetos y relaciones con evidencia; archivos o referencias no resueltos. |
| RF-02 | Investigar origen y consumidores. | Tabla o columna dentro del conocimiento disponible. | Productores/consumidores técnicos y caminos de relaciones de valor o condición, con pasos STG cuando correspondan. |
| RF-03 | Ampliar y mantener conocimiento. | Nuevo DAG, nueva versión o baja explícita de uno analizado. | Conocimiento actualizado y cobertura identificable; reglas detalladas de actualización por diseñar. |
| RF-04 | Investigar un cambio. | Renombrado, eliminación o cambio de tipo, objeto y columna cuando aplique. | Candidatos directos/transitivos, sustento y límites; sin ejecutar el cambio. |
| RF-05 | Entender qué revisar. | Resultados de RF-04 y su evidencia. | Explicación asistida por IA con citas y verificaciones sugeridas; ninguna inferencia presentada como certeza. |

Propuesta de corpus mínimo: tres DAGs conectados del caso narrativo, un recorrido por STG, un consumidor que use la columna afectada y otro que no dependa de su valor. Incluir una vista para representar el compromiso del PDI, además de tablas y scripts extraídos. Cantidad de tareas, esquemas y patrones se fijará al inspeccionar artefactos; no se han creado aún.

El reporte debería permitir responder: qué versión se analizó, qué se encontró, dónde está la evidencia, qué se recomienda revisar y qué no se pudo conocer. El medio de entrega puede decidirse después; una interfaz web no es requisito de este recorrido.

## Límites funcionales propuestos

No ejecutar migraciones ni autorizar despliegues; no descubrir personas propietarias sin datos explícitos; no certificar integridad de toda la plataforma; no incluir gestión de permisos, datos reales, conexiones productivas o ejecución de pipelines como requisito de la demostración. Estas exclusiones acotan la propuesta de MVP y deben confirmarse con su alcance definitivo.

El análisis de cambio de tipo identificará dependencias candidatas. Determinar compatibilidad exacta de tipos, pérdida de información o comportamiento en ejecución requiere semántica y pruebas adicionales; no prometerlo como consecuencia automática de disponer del linaje.

## Pendiente antes de cerrar alcance

Revisar ejemplos y esquemas; fijar versión/patrones SQL, tratamiento de vistas, parámetros y condiciones; acordar criterios CA-01 a CA-06; estimar trabajo; resolver el camino del parser. El Excel vigente conserva “Revisar el parser existente” en Plan de Carrera C23 y dice “Desarrollar el parser de SQL” en PDI C11. La revisión previa sigue siendo pertinente, pero reutilización, adaptación o sustitución no se deducen de esa redacción.
