# TOGAF Standard, 10th Edition — Notas de estudio

**Curso:** Learn about the TOGAF 10.0 Standard and prepare for the certification.  
**Plataforma:** Udemy.  
**Certificado de finalización:** [Ver certificado en Udemy](https://www.udemy.com/certificate/UC-247dda35-5c2e-467a-acde-b7f5acbe27f6/).  
**Estado:** Completado
**Documentación:** 28/10/2026.  
**Enfoque:** manual personal de conceptos esenciales y criterios de aplicación profesional. La organización es temática; no reproduce el índice de lecciones de Udemy. 

## Resumen ejecutivo

La arquitectura empresarial conecta los objetivos de una organización con sus capacidades, procesos, información, aplicaciones y tecnología. Su utilidad está en hacer coherentes las decisiones y orientar la transformación: comprender dónde estamos, definir hacia dónde queremos avanzar y organizar cambios viables.

TOGAF proporciona un marco y un método adaptable para realizar ese trabajo. El **Architecture Development Method (ADM)** articula la visión, el desarrollo de arquitecturas, la planificación de la transformación y su gobierno. La gestión de requisitos acompaña todo el ciclo; no queda resuelta al escribir una lista inicial.

Las ideas que conviene conservar son:

- **El problema y el valor de negocio preceden a la tecnología.** Una compra de infraestructura no resuelve por sí misma un proceso mal diseñado.
- **Negocio, datos, aplicaciones y tecnología se condicionan mutuamente.** Optimizar una capa de forma aislada puede trasladar el problema a otra.
- **Baseline, Target y brechas permiten justificar el cambio.** Una brecha describe lo que falta o debe cambiar; no prescribe automáticamente un producto.
- **Las dependencias y la capacidad de cambio condicionan el roadmap.** La iniciativa con mayor beneficio puede necesitar antes una inversión habilitadora.
- **La transformación necesita estados intermedios operables.** Una transición debe mantener continuidad, consistencia y control del riesgo.
- **La arquitectura se mantiene durante la implementación y la operación.** Debemos gestionar desviaciones, cambios y requisitos, además de producir diagramas.

## Mapa de contenidos

- [1. Fundamentos y lenguaje de arquitectura](#fundamentos)
- [2. ADM y gestión transversal de requisitos](#adm)
- [3. Arquitectura de negocio](#negocio)
- [4. Datos, aplicaciones y tecnología](#dominios)
- [5. Brechas, opciones y migración](#migracion)
- [6. Gobierno y evolución de la arquitectura](#gobierno)
- [7. Criterios de aplicación profesional](#aplicacion)
- [8. Repaso rápido](#repaso)

<a id="fundamentos"></a>
## 1. Fundamentos y lenguaje de arquitectura

### Arquitectura empresarial, de solución y de software

La diferencia principal está en el alcance de las decisiones y en las relaciones que necesitamos comprender.

| Disciplina | Enfoque | Ejemplo ilustrativo |
| --- | --- | --- |
| Enterprise Architecture | Coherencia entre estrategia, capacidades, procesos y sistemas en un ámbito empresarial definido. | Decidir cómo coordinar la gestión de clientes entre varias áreas y sistemas. |
| Solution Architecture | Diseño integrado para resolver una necesidad concreta dentro del contexto empresarial. | Diseñar una solución de onboarding que conecte canales, validaciones y sistemas existentes. |
| Software Architecture | Estructura interna del software, sus componentes, interfaces y atributos de calidad. | Delimitar módulos y dependencias de un servicio de validación. |

Son enfoques complementarios. Una solución puede estar bien construida internamente y aun así duplicar capacidades, romper reglas de datos o introducir dependencias inconvenientes para la organización.

Una analogía útil es la planificación de una ciudad: comprender usos del suelo, movilidad y servicios tiene un alcance diferente del diseño de un edificio o de sus instalaciones. Los tres niveles necesitan coordinarse.

### Qué aporta TOGAF

El marco ayuda a organizar el desarrollo, uso y gobierno de la arquitectura. No obliga a adoptar una nube, un producto, microservicios o una notación única.

La 10th Edition distingue **Fundamental Content**, que reúne conceptos y prácticas centrales, y **Series Guides**, que aportan orientación para aplicarlos en distintos contextos. Conviene separar el núcleo del método de la adaptación que necesita cada organización.

Adaptar el marco implica elegir profundidad, alcance, participantes y productos de trabajo útiles. No significa producir todos los documentos posibles ni omitir el razonamiento que sostiene una decisión.

### Interesados, preocupaciones, vistas y puntos de vista

| Concepto | Significado práctico | Ejemplo ilustrativo |
| --- | --- | --- |
| Stakeholder | Persona o grupo con interés en la arquitectura o en sus resultados. | Operaciones, seguridad, negocio o equipo de desarrollo. |
| Concern | Interés o preocupación que debe atenderse. | Continuidad del servicio, costos o trazabilidad de los datos. |
| Viewpoint | Convenciones para construir e interpretar una vista destinada a ciertas preocupaciones. | Reglas para representar aplicaciones, interfaces y dependencias. |
| View | Representación concreta de la arquitectura desde ese punto de vista. | Diagrama de integración de los sistemas de facturación. |

El viewpoint se parece a las reglas de un plano; la view es el plano concreto. La elección de una vista debe responder a una necesidad de comunicación o decisión. Un mismo diagrama rara vez explica todo con suficiente claridad a todos los interesados.

### Principios, requisitos y restricciones

Un **principio de arquitectura** orienta decisiones de forma relativamente estable. Un **requisito** expresa una necesidad que debemos satisfacer; una **restricción** limita las alternativas disponibles.

Ejemplo ilustrativo:

- Principio: cada dato relevante debe tener una autoridad de actualización definida.
- Requisito: las aplicaciones consumidoras deben identificar el origen y la versión del dato utilizado.
- Restricción: durante la transición debemos mantener una aplicación que aún depende del esquema anterior.

Para que un principio sea útil, conviene explicitar su fundamento y consecuencias. “Usar buenas prácticas” no permite resolver un conflicto concreto. En cambio, definir quién puede modificar un dato obliga a revisar interfaces, responsabilidades y excepciones.

### Entregables, artefactos y bloques de construcción

| Concepto | Qué representa | Ejemplo ilustrativo |
| --- | --- | --- |
| Deliverable | Producto de trabajo formalmente definido, revisado o acordado. | Documento de definición de arquitectura. |
| Artifact | Representación específica que describe un aspecto arquitectónico. | Catálogo de aplicaciones, matriz aplicaciones-datos o diagrama de procesos. |
| Building Block | Elemento arquitectónico combinable y potencialmente reutilizable. | Gestión de identidad o servicio de consulta de clientes. |

Un entregable puede reunir varios artefactos; estos pueden describir bloques y sus relaciones. No conviene usar los tres términos como sinónimos.

Un **Architecture Building Block (ABB)** describe una capacidad o elemento requerido y sus características arquitectónicas. Un **Solution Building Block (SBB)** concreta una solución que satisface esa especificación. Por ejemplo, la gestión centralizada de identidades puede expresarse como ABB; una plataforma configurada, sus conectores y su integración pueden formar parte de los SBB correspondientes.

La relación no tiene por qué ser uno a uno. Un bloque arquitectónico puede necesitar varios componentes de solución, y un componente puede contribuir a varias capacidades. Tampoco todos los bloques son productos comprados.

### Enterprise Continuum, repositorio y capacidad de arquitectura

El **Enterprise Continuum** permite clasificar activos desde lo genérico hasta lo específico de una organización. Incluye la perspectiva de arquitecturas —Architecture Continuum— y la de sus realizaciones —Solutions Continuum—. Facilita reconocer qué podemos reutilizar y qué debemos adaptar.

El **Architecture Repository** conserva y organiza los activos. La distinción práctica es sencilla: el continuum ofrece una perspectiva de clasificación; el repositorio contiene los elementos que clasificamos y utilizamos.

La **Architecture Capability** es la capacidad organizacional de realizar y mantener arquitectura: personas, competencias, responsabilidades, procesos, gobierno y herramientas. Se establece o adapta principalmente en Preliminary. No debe confundirse con una Business Capability como gestionar pedidos o atender reclamaciones.

<a id="adm"></a>
## 2. ADM y gestión transversal de requisitos

### Panorama del método

| Fase | Propósito | Pregunta que ayuda a resolver |
| --- | --- | --- |
| Preliminary | Preparar la capacidad de arquitectura, principios, gobierno y adaptación del marco. | ¿Cómo vamos a realizar y gobernar este trabajo? |
| A — Architecture Vision | Alinear objetivos, alcance, interesados y visión de alto nivel. | ¿Qué cambio necesitamos y por qué merece hacerse? |
| B — Business Architecture | Describir el negocio actual y objetivo. | ¿Qué capacidades, procesos y responsabilidades deben cambiar? |
| C — Information Systems Architectures | Desarrollar las arquitecturas de datos y aplicaciones. | ¿Qué información y sistemas necesita el negocio? |
| D — Technology Architecture | Desarrollar la arquitectura tecnológica que las soporta. | ¿Qué capacidades técnicas y atributos de calidad se requieren? |
| E — Opportunities & Solutions | Integrar opciones, paquetes de trabajo y posibles transiciones. | ¿Cómo agrupamos los cambios para alcanzar la arquitectura objetivo? |
| F — Migration Planning | Priorizar y organizar la implementación y migración. | ¿En qué orden podemos ejecutar los cambios con recursos y riesgos aceptables? |
| G — Implementation Governance | Supervisar la conformidad de la implementación con la arquitectura. | ¿Lo construido conserva las decisiones y requisitos acordados? |
| H — Architecture Change Management | Gestionar la evolución de la arquitectura. | ¿El cambio exige ajustar la arquitectura o iniciar otro ciclo? |
| Requirements Management | Mantener requisitos y evaluar su impacto en todas las fases. | ¿Qué necesidad sustenta cada decisión y qué ocurre si cambia? |

**El ADM es iterativo.** Una restricción tecnológica puede obligarnos a revisar una alternativa de aplicaciones; un cambio de negocio puede modificar datos, prioridades o alcance. No debemos interpretarlo como una cascada en la que cada fase se cierra para siempre.

Los cuatro dominios tampoco son cuatro proyectos independientes. La fase C incluye datos y aplicaciones; su desarrollo requiere coordinación, no una secuencia universal e invariable entre ambos.

### Baseline, Target y trazabilidad

La **Baseline Architecture** describe el estado actual relevante para el alcance. La **Target Architecture** describe el estado que buscamos alcanzar. El nivel de detalle debe permitir tomar decisiones: inventariar todo sin una finalidad puede consumir esfuerzo sin aclarar la transformación.

Una cadena útil para razonar es:

**Objetivo → capacidad necesaria → requisito → decisión → cambio implementado → evidencia de resultado.**

Ejemplo ilustrativo: reducir el tiempo de alta de un producto comercial requiere comprender la capacidad de gestión de productos. De ahí podemos derivar requisitos sobre configuración de reglas y responsabilidades, comparar alternativas y después comprobar si la implementación redujo las modificaciones repetidas.

### Gestionar requisitos durante todo el ciclo

No basta con capturar necesidades al inicio. Debemos identificar su origen, prioridad, criterio de aceptación y relaciones con decisiones y entregables. Cuando cambia un requisito, conviene evaluar impactos sobre fases ya trabajadas, dependencias, costos y riesgos.

Una necesidad ambigua como “el sistema debe ser rápido” requiere acordar operación, carga, medida y umbral. El valor concreto debe provenir del negocio y del contexto; no debemos inventarlo para que el documento parezca completo.

Si dos requisitos entran en conflicto, la arquitectura debe hacer visible el trade-off. Por ejemplo, exigir datos inmediatamente consistentes entre sistemas puede aumentar el acoplamiento y afectar su autonomía ante fallos.

<a id="negocio"></a>
## 3. Arquitectura de negocio

### Capacidad, proceso y servicio

| Concepto | Pregunta central | Ejemplo ilustrativo |
| --- | --- | --- |
| Business Capability | ¿Qué necesita poder hacer la organización? | Gestionar crédito. |
| Business Process | ¿Cómo ejecutamos actividades para obtener un resultado? | Recibir solicitud, validar información, evaluar riesgo y autorizar. |
| Business Service | ¿Qué valor o prestación ofrecemos a un consumidor? | Evaluación de una solicitud de crédito. |

Una capacidad es relativamente estable frente a cambios en procesos, equipos o tecnologías. Puede mantenerse aunque automaticemos tareas o cambiemos de aplicación.

El **Capability Map** organiza capacidades y ayuda a relacionar estrategia e inversiones. Permite identificar brechas o duplicidades, pero no reemplaza un mapa de procesos ni un organigrama.

También debemos distinguir **stakeholder**, **rol** y **unidad organizacional**: el primero tiene un interés, el segundo expresa una responsabilidad o función y la tercera agrupa personas dentro de la estructura. Una persona puede desempeñar varios roles; un proceso puede atravesar varias unidades.

### Diagnosticar antes de comprar tecnología

**Ejemplo trabajado: reducir el tiempo de aprobación de créditos.** Comprar servidores más potentes solo tendría sentido si la capacidad de cómputo fuese una restricción relevante. Primero necesitamos observar tiempos de procesamiento y espera, pasos manuales, validaciones duplicadas y aprobaciones redundantes.

Si la mayor demora está en una aprobación humana que ocurre una vez al día, acelerar una consulta unos milisegundos no elimina esa espera. Debemos distinguir tiempo de actividad y tiempo total del proceso, y medir el resultado de la intervención.

**Ejemplo trabajado: lanzar productos más rápido.** Si cada cambio comercial requiere modificar tres aplicaciones, conviene examinar la capacidad de definir y administrar productos. El requisito puede ser gestionar reglas y definiciones de forma gobernada y trazable, evitando modificaciones repetidas.

Centralizar reglas es una posible alternativa, no una conclusión automática. Puede reducir duplicidad, pero también introducir dependencias y concentrar responsabilidades. Primero debemos definir propiedad, variabilidad y necesidades de los consumidores.

**Idea clave:** formular el problema en términos de capacidades y resultados amplía las alternativas; nombrar un producto demasiado pronto las reduce sin suficiente fundamento.

<a id="dominios"></a>
## 4. Datos, aplicaciones y tecnología

### Arquitectura de datos

Incluye entidades, relaciones, flujos, calidad, seguridad, propiedad y uso de la información. Debemos reconocer quién responde por el significado y las reglas del dato y quién realiza su custodia técnica u operativa.

Una **fuente autorizada** define dónde se considera válida una información y quién tiene autoridad para modificarla. No exige una única base de datos física para toda la empresa. La autoridad puede definirse por dominio, entidad o atributo, con reglas claras de distribución y actualización.

Los modelos de datos, catálogos y matrices aplicaciones-datos permiten identificar responsabilidades y dependencias. Conviene registrar quién crea, consulta y modifica cada información crítica; tener varias copias no es necesariamente un error, pero mantener versiones contradictorias sin reglas sí introduce riesgo.

### Arquitectura de aplicaciones

Describe responsabilidades, servicios, interacciones y dependencias de las aplicaciones. Busca evitar duplicidad innecesaria y hacer explícito cómo colaboran. Su alcance es distinto del diseño interno de clases o módulos de una aplicación.

**Ejemplo trabajado: integración CRM–facturación.** Antes de decidir que necesitamos una API, debemos aclarar el caso de uso, los datos intercambiados, quién los mantiene, qué calidad requieren y cuánta demora es tolerable.

| Alternativa | Cuándo puede encajar | Consecuencias que debemos evaluar |
| --- | --- | --- |
| Interacción síncrona | El consumidor necesita una respuesta para continuar una operación. | Dependencia temporal, latencia, timeouts y propagación de fallos. |
| Interacción asíncrona | Podemos procesar cambios de forma diferida o desacoplar ritmos de trabajo. | Retrasos, duplicados, orden de eventos, reintentos y consistencia eventual. |

Ninguna alternativa elimina la necesidad de contratos, seguridad y observabilidad. Si un reintento puede generar dos facturas, necesitamos una estrategia de idempotencia o deduplicación adecuada al proceso. Si falla una actualización, debemos saber cómo detectarla y reconciliarla.

**Idea clave:** el mecanismo de integración debe responder al intercambio de negocio y a sus condiciones de consistencia, disponibilidad y recuperación.

### Arquitectura tecnológica y atributos de calidad

La arquitectura tecnológica define las capacidades de infraestructura y plataforma que soportan las aplicaciones: cómputo, red, almacenamiento, middleware, seguridad y operación. Elegir nube o infraestructura propia requiere considerar restricciones, capacidades del equipo, costos y riesgos; ninguna opción sustituye ese análisis.

Los siguientes criterios de ingeniería complementan la lectura arquitectónica; no representan productos obligatorios de TOGAF.

| Atributo o medida | Qué conviene distinguir |
| --- | --- |
| Rendimiento | Tiempo de respuesta y trabajo procesado bajo condiciones definidas. |
| Capacidad | Carga que podemos sostener con los recursos y límites actuales. |
| Escalabilidad | Posibilidad de sostener mayor carga al ajustar recursos o diseño. |
| Disponibilidad | Grado en que el servicio está utilizable cuando se necesita. |
| Resiliencia | Capacidad de afrontar fallos, limitar su impacto y recuperarse. |
| SLO | Objetivo medible de nivel de servicio. |
| SLA | Acuerdo sobre el servicio y sus condiciones. |
| RTO | Tiempo objetivo para recuperar el servicio tras una interrupción. |
| RPO | Pérdida de datos tolerable expresada como intervalo temporal. |

### Picos de carga y diagnóstico de disponibilidad

**Ejemplo trabajado, con cifras sintéticas:** 100 000 solicitudes distribuidas durante un día equivalen a unas **1,16 solicitudes por segundo en promedio**. Si 90 000 se concentran en diez minutos, ese intervalo promedia **150 solicitudes por segundo**. El pico instantáneo puede ser todavía mayor.

Por tanto, el volumen diario no describe por sí solo la carga que debemos soportar. Necesitamos distribución temporal, concurrencia, duración de las operaciones, crecimiento y capacidad de cada capa.

Si un servicio cae durante los picos, la disponibilidad es el atributo afectado; una limitación de capacidad o escalabilidad es una posible causa. No queda demostrada solo por la coincidencia temporal.

Conviene contrastar hipótesis con:

- Solicitudes por segundo, concurrencia, errores y latencias p95/p99.
- CPU, memoria, número de instancias y eventos de escalado.
- Conexiones y saturación de la base de datos, bloqueos y consultas lentas.
- Profundidad de colas, timeouts y comportamiento de dependencias externas.

El máximo de instancias, una política de autoscaling inadecuada o la falta de capacidad física pueden limitar el crecimiento. También pueden hacerlo un pool de conexiones, una sección serializada o una base de datos saturada.

Añadir réplicas de aplicación no resuelve necesariamente el cuello de botella de datos; incluso puede aumentar la presión sobre él. Kubernetes tampoco garantiza disponibilidad por sí mismo. Las opciones incluyen redundancia, balanceo, ajustes de capacidad, servicios administrados, colas, control de carga y degradación controlada, según el diagnóstico.

**Idea clave:** elegir una plataforma es una decisión posterior a comprender el fallo, los objetivos de servicio y los costos de operar la alternativa.

<a id="migracion"></a>
## 5. Brechas, opciones y migración

### Gap Analysis

El análisis de brechas compara Baseline y Target para identificar elementos que debemos incorporar, modificar, mantener o retirar. Se aplica a negocio, datos, aplicaciones y tecnología.

| Situación actual ilustrativa | Estado objetivo ilustrativo | Brecha que debemos resolver |
| --- | --- | --- |
| Captura manual de pedidos. | Captura integrada con validación. | Falta integración y control de datos en el ingreso. |
| Inventario actualizado con retraso. | Disponibilidad de inventario acorde con la operación. | Latencia o frecuencia de actualización incompatible con la necesidad. |
| Validación manual de pagos. | Validación trazable con intervención definida para excepciones. | Falta automatización y tratamiento explícito de excepciones. |
| Notificaciones manuales. | Comunicación oportuna según eventos del proceso. | Falta un mecanismo coordinado de emisión y seguimiento. |

“Nos falta un producto determinado” suele mezclar una brecha con una solución candidata. Una misma brecha puede resolverse mediante cambios de proceso, configuración, integración o desarrollo.

### Fase E: organizar opciones y transiciones

En Opportunities & Solutions integramos las brechas de los dominios, evaluamos alternativas y agrupamos cambios relacionados en **Work Packages**. Un paquete de trabajo es una unidad útil para organizar la transformación; no necesariamente equivale a un proyecto completo.

Una **Transition Architecture** representa un estado intermedio viable entre Baseline y Target. Debe poder operar: necesita reglas para integraciones, datos, seguridad, continuidad y coexistencia. No basta con dibujar la mitad de la arquitectura final.

Ejemplo ilustrativo: extraer gradualmente funciones de un monolito puede permitir entregas incrementales, pero exige controlar las dependencias entre componentes nuevos y existentes. Un API Gateway o microservicios serían opciones de diseño, no requisitos del método.

La transición introduce costos temporales: operación de dos entornos, interfaces de compatibilidad, soporte adicional y reconciliación de información. Estos costos deben formar parte de la evaluación.

### Fase F: priorizar y hacer viable la ejecución

Migration Planning desarrolla la priorización y organización de la migración considerando valor, dependencias, riesgo, esfuerzo, recursos y capacidad de cambio.

| Elemento | Enfoque |
| --- | --- |
| Architecture Roadmap | Evolución arquitectónica, principales cambios e hitos. |
| Implementation and Migration Plan | Organización de implementación y migración, con secuencia, recursos y coordinación. |
| Cronograma operativo de proyecto | Detalle de actividades, asignaciones y seguimiento de ejecución. |

E y F se retroalimentan. La regla de estudio “E organiza opciones; F prioriza su ejecución” ayuda a recordar el énfasis, pero no significa que la planificación empiece exclusivamente en F. En E ya se construye una primera organización del roadmap y de la migración; F la refina y consolida.

### Migrar facturación: datos, aplicaciones y operación

**Ejemplo trabajado:** existe un sistema nuevo de facturación, pero cinco aplicaciones todavía acceden directamente a la base de datos del legado. Que el nuevo sistema esté desarrollado no significa que podamos apagar el anterior.

Necesitamos analizar tres dimensiones:

| Dimensión | Aspectos que debemos comprender |
| --- | --- |
| Datos | Volumen, calidad, transformación, integridad, histórico y validación. |
| Aplicaciones | Consumidores, tablas, vistas, procedimientos, triggers y semántica transaccional. |
| Operación | Ventana de corte, continuidad, soporte, reconciliación, rollback y criterios de aceptación. |

El tamaño de la base de datos no decide por sí solo la estrategia. Debemos estimar y ensayar el tiempo total de extracción, transformación, carga, construcción de índices, validación y corte frente a la ventana disponible.

| Estrategia | Posible ventaja | Riesgo o condición relevante |
| --- | --- | --- |
| Corte completo o one-shot | Reduce el período de coexistencia. | Necesita una ventana suficiente y un corte controlado. |
| Migración progresiva por módulos | Permite limitar el alcance de cada cambio. | Requiere separabilidad y control de dependencias durante la coexistencia. |
| Datos operativos primero e histórico después | Puede reducir el volumen inicial. | Solo encaja si los procesos actuales y la trazabilidad permiten separar ambos conjuntos. |

Durante la coexistencia debemos definir qué sistema tiene autoridad sobre cada dato. La sincronización bidireccional no es una solución automática: introduce conflictos, reglas de resolución y más puntos de fallo.

El rollback cambia de naturaleza después de aceptar transacciones nuevas. Antes del corte puede bastar con volver al sistema anterior; después debemos preservar facturas, pagos y cancelaciones generados en el nuevo entorno. Conviene definir el punto de no retorno y distinguir reversión de recuperación hacia adelante.

**Idea clave:** migrar datos no elimina las dependencias de las aplicaciones ni demuestra que la operación pueda continuar sin el legado.

### Priorizar con dependencias y valor habilitador

**Ejemplo trabajado:**

| Iniciativa | Beneficio directo | Complejidad | Dependencia |
| --- | --- | --- | --- |
| A: modernizar CRM | Alto | Alta | Necesita B. |
| B: integración corporativa | Medio | Media | Sin dependencia identificada en el ejemplo. |
| C: automatizar notificaciones | Medio | Baja | Sin dependencia identificada en el ejemplo. |

B tiene valor habilitador porque permite A. Con un único equipo o recursos restringidos, **B → A → C** puede ser una secuencia razonable. Si existen recursos independientes, C puede avanzar como quick win sin retrasar B. Algunas tareas preparatorias de A también pueden comenzar antes de completar toda B.

Las dependencias determinan qué secuencias son viables. Las prioridades permiten elegir entre esas secuencias. El beneficio directo no captura por sí solo el valor estratégico de habilitar otros cambios.

<a id="gobierno"></a>
## 6. Gobierno y evolución de la arquitectura

Esta sección sintetiza los conceptos de gobierno y evolución como material de consulta; no reproduce una lección ni un ejercicio concreto del curso.

### Gobierno de arquitectura

El gobierno establece responsabilidades, autoridad y mecanismos de seguimiento para que las decisiones arquitectónicas se apliquen de forma coherente. Debe permitir resolver conflictos, evaluar excepciones y mantener trazabilidad.

El **Architecture Board** es un órgano de gobierno que puede revisar decisiones, conformidad y asuntos que requieren coordinación entre áreas. No sustituye al equipo de implementación ni necesita aprobar cada detalle del código. Su alcance debe ajustarse a la organización y al impacto de las decisiones.

Conviene distinguir:

- **Principios:** criterios que orientan las decisiones.
- **Estándares:** reglas o convenciones adoptadas para dar consistencia.
- **Revisión de conformidad:** evaluación de la implementación frente a la arquitectura y sus requisitos.
- **Excepción:** desviación que requiere fundamento, evaluación de impacto y tratamiento explícito.

Un gobierno útil busca detectar desviaciones cuando todavía es posible corregirlas con un costo razonable. Concentrar toda la revisión al final puede descubrir demasiado tarde que la implementación incumple un requisito importante.

### Architecture Contracts e Implementation Governance

Un **Architecture Contract** expresa acuerdos entre quienes patrocinan, desarrollan o implementan la arquitectura respecto a responsabilidades, entregables y conformidad. No debe confundirse con un contrato de API ni reducirse a un documento legal de compra.

La fase G se centra en mantener alineada la implementación. Para ello debemos disponer de requisitos y decisiones comprensibles, evaluar evidencia y registrar desviaciones. Gestionar conformidad no equivale a dirigir todas las actividades del proyecto: existe coordinación con la gestión de entrega, pero sus propósitos son distintos.

Ejemplo ilustrativo: si una implementación permite que dos sistemas actualicen un dato para el que habíamos definido una única autoridad, la revisión debe identificar el impacto. Podemos corregir el diseño, justificar una excepción o revisar la decisión arquitectónica. Aprobar sin registrar el cambio elimina trazabilidad.

### Architecture Change Management

La fase H atiende la evolución de la arquitectura ante nuevas necesidades, cambios operativos o condiciones del entorno. Debemos evaluar la magnitud del cambio y su efecto sobre objetivos, requisitos, dependencias y decisiones existentes.

No todo cambio requiere rehacer la arquitectura completa. Tampoco todo cambio puede tratarse como una modificación local: alterar la autoridad de datos o un contrato compartido puede afectar varias áreas. El impacto determina el nivel de revisión y si conviene iniciar un nuevo ciclo del ADM.

### Repositorio y memoria de decisiones

El repositorio permite recuperar modelos, requisitos, principios, estándares y decisiones. Su valor depende de poder distinguir versiones, alcance, responsables y estado de aprobación.

Una propuesta archivada no se convierte en decisión vigente por estar en el repositorio. Del mismo modo, un diagrama sin contexto o fecha puede inducir a utilizar una arquitectura obsoleta. Conviene conservar el fundamento de las decisiones y relacionar cambios con los requisitos que los motivaron.

<a id="aplicacion"></a>
## 7. Criterios de aplicación profesional

### Cómo orientar el trabajo de arquitectura

1. **Precisar el resultado de negocio.** Identificar qué debe mejorar y cómo podríamos comprobarlo.
2. **Delimitar el alcance y los interesados.** Acordar qué sistemas, procesos y decisiones forman parte del análisis.
3. **Comprender el estado actual relevante.** Revisar capacidades, datos, dependencias y restricciones con evidencia.
4. **Formular requisitos antes de elegir productos.** Separar necesidades de mecanismos de implementación.
5. **Comparar alternativas.** Hacer visibles beneficios, costos, supuestos y riesgos.
6. **Diseñar una transición operable.** Incluir coexistencia, responsabilidades y recuperación.
7. **Mantener trazabilidad durante la ejecución.** Relacionar cambios y pruebas con requisitos y decisiones.

Estos pasos son criterios de trabajo adaptables, no un procedimiento rígido que sustituya al ADM.

### Aplicaciones posibles al proyecto de linaje e impacto

Las siguientes son **propuestas de aplicación**, no decisiones aceptadas ni implementaciones verificadas.

| Concepto | Aplicación posible | Evidencia que permitiría evaluar su utilidad |
| --- | --- | --- |
| Stakeholders y concerns | Diferenciar necesidades de quien modifica un DAG, quien mantiene datos y quien decide sobre un cambio. | Vistas del reporte que respondan a preguntas identificadas para cada rol. |
| Baseline y Target | Separar las capacidades del parser realmente inspeccionadas de las capacidades deseadas. | Inventario verificable y comparación con requisitos del analizador. |
| Gap Analysis | Expresar patrones no soportados como brechas de cobertura, sin elegir automáticamente una biblioteca. | Casos representativos y resultados de cobertura. |
| Arquitectura de datos | Definir identidad de tablas y columnas, versión, origen y evidencia del linaje. | Trazabilidad de cada relación a su artefacto de origen. |
| ABB y SBB | Separar la capacidad de descubrir linaje de la tecnología concreta que la implementaría. | Comparación de alternativas contra requisitos. |
| Transición y dependencias | Organizar entregas que permitan evaluar capacidades parciales sin ocultar límites. | Criterios de aceptación por incremento y registro de dependencias. |
| Gobierno y requisitos | Registrar cambios de cobertura y decisiones aceptadas con su fundamento. | Vínculos entre requisitos, ADR y pruebas cuando existan. |

La utilidad del marco en este proyecto no depende de implantar una estructura corporativa completa. Conviene conservar trazabilidad y disciplina de decisión con documentación proporcional al alcance.

<a id="repaso"></a>
## 8. Repaso rápido

### Diferencias que conviene recordar

| Conceptos | Distinción esencial |
| --- | --- |
| Capacidad / proceso / servicio | Qué podemos hacer / cómo lo ejecutamos / qué prestación ofrecemos. |
| Stakeholder / concern | Quién tiene interés / qué le preocupa. |
| Viewpoint / view | Convenciones de representación / representación concreta. |
| Baseline / Target / gap | Estado actual / estado deseado / diferencia que debemos resolver. |
| Deliverable / artifact / building block | Producto formal / representación específica / elemento combinable. |
| ABB / SBB | Especificación arquitectónica requerida / solución que la satisface. |
| Enterprise Continuum / Repository | Perspectiva de clasificación / conservación y organización de activos. |
| Architecture Capability / Business Capability | Capacidad de hacer arquitectura / capacidad del negocio. |
| E / F | Organizar opciones y transiciones / refinar prioridades y plan de ejecución. |
| G / H | Conformidad de implementación / evolución de la arquitectura. |
| Roadmap / plan de migración | Trayectoria arquitectónica / organización de su realización. |
| Disponibilidad / escalabilidad | Servicio utilizable / capacidad de sostener crecimiento de carga. |
| Migración de datos / retiro del legado | Transferir información / eliminar de forma controlada sus dependencias operativas. |

### ADM en una línea por grupo

- **Preliminary y A:** preparar la práctica y alinear la transformación.
- **B, C y D:** desarrollar los dominios y comprender sus brechas.
- **E y F:** convertir las brechas en una transformación viable.
- **G y H:** mantener conformidad y gestionar evolución.
- **Requirements Management:** mantener las necesidades y su impacto durante todo el ciclo.

### Principios de consulta rápida

- Un diagrama es útil cuando ayuda a atender una preocupación o decidir.
- Una brecha no es una marca de producto.
- El volumen diario no permite dimensionar un pico.
- Más réplicas no eliminan cualquier cuello de botella.
- Una arquitectura intermedia debe poder operar.
- El rollback debe preservar las transacciones nuevas.
- El valor habilitador también cuenta al priorizar.
- El ADM admite iteración; los requisitos atraviesan todas sus fases.
- Una propuesta, una decisión aceptada y un resultado comprobado son estados distintos.
