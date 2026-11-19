# Registro de validación — Linaje e impacto asistido por IA

Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17

Mantengo separados los resultados, la revisión documental y las aprobaciones. Actualizaré este registro ante nuevas ejecuciones, cambios de diseño o decisiones de arquitectura. Documentar un método no acredita su ejecución.

## Ejecuciones

Las validaciones funcionales VAL-001 a VAL-007 de [validation.md](validation.md) están planificadas para H2 y no ejecutadas. La revisión textual VAL-008 de la propuesta actual se registra por separado al final. No se crearon todavía el corpus sintético ni la implementación del prototipo. Las comprobaciones aisladas anteriores del extractor se conservan en el [informe de inspección](../../00-pdi/fuentes/arquitectura/revision-parser-y-dags.md#comprobaciones-realizadas-y-límites); no equivalen a ejecutar las nuevas definiciones ni a resultados de H2.

## Revisiones e impacto

| Fecha | Puerta / cambio | Artefactos y revisión | Resultado de revisión | Revisor | Impacto y pendientes |
|---|---|---|---|---|---|
| 16/11/2026 | Continuidad sin framework | Ficha, SAD 0.1, requisitos y validaciones iniciales | Borrador preparado; G1–G4 pendientes de revisión formal | ArchFlow, apoyo documental; revisión formal pendiente | Framework no disponible deja de ser una entrada bloqueante. Semántica de ejecución desconocida; tecnologías, corpus y umbrales pendientes |

Comprobación estructural ejecutada el 16/11/2026: `python -B scripts/check_docs.py projects/linaje-impacto-ia-pdi`, salida 0; 0 errores y 3 advertencias PENDING en SAD, ficha y este log. Los pendientes corresponden a decisiones y trabajo no completados; no se retiran para obtener una comprobación limpia. Un resultado satisfactorio no acredita corrección del linaje ni aprobación arquitectónica.

Revisión semántica de preparación: se mantienen el alcance y los estados de origen, correspondencia RF/CA explícita, cobertura pendiente y resultados no ejecutados. La restricción de framework ausente está reflejada en contratos, diagnósticos y pruebas previstas. La revisión formal de publicabilidad, escenarios y alternativas sigue pendiente; no hay puertas aprobadas.

## Hallazgos

Avance de diseño del 16/11/2026: ventanas y ejecución local/manual confirmadas en [SRC-017](../README.md#SRC-017). Preparados recorrido 0.1, contratos refinados del SAD 0.2 y [ADR-0001](../adr/ADR-0001-relaciones-y-publicacion.md#ADR-0001) Propuesto. Revisión de coherencia: 16 relaciones base de valor, condiciones separadas y variante de ventana diferenciada. No constituye ejecución de VAL ni aceptación de criterios o del ADR. Fixtures, variantes precisas y motor físico continúan pendientes.

Comprobación posterior a este avance: mismo comando documental, salida 0, 0 errores y 1 advertencia PENDING en README. Comprobación adicional del esperado documental: 16 filas base de relación de valor, sin duplicados. La disminución de advertencias no cierra brechas semánticas ni aprueba el diseño; los resultados anteriores se conservan como historial.

El [informe](../../00-pdi/fuentes/arquitectura/revision-parser-y-dags.md#hallazgos-del-analizador) conserva evidencia y localizadores de los defectos observados del extractor previo; no se declaran corregidos. Deben convertirse en casos sintéticos y decisiones de adaptación antes de usar sus salidas como catálogo. Responsable de seguimiento: Óscar; fecha de resolución pendiente de planificación H1-03.

Brechas del diseño: publicabilidad de cada diagnóstico, matriz de patrones, esquema destino sintético, almacenamiento, empaquetado, proveedor IA y límites de operación. El SAD mantiene estas propuestas abiertas. No hay excepciones ni riesgos aceptados por esta elaboración.

## Aprobaciones

No se registran nuevas aprobaciones de puertas, ADR ni criterios. La autorización para continuar sin framework consta en [SRC-016](../README.md#SRC-016); autoriza avanzar con los insumos actuales y no equivale a aceptar el diseño detallado.

## Consolidación técnica del 16/11/2026

Consolido la propuesta con el criterio definido en [SRC-018](../README.md#SRC-018). SAD 0.3 y diseño técnico 1.0 seleccionan cobertura, adaptador, almacenamiento, contratos, consulta y explicación. ADR-0002 a ADR-0004 comparan alternativas; ADR-0001 enlaza el mecanismo físico. Cuatro vistas Mermaid son fuentes editables, todavía sin revisión de renderizado.

Revisión semántica documental: correspondencia entre los cuatro componentes, contratos y decisiones; 16 relaciones base se conservan, ventanas tienen variantes delimitadas, diez cambios quedan separados de cinco comparaciones. Cobertura pasa a Cubierto por diseño, sin atribuir implementación ni ejecuciones. Los pendientes tecnológicos mencionados en entradas anteriores describen ese estado histórico; ahora permanecen pruebas, revisión formal y capacidad para H1-03.

Fuentes técnicas oficiales consultadas mediante Context7 y páginas oficiales, enlazadas en ADR-0002 a ADR-0004. No se instalaron dependencias del prototipo ni se hicieron llamadas IA pagadas. No se modificaron PDI, parser ni DAGs.

Las puertas no se aprueban mediante esta elaboración. G3/G4 requieren revisar evidencia técnica o una excepción explícitamente aprobada conforme a quality-gates; ninguna se registra aquí.

Comprobación de esta consolidación: `python -B scripts/check_docs.py projects/linaje-impacto-ia-pdi`, salida 0, **0 errores y 1 advertencia PENDING en README.md**. La advertencia conserva los pendientes reales de capacidad, evidencia y revisión. La inspección de Git mostró cambios previos fuera del proyecto; se conservaron y no forman parte de esta intervención. El comprobador no valida sintaxis/renderizado Mermaid ni funcionamiento del sistema.


## Complemento web del 16/11/2026

SRC-019 convierte web opcional en requisito. Leída la guía osutils-web indicada, sin modificar ese repositorio. Preparados experiencia web 1.0, ADR-0005, SAD 0.4, diseño técnico 1.1 y vistas 1.1. Se conservan SQLGlot, SQLite, relaciones y reporte; SPA/FastAPI sustituyen la CLI como flujo del usuario.

Revisión de diseño: selección de carpeta se traduce a archivos/rutas relativas, guardado no equivale a publicación, jobs sobreviven a navegación y exponen interrupciones, descarga usa resultado/revisión fijos. Impacto amplía tabla/columna y esquema hipotético; no ejecuta cambios. FR-006 y VAL-006/VAL-007 amplían trazabilidad.

Consultadas documentación FastAPI mediante Context7 y documentación del selector de carpetas; referencias en experiencia web y ADR-0005. No hay implementación, renderizado de interfaz ni pruebas funcionales nuevas. El incremento de esfuerzo debe incorporarse a H1-03; decisiones siguen Propuesto y puertas no aprobadas.

Verificación documental del complemento: `python -B scripts/check_docs.py projects/linaje-impacto-ia-pdi`, salida 0, 0 errores y 1 advertencia PENDING en README. La primera pasada detectó campos Qué/Por-qué combinados en las nuevas VAL; se separaron y la comprobación posterior pasó. No es evidencia de funcionamiento web ni revisión visual.


<a id="revision-documental-propuesta-1-0"></a>
## Revisión documental de propuesta 1.0

Fecha: 2026-11-16  
Validación: [VAL-008](validation.md#VAL-008)  
Resultado: Conforme  
Evidencia: [propuesta 1.0](revision-propuesta.md), [SAD 1.0](sad.md), [contratos 1.2](diseno-tecnico.md), [experiencia web 1.1](experiencia-web.md), [vistas 1.2](../diagrams/README.md).
Revisor: revisión documental asistida en ArchFlow; sin aprobación formal.
Revisión: propuesta 1.0, SAD 1.0, diseño técnico 1.2, experiencia web 1.1 y vistas 1.2 del 16/11/2026; documentos sin commit.
Condiciones: lectura de fuentes del proyecto, contraste textual de escenarios y separación de etapas; sin sistema implementado ni renderizado visual.
Ejecutor: ArchFlow, revisión documental asistida; no acredita una revisión personal del responsable.
Observaciones: conformidad limitada a texto y trazabilidad; no acredita pruebas funcionales, aceptación de riesgos ni aprobación de puertas.

Reconciliados alcance web obligatorio, entrada por carpeta y publicación automática solo del completo sin conflicto. ADR-0002/ADR-0004 describen ahora la base vigente conservando su evolución. VIEW-002 ubica la SPA dentro del equipo local; VIEW-004 diferencia acción del usuario y orquestación del trabajo en backend. Los siete escenarios de VAL-008 tienen responsabilidades, respuesta y límite trazables; es razonamiento de diseño, no ejecución.

Hallazgos resueltos: redacción que mezclaba cierre documental con construcción/pruebas, CLI como decisión principal pese a web obligatoria, reporte estático como suficiente y referencia a contraste visual como si estuviera verificado. Quedan diferenciados revisión actual, planificación H1-03, comprobaciones de implementación H2 y comparación H3.

La mención previa de G3/G4 se precisa: no todas las pruebas de H2 son prerrequisitos de entregar H1-02. Las puertas permanecen pendientes de evaluación formal; ninguna prueba requerida por una decisión concreta se da por ejecutada o exenta. SRC-020 solicita propuesta sin construcción, no aceptación automática de riesgos.

Límites: renderizado/legibilidad de diagramas y aceptación formal pendientes de la revisión del entregable. No se ejecutaron VAL-001 a VAL-007, builds, pruebas de navegador, prototipos ni despliegues. La conformidad indicada solo cubre coherencia textual y clasificación de etapa; no demuestra rendimiento, seguridad efectiva, recuperación ni calidad de IA.

Comprobación estructural: `python -B scripts/check_docs.py projects/linaje-impacto-ia-pdi`, salida 0, 0 errores y 1 advertencia PENDING en README. Se corrigieron los campos obligatorios del registro de revisión detectados en la primera pasada. La advertencia conserva información por revisar y tareas de etapas posteriores; no implica que deba construirse el sistema para entregar la propuesta.

## Ajuste editorial del 17/11/2026

Unifico la voz de la documentación en primera persona para expresar mis objetivos, criterios y decisiones de arquitectura. Conservo ArchFlow como herramienta y marco de trabajo. Los resultados de comprobaciones se mantienen en redacción neutral, sin atribuirme ejecuciones o aprobaciones no acreditadas.

El ajuste es editorial: no cambia alcance, contratos, tecnologías, resultados históricos ni estados de aprobación. Las menciones a IA que describen funciones del sistema se conservan. Las versiones de diseño mantienen su número; la fecha revisada identifica esta actualización de redacción.

Verificación editorial: la búsqueda de referencias a autoría del agente y decisiones delegadas no encontró coincidencias. Comprobación estructural del 17/11/2026: `python -B scripts/check_docs.py projects/linaje-impacto-ia-pdi`, salida 0, 0 errores y 1 advertencia PENDING en README por pendientes explícitos. No se ejecutaron pruebas del producto.

## Integración en el PDI — 17/11/2026

La propuesta 1.0 se incorpora a `02-arquitectura/` con estructura ArchFlow por SRC-021. Se conserva el historial de las revisiones anteriores: las rutas y comandos relativos a `projects/linaje-impacto-ia-pdi/` en esas entradas describen su ejecución original en ArchFlow, no una ejecución en el PDI. La nueva ubicación es canónica y las siguientes observaciones se registran aquí. La integración no ejecuta pruebas del producto ni acepta decisiones. El [registro de integración](validation-log.md#procedencia-transferencia) mantiene huellas, equivalencias y verificaciones de transferencia.


<a id="procedencia-transferencia"></a>
## Procedencia y verificaciones de la transferencia

Consolidación editorial: 18/11/2026. Los resultados siguientes corresponden a la integración y depuración del 17/11/2026; no son comprobaciones nuevas. Las reglas vigentes están en [AGENTS.md](../../AGENTS.md) y las fuentes en la [ficha](../README.md). El insumo de inicio ya fue consumido; el contexto vigente se mantiene en la ficha y los documentos del caso de negocio.

Verificación de la limpieza del 18/11/2026: sin referencias restantes a los dos documentos retirados en los archivos de texto revisados y sin nuevos destinos o anclas faltantes. Se conservan los cuatro enlaces históricos a dos fuentes ausentes ya registrados. Las 16 filas de compromisos permanecen idénticas. Los manifiestos JSON son válidos y conservan sus 17 registros de entrega y 37 registros de fuentes; ambos manifiestos y el informe de inspección mantienen su contenido íntegro. El comprobador Python no pudo iniciarse por denegación de ejecución del entorno (EPERM); se realizó una comprobación alternativa de enlaces, anclas, referencias e integridad mediante lectura de archivos. No equivale a ejecutar el comprobador ArchFlow completo ni a validar el producto.

### Procedencia y revisiones

Fuente: `C:/Oscar/Repositorios/ArchFlow/projects/linaje-impacto-ia-pdi`, propuesta 1.0 revisada editorialmente el 17/11/2026. Incluye SAD 1.0, diseño técnico 1.2, experiencia web 1.1 y vistas 1.2. Sin commit nuevo; no se presupone una revisión Git cerrada. [Manifiesto de entrega](../../00-pdi/fuentes/arquitectura/entrega-archflow.json) registra las 17 rutas y SHA-256 de los bytes fuente anteriores a su marca de transferencia.

La copia preserva los IDs y contenido técnico; adapta la ficha para referenciar este repositorio como ubicación canónica y añade procedencia al log/propuesta. Los resultados históricos con comandos de ArchFlow conservan su fecha y contexto; no se presentan como pruebas nuevas realizadas en el PDI.


### Fuentes de verdad y equivalencias

| Contenido anterior | Fuente vigente / tratamiento |
|---|---|
| arquitectura-de-solucion.md (retirado) | [docs/sad.md](sad.md); diseño consolidado |
| alternativas.md (retirado) | [adr/README.md](../adr/README.md); alternativas dentro de cada decisión |
| riesgos-y-limitaciones.md (retirado) | [SAD, sección 11](sad.md#11-riesgos-y-deuda-técnica) |
| diagramas/README.md (retirado) | [diagrams/README.md](../diagrams/README.md); única carpeta de vistas |
| decisiones/ADR-001 | Registro original de prioridad, distinto de ADR-0001 |
| decisiones/ADR-002 | Aplicaciones de lecturas, distinto de ADR-0002 |
| decisiones/ADR-003 | Alcance original aceptado; web obligatoria posterior en SRC-019 y ADR-0005 propuesto |
| decisiones/revision-simulada-hito-1.md | Se conserva íntegra como evidencia de H1-04 |
| 04-evaluacion/plan-de-evaluacion.md (retirado) | [validation.md](validation.md); criterios útiles consolidados en el plan |
| 04-evaluacion/casos-de-prueba.md (retirado) | [Diez cambios](validation.md#diez-cambios-de-impacto-seleccionados) y [recorrido](recorrido-sintetico.md) |
| RF-01 a RF-05 y CA-01 a CA-06 del caso | Fuentes de negocio; correspondencia en [requirements.md](requirements.md), sin renumerar |
| FR/NFR/QS/VAL, EL/VIEW y SRC de ArchFlow | Se conservan con sus definiciones y referencias dentro de la arquitectura |
| Resultados funcionales futuros | Archivos de evidencia en 04-evaluacion/resultados; registro/enlaces en validation-log.md |

La coexistencia de ADR-001 y ADR-0001 no es duplicación: representan decisiones diferentes. Nuevas decisiones usan cuatro dígitos y siguiente número libre en adr/. Ningún ADR original cambia de estado por migración. Conservo registros con evidencia propia, como los ADR originales, la revisión simulada . Retiro los borradores sustituidos tras consolidar el contenido útil y ajustar enlaces; no mantengo copias paralelas del diseño.


### Verificación de transferencia

Antes de integrar se inventariaron las fuentes, los cambios locales y las huellas del PDI. Ya existían cuatro enlaces rotos a dos archivos históricos ausentes: revision-excel-2026-10-28.md y contexto-transferido-2026-10-28.md en 00-pdi/fuentes. No se reconstruye su evidencia ni se reescriben ADR aceptados para ocultarlos.

Verificación posterior realizada el 17/11/2026:

- Comprobador ArchFlow sobre la raíz completa del PDI: 4 errores LINK_MISSING, los mismos cuatro hallazgos previos, y 1 advertencia PENDING en la ficha por pendientes explícitos. No aparecen errores nuevos de estructura, IDs, estados, cobertura o enlaces creados. El resultado global no es una validación limpia porque las fuentes históricas siguen ausentes.
- Inventario de integridad: 55 archivos preexistentes contrastados por SHA-256; ningún archivo eliminado y ninguna modificación fuera de los 13 documentos previstos. Objetivos, Excel, ADR originales y revisión simulada conservan sus bytes.
- Seguimiento: las 16 filas conservan IDs, hitos, entregables, fechas originales/revisadas y estados. Solo se actualizan procedencia, evidencia y observaciones de integración.
- Transferencia: 14 de los 17 documentos fuente mantienen contenido idéntico tras normalizar finales de línea. Ficha, guía de revisión y log solo adaptan ubicación canónica, navegación y registro de transferencia. El manifiesto conserva hashes de bytes originales.
- La copia de origen en ArchFlow recibe únicamente un aviso de transferencia en su ficha. No se mantiene allí una segunda arquitectura activa.

Comando de referencia desde el PDI: `python -B C:/Oscar/Repositorios/ArchFlow/scripts/check_docs.py .`. Validar el repositorio completo permite revisar enlaces entre secciones; comprobar solo 02-arquitectura/ advertiría de enlaces legítimos fuera de esa subcarpeta. El comprobador no verifica renderizado Mermaid, resultados del producto ni aceptación de decisiones.


### Consolidación de diagramas — 17/11/2026

En la integración inicial conservé `diagramas/` como antecedente y añadí `diagrams/` con las vistas de ArchFlow. Esto dejó dos carpetas para el mismo propósito. La carpeta anterior solo contenía un README de planificación, sin diagramas ni fuentes editables.

Retiro ese índice y su carpeta vacía. Mantengo las cuatro vistas existentes en [diagrams/README.md](../diagrams/README.md), sin modificar su contenido. Actualizo el destino en el contexto de transferencia y fijo en AGENTS.md una única carpeta, `02-arquitectura/diagrams/`. Las menciones a la ruta retirada en este registro documentan la corrección, no indican un destino de trabajo. La comprobación de integridad anterior corresponde a la transferencia inicial; esta corrección elimina únicamente el índice obsoleto.


### Depuración de documentos sustituidos — 17/11/2026

Retiro los tres borradores anteriores de arquitectura y los dos de evaluación indicados en el mapa. El SAD, los ADR y validation.md cubren sus responsabilidades; traslado al plan único los criterios de medición, denominadores, evidencia y control de sesgos que aún eran útiles. Elimino también la sección duplicada «Índice anterior» de decisiones/README.md. Actualizo los enlaces de negocio, aprendizaje, continuidad y fuentes históricas; en la revisión simulada identifico expresamente los enlaces a sucesores para no atribuirle una revisión del diseño actual.

Conservo los ADR originales y la revisión simulada por sus decisiones y evidencias propias; objetivos, caso de negocio y notas por su propósito distinto. Las plantillas de implementación, resultados, aprendizaje y presentación organizan entregables futuros, no duplican el SAD ni acreditan avances. Los hashes de la entrega original conservan su significado histórico y no se recalculan como si correspondieran a esta revisión.
