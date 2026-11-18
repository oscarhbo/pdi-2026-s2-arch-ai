# Registro de decisión

<a id="ADR-0005"></a>
## ADR-0005 — Interfaz web local y trabajos persistidos

Estado: Propuesto  
Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17  
Sustituye: No aplica  
Sustituido-por: No aplica

### Contexto y criterios

[SRC-019](../README.md#SRC-019) convierte la web en requisito y aporta osutils-web como referencia tecnológica/visual. Evoluciona la entrada CLI de ADR-0002 y complementa el reporte de ADR-0004; no retira su motor ni sus contratos. Prioridad: flujo completo desde navegador, evidencia consistente, familiaridad y operación local.

### Alternativas

| Opción | Evaluación |
|---|---|
| CLI y reporte HTML estático | Mantiene diseño previo, pero ya no satisface la interacción solicitada |
| SPA React + FastAPI local | Reutiliza stack conocido y servicios Python; añade contratos, jobs y pruebas web |
| Aplicación remota multiusuario | Acceso compartido, pero añade identidad, operación y almacenamiento fuera del alcance confirmado |

### Decisión

Selecciono SPA React/TypeScript/Vite/Tailwind, React Query/Axios/Lucide y backend modular FastAPI/Pydantic. Mantengo SQLite y SQLGlot. Tres pantallas: DAGs, explorador y evaluación de impacto. Carga de carpeta mediante archivos seleccionados en navegador, trabajos persistidos y seguimiento por polling. Guardar no equivale a publicar; «Analizar y guardar» publica automáticamente solo resultados completos sin conflicto.

Contrato y experiencia en [experiencia web](../docs/experiencia-web.md). La consulta de impacto acepta cambios tipados y conserva esquema hipotético, sin ejecutar DDL. Reportes descargados y pantalla usan el mismo resultado/revisión. Referencia técnica consultada: [frontend estático FastAPI](https://fastapi.tiangolo.com/tutorial/frontend/), [tareas](https://fastapi.tiangolo.com/tutorial/background-tasks/) y selector de carpeta enlazado en el contrato web.

### Consecuencias y revisión

Crece el esfuerzo por frontend, API, carga de archivos, cola local y visualización. La cola y recuperación son responsabilidad de la aplicación, no garantía del framework. Solo un ejecutor local; multiproceso de API o acceso LAN requieren revisar la decisión. Grafo acotado con tabla equivalente evita ocultar relaciones por saturación visual. Propongo mejoras explícitas de accesibilidad y contratos respecto de la referencia.

Trazabilidad: [FR-006](../docs/requirements.md#FR-006), [FR-004](../docs/requirements.md#FR-004), [EL-010](../diagrams/README.md#EL-010) y [EL-006](../diagrams/README.md#EL-006). Validación: [VAL-006](../docs/validation.md#VAL-006), [VAL-007](../docs/validation.md#VAL-007), No ejecutado. Revisar ante nueva cobertura, fallos, escala o cambio de despliegue.

### Historial

16/11/2026: incorporo el complemento web al alcance de la propuesta. No implica aceptación formal de puertas ni funcionalidades implementadas.
