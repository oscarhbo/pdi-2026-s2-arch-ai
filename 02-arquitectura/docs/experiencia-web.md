# Experiencia web y contrato de aplicación

Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17  
Estado: Propuesta 1.1 para revisión

La interfaz web pasa a ser parte obligatoria del MVP por [SRC-019](../README.md#SRC-019). Adapto la referencia osutils-web; no incorporo sus funciones Oracle, agentes ni experimentos. En este documento defino los flujos, el transporte y la experiencia web. El [diseño técnico](diseno-tecnico.md) conserva el motor y la persistencia. Es diseño, no interfaz implementada. Según [SRC-020](../README.md#SRC-020), esta entrega no incluye bocetos obligatorios, prototipos navegables ni validaciones ejecutadas de UI. Los flujos y contratos siguientes son la propuesta revisable; tamaños, límites y estilos orientan el diseño posterior.

## Capas y tecnología

| Capa | Selección | Responsabilidad |
|---|---|---|
| Navegador | React, TypeScript estricto, Vite | SPA con navegación y estado de formularios |
| Presentación | Tailwind con tokens CSS, Lucide | Layout, controles, estados y temas de la referencia |
| Datos remotos | TanStack React Query y Axios centralizado | Consultas, mutaciones, seguimiento de trabajos e invalidación por revisión |
| Transporte | FastAPI, Uvicorn, modelos Pydantic y Pydantic Settings | API /api/v1, validación, errores y configuración |
| Aplicación | Servicios Python y ejecutor local de trabajos | Coordinar ingesta, análisis, publicación, consulta y reporte |
| Dominio e infraestructura | Adaptador SQLGlot, repositorio SQLite y adaptador IA | Mantener los contratos ya definidos |

Conservo las tecnologías conocidas, no copio las versiones del documento como si fueran las versiones instaladas del proyecto. npm y uv con lockfiles coherentes al implementar. Contratos TypeScript derivados de OpenAPI como mejora propuesta para evitar divergencia manual. No necesito Oracle, ChromaDB, Vanna, Google ADK ni agentes con herramientas.

Organización prevista: frontend/src/{pages,components,contexts,api}; backend/app/{core,routers,services,analysis,db,providers,jobs}. Los routers traducen HTTP, los servicios coordinan y el dominio no depende de React ni FastAPI. La CLI deja de ser requisito de usuario; puede conservarse como adaptador interno de desarrollo.

## Tres pantallas

### DAGs: documentar y consultar historial

Lista de DAGs con nombre, fecha de carga, revisión publicada, estado del último análisis y vigencia. Acción «Documentar DAG» abre el flujo:

1. «Seleccionar carpeta» abre el selector del navegador. Se previsualizan archivos elegibles y rutas relativas; la carga empieza al pulsar «Cargar y revisar».
2. El servidor inventaría archivos, calcula hashes y propone DAG/configuración a partir de declaraciones estáticas. Si hay varios DAGs o asociaciones ambiguas, el usuario elige; no se omite silenciosamente ninguno. Parámetros/esquemas faltantes pueden completarse en el formulario o adjuntarse.
3. «Analizar y guardar» crea trabajo persistido. Muestra fase y conteos reales, sin porcentaje ficticio: inventario, extracción, resolución, persistencia y reporte.
4. Todos los resultados válidos como documentos de análisis se guardan, también parciales y fallidos. Si el resultado es completo, se publica automáticamente con revisión esperada y operación idempotente. La acción inicial autoriza esa publicación; no exige otro clic rutinario. Un conflicto conserva el análisis preparado y ofrece revisar/reintentar sobre la revisión actual.
5. Abre el resultado: resumen, «Ver linaje», diagnósticos y descargas. Un parcial muestra «Guardado, no publicado»; el catálogo conserva la última contribución completa.

El selector usa archivos y rutas relativas proporcionados por el navegador; no promete entregar al servidor una ruta absoluta C:\ ni acceso libre al disco. La API no recibe una ruta arbitraria para leer. Si faltan archivos externos a la carpeta, se solicita adjuntarlos explícitamente. Sin selección de carpeta compatible, permitir selección múltiple y mapeo explícito de rutas. Referencia: [selección de directorios y rutas relativas](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/webkitdirectory).

Para el primer prototipo, paquetes sintéticos. No cambia la restricción sobre enviar archivos empresariales a IA. Admitir solo extensiones del perfil (.py, .yaml, .yml, .sql, .sh y contexto .json); excluir .git, entornos, .env y claves. Validar extensión, contenido, rutas, duplicados y cuotas en servidor, sin confiar en filtros del navegador. Defaults de protección propuestos: 1 000 archivos, 10 MiB por archivo y 100 MiB por paquete; contar bytes recibidos y rechazar con causa visible. No extraer ZIP en este primer flujo. Un archivo relevante rechazado impide tratar el paquete como completo. El servidor genera nombres físicos y conserva rutas lógicas; bloquea traversal, rutas absolutas y colisiones.

### Explorador de linaje

Cabecera con DAG/objeto, revisión del catálogo o análisis preparado, vigencia y cobertura. Filtros upstream/downstream, tipo de relación y profundidad. Vista inicial por tablas/vistas/STG; expandir un objeto muestra columnas. Consultas disponibles aunque no se active IA.

Centro: grafo dirigido de solo lectura, con zoom, desplazamiento, selección y foco en un objeto. Panel derecho: nombre cualificado, esquema, DAG/tarea, expresión original, evidencias y diagnósticos. Debajo: tabla accesible con las mismas relaciones, ordenable y filtrable. Valor, condición, consumo y orquestación usan etiquetas y trazos distintos, además de color. El orden de tareas no se presenta como flujo de datos.

No dibujo todas las columnas de todos los DAGs por defecto. Selecciono grafo SVG propio de alcance acotado, alineado con la referencia, con disposición por niveles y tratamiento explícito de ciclos; si su usabilidad no supera VAL-006, revisar biblioteca especializada antes de ampliar escala. Límite inicial de presentación: 200 nodos y 400 aristas, independiente del límite del motor. «Vista acotada» muestra conteo total conocido y permite enfocar/filtrar; la tabla paginada conserva acceso al resultado completo disponible. No se vuelve a ejecutar el análisis al mover el grafo.

Un análisis parcial puede explorarse como borrador aislado; nunca se mezcla visualmente con el catálogo vigente sin indicación. Al seleccionar una relación, se muestra la evidencia exacta de esa revisión. «Sin relaciones encontradas» incluye cobertura y fronteras, sin afirmar ausencia universal de linaje.

### Impacto de cambios

Formulario: catálogo/revisión, tabla o vista cualificada, lista ordenada de cambios y motivo opcional. Selector con búsqueda sobre el catálogo; una identidad ambigua o ausente no se resuelve por texto parecido. «Modificar tabla» abre operaciones concretas, no dispara una interpretación libre de la frase.

| Operación | Entrada requerida | Evaluación |
|---|---|---|
| Eliminar tabla/vista | object_id | Lectores, escritores, usos de columnas y consumidores transitivos; no ejecutar DROP |
| Renombrar tabla/vista | object_id y nuevo nombre | Usos del nombre anterior y dependencias; validar colisiones |
| Agregar columna | Nombre, tipo, nulabilidad, posición opcional y default declarado | Escrituras por posición/aridad, contratos de esquema y consumidores con cobertura desconocida; no crear aristas de valor para una columna inexistente |
| Quitar columna | column_id | Usos directos y transitivos por valor, condición y escritura |
| Renombrar columna | column_id y nuevo nombre | Referencias existentes y candidatos; no reescribir SQL |
| Modificar columna | column_id y cambios concretos de tipo/nulabilidad/default/posición | Expresiones y escrituras que deben revisarse; compatibilidad de ejecución no demostrada |

Cambios se aplican secuencialmente a una copia hipotética del esquema base. Cada operación tiene change_id y referencias estables; rechazar contradicciones como modificar una columna ya eliminada. Agregar y luego usar una columna nueva se identifica en el esquema hipotético sin atribuirle consumidores existentes. Persistir versión antes/después y resultados por cambio, deduplicando candidatos pero conservando todos los motivos. Nunca mutar esquema vigente ni ejecutar DDL por evaluar impacto.

Al agregar columna, un SELECT explícito no recibe una dependencia de valor nueva; un INSERT sin lista destino puede ser candidato por aridad/posición. SELECT * sigue fuera de cobertura inicial: si hay un análisis parcial conocido con ese patrón, mostrar limitación y consumidor potencial sin certeza de mapeo. Si el consumidor no se ha cargado, no se supone conocido. Tipo/nulabilidad/default se tratan como compatibilidad por revisar, no simulación de datos ni diagnóstico cierto de rotura.

Resultado en pantalla: resumen de objetos/DAGs candidatos, directos/transitivos, motivos y cobertura; tabla priorizada por evidencia y cercanía, sin puntuación de riesgo inventada; grafo enfocado del impacto; panel de evidencia. Etiquetas «Dependencia verificada», «Compatibilidad por revisar» y «Contexto insuficiente» separan conceptos. Explicación IA opcional en otra sección. Descargas JSON y HTML corresponden al mismo result_id/revisión que la pantalla, sin recalcular sobre el catálogo más reciente.

## Contrato HTTP y trabajos

API propuesta, no implementada. Versionar transporte independientemente del contrato interno de análisis. Objetos de respuesta incluyen schema_version, request_id, IDs y enlaces relativos de consulta.

| Método y recurso | Contrato esencial |
|---|---|
| POST /api/v1/packages | Multipart de archivos y manifiesto de rutas relativas; devuelve package_id, inventario, candidatos y diagnósticos |
| POST /api/v1/analyses | package_id, dag_id, contexto, expected_catalog_revision y publish_on_complete=true; 202 con job_id |
| GET /api/v1/jobs/{id} | kind, estado, fase, conteos, timestamps, result_id, error y publication_status |
| GET /api/v1/dags | Lista paginada, revisión activa, último intento y vigencia |
| GET /api/v1/analyses/{id} | Análisis preparado, cobertura, evidencia y estado de publicación |
| POST /api/v1/analyses/{id}/publish | Revisión global/aporte esperado; publicar análisis completo tras conflicto, sin reanalizar silenciosamente |
| POST /api/v1/lineage-queries | Objeto, dirección, relaciones y revisión; 202 con job_id; guarda resultado |
| POST /api/v1/impact-assessments | Revisión base, objeto y changes[] tipados; 202 con job_id |
| GET /api/v1/results/{id} | Resumen y metadatos de resultado inmutable; enlaces a relaciones/evidencias paginadas |
| GET /api/v1/results/{id}/relations | Cursor, filtros, nodos/aristas y límites; misma revisión del resultado |
| GET /api/v1/results/{id}/report?format=json\|html | Archivo persistido del resultado; no relanza evaluación |
| POST /api/v1/results/{id}/explanation | Opcional; job separado y autorizado, no retrasa disponibilidad del resultado determinista |

Error uniforme: code, message, field_errors, diagnostic_refs, retryable, request_id. Distinguir 404 inexistente, 409 conflicto/idempotencia contradictoria, 413 cuota, 422 entrada inválida y 503 indisponibilidad. Idempotency-Key en mutaciones: misma clave/payload devuelve el mismo trabajo; mismo identificador con otro payload devuelve conflicto. La UI no reintenta mutaciones automáticamente sin esta protección.

Persistir jobs, packages, escenarios y resultados en SQLite más archivos de contenido en workspace propio. Un único ejecutor de trabajos por instancia local consume la cola persistida; análisis CPU en proceso hijo controlado, sin ejecutar el código de entrada. API y ejecutor comparten la aplicación modular, no requieren broker ni microservicios. Transacciones de escritura breves, espera acotada y conflicto visible.

Estados de trabajo: queued, running, succeeded, failed, interrupted; resultado de análisis complete/partial/failed es un estado distinto. Reanudar navegación o recargar consulta job_id y no inicia otro análisis. Polling cada 2 s mientras esté activo, desacelerado si la pestaña no está visible. Al reiniciar el servicio, los running sin ejecutor se marcan interrupted; los queued permanecen disponibles; reintentar crea un intento vinculado. Si la activación ya hizo COMMIT, reconciliar por operation_id y recuperar revisión antes de mostrar fallo o repetir publicación. No se promete continuidad garantizada de tareas solo por usar BackgroundTasks; [documentación FastAPI](https://fastapi.tiangolo.com/tutorial/background-tasks/).

Paquetes se almacenan completos antes de encolar; carga interrumpida nunca genera paquete listo. Conservar durante el MVP paquetes, resultados y revisiones referenciados, sin borrar evidencia automáticamente. Temporales de cargas abortadas se limpian al arrancar, solo dentro del workspace temporal del sistema. Backup consistente incluye SQLite y contenidos referenciados; hashes y manifiesto permiten verificar restauración.

## Despliegue y estado

MVP de un usuario en el mismo equipo: navegador + backend local, enlazado solo a loopback. Build React servido por la aplicación para mismo origen; Vite con proxy /api en desarrollo. Docker/Nginx queda como opción posterior de empaquetado de la referencia, sin requisito de despliegue ahora. Validar Host/Origin y proteger mutaciones con sesión local y token CSRF; no CORS comodín. Credenciales IA solo en backend. Multiusuario/LAN exige otra decisión de identidad, permisos y despliegue.

Navegación con URLs de fragmento para enlaces recargables: #/dags, #/lineage?result=..., #/impact?result=...; resolver estados en App sin exigir otra biblioteca inicialmente. Formularios de impacto se conservan como borradores en servidor; selección de archivos aún no cargados debe repetirse al recargar. React Query mantiene caché por revisión; la fuente de verdad es servidor. Tema es la única preferencia persistida localmente por defecto.

## Look and feel y mejoras propuestas

Adopto sidebar 224 px, topbar 48 px, padding 24 px, paneles de radio 8 px, acento turquesa, Barlow para UI y JetBrains Mono para datos. Dark predeterminado, Light y Cyber con tokens de la referencia; temas validados contra lista permitida. Fuentes empaquetadas localmente con sus licencias para conservar uso offline; no dependencia de Google Fonts en ejecución.

Todo el flujo en español. Sidebar: DAGs, Linaje e Impacto; accesos a historial dentro de cada pantalla. No incluir búsqueda, notificaciones ni ajustes decorativos sin función. Estados vacío, cargando, en cola, parcial, error, éxito, conflicto y contexto obsoleto visibles y con próxima acción concreta.

Mejoras sobre la referencia: enlaces recargables, trabajos persistidos, tipos derivados del contrato, tabla alternativa al grafo, foco/teclado/Escape, movimiento reducido, sidebar colapsable y objetivo de contraste a verificar durante implementación. Datos técnicos de 12 px como mínimo habitual; no usar 9 px para información necesaria. Validar escritorio a 1280 px y adaptación a 768 px, zoom 200% y los tres temas. El detalle de un nodo debe poder abrirse desde tabla sin usar ratón.

## Validación y alcance del cambio

[VAL-006](validation.md#VAL-006) comprueba el recorrido web; [VAL-007](validation.md#VAL-007) comprueba los nuevos cambios de esquema. Conservan las validaciones previas de motor. No hay UI construida ni pruebas de navegador en esta ronda. La ampliación modifica esfuerzo de H1-03: añade frontend, API, carga, trabajos, visualización y pruebas de integración; no se presume que cabe en la estimación anterior.
