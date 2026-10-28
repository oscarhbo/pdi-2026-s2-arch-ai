# Plan de trabajo

**Estado: En curso.** Borrador de planificación actualizado al 28/10/2026; las ventanas intermedias son propuestas, no nuevos compromisos aceptados. Responsable del PDI: Óscar Hernández Barrón. [Compromisos originales](Objetivos_PDI_2026.md) y [seguimiento](seguimiento-compromisos.md).

Ningún hito profesional ha vencido a esta fecha. No se propone mover sus fechas. La fecha personal de 2025 requiere aclaración en [fuentes](fuentes/README.md).

| Ventana propuesta | Trabajo y salida verificable | Condición para avanzar |
| --- | --- | --- |
| 25/10–01/11/2026 | Precisar caso de linaje e impacto; revisar parser y ejemplos cuando estén disponibles; delimitar patrones Spark SQL, convenciones y evaluación, con tablas y columnas prioridad 1. | Caso explicable y límites explícitos; metas pendientes resueltas o justificadas. |
| 02/11–08/11/2026 | Comparar alternativas, definir responsabilidades y trazabilidad de evidencias; estimar esfuerzo y costos. Lecturas aplicadas al diseño. | Borrador de arquitectura y ADR con alternativas y consecuencias; sin selección implícita de stack. |
| 09/11–15/11/2026 | Simular revisión con IA, contrastar observaciones y registrar aceptación/rechazo; cerrar caso, arquitectura, plan y notas. | Evidencia enlazada para H1-01 a H1-05. Hito original: 15/11/2026. |
| 16/11–29/11/2026 | Tras definir alcance, preparar corpus y resultados esperados; reutilizar/adaptar parser según revisión e implementar extracción, linaje de tablas/columnas y registro incremental según diseño. | Dependencias verificables en ejemplos sintéticos; pruebas reproducibles. |
| 30/11–13/12/2026 | Mantenimiento de relaciones y motor de impacto; explicación IA citando evidencia; ejecutar diez cambios y documentar fallos. | Resultados con versión, entrada, salida esperada/obtenida y limitaciones. |
| 14/12–20/12/2026 | Corregir, repetir casos afectados y cerrar reporte; reunir retrospectivas y aplicación de formación. | Evidencia H2-01 a H2-05. Hito original: 20/12/2026. |
| 21/12/2026–10/01/2027 | Comparar cinco casos con análisis manual bajo protocolo común; elaborar propuesta de piloto. | Mediciones separadas de hipótesis de beneficio; costos y supuestos explícitos. |
| 11/01–24/01/2027 | Preparar demo, relato ejecutivo, ensayo con IA, objeciones y mejoras; notas de Views and Beyond. | Ensayo registrado, mejoras trazables y demo reproducible. |
| 25/01–31/01/2027 | Grabar demo/presentación y registrar enlaces accesibles; cerrar propuesta y evidencias. | Evidencia H3-01 a H3-06. Hito original: 31/01/2027. |

## Capacidad y control semanal

El primer objetivo registra 4 h/semana de práctica y 2 h/semana de revisión/reflexión. Formación sin cantidad global fijada. En el arranque del 25/10 se estimaron 12 h de práctica y 6 h de revisión para tres semanas. Esa referencia histórica no equivale a capacidad restante: al 28/10 faltan 18 días para el hito 1 y no hay horas consumidas registradas. Recalcular disponibilidad real antes de aceptar tareas; no atribuir avance al tiempo transcurrido. Desglosar esfuerzo en la próxima sesión y ajustar alcance si no cabe. Los objetivos posteriores comparten práctica/revisión sin un total global confirmado. Validar disponibilidad de fin de año.

Propuesta: al cierre de cada semana registrar compromiso, evidencia, bloqueo, desviación, acción correctiva y próxima verificación en [seguimiento](seguimiento-compromisos.md) y [retrospectivas](retrospectivas/README.md). Actualizar [estado actual](estado-actual.md) también al cerrar sesiones significativas.

Si una entrega peligra: conservar fecha original, explicar causa, proponer alcance o fecha revisada y registrar su aceptación. Las revisiones con IA permiten avanzar autónomamente; no sustituir evidencia técnica por aprobación simulada. Consultas humanas opcionales.

## Próxima unidad de trabajo

Precisar el recorrido mínimo del [caso](../01-caso-de-negocio/problema-y-contexto.md). Revisar código/ruta del parser y ejemplos representativos sin datos reales cuando el usuario los proporcione; documentar capacidades verificadas y matriz de patrones Spark SQL. Mientras faltan, concretar preguntas, entradas/salidas y criterios propuestos en [alcance](../01-caso-de-negocio/alcance-y-exclusiones.md), sin inventar convenciones. Mantener ambas granularidades y ajustar cobertura, estimar esfuerzo y acordar metas antes de cerrar MVP. No implementar todavía.

El refinamiento incorpora trabajo de extracción de YAML, columnas, STG y mantenimiento incremental que antes no estaba desglosado. No se declara viable por defecto dentro de la capacidad disponible. Revisar riesgos y alcance antes del hito de diseño; no mover fechas ni crear entregables nuevos silenciosamente.
