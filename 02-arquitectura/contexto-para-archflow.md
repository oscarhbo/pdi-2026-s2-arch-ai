# Contexto de entrada y transferencia con ArchFlow

> **Contexto histórico de salida hacia ArchFlow.** La integración del 17/11/2026 ya se realizó y adopta la estructura nativa en este PDI. La tabla de traslado y la instrucción de no modificar el PDI que siguen pertenecen al encargo original; quedan reemplazadas para el trabajo actual por [integración](integracion-archflow.md) y [AGENTS.md](../AGENTS.md).

Fecha: 15/11/2026. ArchFlow inspeccionado: 0.2.0, en `C:/Oscar/Repositorios/ArchFlow`. Este documento prepara el inicio y la integración; no acredita generación ni aprobación de una arquitectura en ArchFlow.

## Objetivo de la sesión

Quiero completar el diseño de arquitectura de un analizador de linaje e impacto asistido por IA, correspondiente a H1-02 del PDI. El caso de negocio, la revisión simulada y las lecturas aplicadas al diseño ya están cerrados. Necesito concretar requisitos arquitectónicos, contratos, modelo de relaciones, componentes, actualización, consultas, explicación, riesgos, tecnologías y validaciones. El plan de ejecución H1-03 se completará con el esfuerzo y las dependencias derivados del diseño.

## Contexto del caso

Ingenieros y analistas necesitan conocer origen y consumidores de tablas y columnas, recorrer dependencias upstream/downstream, orientar investigaciones de problemas y evaluar cambios. Hoy deben buscar manualmente entre repositorios y código; el resultado depende de acceso, experiencia y memoria del equipo. Investigar puede ocupar horas o una jornada; es una referencia cualitativa, no una línea base medida.

Prioridades de valor: primero encontrar dependencias omitidas, después reducir tiempo y finalmente reducir la dependencia del conocimiento individual. El conocimiento descubierto debe poder reutilizarse y mantenerse al cambiar los DAGs.

El MVP entrega un reporte con origen, consumidores, caminos, pasos STG pertinentes, DAG, archivo, SQL de soporte, versiones y cobertura. La IA explica posibles afectaciones y recomienda qué revisar sin inventar relaciones. La interfaz web y la descarga desde ella son ampliaciones opcionales.

Solo se analizan DAGs, configuración, YAML, SQL y esquemas proporcionados. No se incluyen logs de pipelines, estados de ejecución, diagnóstico automático de causa raíz, cambios productivos ni gestión de permisos. La instrumentación interna del propio analizador para comprobar errores y consumo es distinta de consultar logs de los pipelines. Una relación ausente en el catálogo no demuestra ausencia universal de impacto.

## Contexto técnico y reglas existentes

- Spark SQL embebido en YAML; DAGs y configuración describen procesos. Tablas y columnas tienen prioridad 1; STG, vistas y scripts deben quedar representados en la cobertura.
- El entorno de referencia incluye GCP, GCS/Parquet, Hive Metastore, Spark/Dataproc y Airflow. No obliga a desplegar ese entorno para el prototipo.
- Existe un parser por revisar. Su código y muestras representativas todavía no están disponibles en este repositorio. No atribuirle capacidades ni decidir reemplazarlo sin inspección.
- El diseño vigente separa extracción/análisis, publicación de conocimiento, consulta y explicación. Analizar no publica. El modelo de salida conserva evidencia y diagnósticos sin exponer detalles internos del parser.
- La actualización reemplaza aportes por DAG, evita duplicados, conserva contribuciones ajenas y mantiene el último resultado completo ante fallo con vigencia visible. La baja es explícita. El mecanismo físico todavía requiere diseño.
- La explicación no modifica el catálogo; distingue evidencia, inferencias y desconocidos. Si falla, permanece accesible el resultado estructurado.
- No están seleccionados almacenamiento, proveedor/modelo de IA, despliegue, formato de reporte ni cobertura SQL exacta. RAG, agentes, microservicios y base de grafos no son requisitos.
- Se usan datos sintéticos. No se autoriza gasto: USD 20 es presupuesto API propuesto. No crear commits, remotos, publicaciones ni infraestructura como parte de la preparación documental.

## Fuentes que deben leerse

Nota de mantenimiento del 17/11/2026: los enlaces de arquitectura y evaluación apuntan ahora a sus sustitutos canónicos. Las descripciones de diseño parcial y preguntas abiertas corresponden al contexto original, no al estado de esos documentos actuales.

Raíz de las fuentes: `C:/Oscar/Repositorios/pdi-2026-s2-arch-ai`. Los enlaces de esta tabla son relativos a este documento dentro del PDI; si se copia a otro repositorio deben resolverse desde la raíz indicada o ajustarse.

| Orden | Fuente | Uso |
| --- | --- | --- |
| 1 | [Estado actual](../00-pdi/estado-actual.md) y [seguimiento](../00-pdi/seguimiento-compromisos.md) | Estado vigente; distinguir historial de decisiones actuales. |
| 2 | [Objetivos](../00-pdi/Objetivos_PDI_2026.md) y [fuentes y aclaraciones](../00-pdi/fuentes/README.md) | Compromisos originales, fechas y aclaraciones posteriores. |
| 3 | [Caso de negocio](../01-caso-de-negocio/problema-y-contexto.md) | Usuarios, problema, alternativas y justificación. |
| 4 | [Alcance](../01-caso-de-negocio/alcance-y-exclusiones.md) | RF-01 a RF-05, reporte y exclusiones. |
| 5 | [Beneficios y criterios](../01-caso-de-negocio/beneficios-y-criterios-de-exito.md) | Prioridades y criterios técnicos aún por concretar; no son mediciones. |
| 6 | [Arquitectura](docs/sad.md), [alternativas](adr/README.md) y [riesgos](docs/sad.md#11-riesgos-y-deuda-técnica) | Diseño parcial y preguntas abiertas. |
| 7 | [ADR-001](decisiones/ADR-001-prioridades-de-linaje.md), [ADR-002](decisiones/ADR-002-aplicacion-lecturas-al-diseno.md) y [ADR-003](decisiones/ADR-003-alcance-funcional-del-caso.md) | Prioridades, criterios incorporados y alcance funcional. Conservar sus distintos estados. |
| 8 | [Aplicación del aprendizaje](../05-aprendizaje/aplicacion-al-proyecto.md) | Diez aplicaciones y contratos que forman parte del diseño vigente. |
| 9 | [Revisión simulada](decisiones/revision-simulada-hito-1.md) | Respuestas finales; observaciones IA como material de consideración, no requisitos aceptados en bloque. |
| 10 | [Plan de evaluación](docs/validation.md) y [casos](docs/validation.md#diez-cambios-de-impacto-seleccionados) | Evaluación prevista; no ejecuciones acreditadas. |
| 11 | [Plan de trabajo](../00-pdi/plan-de-trabajo.md) y [costos y esfuerzo](../01-caso-de-negocio/costos-y-esfuerzo.md) | Capacidad, dependencias y estimación pendiente. |

No es necesario cargar todas las notas de los libros, datos personales del Excel ni materiales auxiliares para iniciar. El registro de aplicaciones recoge lo pertinente. Ante contradicción, distinguir el compromiso original de una aclaración posterior explícita; no convertir frases históricas de propuesta en el estado actual. Si una referencia no está disponible, declararlo sin reconstruir su contenido.

## Lo que falta aportar para cerrar decisiones técnicas

1. Ubicación/código del parser, modo de uso y resultados reales existentes, si los hay.
2. Un conjunto representativo de DAG, configuración y YAML con SQL, esquemas y parámetros, sin información sensible.
3. Un recorrido sintético con resultados esperados: A calcula total desde precio y cantidad; B consume total; C consume solo identificadores. Incluir STG y una vista. El recorrido existe como narrativa, no como corpus ejecutable.
4. Volumen de prueba, restricciones de ejecución, disponibilidad semanal y criterios cuantitativos necesarios. No asignar números por defecto ni equiparar las 4 h de práctica y 2 h de revisión históricas con capacidad restante.

Se puede iniciar descubrimiento, requisitos y diseño lógico con lo disponible. La evidencia ausente limita la selección del parser, la cobertura, la estimación y la comprobación de viabilidad, no la comprensión del caso.

## Instrucción de arranque para usar en ArchFlow

```text
Lee las instrucciones de ArchFlow y .agents/skills/archflow-init/SKILL.md.
Inicia el proyecto linaje-impacto-ia-pdi con el nombre
«Linaje de datos y análisis de impacto asistido por IA».
Usa como entrada C:/Oscar/Repositorios/pdi-2026-s2-arch-ai/02-arquitectura/contexto-para-archflow.md
y lee las fuentes enumeradas allí desde su repositorio de origen.
Las fuentes del PDI son de consulta; no las modifiques desde esta sesión.

Parte del perfil Estándar, por tratarse de un sistema nuevo con varios
contratos, persistencia, actualización e integración IA. Mantén la profundidad
proporcional a un prototipo individual; no añadas infraestructura empresarial.

Prepara la ficha con el contexto disponible. No repitas preguntas de negocio
ya resueltas. Pregunta primero por el parser y los ejemplos de entrada faltantes.
Mantén diferenciados alcance definido, diseño vigente, alternativas y datos
desconocidos. No reabras H1-01, H1-04 ni H1-05 por falta de implementación.

Redacta la documentación con mi voz, sin «Óscar dice» ni «según Óscar».
Usa metadatos de procedencia para la trazabilidad, sin atribuir aprobaciones
nuevas ni pruebas no ejecutadas. El diseño debe permitir preparar H1-02
y estimar H1-03. H2 conserva los diez cambios; H3, las cinco comparaciones.
No ejecutes infraestructura, gastes, publiques ni crees commits.
```

Este bloque es una instrucción para una sesión de inicio futura. Prepararlo no crea el proyecto ni ejecuta la skill.

## Cómo incorporar los resultados al PDI

La propuesta es usar ArchFlow como espacio de elaboración y trasladar una entrega identificada al PDI, que mantiene la versión canónica del proyecto. No copiar todo ArchFlow ni mantener dos arquitecturas editadas en paralelo.

| Salida de ArchFlow | Integración en el PDI |
| --- | --- |
| Ficha README | Contexto y procedencia de la entrega; enlazar el caso ya existente sin sustituirlo. |
| docs/sad.md | Evolucionar arquitectura-de-solucion.md con las doce secciones, preservando decisiones y enlazando detalle existente. |
| docs/requirements.md | Incorporar un catálogo de requisitos arquitectónicos en 02-arquitectura; mapear RF/CA existentes sin crear compromisos duplicados. |
| diagrams/ | Incorporar fuentes editables e índice en 02-arquitectura/diagrams/ (destino actualizado tras la integración). |
| adr/ | Reconciliar con 02-arquitectura/decisiones. No sobrescribir ADR-001 a ADR-003 ni atribuir aceptación por generación. |
| docs/validation.md | Integrar métodos y criterios en 04-evaluacion; una definición de arquitectura no equivale a prueba de H2 ejecutada. |
| docs/validation-log.md | Incorporar evidencia y hallazgos donde corresponda en 04-evaluacion/resultados; mantener ejecuciones y revisión documental diferenciadas. |
| Costos, riesgos y dependencias | Actualizar sus documentos existentes y el plan H1-03, sin duplicar registros completos en el SAD. |
| PDF, imágenes o presentación | Derivados opcionales; conservar fuentes editables y comprobar la representación antes de entregar. No hay exportador general de ArchFlow implementado. |

Antes de integrar: identificar versión/revisión fuente o hashes de archivos; comparar contra las decisiones vigentes; mantener una tabla de equivalencias de IDs; ajustar enlaces; verificar coherencia y actualizar seguimiento. ArchFlow usa ADR de cuatro dígitos y FR/NFR/QS/VAL, mientras el PDI usa ADR de tres dígitos y RF/CA. Es una adaptación explícita, no una renumeración silenciosa de registros históricos.

El comprobador de ArchFlow espera ficha y docs/sad.md, requirements.md, validation.md y validation-log.md. Sus enlaces deben quedar dentro de la raíz inspeccionada. No se puede prometer compatibilidad directa después de repartir los documentos en las carpetas del PDI: comprobar la estructura nativa antes del traslado y los enlaces/IDs adaptados después. Si se exige conservar el validador sin adaptación, la alternativa es mantener un subproyecto con estructura nativa y convertir los documentos anteriores en índices; esa reorganización no se ha realizado.

H1-02 sigue En curso. La revisión fue estática sobre instrucciones, plantillas y código del inicializador/comprobador; no se ejecutaron scripts ni se generó un proyecto dentro de ArchFlow.
