# Revisión del analizador y los DAGs de referencia

Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17  
Estado: Borrador de análisis para H1-02

En esta revisión delimito qué puedo reutilizar del analizador, cómo se organizan las entradas y qué falta para construir linaje confiable. Esta revisión no modifica los repositorios de origen, no selecciona un reemplazo ni aprueba la arquitectura. La [ficha](README.md) conserva fuentes, preguntas y progreso.

## Alcance y procedencia

Fuentes autorizadas en la conversación del 16/11/2026:

- Analizador: `C:/Oscar/Walmart/dag_analyzer`, seis módulos Python en `analyzer_code/`, README y estructura de resultados guardados en `code_analyzer.json`. Los CSV se inventariaron, sin revisión exhaustiva de sus filas.
- Ejemplo principal: `C:/Oscar/Walmart/Dags_CRM/sams_ords_hdr_trans`, aportado durante la revisión para continuar con otro DAG.
- Contraste previo: `C:/Oscar/Walmart/Dags_CRM/ecomm_ords_hdr_trans_dcp`. Sus inconsistencias no se atribuyen al ejemplo principal.

Los localizadores de código que siguen son relativos a esas raíces y sus líneas corresponden a esta lectura. El [manifiesto de huellas](fuentes-inspeccionadas.json) identifica los archivos inventariados; no implica igual profundidad de revisión para todos. HEAD del analizador observado: `9b6f0f22867a19da40db969e18e265723267ac56`. No se comprobó que todos los archivos de trabajo coincidan con ese commit: las huellas SHA-256 identifican el contenido consultado.

Los ejemplos contienen metadatos y referencias de un entorno empresarial; no los clasifico como corpus sintético ni copio sus archivos al PDI. La autorización de lectura permite esta revisión. El corpus de evaluación del prototipo sigue siendo sintético, separado de las fuentes de referencia. No se ejecutaron DAGs, scripts shell, SQL, consultas a metastore ni conexiones a nube. No se recopilaron registros de clientes. Se omiten aquí contactos y valores de infraestructura que no hacen falta para explicar los hallazgos.

## Conclusión de reutilización

**PROPUESTO:** propongo aprovechar la organización de extracción de artefactos y la experiencia del analizador, y revisar su descubrimiento, normalización y salida antes de usarlo como productor de un catálogo. El código actual es principalmente un extractor de inventario SQL y expresiones. No implementa todavía un resolvedor de relaciones columna-origen → columna-destino, ni publicación versionada por DAG, consultas transitivas o explicación IA.

Mi propuesta no implica sustituir SQLGlot ni escribir un parser desde cero. Primero necesito una versión reproducible y casos sintéticos con esperados independientes para separar limitaciones del adaptador de capacidades de la biblioteca. El alcance de esta revisión es el código local; no acredita compatibilidad con versiones actuales de bibliotecas.

## Cómo está armado el ejemplo principal

SAMS contiene nueve archivos: un DAG Python, un YAML de configuración, uno de propiedades, un shell y cinco YAML de SQL. El DAG construye tareas mediante `workflow_framework.Framework(...).build_task(...)`. La configuración identifica las mismas tareas y enlaza sus propiedades; cada YAML SQL tiene un bloque `queries: |` con varias sentencias.

Hay ocho tareas y siete enlaces de orden. Python, líneas 67–77, y `parents` de la configuración, líneas 25–101, describen la misma secuencia:

`start → set_dates → sale_hdr_fact → identity_mapping → cust_afflt → sales_order_detail → load_target → end`

Esta coincidencia permite describir el ejemplo, pero no demuestra qué mecanismo usa internamente el framework. Su código no está entre los archivos proporcionados. El orden de ejecución y el flujo de datos son relaciones distintas: la tarea de identidades sigue a la primera STG, pero su SQL lee una fuente externa, no esa STG.

| Pieza | Comportamiento observable | Consecuencia para el modelo |
|---|---|---|
| DAG Python | Declara identidad, archivo de configuración, tareas y enlaces `>>` | Extraer estáticamente; no importar el DAG para descubrirlo |
| Configuración | Incluye `name`, `type`, `curr_id`, `parents` y `properties_file` | Conservar referencias y comprobar consistencia, sin usar la posición o `long_name` como identidad de archivo |
| Propiedades | Define esquemas, tablas, nombres STG y otros parámetros | Conservar plantilla, valor resuelto y procedencia de la sustitución |
| Shell de fechas | Consulta particiones del destino y fecha actual mediante Hive; imprime fechas | Contexto dinámico no disponible mediante sustitución estática. Declararlo sin ejecutar el shell |
| YAML de SQL | Cuatro grupos DROP/CREATE/INSERT para STG y un INSERT final | Modelar sentencias ordenadas dentro de cada tarea y varios objetos por DAG |
| Carga final | INSERT OVERWRITE particionado, con agregaciones y proyecciones posicionales | Hace falta el DDL del destino; los alias no sustituyen al esquema ordenado |

Las siete referencias explícitas a archivos de propiedades y script de SAMS existen. En el ejemplo DCP existen también sus quince referencias explícitas. Es una comprobación de destinos, no una ejecución del framework ni una validación completa del YAML.

## Patrones observados y cobertura aún no acreditada

| Patrón | Evidencia en SAMS | Estado del analizador / pendiente |
|---|---|---|
| DROP y CREATE con esquema de columnas | YAML 01 a 04 | Existen ramas de extracción, pero la identidad de CREATE requiere corrección y prueba |
| INSERT OVERWRITE posicional | YAML 01 a 05 | Extrae proyecciones; no las vincula con posiciones del DDL para construir aristas de columnas |
| Alias, constantes, NULL y funciones | YAML 01 a 05 | Conserva nombres y algunas expresiones; falta separar orígenes, expresiones y valores sin origen de columna |
| Subconsulta y funciones de ventana | YAML 01, líneas 84–120 | Tabla derivada `sts` y COUNT OVER; falta resolución por ámbito y propagación hasta las fuentes |
| JOIN y filtros compuestos | YAML 01, 03 y 04 | Se extraen tablas y textos de JOIN. No hay modelo de dependencia de condición ni tratamiento completo de filtros interiores |
| GROUP BY y agregaciones | YAML 04, líneas 65–95; YAML 05, líneas 39–125 | Falta conservar dependencias de agrupación y distinguirlas de proyecciones |
| Particiones del destino | YAML 05, línea 39 | Requiere esquema, orden de columnas y semántica de escritura explícita |
| Parámetros de ejecución | Shell y YAML 01, línea 119 | Fechas no definidas en las propiedades; deben figurar como contexto no resuelto |
| UNION ALL | No aparece en SAMS; sí en carga final DCP, líneas 50–199 | La salida histórica DCP pierde las columnas de las ramas |
| Vistas, CTE y SELECT * de proyección | Sin evidencia suficiente en estos ejemplos | Mantenerlos pendientes; no afirmar soporte ni retirarlos del compromiso de vistas |

El ejemplo SAMS sirve para comprender convenciones; no es automáticamente el mínimo viable. Incluye complejidad de subconsultas y ventanas desde su primera transformación. Acotar patrones requiere una decisión explícita y una muestra sintética correspondiente.

## Hallazgos del analizador

| Prioridad | Hallazgo y evidencia | Implicación / tratamiento propuesto |
|---|---|---|
| Alta | El patrón de `orchestrator.py:16` acepta el nombre DCP pero rechaza el de SAMS. Comprobado con la expresión regular exacta y los dos nombres | SAMS se omite antes de analizar SQL. Descubrir por un manifiesto o asociación validada DAG–configuración, sin depender exclusivamente de una nomenclatura rígida |
| Alta | `orchestrator.py:42–47` solo registra la configuración si ya encontró el DAG en el recorrido de nombres. Prueba aislada: configuración primero → 0; DAG primero → 1 | El resultado depende del orden de enumeración. Recoger candidatos antes de resolver asociaciones; detectar múltiples candidatos |
| Alta | `sql_analyzer.py:10–20` limpia SQL con expresiones regulares sin contexto léxico. La comprobación sintética altera un literal y deja restos de un SET | Conservar SQL original y ubicación; eliminar o clasificar sentencias sin modificar cadenas ni comentarios por coincidencias parciales |
| Alta | `sql_analyzer.py:161–184` produce nombres proyectados y textos de reglas, sin aristas de columna ni enlace con DDL | No confundir inventario de columnas con linaje. Incorporar resolución de alias, ámbitos, posiciones y orígenes múltiples |
| Alta | La salida histórica DCP contiene 15 CREATE con `target_table` vacío. Código relevante: `sql_analyzer.py:22–28,53–60` | Las columnas DDL quedan sin identidad de tabla fiable. Desenvolver y validar la forma del destino antes de aceptar el resultado |
| Alta | En esa misma salida, el INSERT final registra dos fuentes pero `columns=[]` y reglas vacías, pese al UNION ALL en el SQL guardado | Recorrer ramas y alinear posiciones; una consulta con SQL presente y columnas ausentes no debe publicarse como completa |
| Alta | La salida DCP contiene 18 JOIN clasificados INNER, mientras su propio SQL guardado incluye 16 LEFT JOIN y 2 JOIN sin LEFT. Código: `sql_analyzer.py:141–159` | Se pierde la modalidad de unión. Confirmar la representación del AST en la versión elegida y probar tipos/condiciones antes de afirmar cobertura |
| Alta | `ErrorLevel.IGNORE` en `sql_analyzer.py:20`; `except` general con `querys_parser=None` en `orchestrator.py:94–99`; tipos no contemplados no generan diagnóstico | Diferenciar completo, parcial, fallido y no soportado. Conservar error, sentencia, localización y motivo; no convertir silencio en ausencia de relaciones |
| Media | `code_analyzer.py:60–64` reconoce cadenas con comillas simples, no dobles; omite caracteres desconocidos en líneas 77–79 | DCP usa dobles en `workflow` y `priority`; el fallback de nombre puede ocultar extracción incompleta. Proponer lectura estática de asignaciones literales con diagnóstico para expresiones no resolubles |
| Media | `utils.py:16–25` hace reemplazo textual por longitud, sin reporte de parámetros pendientes y sin recorrer estructuras anidadas | Conserva sufijos útiles, pero puede sustituir prefijos no deseados. Definir gramática y procedencia sin romper las convenciones existentes |
| Media | `config_analyzer.py:27–36` agrupa errores de lectura y parseo como archivo no encontrado; no valida coherencia de tareas | Validar contrato de entrada y conservar causa; referencias ausentes y sintaxis inválida son diagnósticos diferentes |
| Media | `orchestrator.py:83` usa el archivo descubierto aunque registra `config_file` extraído del DAG; no comprueba correspondencia | Resolver la referencia declarada y registrar discrepancias, sin seleccionar otro YAML silenciosamente |
| Media | Exportación en `orchestrator.py:121–165` escribe JSON y CSV, con rutas CSV fijas; no hay control transaccional ni versiones por aporte | Mantener exportación separada de publicación. No usar estos archivos como implementación de las invariantes de actualización |

La ausencia de un `requirements.txt`, lockfile y pruebas en el árbol inspeccionado impide reconstruir el entorno solo con el README, que sí menciona ese archivo. El README describe capacidades más amplias que las demostradas por la implementación; la referencia de esta revisión es el código y la evidencia disponible.

## Límites específicos del primer ejemplo

En DCP las relaciones Python no coinciden con varios `parents`: por ejemplo, las tareas de ventas dependen en Python de la etapa de estado de órdenes, pero en YAML apuntan a fechas; la carga final tiene un padre YAML que no coincide con ningún nombre de tarea. El analizador copia `parents`, no extrae los enlaces `>>`. Queda como caso de inconsistencia de entrada; no demuestra por sí mismo un fallo de ejecución.

El JSON histórico contiene el mismo identificador DCP, pero los trece nombres de YAML SQL no coinciden con los de la carpeta aportada: carecen del segmento `_hs_`. Por tanto, sus resultados se contrastan con el SQL incorporado en ese JSON, no se atribuyen como ejecución reproducida de los archivos actuales. No contiene un DAG CRM-SAMS. Cambiar al ejemplo SAMS no corrige las brechas generales del analizador.

Ambos ejemplos declaran el mismo destino lógico con valores distintos de la partición vertical. Esto refuerza la necesidad de conservar varios productores/aportes por tabla y de describir la escritura o partición cuando exista evidencia. No presupone que ambos snapshots se ejecuten juntos ni autoriza simular escrituras reales.

## Comprobaciones realizadas y límites

Fecha: 16/11/2026. Python disponible: 3.14.4. La comprobación de dependencias encontró pandas 3.0.3 y no encontró SQLGlot, PyYAML ni PLY en ese intérprete. No se instalaron paquetes ni se ejecutó el análisis SQL completo; las versiones originales siguen pendientes.

| Comprobación | Resultado observado | Alcance |
|---|---|---|
| Parseo AST de los seis módulos Python | Sin errores de sintaxis | No acredita importación ni ejecución funcional |
| Regex de nombres tomada del AST del orquestador | DCP=True; SAMS=False | Demuestra el rechazo por nombre, sin importar DAGs |
| `search_files` original con dobles de `os.walk`, analizador de código y configuración | Configuración primero: 0 resultados; DAG primero: 1 | Aísla la dependencia del orden, sin abrir archivos reales ni exportar |
| Función `replace_vars` original sobre datos sintéticos | `$target_table_stg` → `demo_stg`; `$beg_date` permanece; estructura anidada no cambia; `$xyz` con clave `x=A` → `Ayz` | Expone comportamiento literal y límites; no ejecuta el framework |
| Regex de cadenas de CodeAnalyzer | Comillas simples reconocidas; dobles no | Prueba de la regla léxica, no del analizador completo con PLY |
| Limpieza original sobre SQL sintético | `set spark.sql.files.maxpartitionbytes='500mb'; SELECT 'ASSET x=y' AS label;` termina como `'500mb'; SELECT 'AS' AS label;` | Demuestra alteración antes de SQLGlot; no se ejecuta SQL |
| Inspección JSON histórico | 40 DAGs, 334 tareas, 228 SQL; una con parser nulo; 301 DROP, 261 CREATE y 267 INSERT registrados | Es contenido guardado de procedencia de ejecución no acreditada, no benchmark ni cobertura del universo |
| Referencias de archivos de ejemplos | DCP: 15 existentes; SAMS: 7 existentes | Comprobación de rutas declaradas, sin interpretar el framework |

Las comprobaciones aisladas son evidencia de revisión del analizador; no cuentan como los diez cambios de H2, las cinco comparaciones de H3 ni pruebas del prototipo de linaje. Los hallazgos no se corrigieron en el repositorio de origen. Al cierre se compararon las huellas de los 37 archivos externos inventariados: ninguno cambió respecto de su captura durante la revisión. El comprobador documental devuelve 0 errores y 1 advertencia DISCOVERY por no existir aún el núcleo de diseño; se comprobaron además los destinos de enlaces locales de este informe y la ficha, sin faltantes.

## Consecuencias para H1-02

**PROPUESTO, pendiente de revisión:** usar cuatro fronteras lógicas del diseño vigente y concretarlas a partir de la evidencia:

1. **Entrada y extracción:** manifiesto de DAG/archivos, hashes, tareas, parámetros estáticos y dinámicos, DDL disponible, SQL original y localización. Conservar separadas dependencias de orquestación y de datos.
2. **Análisis:** resolver sentencias y ámbitos, identidad de tablas/columnas, lecturas/escrituras, relaciones de valor y condición, evidencia por tramo, diagnósticos y cobertura. Las STG de estos ejemplos tienen esquema y DDL físico; no asumir que todas son temporales de sesión ni fusionarlas solo por nombre corto.
3. **Publicación y consulta:** adaptar ese resultado al contrato independiente del parser; reemplazar aportes por DAG únicamente cuando sean publicables, conservar contribuciones ajenas y consultar versiones explícitas. Historial, almacenamiento y consultas permanecen sin seleccionar.
4. **Explicación y reporte:** recibir relaciones ya sustentadas, cobertura y límites. Los datos faltantes o errores de extracción deben seguir visibles; la IA no reconstruye DDL ni relaciones por plausibilidad.

Antes de decidir reutilización, comparar corrección y esfuerzo mediante una secuencia sintética: descubrimiento del nombre SAMS; DROP/CREATE/INSERT; columna directa y cálculo; filtro/JOIN; subconsulta; agrupación/ventana; unión de ramas; escritura particionada; parámetros pendientes; entrada inválida. Es una propuesta de familias de comprobación, no diez cambios nuevos ni sustitución del plan de evaluación.

Actualización posterior del 16/11/2026: no hay acceso a `workflow_framework` y se solicita continuar con lo disponible, según [SRC-016](README.md#SRC-016). Su ubicación deja de ser un requisito para avanzar. La regla general de precedencia y la resolución dinámica permanecen desconocidas; el diseño conserva declaraciones y límites sin ejecutar el framework. DDL destino y versiones del entorno siguen ausentes, sin bloquear contratos lógicos.

Impacto documental posterior: ficha actualizada y núcleo de diseño iniciado en [SAD](docs/sad.md), [requisitos](docs/requirements.md), [validaciones](docs/validation.md) y [log](docs/validation-log.md). ADR y diagramas pendientes. Este informe conserva evidencia de inspección y no duplica el catálogo de requisitos. Sin selección de tecnologías, estimación de horas ni aprobación de puertas.
