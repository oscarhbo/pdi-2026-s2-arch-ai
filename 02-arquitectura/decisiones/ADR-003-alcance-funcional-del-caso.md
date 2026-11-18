# ADR-003 · Alcance funcional y entrega del caso de negocio

Fecha: 15/11/2026. **Estado: Aceptada.** Registro mis decisiones de alcance para el cierre de H1-01.

## Contexto

Investigar origen y consumidores exige recorrer código y depender de conocimiento disperso. Con cadenas de muchos procesos, la búsqueda puede dejar relaciones sin identificar. Necesito una base de linaje consultable por ingenieros y analistas, tanto para entender los datos como para orientar investigaciones y evaluar cambios.

## Alternativas

- Continuar con búsquedas manuales: poco trabajo inicial, pero repite esfuerzo y depende de acceso, experiencia y memoria.
- Documentar relaciones manualmente: comparte conocimiento, pero requiere mantenerlo frente a cambios del código.
- Construir un MVP que conserve linaje y entregue un reporte: exige desarrollo y actualización, a cambio de conocimiento reutilizable y evidencia localizable.
- Exigir una interfaz web desde el inicio: facilita navegación, pero añade esfuerzo que no es necesario para entregar el valor mínimo.

## Decisión

Defino el reporte como entrega suficiente del MVP. Incluye origen y consumidores de tablas y columnas, caminos upstream/downstream, DAG, archivo y fragmento SQL de soporte, versiones y referencias no resueltas. La IA complementa la investigación con explicaciones y recomendaciones sustentadas.

El análisis se limita a los artefactos proporcionados. Excluyo logs, estados de ejecución y diagnóstico automático de causas de fallo. La interfaz web con visualización y descarga queda como ampliación opcional si hay tiempo. Priorizo identificar dependencias omitidas, reducir tiempo y reducir dependencia del conocimiento individual, en ese orden.

## Consecuencias

La cobertura debe ser visible y la ausencia de relaciones no equivale a ausencia universal de impacto. El reporte orienta la investigación; no garantiza ejecución correcta. No condiciono el cierre del MVP a una interfaz web ni a acceso a producción. La cobertura SQL, tecnologías y esfuerzo se concretan en H1-02/H1-03. Los beneficios se medirán posteriormente.

Evidencia documental: [caso de negocio v1.0](../../01-caso-de-negocio/problema-y-contexto.md), [alcance](../../01-caso-de-negocio/alcance-y-exclusiones.md) y [prioridades de valor](../../01-caso-de-negocio/beneficios-y-criterios-de-exito.md).
