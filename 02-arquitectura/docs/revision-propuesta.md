# Propuesta de arquitectura — versión para revisión

Responsable: Óscar Hernández Barrón  
Fecha-revisada: 2026-11-17  
Versión de propuesta: 1.0  
Estado: Lista para revisión y ajustes; no aprobada

## Ubicación canónica

Reviso y ajusto esta propuesta en el PDI, dentro de `02-arquitectura/`, con estructura ArchFlow. La transferencia del 17/11/2026 y sus reglas de continuidad están en [integración](validation-log.md#procedencia-transferencia). Las versiones e IDs de diseño se conservan; no hay aceptación automática por la transferencia.

## Propósito de esta entrega

Defino la arquitectura de un sistema local de linaje e impacto con interfaz web. La propuesta establece alcance, responsabilidades, contratos, decisiones y límites para orientar su implementación posterior. El cierre de esta entrega es documental: no incluye construir pantallas, ejecutar un prototipo ni demostrar capacidades funcionales. Esta delimitación está confirmada en [SRC-020](../README.md#SRC-020).

Presento aquí una guía para revisar la propuesta; mantengo las definiciones completas en sus fuentes enlazadas. Las decisiones seleccionadas son la base propuesta, no asuntos sin resolver ni decisiones formalmente aceptadas. Puedo ajustarlas durante la revisión sin atribuirles resultados de pruebas.

## Solución propuesta

El usuario selecciona una carpeta de archivos del DAG desde la web. El backend inventaría los archivos, obtiene contexto explícito y analiza SQL sin ejecutar DAGs, shell ni Spark. Guarda el resultado y sus diagnósticos. Solo una contribución completa y sin conflicto de revisión se activa en el catálogo. El usuario explora relaciones con evidencia o formula cambios hipotéticos y recibe candidatos de impacto, resumen y reporte descargable.

La interfaz ofrece tres áreas: DAGs, Linaje e Impacto. Utilizo React/TypeScript, Vite, Tailwind, React Query, Axios y Lucide conforme a la referencia aportada; FastAPI y servicios Python en backend; SQLGlot detrás de un adaptador; SQLite y archivos locales para resultados. La IA es una integración opcional de explicación, separada del descubrimiento de hechos y sin escritura al catálogo.

El despliegue objetivo inicial es de un usuario en su equipo, con API en loopback y navegador local. El grafo muestra relaciones tipadas y dispone de tabla alternativa y evidencia por selección. La descarga usa el mismo resultado y revisión que la pantalla.

## Alcance que presento a revisión

| Incluido | Límite explícito |
|---|---|
| Selección de carpeta, DAG y contexto desde web | Archivos seleccionados y rutas relativas; sin lectura arbitraria del disco ni acceso automático a repositorios |
| Linaje de tablas, columnas, STG y vistas | Perfil SQL delimitado; no cobertura universal ni conocimiento del framework ausente |
| Ventanas | Subconjunto SUM, COUNT y ROW_NUMBER definido en el diseño técnico; patrones restantes con diagnóstico |
| Guardado, actualización e historial por DAG | Parcial/fallido no reemplaza el último completo; vigencia visible |
| Eliminación/renombrado de tabla y cambios de columnas | Escenario hipotético; no ejecutar DDL ni garantizar compatibilidad en Spark |
| Visualización y reportes JSON/HTML | Límites y fronteras visibles; resultado vacío no equivale a ausencia universal de impacto |
| Explicación IA opcional | Solo contexto sintético permitido, presupuesto previo y respaldo revisable; reporte disponible sin IA |

Quedan fuera del MVP propuesto: multiusuario/LAN, despliegue cloud, ejecución de pipelines, datos productivos, cambios automáticos a código/SQL, diagnóstico de fallos de ejecución, agentes con herramientas y cobertura SQL universal. Detalle de CTE, SELECT *, SQL dinámico y otras exclusiones en [cobertura seleccionada](diseno-tecnico.md#cobertura-seleccionada).

## Decisiones seleccionadas y compromisos

| Decisión | Motivo y compromiso | Fuente |
|---|---|---|
| Separar valor, condición, consumo y orquestación | Evita presentar influencia sobre filas como valor de cada columna; requiere modelo con procedencia | [ADR-0001](../adr/ADR-0001-relaciones-y-publicacion.md#ADR-0001) |
| Adaptar análisis con SQLGlot y extracción estática | Aprovecha el parser conocido y conserva independencia del framework; requiere corregir heurísticas y probar cobertura en H2 | [ADR-0002](../adr/ADR-0002-adaptador-estatico.md#ADR-0002) |
| SQLite con revisiones y activación atómica | Reduce operación para uso local; restringe escala y coordinación de escritores | [ADR-0003](../adr/ADR-0003-sqlite-y-revisiones.md#ADR-0003) |
| JSON canónico, HTML y explicación opcional | Mantiene hechos consultables sin proveedor; calidad del texto generado requiere evaluación posterior | [ADR-0004](../adr/ADR-0004-reporte-e-ia.md#ADR-0004) |
| SPA React, API FastAPI y trabajos persistidos | Respeta el flujo web y stack conocido; añade frontend, transporte y recuperación de trabajos | [ADR-0005](../adr/ADR-0005-interfaz-web-y-trabajos.md#ADR-0005) |

## Orden de lectura recomendado

1. [SAD 1.0](sad.md): objetivos, alcance, estrategia, componentes, riesgos y viabilidad conceptual.
2. [Vistas 1.2](../diagrams/README.md): contexto, contenedores, componentes y publicación. Son fuentes Mermaid editables; su revisión visual puede hacerse al abrirlas en un visor compatible, sin construir el producto.
3. [Experiencia web 1.1](experiencia-web.md): flujos de usuario, estados y responsabilidades de API. No exige maquetas ni diseño visual exhaustivo para revisar arquitectura.
4. [Diseño técnico 1.2](diseno-tecnico.md): identidad, cobertura, persistencia, recorrido y contratos. Valores de protección son configuraciones propuestas, no capacidad medida.
5. [Requisitos](requirements.md), ADR y [plan de validación](validation.md): trazabilidad y separación entre revisión actual y pruebas futuras.

## Criterio de entrega y aceptación

La propuesta está preparada para revisión cuando existe una alternativa seleccionada por decisión significativa, cada flujo tiene responsabilidades y límites, no hay contradicciones conocidas que impidan entender el diseño y las incertidumbres tienen tratamiento. La revisión editorial y por escenarios se registra como [VAL-008](validation.md#VAL-008); no equivale a pruebas del sistema.

La aprobación de la arquitectura será una decisión posterior sobre esta versión, con observaciones o ajustes si corresponden. No marco ADR Aceptado, puertas Aprobadas ni H1-02 completado en el PDI por preparar este documento. Tampoco se necesita aprobar ahora gasto, fechas de implementación o rendimiento no medido.

| Momento | Qué corresponde |
|---|---|
| H1-02, esta propuesta | Diseño, alternativas, contratos, revisión de escenarios, riesgos y plan de verificación |
| Revisión formal de H1-02 | Aceptar o ajustar decisiones y límites; identificar versión y registrar observaciones |
| H1-03 | Estimar esfuerzo, capacidad y secuencia incluyendo web; no es prerrequisito para recibir esta propuesta |
| H2 | Construcción, fixtures, versiones reproducibles y ejecución de VAL-001 a VAL-007; no ejecutadas aquí |
| H3 | Comparaciones manual/asistido y medición de resultados previstas en el PDI |

No se impone una PoC como condición de esta entrega. Si una revisión identifica una incertidumbre crítica que requiera evidencia previa a aceptar una decisión, se registra esa condición específica y se acuerda cómo resolverla. No extenderé esa condición a todas las pruebas del producto ni registraré una excepción sin aprobación explícita.

## Supuestos y condiciones de revisión

| Supuesto o condición | Tratamiento seleccionado | Cuándo reabrir |
|---|---|---|
| Uso local, un usuario, carga manual | API loopback, ejecutor único, SQLite local | Acceso LAN, usuarios concurrentes o necesidad operativa diferente |
| Esquemas y parámetros pueden faltar | Diagnóstico y parcial no publicable donde afecten identidad/mapeo | Nuevo contexto que resuelva la ambigüedad |
| Framework no disponible | Analizar declaraciones y conservar conflictos, sin atribuir ejecución | Acceso futuro a evidencia de su semántica |
| Capacidad real aún no medida | Límites configurables y resultados truncados explícitos; sin SLA de latencia | Datos de carga de H2 o alcance mayor |
| Modelo/proveedor aún no evaluado con el corpus | Adaptador y fallback determinista | Evidencia de calidad, costo o disponibilidad incompatible |
| Versiones de dependencias aún no fijadas | Stack seleccionado; lockfiles y compatibilidad pertenecen al primer build de H2 | Incompatibilidad que afecte contratos o responsabilidades |

Los riesgos y responsables se mantienen en [SAD, sección 11](sad.md#11-riesgos-y-deuda-técnica). La selección tecnológica no constituye aceptación automática de esos riesgos.

## Puntos concretos para anotar ajustes

Durante la revisión puedo indicar el documento/sección, observación y cambio deseado. Los puntos de mayor impacto son: cobertura SQL inicial, publicación automática solo del completo, límites de uso local, presentación de incertidumbre y carácter opcional de la explicación IA. No hace falta repetir preguntas de negocio ya contestadas.

La revisión visual de diagramas y la aceptación formal siguen abiertas como actividades de revisión del entregable. No se han creado pantallas, maquetas funcionales, código del producto ni despliegues para cerrar esta propuesta.
