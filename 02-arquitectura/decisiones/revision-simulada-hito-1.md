# Revisión simulada del diseño — Hito 1

**Fecha:** 06/11/2026 · **Versión:** 0.1 · **Compromiso:** H1-04 · **Estado:** En progreso de revisión.

## 1. Propósito y cómo usar este documento

Este ejercicio desarrolla la actividad de adaptación autónoma del primer hito: simular preguntas de cliente, revisar linaje y actualización entre DAGs, examinar evidencia e incertidumbre y fundamentar ajustes al diseño. Fecha original del compromiso: 15/11/2026.

Las preguntas por parte del cliente son una simulación por IA, que me sivieron como ejercicio para poner en práctica el criterio y el aprendizaje adquierido durante la formación mediante libros , cursos y de lo desarrollado con l apropia experiencia laboral que tengo.

Las decisiones importantes se documentarán en ADR según el [índice de decisiones](README.md). No se requiere revisión de un cliente real ni de un tercero para este ejercicio.

## 2. Base revisada y escenario

Documentos de base para la IA, para la elaboración de la simulación:

| Ref. | Documento y versión identificable | Uso en el contraste |
| --- | --- | --- |
| F1 | [Objetivos del PDI](../../00-pdi/Objetivos_PDI_2026.md), transcripción sincronizada el 28/10/2026 | Entregable, fecha, prioridad de tablas/columnas y separación de hitos. |
| F2 | [Caso de negocio](../../01-caso-de-negocio/problema-y-contexto.md), v0.1 del 28/10/2026 | Usuario, problema, escenario y beneficios por validar. |
| F3 | [Alcance](../../01-caso-de-negocio/alcance-y-exclusiones.md), refinamiento del 28/10/2026 | RF-01 a RF-05, cobertura pendiente y exclusiones propuestas. |
| F4 | [Arquitectura de solución](../arquitectura-de-solucion.md), modelo conceptual del 28/10/2026 | Responsabilidades, contratos por definir, evidencia y actualización. |
| F5 | [Alternativas](../alternativas.md), matriz del 28/10/2026 | Parser, persistencia, activación y función de IA. |
| F6 | [Riesgos y limitaciones](../riesgos-y-limitaciones.md), versión disponible al consultar | Obsolescencia, contexto ausente, tiempo y explicaciones sin soporte. |
| F7 | [Beneficios y criterios](../../01-caso-de-negocio/beneficios-y-criterios-de-exito.md), v0.1 del 28/10/2026 | B-01 a B-04 y CA-01 a CA-06, todavía propuestos. |
| F8 | [Plan de evaluación](../../04-evaluacion/plan-de-evaluacion.md), propuesta del 28/10/2026 | Evidencias por preparar y distinción entre pruebas de mantenimiento y diez cambios. |


Se toma como base el caso de negocio ya redactado. El ejercicio se centra en concretar su diseño, sin reescribirlo ni cambiar su estado de aceptación mediante esta simulación.

**Escenario sintético narrativo:**

- DAG 1 lee raw.ventas y raw.clientes; mediante un paso STG produce curated.ventas. El valor de total procede de precio y cantidad.
- DAG 2 consume total para producir un indicador en mart.indicadores.
- DAG 3 consume identificadores y región de curated.ventas, sin usar total en expresiones ni condiciones en esta variante.
- El cambio planteado es eliminar raw.ventas.precio.
- Variantes: falta el YAML de DAG 2; DAG 2 cambia su cálculo; se retira un DAG; falla un reanálisis; una vista introduce otra referencia.

Son definiciones del ejercicio, no artefactos ejecutables ni relaciones descubiertas por el parser. En especial, la ausencia de dependencia de valor en DAG 3 no garantiza que pueda ejecutarse si falla su productor.

## 3. Ronda de cliente: utilidad, alcance y confianza

**RS:** Revisión simulada o pregunta Simulada por IA.

### RS-01. ¿Qué recibiría si quiero eliminar la columna precio?

**Cliente simulado:** Necesito decidir si puedo eliminar el precio. ¿Qué me entregas y qué decisión me permites tomar?

**Respuesta Óscar:** Te entregaría un reporte de los consumidores candidatos, el camino que los conecta con precio y la evidencia de cada relación. En nuestro ejemplo te mostraría el recorrido hasta total y el indicador de DAG 2. Además incluiría las versiones analizadas y lo que falta conocer. Con eso puedes decidir qué artefactos y pruebas revisar antes de aprobar el cambio. Cabe aclarar que el reporte por sí solo no autoriza eliminar la columna.

**Réplica del cliente:** Entonces, ¿la herramienta me dice si romperé producción?

**Respuesta Óscar:** Más bien lo que la herramienta hace es identificar dependencias y riesgos dentro de lo analizado. Para poder confirmar una rotura de producción sería necesario validar el cambio en su contexto de ejecución. Es decir, distinguiría dependencia detectada, posible impacto y fallo demostrado; en este hito todavía no tenemos resultados de ejecución.

**Observación del revisor:** El reporte necesita separar esas categorías de forma visible, también cuando el texto lo produzca la IA.

**Contraste:** F2, “Recorrido ilustrativo de la decisión”, y F3, RF-04, ya limitan la salida a candidatos y evidencia. La distinción concuerda con el caso; falta concretarla en el contrato del reporte.

**Recomendación:** Aceptar esa separación como requisito candidato. Verificación prevista: V-01 y V-06.

### RS-02. Si tres DAGs están conectados, ¿por qué no marcar todos como afectados?

**Cliente simulado:** Prefiero que marques todos los consumidores para evitar omisiones.

**Respuesta Óscar:** Puedo mostrar todos los consumidores conocidos de la tabla, pero explicaría cuáles dependen de la columna por su valor, cuáles por una condición y cuáles requieren otra revisión. En el ejemplo, DAG 2 usa total; DAG 3 consume la tabla, pero no ese valor. Aun así, DAG 3 podría verse afectado si deja de ejecutarse el productor. Separar estos motivos permite priorizar sin presentar todas las alertas como la misma conclusión.

**Réplica del cliente:** ¿No vuelve eso más difícil de interpretar?

**Respuesta Óscar:** En si parecería más difícil de interpretar, pero al tener este detalle, puedes aber exactamente que DAGs se tienen que modificar y cuales no. No es lo mismo tener que modificar 3 DAGs a sólamente 1 por eso es necesario identificar exactamente los DAGS afectados. Pero para hacerlo más claro propondría un resumen por motivo y un detalle consultable. El costo es diseñar categorías claras; la ventaja es evitar tanto falsas certezas como una lista indiferenciada de alarmas.

**Observación del revisor:** No inferir dependencia operacional únicamente desde una relación de datos; identificar cuándo hay evidencia de orquestación y cuándo es una posibilidad.

**Contraste:** F2 distingue explícitamente consumidores de tabla, dependencia de valor y ejecución del productor; F4 pide separar valores y condiciones.

**Recomendación:** Rechazar la clasificación uniforme de “afectado confirmado”. Conservar candidatos y motivos diferenciados. V-01.

### RS-03. ¿Por qué construir esto si hoy puedo revisar los archivos?

**Cliente simulado:** Ya tenemos un proceso manual. ¿Dónde estaría el beneficio?

**Respuesta Óscar:** La hipótesis es reutilizar conocimiento entre consultas y facilitar que otro ingeniero compruebe una conclusión. Descubrir y mantener ese conocimiento también cuesta. Compararía cinco casos con análisis manual, registrando preparación, actualización, consulta y verificación. Si no hay ahorro, lo reportaría; una mejora de trazabilidad podría ser valiosa, pero necesitaría evidencia propia.

**Réplica del cliente:** ¿Puedes comprometer un porcentaje de ahorro?

**Respuesta Óscar:** Todavía no puedo comprometer un porcentaje de ahorro. Aunque, por la experiencia que comentamos, este análisis suele tomar varios días, necesitamos comparar cuánto tarda un ingeniero en hacerlo manualmente y cuánto tarda con la herramienta, incluyendo la revisión de los resultados. Empezaríamos con cinco casos de prueba para tener una primera referencia del ahorro, sin asumir que será igual en todos los procesos.

**Observación del revisor:** La comparación debe contar el trabajo de preparar y corregir resultados, además del tiempo de consulta.

**Contraste:** F7, B-01 y “Decisión sobre valor y continuidad”, ya exige considerar mantenimiento y tamaño de muestra. F1 conserva cinco comparaciones en el tercer hito.

**Recomendación:** Rechazar ahorros garantizados; mantener el protocolo de comparación como evidencia futura. V-08.

### RS-04. ¿Esto incluye una plataforma, una interfaz y todos nuestros procesos?

**Cliente simulado:** Me gustaría un portal conectado automáticamente a todos los DAGs y con todos los patrones SQL.

**Respuesta Óscar:** Para empezar propondría trabajar con un grupo pequeño de DAGs y datos de ejemplo, incluyendo tablas, columnas, pasos intermedios STG y una vista. Buscaría que podamos ejecutar el análisis manualmente y obtener un reporte claro sobre qué revisar y por qué. Un portal y la conexión automática con todos los procesos podrían venir después, si el beneficio justifica el trabajo adicional.

**Réplica del cliente:** ¿Qué patrón vas a excluir entonces?

**Respuesta Óscar:** Todavía necesitamos revisar ejemplos de las consultas y el analizador SQL que ya existe para decidirlo. Con eso haría una tabla sencilla que indique qué consultas podemos analizar, cuáles necesitan ajustes y cuáles quedarían fuera inicialmente. Mantendría el análisis de tablas y columnas; limitaría los tipos de consultas que cubrimos y señalaría claramente lo que no podamos resolver.

**Observación del revisor:** La cobertura no puede quedar definida simplemente como “Spark SQL soportado”.

**Contraste:** F1 fija tablas y columnas como prioridad 1. F3 mantiene vistas y cobertura por resolver; portal e integración automática no son compromisos aprobados.

**Recomendación:** Aceptar una matriz de cobertura como ajuste candidato; dejar tecnologías y patrones concretos pendientes de evidencia. V-02.

## 4. Ronda técnica: responsabilidades y actualización entre DAGs

### RS-05. ¿Cómo se divide la solución y qué intercambian sus componentes?

**Revisor técnico simulado:** El flujo conceptual se entiende, pero ¿cómo puede empezar a implementarlo alguien?

**Respuesta Óscar:** Primero definiría qué recibe y qué entrega cada parte. Una extraería el SQL y guardaría de qué archivo, DAG y tarea viene. Otra identificaría las relaciones y señalaría qué pudo resolver y qué información le faltó. Después guardaríamos esos resultados con su versión para consultarlos ante un cambio. La IA usaría esa información para explicar qué procesos conviene revisar y dónde está la evidencia. Así podemos desarrollar y probar cada parte con una responsabilidad clara.

**Réplica del revisor:** ¿Eso significa cinco microservicios?

**Respuesta Óscar:** No necesariamente. Esas partes podrían funcionar dentro de una sola aplicación. Separar sus responsabilidades nos ayudaría a probarlas y modificarlas sin tener que cambiar todo. Llevarlas a servicios independientes añadiría trabajo de despliegue y mantenimiento; primero tendría que haber una necesidad que lo justifique.

**Observación:** Definir campos mínimos y errores de cada contrato antes de elegir frameworks.

**Contraste:** F4 describe exactamente esas responsabilidades, pero mantiene los contratos por completar. F5 mantiene un proceso frente a servicios como alternativa abierta.

**Recomendación:** Aceptar concretar contratos; no adoptar microservicios por la cantidad de componentes. V-02 y V-06.

### RS-06. ¿Cómo evitas perder los pasos STG o conectar objetos distintos?

**Revisor técnico simulado:** Si dos DAGs usan el nombre stg_ventas, ¿los unes? ¿Podemos eliminar STG para simplificar el grafo?

**Respuesta Óscar:** No asumiría que son la misma tabla solo porque se llaman igual. Primero revisaría dónde se crean y en qué contexto se usan. También conservaría los pasos STG, porque ayudan a entender cómo se transforma un dato hasta llegar al resultado final. Para facilitar la lectura, mostraría un resumen con la opción de consultar el detalle. Si falta información para distinguir dos objetos, lo dejaría indicado.

**Réplica del revisor:** ¿Y el orden de tareas demuestra que una tabla alimenta a la siguiente?

**Respuesta Óscar:** El orden ayuda a entender el proceso, pero no basta para demostrar que una tarea usa los datos de otra. Revisaría qué tablas lee y escribe cada una. De la misma forma, no daría por hecho que la última tarea genera la tabla final; tendría que comprobarlo en las consultas y la configuración.

**Observación:** Especificar reglas de identidad y evidencia; no inventar convenciones del framework.

**Contraste:** F4, “Límites por resolver”, advierte sobre orden, STG y targets; F6 registra las convenciones desconocidas como riesgo.

**Recomendación:** Rechazar fusionar por nombre o borrar intermediarios como simplificación automática. V-03.

### RS-07. ¿Qué ocurre cuando incorporas un DAG después de haber analizado otros?

**Revisor técnico simulado:** Analizas DAG 1 hoy y DAG 2 mañana. ¿Hay que reconstruir todo en cada consulta?

**Respuesta Óscar:** La idea es aprovechar lo que ya analizamos. Al incorporar DAG 2, guardaríamos sus relaciones y las conectaríamos con las tablas y columnas que ya conocemos, comprobando que se trate de los mismos objetos. Así, la siguiente consulta podría usar esa información sin empezar de cero. El reporte también mostraría qué DAGs se han analizado y qué referencias siguen pendientes.

**Réplica del revisor:** ¿Cómo garantizas que nunca tendrás que reanalizar otros DAGs?

**Respuesta Óscar:** No podría garantizar que nunca haya que volver a analizarlos. Por ejemplo, si cambia la estructura de una tabla, algunas relaciones anteriores podrían dejar de ser válidas. Necesitamos detectar qué resultados dependen de esa información y marcar cuáles deben revisarse o analizarse de nuevo.

**Observación:** Incrementalidad también exige gestionar invalidaciones, no solo añadir relaciones.

**Contraste:** F3, RF-03, exige ampliar y mantener conocimiento; F4 pide evitar mezclar versiones. El tratamiento de invalidaciones es un refinamiento propuesto, aún no diseñado.

**Recomendación:** Aceptar procedencia por DAG y registrar como pendiente la política de invalidación. V-04.

### RS-08. ¿Cómo actualizas un DAG que deja de usar una columna?

**Revisor técnico simulado:** La versión nueva de DAG 2 ya no consume total. Si solo agregas relaciones, la dependencia anterior seguirá apareciendo.

**Respuesta Óscar:** Primero analizaría la nueva versión y comprobaría que el resultado está listo para usarse. Después reemplazaría las relaciones anteriores de ese DAG por las nuevas. Si ya no utiliza total, esa dependencia dejaría de aparecer como actual para ese proceso. Cuidaría que eso no borre relaciones aportadas por otros DAGs y que analizar dos veces la misma versión no duplique la información.

**Réplica del revisor:** ¿Reemplazo directo o historial de versiones?

**Respuesta Óscar:** Reemplazar directamente lo anterior puede ser más sencillo, pero guardar un historial nos permitiría entender qué cambió y recuperar información si hace falta. Mi propuesta inicial sería identificar claramente la versión que estamos usando y conservar lo necesario para explicar los cambios. Antes de decidir cuánto historial guardar, compararía su utilidad con el esfuerzo de implementarlo y mantenerlo.

**Observación:** Separar propiedad de los aportes y relaciones agregadas; definir cuándo un análisis es publicable.

**Contraste:** F5 compara reemplazo frente a versiones; F7, CA-04, exige idempotencia y conservación de aportes ajenos. No hay motor seleccionado que garantice esta operación.

**Recomendación:** Aceptar el comportamiento deseado; dejar estrategia de persistencia e historial pendiente. V-04.

### RS-09. ¿Qué pasa si falla la actualización a mitad del análisis?

**Revisor técnico simulado:** ¿Borras primero lo anterior y después analizas? ¿Qué ve un usuario si falla?

**Respuesta Óscar:** No borraría lo anterior antes de tener listo el nuevo análisis. Si la actualización falla, propondría conservar el último resultado disponible y avisar que corresponde a una versión anterior, mostrando qué falló y qué información puede estar desactualizada. Si es la primera vez que analizamos ese DAG, indicaríamos que todavía no hay un resultado disponible. Así evitamos presentar una mezcla de versiones como si todo estuviera actualizado.

**Réplica del revisor:** ¿Y un análisis parcial que sí obtiene algunas relaciones?

**Respuesta Óscar:** Guardaría ese resultado parcial para revisar qué se pudo analizar y dónde estuvo el problema, pero inicialmente no lo usaría para reemplazar el resultado anterior. Si faltan relaciones porque el análisis quedó a medias, no deberíamos interpretarlo como que esas dependencias ya no existen. Podríamos permitir consultas parciales después, siempre que definamos cómo mostrar sus límites.

**Observación:** Retener el resultado anterior protege información, pero puede inducir a error si no se comunica su antigüedad. La respuesta necesita ambos controles.

**Contraste:** F4 y F6 plantean fallo de actualización y obsolescencia, sin resolverlos. Esta política es propuesta de la simulación, no una propiedad implementada.

**Recomendación:** Aceptar preparación previa y vigencia explícita como candidatos; rechazar borrado previo incondicional. V-05.

### RS-10. ¿Archivo ausente significa DAG retirado?

**Revisor técnico simulado:** Si un YAML desaparece de la entrada, ¿eliminas sus dependencias?

**Respuesta Óscar:** No lo eliminaría solo porque falta un archivo. Puede ser una entrega incompleta y no que el DAG haya dejado de existir. En ese caso señalaría que falta información. Para retirar sus relaciones pediría una indicación explícita de que el DAG fue dado de baja. Además, conservaría las referencias de otros procesos que todavía consumen su salida, porque son justamente los que podrían necesitar revisión.

**Réplica del revisor:** ¿Conservarías una tabla sin productor?

**Pregunta Óscar:** ¿Podrías desarrollar un poco más tu pregunta? ¿A qué te refieres con una tabla sin productor?

**Aclaración del revisor:** Me refiero a que sabemos que otros DAGs leen una tabla, pero ya no tenemos identificado un DAG vigente que la genere o la actualice. Por ejemplo, retiramos del catálogo el DAG que producía curated.ventas, pero DAG 2 y DAG 3 siguen apareciendo como consumidores. En ese caso, ¿conservarías en el catálogo la tabla y esas relaciones, indicando que falta un productor conocido, o eliminarías también esa información? La pregunta se refiere al registro de linaje, no a borrar la tabla física ni sus datos.

**Respuesta Óscar:** Sí, si otros procesos todavía la utilizan. Mostraría que conocemos sus consumidores, pero que ya no tenemos identificado un proceso vigente que la genere. Si borramos toda esa información, podríamos ocultar dependencias importantes para el análisis.

**Observación:** Definir la entrada que expresa una baja y las reglas para relaciones compartidas.

**Contraste:** F3, RF-03, contempla baja explícita; F7, CA-04, exige conservar relaciones ajenas. La representación concreta queda por diseñar.

**Recomendación:** Rechazar ausencia de archivo como baja automática; aceptar un evento explícito de retirada como candidato. V-04 y V-05.

### RS-11. ¿Qué parser, motor y patrones concretos vas a utilizar?

**Revisor técnico simulado:** ¿Puedes confirmar hoy que resolverás comodines, escrituras, vistas y parámetros?

**Respuesta Óscar:** Todavía no puedo confirmarlo. Primero probaría el analizador SQL existente con consultas y datos de ejemplo, dejando claro qué esperamos encontrar y qué obtiene realmente. Eso nos permitiría decidir si podemos reutilizarlo, si necesita adaptaciones o si hay razones para sustituirlo. También elegiría cómo guardar los resultados después de definir qué consultas y actualizaciones necesitamos hacer.

**Réplica del revisor:** El PDI dice desarrollar el parser. ¿Eso obliga a reemplazarlo?

**Respuesta Óscar:** No lo interpretaría así automáticamente. El plan también menciona revisar el analizador existente. Primero comprobaría qué resuelve y qué le falta; después justificaría si conviene aprovecharlo, ampliarlo o desarrollar otro. La decisión debería apoyarse en esa revisión y en el esfuerzo necesario.

**Observación:** Hace falta contraste externo de semántica para los patrones seleccionados, además de observar el parser.

**Contraste:** F1 registra la discrepancia; F3 y F5 mantienen la elección pendiente. No hay evidencia de soporte ni versión técnica fijada. Cuando se concrete una pregunta de comportamiento de Spark SQL o de una librería, consultar documentación vigente mediante Context7 y registrar fuente, versión y sección.

**Recomendación:** Dejar la selección pendiente; rechazar promesas de cobertura universal. V-02 y contraste técnico CT-01.

## 5. Ronda de evidencia, incertidumbre y cierre

### RS-12. ¿Qué significa exactamente que una relación esté verificada?

**Cliente simulado:** Si el reporte pone “verificado”, ¿puedo confiar en que se probó el cambio?

**Respuesta Óscar:** En este reporte, verificado significaría que podemos mostrar de dónde sale la relación, el archivo, la parte de la consulta y la versión analizada. No significaría que ya ejecutamos el cambio y comprobamos su efecto. Si solo tenemos indicios, lo presentaríamos como una posible relación; si no hay información suficiente, indicaríamos que no podemos determinarla.

**Réplica del cliente:** ¿Y si no hay ninguna relación registrada?

**Respuesta Óscar:** Diría que no encontramos relaciones en los archivos y procesos que pudimos analizar. Si faltan archivos o hay consultas que no podemos interpretar, también lo mostraría. Eso permite al ingeniero saber dónde debe seguir investigando, sin dar por hecho que no habrá afectaciones en otras partes de la plataforma.

**Observación:** Una cita existente puede ser insuficiente: también hay que comprobar que respalda la conclusión y cada tramo del recorrido.

**Contraste:** F3 define los estados; F7, CA-01 y CA-03, exige evidencia localizable y límites explícitos.

**Recomendación:** Aceptar evidencia por tramo y cobertura visible; rechazar “sin relaciones” como equivalente a “sin impacto”. V-01 y V-06.

### RS-13. Si la IA reconoce un patrón, ¿puede completar relaciones que faltan?

**Cliente simulado:** Si falta el YAML de DAG 2, que la IA deduzca el cálculo por su nombre y termine el reporte.

**Respuesta Óscar:** El nombre del DAG puede dar una pista, pero no demuestra cómo calcula sus datos. Si falta el YAML, indicaría que todavía no podemos determinar qué columnas utiliza. La IA podría sugerir qué revisar o qué información pedir; para esta primera versión propondría que nos ayude a explicar lo que sabemos y lo que falta, sin completar las relaciones como si estuvieran comprobadas.

**Réplica del revisor:** ¿Cómo evitarías que una explicación agregue hechos?

**Respuesta Óscar:** Le daría a la IA las relaciones encontradas y las referencias a los archivos que las respaldan. Cada afirmación sobre una dependencia tendría que apoyarse en esa información. Revisaríamos tanto que la referencia exista como que realmente demuestre lo que dice la explicación. Si no lo podemos validar, propondría mostrar los resultados del análisis y avisar que la explicación no está validada.

**Observación:** El formato de entrada y la evaluación deben cubrir invenciones y contradicciones, no solo errores de formato.

**Contraste:** F4 mantiene abierta la separación análisis/IA; F7, B-04 y CA-05, plantea fidelidad. La alternativa de respuesta sin explicación es nueva propuesta funcional.

**Recomendación:** Rechazar completar hechos por plausibilidad; aceptar explicación trazable y salida alternativa como candidatos. V-06.

### RS-14. ¿Qué pasa si la IA falla o el material contiene instrucciones extrañas?

**Revisor técnico simulado:** El servicio puede no estar disponible y un comentario del artefacto podría pedir ignorar la evidencia. ¿Qué esperas del sistema?

**Respuesta Óscar:** Si falla la IA, propondría que el ingeniero todavía pueda consultar las relaciones encontradas y su evidencia, aunque no tenga la explicación en texto. Además, un comentario dentro de un archivo debería tratarse como contenido a analizar, sin permitir que cambie las instrucciones de la aplicación o modifique los resultados guardados. Usaríamos datos de ejemplo y enviaríamos solo la información necesaria. Esos controles habría que comprobarlos al elegir la integración.

**Réplica del revisor:** ¿Podemos probarlo conectando un proveedor de pago ahora?

**Respuesta Óscar:** Para preparar esta revisión no hace falta contratar ni conectar un servicio de pago. Antes de usarlo definiría qué proveedor necesitamos, cuánto podría costar y si el gasto está autorizado. Los USD 20 del PDI son una propuesta de presupuesto. Mientras tanto, podemos definir cómo probar los fallos y los textos que intenten alterar el comportamiento, dejando claro que esas pruebas todavía no se han ejecutado.

**Observación:** Una instrucción al modelo por sí sola no demuestra aislamiento ni seguridad; revisar también lo que puede ejecutar la aplicación.

**Contraste:** F6 incluye acceso, cuota y costo; F1 no autoriza gasto. El tratamiento de contenido como datos es una propuesta de control a desarrollar y comprobar, no garantía de un proveedor.

**Recomendación:** Aceptar controles y pruebas candidatas; dejar detalles de integración pendientes. V-07 y CT-02.

### RS-15. ¿Cómo comprobarás que funciona sin elegir solo ejemplos favorables?

**Revisor técnico simulado:** Si construyes y evalúas el prototipo tú mismo, ¿cómo evitas definir el esperado después de ver la salida?

**Respuesta Óscar:** Antes de ejecutar la herramienta, prepararía los archivos de ejemplo y anotaría qué relaciones deberían encontrarse. Después compararía lo esperado con lo obtenido, separando tablas, columnas y condiciones. Registraría tanto las relaciones que faltan como las que aparecen por error. También incluiría archivos incompletos, actualizaciones y fallos. Esas comprobaciones complementan los diez cambios del PDI; no los sustituyen.

**Réplica del revisor:** ¿Y si un patrón falla?

**Respuesta Óscar:** Dejaría registrado qué falló e intentaría corregirlo. Si necesitamos limitar lo que cubre el prototipo, explicaría el motivo y conservaría los resultados anteriores. No quitaría un caso solo para que los números se vean mejor. En la comparación manual también indicaría que ya conozco los ejemplos, porque esa familiaridad puede influir en el tiempo que tarde en analizarlos.

**Observación:** Acordar criterios antes de medir; los CA-01 a CA-06 son propuestas actuales, no aceptación ya registrada.

**Contraste:** F7 y F8 contienen esta separación y el protocolo de esperado/obtenido. No existen resultados ejecutados en esta revisión.

**Recomendación:** Aceptar fijar el protocolo antes de ejecutar; revisar los criterios candidatos antes de adoptarlos. V-08.

### RS-16. Con esta revisión, ¿ya queda terminado el diseño?

**Cliente simulado:** El caso de negocio está redactado y hemos contestado preguntas. ¿Podemos pasar directamente a implementar?

**Respuesta Óscar:** Hemos aclarado varias cosas, pero todavía faltan decisiones para empezar a construir con una base suficientemente definida. Necesitamos precisar qué recibe y entrega cada parte, cómo identificamos tablas y columnas, qué consultas cubrimos y qué hacemos cuando hay cambios o fallos. También falta comparar opciones y estimar el esfuerzo. Con esas decisiones documentadas podríamos avanzar; no hace falta tener el prototipo funcionando para cerrar el diseño.

**Réplica del cliente:** ¿Cuál sería el siguiente paso concreto?

**Respuesta Óscar:** Empezaría por decidir qué debe mostrar el reporte y cómo vamos a actualizar las relaciones de un DAG. Después escribiría qué información entra y sale del análisis, y recorrería un ejemplo en el que falla una actualización para comprobar que el comportamiento propuesto sea claro. También identificaría qué falta revisar del analizador SQL para concretar el alcance y estimar el trabajo. Si eso afecta las fechas, registraría el motivo y la nueva propuesta sin borrar las originales.

**Observación:** La simulación entrega material de revisión; no sustituye arquitectura, plan ni aceptación de decisiones.

**Contraste:** F1 separa los entregables del hito; F4 lista contratos y mecanismos aún por completar.

**Recomendación:** Mantener el diseño En curso y esta revisión En revisión. Las respuestas son editables; las decisiones definitivas se registran a continuación.


## 6. Síntesis y reflexión propuesta

**Conclusión del revisor simulado:** El caso de negocio permite entender el problema y el flujo conceptual identifica responsabilidades razonables. Para que el diseño sea implementable faltan contratos, identidad, política de vigencia/publicación, cobertura concreta y comparación técnica. Las preguntas más importantes son qué se sabe, a qué versión corresponde y qué se comunica cuando no hay evidencia suficiente. Esta conclusión es una valoración de IA sobre documentos, no una certificación de arquitectura.

**Reflexión propia después de la simulación:**

> Al revisar el diseño, identifiqué que el valor del catálogo depende tanto de mantenerlo vigente como de descubrir relaciones. Mi propuesta es que el reporte permita distinguir una dependencia de columna de un consumidor de tabla y que muestre cuándo la información está incompleta o desactualizada. También considero necesario conservar la evidencia a través de los pasos STG y limitar la explicación IA a lo que pueda sostenerse. Antes de adoptar estas propuestas, revisaré sus consecuencias para el esfuerzo y el alcance. La simulación me sirve como base para tomar decisiones; todavía no prueba el comportamiento del prototipo ni un ahorro para el negocio.

