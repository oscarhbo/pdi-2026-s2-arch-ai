# Propuesta técnica y comercial de piloto

Lectura 5 de 5 · [Índice del caso de negocio](README.md)

[← Anterior: costos y esfuerzo](costos-y-esfuerzo.md) · [Continuar con arquitectura →](../02-arquitectura/README.md)

**Estado: En curso. Versión 0.1, 28/10/2026.** Propuesta inicial derivada del caso de negocio. Se completará con arquitectura, esfuerzo y mediciones para H3-02; no es una oferta contractual ni un piloto vendido.

## Propuesta al patrocinador simulado

Proponemos evaluar, en un conjunto sintético de pipelines conectados, si descubrir y mantener dependencias entre tablas y columnas permite investigar cambios con mejor trazabilidad y un esfuerzo defendible frente al análisis manual. La demostración entregará resultados con evidencia y límites, no una garantía de ausencia de impacto.

La decisión solicitada al finalizar sería continuar, acotar o no extender la solución según resultados. El ejercicio individual del PDI puede completarse sin un patrocinador o cliente real; esos roles se simulan para practicar la comunicación comercial.

## Alcance de la demostración propuesta

Usar el recorrido RF-01 a RF-05 y corpus propuesto en [alcance](alcance-y-exclusiones.md): incorporar DAGs progresivamente, consultar origen/consumidores, reflejar modificaciones/bajas y analizar cambios con explicación IA. Tablas y columnas prioridad 1, un dialecto de referencia y patrones delimitados. Medir diez cambios y comparar cinco con investigación manual.

No incluye migraciones productivas, garantía de cobertura empresarial, gestión de permisos ni integraciones adicionales por defecto. La selección de tecnologías y la inspección del parser corresponden al diseño; no se compromete una base de grafos, agentes, nube o interfaz web.

## Etapas y entregas

| Etapa del PDI | Fecha original | Entrega para decidir |
| --- | --- | --- |
| Diseño | 15/11/2026 | Caso, alcance acotado, arquitectura justificada, plan y criterios; revisión simulada y notas aplicadas. |
| Implementación y evaluación | 20/12/2026 | Prototipo reproducible y reporte de diez cambios; fallos, compromisos y correcciones trazables. |
| Demostración y propuesta | 31/01/2027 | Cinco comparaciones, demo/presentación grabadas, propuesta con esfuerzo/costos y ensayo mejorado. |

Las fechas son compromisos de desarrollo profesional, no un plazo contratado con un cliente. El esfuerzo todavía necesita estimación; ver [costos](costos-y-esfuerzo.md). No hay precio, tarifa, gasto autorizado ni beneficio económico prometido.

## Insumos, responsabilidades y evidencias

Óscar define el caso, prepara artefactos sintéticos, registra resultados y conduce la evaluación. El parser y ejemplos representativos ayudarán a contrastar viabilidad; no se necesita acceso a datos reales para el ejercicio. La IA apoya explicaciones y revisiones simuladas, cuyos comentarios se identifican como tales. Revisión humana externa opcional.

Entregar corpus/versiones, instrucciones de reproducción, relaciones esperadas, salidas, límites y registro de comparación. Si posteriormente se propone un piloto con artefactos empresariales, tendrá que acordarse por separado quién puede proporcionarlos, cómo se validan y cuáles son las condiciones de acceso y uso.

## Aceptación y riesgos

Proponer CA-01 a CA-06 de [beneficios y criterios](beneficios-y-criterios-de-exito.md) antes de medir. Si la herramienta no supera el esfuerzo manual, reportarlo; puede haber un aprendizaje útil sin recomendar extensión. Si no preserva evidencia o mantiene relaciones obsoletas, no presentarla como apoyo fiable para cambios.

Riesgos principales: cobertura de columnas mayor que la capacidad disponible, convenciones desconocidas, conocimiento incompleto y comparación sesgada por familiaridad. Mitigaciones: patrones acotados, evidencia versionada, límites visibles y medición de preparación/mantenimiento junto a consulta.

## Próximo acuerdo

Revisar la versión 0.1 del caso, delimitar patrones con ejemplos y parser, acordar criterios y estimar esfuerzo. Con esas respuestas se podrá completar el diseño; con resultados medidos se podrá convertir esta propuesta inicial en una propuesta técnica/comercial defendible.

---

[← Anterior: costos y esfuerzo](costos-y-esfuerzo.md) · [Índice del caso de negocio](README.md) · [Continuar con arquitectura →](../02-arquitectura/README.md)
