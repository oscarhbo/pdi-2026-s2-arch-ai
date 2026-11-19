# Instrucciones de trabajo

## Contexto y continuidad

- Al iniciar, leer [objetivos](00-pdi/Objetivos_PDI_2026.md), [estado actual](00-pdi/estado-actual.md), [seguimiento](00-pdi/seguimiento-compromisos.md) y documentos relevantes. Consultar [fuentes](00-pdi/fuentes/README.md) ante discrepancias: Excel final > contexto Markdown > propuestas.
- Trabajar en español claro, explicar decisiones con ejemplos y trade-offs. Distinguir compromisos originales, propuestas, decisiones aceptadas y resultados comprobados.
- Una propuesta de Codex no es una decisión aceptada automáticamente. Registrar aceptación explícita y fundamento en ADR breves: contexto, alternativas, decisión, consecuencias y estado; ver [decisiones vigentes](02-arquitectura/adr/README.md).
- Mantener una ubicación por entregable y enlazarla. No sobrescribir trabajo del usuario ni cambiar objetivos sin instrucción explícita.
- No inventar métricas, avances, certificados, feedback ni resultados de pruebas. Separar evidencia técnica, hipótesis de negocio y sugerencias de IA.
- Aplicar verificaciones proporcionales. Actualizar seguimiento ante avances, bloqueos o cambios significativos y estado actual al cerrar cada sesión significativa, indicando próxima acción concreta.
- Conservar fechas originales; registrar aparte fechas revisadas, motivo y acción correctiva. Una plantilla no equivale a Completado.
- Trabajar autónomamente. Las revisiones externas son opcionales y no se convierten en dependencia del PDI. Entregar un borrador no equivale a aceptar sus ADR; la aprobación formal requiere evidencia explícita del responsable.

## Arquitectura: estructura canónica ArchFlow

Desde el 17/11/2026 la arquitectura se mantiene en `02-arquitectura/`, con estructura ArchFlow 0.2.0 y perfil Estándar. La copia elaborada en el repositorio ArchFlow es antecedente; no mantener dos fuentes editables ni volver a importar encima de observaciones posteriores.

Leer primero [ficha](02-arquitectura/README.md), [propuesta para revisión](02-arquitectura/docs/revision-propuesta.md), [registro de transferencia](02-arquitectura/docs/validation-log.md#procedencia-transferencia) y artefactos afectados. No volver a pedir datos de negocio ya documentados.

| Contenido | Fuente canónica |
|---|---|
| Contexto, perfil, fuentes y reanudación | `02-arquitectura/README.md` |
| Arquitectura y doce secciones arc42; riesgos y viabilidad | `02-arquitectura/docs/sad.md` |
| Requisitos, escenarios y matriz de cobertura | `02-arquitectura/docs/requirements.md` |
| Contratos y experiencia web | `02-arquitectura/docs/diseno-tecnico.md` y `docs/experiencia-web.md` |
| Definiciones de validación y casos previstos | `02-arquitectura/docs/validation.md` y `docs/recorrido-sintetico.md` |
| Ejecuciones, revisión documental y aprobaciones | `02-arquitectura/docs/validation-log.md` |
| Decisiones y alternativas | `02-arquitectura/adr/ADR-NNNN-*.md` |
| Vistas editables e identidad de elementos | `02-arquitectura/diagrams/README.md` |
| Procedencia e inspección previa del parser | `00-pdi/fuentes/arquitectura/`; manifiestos e informe de referencia |
| Evidencia de ejecución futura de H2/H3 | `04-evaluacion/resultados/` y comparaciones; enlazar desde el log canónico |

- No repartir el SAD o el plan de validación nuevamente entre los nombres anteriores del PDI. Los borradores sustituidos se eliminan después de trasladar su contenido útil y corregir referencias; no conservar índices duplicados solo como antecedentes. `decisiones/` conserva los ADR originales y H1-04 porque aportan evidencia propia. No crear nuevos ADR en esa ubicación. La única carpeta de diagramas es `02-arquitectura/diagrams/`, conforme a ArchFlow; no recrear `diagramas/`.
- Conservar ADR-001 a ADR-003 como registros originales distintos de ADR-0001 a ADR-0005. Nuevos ADR: siguiente ID libre de al menos cuatro dígitos en `adr/`. No renumerar ni aceptar automáticamente por integrar.
- Fuente principal por dato: el caso de negocio permanece en `01-caso-de-negocio/`, compromisos en `00-pdi/`, implementación futura en `03-solucion/` y aprendizaje en `05-aprendizaje/`. La estructura ArchFlow organiza la arquitectura; no sustituye el Excel ni los compromisos originales.
- Redactar objetivos, criterios y decisiones de arquitectura en primera persona, con la voz del arquitecto. No narrar «Óscar dice», «según Óscar», decisiones delegadas ni autoría de un agente. Mantener responsable como metadato, ArchFlow como marco/herramienta y evidencias en redacción neutral. No atribuir ejecuciones, aprobaciones o resultados al responsable sin evidencia. Las referencias a IA como función del sistema se conservan.
- Mantener IDs OBJ/FR/NFR/QS/EL/VIEW/VAL/SRC y anclas estables; cada definición tiene encabezado de segundo nivel y ancla explícita. Actualizar referencias al cambiar rutas. RF/CA del PDI conservan significado de origen.
- Estados independientes: información CONFIRMADO/PROPUESTO/SUPUESTO/PENDIENTE; ADR Propuesto/Aceptado/Rechazado/Sustituido; cobertura Pendiente/Cubierto por diseño/Fuera de alcance/No aplica; ejecución No ejecutado/Conforme/No conforme/Inconcluso. Cubierto por diseño no demuestra implementación.
- Para aprobar un ADR, registrar persona, fecha, versión y evidencia. No convertir observaciones o el cierre editorial del borrador en aprobación. H1-02 sigue siendo arquitectura: no construir, prototipar, ejecutar pruebas del producto o desplegar sin encargo posterior. H1-03 estima; VAL-001 a VAL-007 pertenecen a H2 y VAL-008 a revisión documental.
- Conservar las doce secciones del SAD y revisar impacto en requisitos, decisiones, vistas y validación. Un cambio editorial no cambia versiones técnicas ni estados; registrar fecha y motivo.
- Editar documentos de texto mediante `apply_patch` para que los cambios sean visibles en CodexDiff. Conservar trabajo local previo; no reemplazar archivos con una importación antigua. Informar archivos afectados y verificación.
- Validar estructura con el comprobador ArchFlow disponible en `C:/Oscar/Repositorios/ArchFlow/scripts/check_docs.py`, indicando la raíz de este repositorio para comprobar también enlaces entre secciones. Es herramienta externa, no dependencia del producto. Si no está disponible, informar la limitación y revisar enlaces/IDs sin atribuir una ejecución. No copiar el framework completo ni modificarlo para ocultar advertencias.
- Registrar fuentes y hashes de cada entrega transferida; las sucesivas revisiones se realizan en este PDI. No reabrir H1-01, H1-04 o H1-05 ni cambiar fechas/estados por una reorganización documental.

## Datos y límites

- Usar datos sintéticos en ejemplos. No versionar secretos, credenciales, entornos virtuales, temporales ni datos reales de clientes.
- Registrar videos y multimedia grandes mediante [enlaces externos](06-presentacion/recursos-externos.md).
- No crear remotos, publicar, hacer push ni generar gastos sin autorización explícita. USD 20 es presupuesto API propuesto, no autorización.
- En el arranque documental no implementar prototipo ni hacer commits. No introducir skills al inicio; considerarlas después cuando exista un procedimiento repetido y reutilizable.

## Documentación técnica vigente (Context7)

Para preguntas sobre librerías, frameworks, SDK, API, CLI o nube —sintaxis, configuración, migración, depuración específica y setup— usar ctx7 incluso si la respuesta parece conocida. No es necesario para refactorización, scripts desde cero, lógica de negocio, revisión de código o conceptos generales.

1. Ejecutar fuera del sandbox: `npx ctx7@latest library <nombre oficial> "<pregunta completa sin secretos>"`.
2. Elegir por coincidencia, relevancia, cantidad de ejemplos, reputación y puntuación; ajustar consulta si no encaja.
3. Ejecutar: `npx ctx7@latest docs <id /org/project> "<pregunta completa>"`; usar ID versionado devuelto si corresponde. Omitir library solo si el usuario proporcionó el ID.

Máximo tres comandos por pregunta. Ante error de red, ejecutar fuera del sandbox. Ante cuota, informar y sugerir npx ctx7@latest login o CONTEXT7_API_KEY; no sustituir silenciosamente por memoria del modelo. Fundamentar la respuesta en la documentación obtenida.
