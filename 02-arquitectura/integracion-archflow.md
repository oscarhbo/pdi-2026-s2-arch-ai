# Integración de la propuesta ArchFlow en el PDI

Responsable: Óscar Hernández Barrón  
Fecha: 2026-11-17  
Estado: Entrega documental v1.0; pendiente de revisión y ajustes  
Perfil: Estándar; ArchFlow 0.2.0

## Decisión de organización

Adopto la estructura de ArchFlow para la arquitectura dentro de `02-arquitectura/`. Esta carpeta es la fuente canónica desde esta integración; las observaciones posteriores se aplican aquí. No reparto SAD, requisitos y validaciones entre los nombres anteriores del PDI ni mantengo una segunda arquitectura editable en ArchFlow.

Conservo las carpetas de objetivos, caso de negocio, implementación futura, evaluación de resultados, aprendizaje y presentación porque cubren entregables diferentes. No convierto todo el PDI en un framework de arquitectura ni modifico su Excel, objetivos o compromisos originales.

```text
02-arquitectura/
  README.md                       Ficha y reanudación
  docs/
    revision-propuesta.md          Entrada para revisar
    sad.md                        Arquitectura arc42 y riesgos
    requirements.md               Requisitos y trazabilidad
    diseno-tecnico.md              Contratos y mecanismos
    experiencia-web.md             Flujos y API propuestos
    recorrido-sintetico.md         Esperados de diseño
    validation.md                 Plan canónico de validación
    validation-log.md             Revisiones y ejecuciones
  adr/                            Decisiones vigentes propuestas
  diagrams/                       Vistas editables vigentes
  revision-parser-y-dags.md        Evidencia de inspección previa
  fuentes-inspeccionadas.json      Huellas de fuentes técnicas
  entrega-archflow.json            Huellas de la entrega transferida
  integracion-archflow.md          Este registro
  decisiones/                     ADR originales y revisión H1-04
```

## Procedencia y revisiones

Fuente: `C:/Oscar/Repositorios/ArchFlow/projects/linaje-impacto-ia-pdi`, propuesta 1.0 revisada editorialmente el 17/11/2026. Incluye SAD 1.0, diseño técnico 1.2, experiencia web 1.1 y vistas 1.2. Sin commit nuevo; no se presupone una revisión Git cerrada. [Manifiesto de entrega](entrega-archflow.json) registra las 17 rutas y SHA-256 de los bytes fuente anteriores a su marca de transferencia.

La copia preserva los IDs y contenido técnico; adapta la ficha para referenciar este repositorio como ubicación canónica y añade procedencia al log/propuesta. Los resultados históricos con comandos de ArchFlow conservan su fecha y contexto; no se presentan como pruebas nuevas realizadas en el PDI.

## Fuentes de verdad y equivalencias

| Contenido anterior | Fuente vigente / tratamiento |
|---|---|
| arquitectura-de-solucion.md (retirado) | [docs/sad.md](docs/sad.md); diseño consolidado |
| alternativas.md (retirado) | [adr/README.md](adr/README.md); alternativas dentro de cada decisión |
| riesgos-y-limitaciones.md (retirado) | [SAD, sección 11](docs/sad.md#11-riesgos-y-deuda-técnica) |
| diagramas/README.md (retirado) | [diagrams/README.md](diagrams/README.md); única carpeta de vistas |
| decisiones/ADR-001 | Registro original de prioridad, distinto de ADR-0001 |
| decisiones/ADR-002 | Aplicaciones de lecturas, distinto de ADR-0002 |
| decisiones/ADR-003 | Alcance original aceptado; web obligatoria posterior en SRC-019 y ADR-0005 propuesto |
| decisiones/revision-simulada-hito-1.md | Se conserva íntegra como evidencia de H1-04 |
| 04-evaluacion/plan-de-evaluacion.md (retirado) | [validation.md](docs/validation.md); criterios útiles consolidados en el plan |
| 04-evaluacion/casos-de-prueba.md (retirado) | [Diez cambios](docs/validation.md#diez-cambios-de-impacto-seleccionados) y [recorrido](docs/recorrido-sintetico.md) |
| RF-01 a RF-05 y CA-01 a CA-06 del caso | Fuentes de negocio; correspondencia en [requirements.md](docs/requirements.md), sin renumerar |
| FR/NFR/QS/VAL, EL/VIEW y SRC de ArchFlow | Se conservan con sus definiciones y referencias dentro de la arquitectura |
| Resultados funcionales futuros | Archivos de evidencia en 04-evaluacion/resultados; registro/enlaces en validation-log.md |

La coexistencia de ADR-001 y ADR-0001 no es duplicación: representan decisiones diferentes. Nuevas decisiones usan cuatro dígitos y siguiente número libre en adr/. Ningún ADR original cambia de estado por migración. Conservo registros con evidencia propia, como los ADR originales, la revisión simulada y el contexto de origen. Retiro los borradores sustituidos tras consolidar el contenido útil y ajustar enlaces; no mantengo copias paralelas del diseño.

## Reglas para continuar

[AGENTS.md](../AGENTS.md) fija estructura, convenciones, estados y uso de fuentes únicas. Mantengo voz del arquitecto en primera persona y evidencia neutral. La arquitectura continúa como borrador para revisión, sin prototipado ni implementación. No reabrir H1-01/H1-04/H1-05 ni declarar H1-02 Completado por la transferencia. H1-03 mantiene pendiente la estimación de esfuerzo, incluyendo web.

Las futuras ediciones se realizan en este PDI. El proyecto original en ArchFlow solo conserva la entrega fuente y un aviso de ubicación canónica; no sobrescribir aquí ajustes con una importación posterior sin comparar cambios.

## Verificación de transferencia

Antes de integrar se inventariaron las fuentes, los cambios locales y las huellas del PDI. Ya existían cuatro enlaces rotos a dos archivos históricos ausentes: revision-excel-2026-10-28.md y contexto-transferido-2026-10-28.md en 00-pdi/fuentes. No se reconstruye su evidencia ni se reescriben ADR aceptados para ocultarlos.

Verificación posterior realizada el 17/11/2026:

- Comprobador ArchFlow sobre la raíz completa del PDI: 4 errores LINK_MISSING, los mismos cuatro hallazgos previos, y 1 advertencia PENDING en la ficha por pendientes explícitos. No aparecen errores nuevos de estructura, IDs, estados, cobertura o enlaces creados. El resultado global no es una validación limpia porque las fuentes históricas siguen ausentes.
- Inventario de integridad: 55 archivos preexistentes contrastados por SHA-256; ningún archivo eliminado y ninguna modificación fuera de los 13 documentos previstos. Objetivos, Excel, ADR originales y revisión simulada conservan sus bytes.
- Seguimiento: las 16 filas conservan IDs, hitos, entregables, fechas originales/revisadas y estados. Solo se actualizan procedencia, evidencia y observaciones de integración.
- Transferencia: 14 de los 17 documentos fuente mantienen contenido idéntico tras normalizar finales de línea. Ficha, guía de revisión y log solo adaptan ubicación canónica, navegación y registro de transferencia. El manifiesto conserva hashes de bytes originales.
- La copia de origen en ArchFlow recibe únicamente un aviso de transferencia en su ficha. No se mantiene allí una segunda arquitectura activa.

Comando de referencia desde el PDI: `python -B C:/Oscar/Repositorios/ArchFlow/scripts/check_docs.py .`. Validar el repositorio completo permite revisar enlaces entre secciones; comprobar solo 02-arquitectura/ advertiría de enlaces legítimos fuera de esa subcarpeta. El comprobador no verifica renderizado Mermaid, resultados del producto ni aceptación de decisiones.

## Consolidación de diagramas — 17/11/2026

En la integración inicial conservé `diagramas/` como antecedente y añadí `diagrams/` con las vistas de ArchFlow. Esto dejó dos carpetas para el mismo propósito. La carpeta anterior solo contenía un README de planificación, sin diagramas ni fuentes editables.

Retiro ese índice y su carpeta vacía. Mantengo las cuatro vistas existentes en [diagrams/README.md](diagrams/README.md), sin modificar su contenido. Actualizo el destino en el contexto de transferencia y fijo en AGENTS.md una única carpeta, `02-arquitectura/diagrams/`. Las menciones a la ruta retirada en este registro documentan la corrección, no indican un destino de trabajo. La comprobación de integridad anterior corresponde a la transferencia inicial; esta corrección elimina únicamente el índice obsoleto.

## Depuración de documentos sustituidos — 17/11/2026

Retiro los tres borradores anteriores de arquitectura y los dos de evaluación indicados en el mapa. El SAD, los ADR y validation.md cubren sus responsabilidades; traslado al plan único los criterios de medición, denominadores, evidencia y control de sesgos que aún eran útiles. Elimino también la sección duplicada «Índice anterior» de decisiones/README.md. Actualizo los enlaces de negocio, aprendizaje, continuidad y fuentes históricas; en la revisión simulada identifico expresamente los enlaces a sucesores para no atribuirle una revisión del diseño actual.

Conservo los ADR originales y la revisión simulada por sus decisiones y evidencias propias; el contexto de transferencia por su procedencia; objetivos, caso de negocio y notas por su propósito distinto. Las plantillas de implementación, resultados, aprendizaje y presentación organizan entregables futuros, no duplican el SAD ni acreditan avances. Los hashes de la entrega original conservan su significado histórico y no se recalculan como si correspondieran a esta revisión.

## Archivos de la integración

Rutas relativas a la raíz del PDI. La ficha existente se actualiza; no se reemplazan decisiones originales ni evidencias de H1-04.

| Archivo | Acción | Propósito |
|---|---|---|
| `02-arquitectura/adr/ADR-0001-relaciones-y-publicacion.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/adr/ADR-0002-adaptador-estatico.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/adr/ADR-0003-sqlite-y-revisiones.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/adr/ADR-0004-reporte-e-ia.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/adr/ADR-0005-interfaz-web-y-trabajos.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/diagrams/README.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/docs/diseno-tecnico.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/docs/experiencia-web.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/docs/recorrido-sintetico.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/docs/requirements.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/docs/revision-propuesta.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/docs/sad.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/docs/validation-log.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/docs/validation.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/fuentes-inspeccionadas.json` | Crear | Trazabilidad de fuentes y entrega |
| `02-arquitectura/revision-parser-y-dags.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/adr/README.md` | Crear | Documento de arquitectura con estructura ArchFlow |
| `02-arquitectura/entrega-archflow.json` | Crear | Trazabilidad de fuentes y entrega |
| `02-arquitectura/arquitectura-de-solucion.md` | Actualizar al integrar; eliminar al depurar | Contenido útil consolidado en su fuente canónica |
| `02-arquitectura/alternativas.md` | Actualizar al integrar; eliminar al depurar | Contenido útil consolidado en su fuente canónica |
| `02-arquitectura/riesgos-y-limitaciones.md` | Actualizar al integrar; eliminar al depurar | Contenido útil consolidado en su fuente canónica |
| `02-arquitectura/diagramas/README.md` | Actualizar al integrar; eliminar al consolidar | Índice de planificación sustituido por las vistas en diagrams/ |
| `02-arquitectura/contexto-para-archflow.md` | Actualizar | Navegación, ficha o antecedente identificado |
| `02-arquitectura/decisiones/README.md` | Actualizar | Navegación, ficha o antecedente identificado |
| `AGENTS.md` | Actualizar | Reglas de continuidad y fuente canónica |
| `README.md` | Actualizar | Navegación, ficha o antecedente identificado |
| `00-pdi/estado-actual.md` | Actualizar | Estado/evidencia; conservar compromisos |
| `00-pdi/seguimiento-compromisos.md` | Actualizar | Estado/evidencia; conservar compromisos |
| `04-evaluacion/plan-de-evaluacion.md` | Actualizar al integrar; eliminar al depurar | Contenido útil consolidado en su fuente canónica |
| `04-evaluacion/casos-de-prueba.md` | Actualizar al integrar; eliminar al depurar | Contenido útil consolidado en su fuente canónica |
| `02-arquitectura/README.md` | Actualizar | Navegación, ficha o antecedente identificado |
| `02-arquitectura/integracion-archflow.md` | Crear | Mapa, procedencia y comprobaciones de integración |

No se generan remotos, commits, gasto, infraestructura ni código del producto.
