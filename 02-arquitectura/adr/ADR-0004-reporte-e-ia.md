# Registro de decisión

<a id="ADR-0004"></a>
## ADR-0004 — Reporte JSON y HTML con explicación IA opcional

Estado: Propuesto  
Fecha-revisada: 2026-11-17  
Responsable: Óscar Hernández Barrón  
Sustituye: No aplica  
Sustituido-por: No aplica

Selecciono esta base técnica con el criterio definido en [SRC-018](../README.md#SRC-018); su aceptación formal sigue pendiente. Requisito principal: [FR-005](../docs/requirements.md#FR-005). Elemento: [EL-004](../docs/sad.md#EL-004). Contratos y parámetros: [diseño técnico](../docs/diseno-tecnico.md).

### Contexto y problema

El reporte complementa la interfaz web obligatoria por SRC-019 y ADR-0005. Necesito conservar resultado verificable en pantalla y descarga, con funcionamiento local cuando no hay conexión, presupuesto o explicación válida.

### Criterios de decisión

Trazabilidad, integridad, operación local y esfuerzo proporcional al prototipo. No asigno ponderaciones ni atribuyo rendimiento medido.

### Alternativas consideradas

| Alternativa | Ventaja | Costo o límite |
|---|---|---|
| Plantillas deterministas | Offline y hechos controlados | No evalúa por sí sola el componente IA del PDI |
| Modelo local | No requiere API remota | Capacidad y calidad del equipo no acreditadas |
| API detrás de adaptador más fallback | Permite evaluar explicación sin operar un modelo | Costo, conectividad y control de evidencia |

### Decisión

Selecciono JSON canónico y HTML autocontenido generado desde él. Para la evaluación IA selecciono Responses API de OpenAI con snapshot gpt-5.4-mini-2026-03-17, detrás de un adaptador, sin herramientas ni escritura al catálogo. El modo predeterminado es determinista/offline; habilitar consumo requiere presupuesto y credencial autorizados. La explicación recibe solo resultados y evidencia sintéticos permitidos.

### Consecuencias y trade-offs

La salida estructurada permite comprobar formato y referencias, pero no demuestra verdad semántica. Los hechos se muestran desde el resultado determinista; texto generado conserva estado de verificación y recomendaciones separadas. Un fallo, rechazo o falta de respaldo no elimina el reporte. La prueba con doble no acredita calidad del modelo real. No se incorpora RAG ni agentes.

### Validación y revisión

[VAL-005](../docs/validation.md#VAL-005) y [VAL-004](../docs/validation.md#VAL-004), aún No ejecutado. Responsable: Óscar. Revisar ante fallo del corpus, cambio de restricciones, volumen, contratos o versiones. Fuentes técnicas: [Modelo seleccionado](https://developers.openai.com/api/docs/models/gpt-5.4-mini) y [Structured Outputs](https://developers.openai.com/api/docs/guides/structured-outputs?api-mode=responses), consultados el 16/11/2026. La elección es una base de evaluación, no un benchmark realizado.

### Historial y aprobación

16/11/2026: alternativa seleccionada para consolidar H1-02. No registra aceptación de puertas, ejecución de pruebas ni gasto.



### Evolución por interfaz web

SRC-019 y [ADR-0005](ADR-0005-interfaz-web-y-trabajos.md#ADR-0005) actualizan la interacción: SPA React y FastAPI sustituyen la entrada CLI y añaden visualización. Se conservan motor y reportes. La decisión original permanece Propuesto, sin atribuir aprobación. Detalle vigente en [experiencia web](../docs/experiencia-web.md).
