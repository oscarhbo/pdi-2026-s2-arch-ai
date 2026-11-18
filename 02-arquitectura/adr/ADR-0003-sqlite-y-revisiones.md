# Registro de decisión

<a id="ADR-0003"></a>
## ADR-0003 — SQLite con revisiones inmutables y activación atómica

Estado: Propuesto  
Fecha-revisada: 2026-11-17  
Responsable: Óscar Hernández Barrón  
Sustituye: No aplica  
Sustituido-por: No aplica

Selecciono esta base técnica con el criterio definido en [SRC-018](../README.md#SRC-018); su aceptación formal sigue pendiente. Requisito principal: [FR-003](../docs/requirements.md#FR-003). Elemento: [EL-002](../docs/sad.md#EL-002). Contratos y parámetros: [diseño técnico](../docs/diseno-tecnico.md).

### Contexto y problema

La ejecución local/manual y la publicación por DAG requieren consistencia, historial y recuperación sin operar servicios adicionales. Debo conservar aportes ajenos incluso si comparten una relación.

### Criterios de decisión

Trazabilidad, integridad, operación local y esfuerzo proporcional al prototipo. No asigno ponderaciones ni atribuyo rendimiento medido.

### Alternativas consideradas

| Alternativa | Ventaja | Costo o límite |
|---|---|---|
| JSON por archivo | Fácil inspección | Activación multiartefacto y concurrencia quedarían a cargo de la aplicación |
| SQLite local | Transacciones y archivo local, sin servidor | Un escritor; necesita disciplina de conexiones y backup |
| PostgreSQL o motor de grafos | Escala y consultas especializadas | Operación adicional sin necesidad demostrada en el corpus |

### Decisión

Selecciono SQLite en disco local con journal DELETE, claves foráneas habilitadas y activación en una transacción explícita. Análisis y revisiones son inmutables; un mapa por revisión selecciona un análisis por DAG. Publicar compara revisión global y aporte esperado, reemplaza solo el miembro propio y cambia el puntero vigente en el mismo COMMIT. La baja crea otra revisión. Conservo historial completo durante el MVP.

### Consecuencias y trade-offs

El diseño prioriza sencillez y recuperación frente a concurrencia de múltiples escritores. No ubico la base en red o sincronización. Lectores fijan revisión y liberan transacción antes de IA. Los conflictos se rechazan; no se resuelven con sobrescritura automática. Cambios de contexto generan invalidaciones visibles.

### Validación y revisión

[VAL-003](../docs/validation.md#VAL-003) y [VAL-004](../docs/validation.md#VAL-004), aún No ejecutado. Responsable: Óscar. Revisar ante fallo del corpus, cambio de restricciones, volumen, contratos o versiones. Fuentes técnicas: [Transacciones SQLite](https://www.sqlite.org/lang_transaction.html), [claves foráneas](https://www.sqlite.org/foreignkeys.html) y [atomic commit](https://www.sqlite.org/atomiccommit.html), consultados el 16/11/2026. Las revisiones son un mecanismo de aplicación, no la API C de snapshots.

### Historial y aprobación

16/11/2026: alternativa seleccionada para consolidar H1-02. No registra aceptación de puertas, ejecución de pruebas ni gasto.
