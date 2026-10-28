# Costos y esfuerzo

**Estado: En curso. Versión 0.1, 28/10/2026.** Modelo de esfuerzo y costo para revisar el caso. No es cotización ni consumo medido. No se conoce todavía la capacidad del parser ni el esfuerzo de resolver los patrones seleccionados.

## Recursos conocidos y límites

El PDI registra 4 h/semana de práctica y 2 h/semana de revisión para el primer objetivo. Formación sin cantidad global fijada. USD 20 es presupuesto API propuesto, no gasto autorizado ni costo total del proyecto. Los objetivos posteriores no tienen una capacidad global confirmada. Ver [plan](../00-pdi/plan-de-trabajo.md).

No se asume costo de nube cero ni uso gratuito de IA. No se necesita autorizar gasto para completar el caso documental. Cualquier precio futuro deberá indicar proveedor, unidad, moneda y fecha de consulta.

## Desglose para estimar antes de comprometer ejecución

| Trabajo | Principal factor de esfuerzo | Evidencia necesaria para estimar |
| --- | --- | --- |
| Delimitar cobertura y resultados esperados | Variación de SQL, vistas, esquemas y contexto del framework. | Muestra representativa y matriz incluir/no resolver. |
| Evaluar y desarrollar/adaptar parser | Distancia entre capacidad existente y patrones elegidos. | Inspección de código y pruebas de comportamiento. |
| Extraer artefactos | Convenciones YAML, parámetros y orden de tareas. | Ejemplos y reglas verificadas. |
| Resolver linaje y STG | Alias, columnas derivadas, condiciones y ámbito de intermedios. | Relaciones esperadas y casos límite acotados. |
| Mantener conocimiento y consultar impacto | Identidad, versiones, cambios/bajas y recorrido transitivo. | Diseño de actualización y consultas requeridas. |
| Explicar y validar con IA | Tamaño de evidencia, llamadas, reintentos y revisión. | Formato de entrada/salida y límites de consumo. |
| Evaluar, corregir y presentar | Fallos encontrados, diez cambios y cinco comparaciones. | Protocolo, resultados y registro de horas. |

Asignar horas por paquete después de esa inspección y separar tiempo de aprendizaje del de construcción. No sumar horas sin estimar como cero. La principal incertidumbre de esfuerzo es la resolución de columnas y su mantenimiento, no el texto de la explicación.

## Modelo de costo del piloto

Costo de construcción = horas efectivas de construcción × costo por hora, más gastos de herramientas/infraestructura autorizados. Si el objetivo es solo registrar dedicación personal, informar horas sin monetizarlas con una tarifa inventada.

Costo recurrente por periodo = incorporación y actualización de DAGs + consultas y revisión de reportes + consumo IA + operación. No contabilizar la misma hora simultáneamente como mantenimiento y consulta. La formación se reporta aparte.

Para estimar consumo IA, usar número de llamadas, tokens de entrada/salida y precios verificados del modelo seleccionado; incluir reintentos. Hasta seleccionar proveedor y autorización, importe desconocido. La existencia de USD 20 propuestos no habilita su consumo.

## Comparación de esfuerzo y punto de compensación

Definir, sobre el mismo corpus y periodo:

- I: horas de incorporación inicial del conocimiento, separadas de construcción del software.
- U: horas de mantenimiento durante el periodo.
- M: horas de análisis manual por consulta comparable.
- S: horas de uso de la solución y verificación de su reporte por consulta.
- N: número de consultas comparables.

Horas manuales = N × M. Horas asistidas = I + U + N × S. Diferencia neta = N × (M − S) − I − U.

Si M > S y U se fija para ese periodo, el volumen de compensación del esfuerzo operativo sería N = (I + U) / (M − S). Si M ≤ S, no hay compensación por volumen bajo estos supuestos. Si U crece con N o cambia el corpus, recalcular; no extrapolar una cifra fija.

Ejemplo exclusivamente aritmético, no pronóstico: I = 4 h, U = 1 h, M = 1 h y S = 0.5 h darían compensación operativa a 10 consultas y ahorro después de ese volumen. Estas cifras no describen al usuario ni al prototipo. El cálculo excluye construcción, formación y gastos, por lo que no es retorno total de inversión.

Para justificar una inversión real, añadir construcción y gastos al modelo monetario y contrastar frecuencia/mezcla de consultas, costo de hora y calidad con evidencia. No monetizar incidentes evitados sin una base observada.

## Sensibilidad y decisión

Con cambios poco frecuentes o mucha actualización, la alternativa manual puede ser preferible. Con patrones repetidos y conocimiento estable, puede aumentar la reutilización. Más llamadas IA pueden mejorar presentación o elevar costo sin mejorar detección: medir ambas cosas por separado. Son escenarios a contrastar, no una recomendación de compra.

Antes de aceptar la implementación del MVP: confirmar disponibilidad semanal restante, horas por paquete, contingencia explícita y cobertura que cabe. Si no cabe, proponer reducir patrones y automatización, conservando tablas/columnas y fechas originales hasta que se acuerde otra cosa.
