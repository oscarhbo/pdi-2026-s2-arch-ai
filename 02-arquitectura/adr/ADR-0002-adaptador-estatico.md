# Registro de decisión

<a id="ADR-0002"></a>
## ADR-0002 — Adaptador estático y aplicación Python modular

Estado: Propuesto  
Fecha-revisada: 2026-11-17  
Responsable: Óscar Hernández Barrón  
Sustituye: No aplica  
Sustituido-por: No aplica

Selecciono esta base técnica con el criterio definido en [SRC-018](../README.md#SRC-018); su aceptación formal sigue pendiente. Requisito principal: [FR-001](../docs/requirements.md#FR-001). Elemento: [EL-001](../docs/sad.md#EL-001). Contratos y parámetros: [diseño técnico](../docs/diseno-tecnico.md).

### Contexto y problema

El extractor inspeccionado conserva convenciones útiles, pero sus heurísticas de descubrimiento, sustitución y extracción no acreditan linaje de columnas. Necesito reutilizar conocimiento sin importar ni ejecutar el framework.

### Criterios de decisión

Trazabilidad, integridad, operación local y esfuerzo proporcional al prototipo. No asigno ponderaciones ni atribuyo rendimiento medido.

### Alternativas consideradas

| Alternativa | Ventaja | Costo o límite |
|---|---|---|
| Conservar extractor sin cambios | Menor cambio inicial | Defectos observados y contrato insuficiente |
| Adaptador propio con SQLGlot | Reutiliza AST, scopes y dialecto; permite contrato independiente | Hay que implementar mapeos, condiciones y evidencia |
| Parser SQL desde cero | Control total | Mayor esfuerzo y riesgo para este prototipo |

### Decisión

Selecciono servicios Python modulares locales y SQLGlot detrás de un adaptador; la interacción de usuario es web mediante FastAPI conforme a ADR-0005. La CLI inicial deja de ser un requisito del MVP. AST estático para Python y carga segura de YAML; sin PLY para extraer cadenas ni regex destructivas sobre SQL. Reutilizo convenciones y conocimiento del parser previo, no su JSON como catálogo publicado. Fijo dependencias al preparar el primer build reproducible; eso no reproduce el entorno original desconocido.

### Consecuencias y trade-offs

La biblioteca resuelve parte del análisis SQL, no todo el contrato. El adaptador debe distinguir scopes, mapear INSERT y ventanas, conservar originales y emitir diagnósticos. Las capacidades seleccionadas siguen pendientes de pruebas. Mantener una interfaz facilita cambiar biblioteca si falla el corpus.

### Validación y revisión

[VAL-001](../docs/validation.md#VAL-001) y [VAL-002](../docs/validation.md#VAL-002), aún No ejecutado. Responsable: Óscar. Revisar ante fallo del corpus, cambio de restricciones, volumen, contratos o versiones. Fuentes técnicas: [SQLGlot: lineage](https://github.com/tobymao/sqlglot/blob/main/_autodocs/api-reference/lineage.md) y [AST](https://github.com/tobymao/sqlglot/blob/main/posts/ast_primer.md). Consultados mediante Context7 el 16/11/2026. La API de linaje trabaja sobre consultas; no sustituye el tratamiento de DDL/escrituras.

### Historial y aprobación

16/11/2026: alternativa seleccionada para consolidar H1-02. No registra aceptación de puertas, ejecución de pruebas ni gasto.



### Evolución por interfaz web

SRC-019 y [ADR-0005](ADR-0005-interfaz-web-y-trabajos.md#ADR-0005) actualizan la interacción: SPA React y FastAPI sustituyen la entrada CLI y añaden visualización. Se conservan motor y reportes. La decisión original permanece Propuesto, sin atribuir aprobación. Detalle vigente en [experiencia web](../docs/experiencia-web.md).
