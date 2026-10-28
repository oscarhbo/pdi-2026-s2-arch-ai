# Instrucciones de trabajo

## Contexto y continuidad

- Al iniciar, leer [objetivos](00-pdi/Objetivos_PDI_2026.md), [estado actual](00-pdi/estado-actual.md), [seguimiento](00-pdi/seguimiento-compromisos.md) y documentos relevantes. Consultar [fuentes](00-pdi/fuentes/README.md) ante discrepancias: Excel final > contexto Markdown > propuestas.
- Trabajar en español claro, explicar decisiones con ejemplos y trade-offs. Distinguir compromisos originales, propuestas, decisiones aceptadas y resultados comprobados.
- Una propuesta de Codex no es una decisión aceptada automáticamente. Registrar aceptación explícita y fundamento en ADR breves: contexto, alternativas, decisión, consecuencias y estado; ver [decisiones](02-arquitectura/decisiones/README.md).
- Mantener una ubicación por entregable y enlazarla. No sobrescribir trabajo del usuario ni cambiar objetivos sin instrucción explícita.
- No inventar métricas, avances, certificados, feedback ni resultados de pruebas. Separar evidencia técnica, hipótesis de negocio y sugerencias de IA.
- Aplicar verificaciones proporcionales. Actualizar seguimiento ante avances, bloqueos o cambios significativos y estado actual al cerrar cada sesión significativa, indicando próxima acción concreta.
- Conservar fechas originales; registrar aparte fechas revisadas, motivo y acción correctiva. Una plantilla no equivale a Completado.
- Trabajar autónomamente; revisiones humanas opcionales, sin convertirlas en dependencia de cierre.

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
