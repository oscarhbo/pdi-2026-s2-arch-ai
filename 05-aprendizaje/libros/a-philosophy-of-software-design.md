# A Philosophy of Software Design — Notas de estudio

**Autor:** John Ousterhout.  
**Documentación:** 28/10/2026.  
**Alcance:** notas consolidadas de un estudio realizado en 22 fases del Libro completo.  
**Enfoque:** diseño de software, contratos, modularidad y criterios profesionales, con ejemplos de seguros, datos e IA.

Se incluye unas reconstrucción temática de lo que estudie en esas 22 fases. Incluyo algunos ejemplos, analogías, criterios de revisión y aplicaciones de IA  que sirven como elaboraciones para un apoyo del estudio; no son citas del autor. Los fragmentos de código son solo con fines ilustrativos.

## Resumen General

Diseñar software consiste en conservar la capacidad de comprenderlo y modificarlo a medida que evoluciona. Que una implementación funcione es necesario, pero no demuestra que su estructura sea fácil de mantener. El costo aparece cuando un cambio exige investigar demasiados detalles, modificar muchos lugares o descubrir dependencias inesperadas.

La filosofía del libro la podemos resumir alrededor de cinco ideas principales:

1. **Reconocer la complejidad por sus efectos.** Amplificación del cambio, carga cognitiva e incógnitas desconocidas permiten detectar problemas que no se ven contando líneas o clases.
2. **Construir módulos profundos.** Una interfaz sencilla debe ofrecer funcionalidad coherente y ocultar decisiones que el consumidor no necesita conocer.
3. **Diseñar contratos y alternativas antes de implementar.** Explicar responsabilidades, efectos, errores e invariantes revela dificultades antes de convertirlas en código.
4. **Conservar conocimiento mediante nombres y comentarios.** El código expresa la ejecución; el contrato y los comentarios explican intención, límites y decisiones que no resultan evidentes.
5. **Invertir en diseño de manera continua.** El mantenimiento, la consistencia y la optimización medida determinan si el sistema mejora o acumula excepciones.

El criterio transversal es la **complejidad total**: una solución no mejora por tener menos líneas, más clases o una tecnología más sofisticada. Mejora cuando reduce lo que necesitamos conocer y coordinar para realizar cambios, sin desplazar costos injustificados a otros módulos o a la operación.

## Mapa de contenidos

Para repasar en pocos minutos: [mapa de principios](#repaso) y [glosario](#glosario). Para aplicar las ideas: [criterios de revisión](#revision), [contrato de módulo](#contrato) y [propuestas para el analizador de linaje](#aplicacion).

- [1. Introduction](#tema-1)
- [2. The Nature of Complexity](#tema-2)
- [3. Working Code Isn't Enough](#tema-3)
- [4. Modules Should Be Deep](#tema-4)
- [5. Information Hiding (and Leakage)](#tema-5)
- [6. General-Purpose Modules Are Deeper](#tema-6)
- [7. Different Layer, Different Abstraction](#tema-7)
- [8. Pull Complexity Downwards](#tema-8)
- [9. Better Together or Better Apart?](#tema-9)
- [10. Define Errors Out of Existence](#tema-10)
- [11. Design It Twice](#tema-11)
- [12. Why Write Comments? The Four Excuses](#tema-12)
- [13. Comments Should Describe Things That Aren't Obvious from the Code](#tema-13)
- [14. Choosing Names](#tema-14)
- [15. Write the Comments First](#tema-15)
- [16. Modifying Existing Code](#tema-16)
- [17. Consistency](#tema-17)
- [18. Code Should Be Obvious](#tema-18)
- [19. Software Trends](#tema-19)
- [20. Designing for Performance](#tema-20)
- [21. Decide What Matters](#tema-21)
- [22. Conclusion](#tema-22)

## Parte I — Entender la complejidad

<a id="tema-1"></a>
### 1. Introduction

#### Idea central

El diseño de software no consiste únicamente en hacer que el programa funcione: consiste en **hacer que el sistema siga siendo fácil de comprender y modificar**. La funcionalidad resuelve la necesidad presente; el diseño determina el costo de los cambios futuros.

Una solución puede ser correcta hoy y aun así dejar una estructura costosa para mañana. El diseño debe tratarse como una actividad continua, no como un evento previo a escribir código.

#### Dos maneras de evaluar una solución

| Evaluación inmediata | Evaluación de diseño |
|---|---|
| ¿Cumple el requerimiento? | ¿Qué conocimientos exige para cambiarla? |
| ¿Pasan las pruebas? | ¿El cambio queda localizado? |
| ¿Se entrega a tiempo? | ¿Las decisiones internas están ocultas? |
| ¿Es eficiente hoy? | ¿Podrá evolucionar sin reescribir consumidores? |

#### Ejemplo

Un cotizador de seguros puede calcular correctamente una prima mediante un procedimiento de 2,000 líneas. Si agregar una alianza obliga a modificarlo repetidamente, la corrección funcional no demuestra calidad de diseño.

#### Aplicación

Antes de cerrar una tarea, revisar tanto el comportamiento como la estructura resultante. Preguntar: **«¿Qué dificultad le dejo a la próxima persona que deba modificar esto?»**

---

<a id="tema-2"></a>
### 2. The Nature of Complexity

#### Idea central

La complejidad es aquello que hace difícil **entender y modificar** un sistema. No equivale simplemente a cantidad de código, número de componentes o dificultad inherente del negocio.

#### Tres síntomas

1. **Change amplification:** un cambio pequeño requiere modificaciones en muchos lugares.
2. **Cognitive load:** una tarea exige mantener demasiados detalles simultáneamente en la cabeza.
3. **Unknown unknowns:** existen dependencias o consecuencias que no sabemos que debemos investigar.

#### Causas habituales

- **Dependencias:** un componente no puede entenderse o modificarse sin conocer otros.
- **Obscuridad:** la información necesaria está oculta, dispersa o es ambigua.

#### Ejemplo: seguros

Una regla de edad del conductor está repetida en cotización, emisión, renovación y batch. Cambiar el umbral de 25 a 27 años exige cuatro modificaciones: amplificación del cambio. Si además cada implementación tiene excepciones diferentes, aumenta la carga cognitiva. Si existe un quinto cálculo en un proceso desconocido, aparecen las incógnitas desconocidas.

#### Diagnóstico rápido

- ¿Cuántos lugares cambian por una sola regla de negocio?
- ¿Cuántos archivos hay que leer antes de modificar un método?
- ¿Cuántas veces aparece la frase «nadie sabe qué más depende de esto»?

**Principio:** el diseño debe reducir dependencias innecesarias y hacer visible el conocimiento que sí necesita el consumidor.

---

<a id="tema-3"></a>
### 3. Working Code Isn't Enough

#### Idea central

La **programación táctica** se concentra en terminar el cambio inmediato. La **programación estratégica** cumple el requerimiento mientras invierte en una estructura que reduzca costos futuros.

```text
Táctica: requerimiento → parche → funciona → siguiente ticket
Estratégica: requerimiento → comprender diseño → mejorar abstracción → implementar → verificar
```

No significa reescribir todo con cada ticket. Significa evitar que pequeñas decisiones locales acumulen complejidad de manera permanente.

#### Ejemplo

Llega una alianza nueva. La salida rápida es agregar otro `ELSIF`. Una mirada estratégica pregunta si la diferencia es una regla configurable o si existe una razón real para modificar código por cada alianza.

#### Criterio práctico

Invertir tiempo adicional cuando un cambio:

- Duplica una regla.
- Obliga a revelar detalles internos.
- Agrega parámetros que sólo atraviesan capas.
- Introduce un caso especial que debería ser un concepto del dominio.

**Regla:** la velocidad sostenible importa más que la velocidad aislada del ticket.

---

## Parte II — Diseñar buenas abstracciones

<a id="tema-4"></a>
### 4. Modules Should Be Deep

#### Idea central

Un **módulo profundo** ofrece mucha funcionalidad útil a través de una interfaz pequeña y sencilla. Un **módulo superficial** obliga al consumidor a conocer muchos detalles para obtener poco valor.

```text
Módulo superficial               Módulo profundo
-----------------               ----------------
get_driver_age()                 calculate_premium(quotation)
get_vehicle_age()                     |
get_usage_factor()                    +-- valida
calculate_age_factor()                +-- obtiene factores
calculate_risk()                      +-- aplica reglas
calculate_premium()                   +-- calcula prima
```

#### Ejemplo PL/SQL

**Interfaz que filtra detalles:**

```sql
FUNCTION get_driver_age(p_policy_id NUMBER) RETURN NUMBER;
FUNCTION get_vehicle_age(p_policy_id NUMBER) RETURN NUMBER;
FUNCTION calculate_age_factor(p_age NUMBER) RETURN NUMBER;
FUNCTION calculate_usage_factor(p_policy_id NUMBER) RETURN NUMBER;
FUNCTION calculate_premium(p_policy_id NUMBER) RETURN NUMBER;
```

**Interfaz profunda:**

```sql
-- Package specification
FUNCTION calculate_premium(p_policy_id NUMBER) RETURN NUMBER;

-- Los helpers y el algoritmo quedan dentro del package body.
```

#### Matiz

Una interfaz pequeña no garantiza profundidad si esconde una operación trivial; tampoco significa convertir todas las responsabilidades en una clase gigantesca. El módulo debe representar una abstracción coherente.

**Pregunta:** «¿Cuánto necesita saber el consumidor para obtener el resultado?»

---

<a id="tema-5"></a>
### 5. Information Hiding (and Leakage)

#### Idea central

Ocultar información significa **encapsular decisiones de diseño**, especialmente las que pueden cambiar, no sólo marcar atributos como `private`.

Se pueden ocultar:

- Algoritmos y reglas internas.
- Tablas, SQL y estructura de almacenamiento.
- Proveedores externos (S3, GCS, SMTP).
- Estrategias de caching, reintentos o serialización.
- Formatos y mecanismos técnicos que no forman parte del contrato.

#### Ejemplo

**Fuga de información:**

```python
storage_client.upload(bucket, object_key, document)
```

Si toda la aplicación conoce el contenedor, la clave del objeto y el proveedor, cambiar de proveedor afecta muchos módulos.

**Abstracción:**

```python
document_id = document_storage.store(document)
```

La implementación decide cómo y dónde almacenar. El consumidor conoce la operación de negocio.

#### Advertencia

Ocultar una decisión no equivale a ocultar información que el consumidor necesita. Un contrato debe ser claro sobre efectos, errores, garantías y datos relevantes.

**Pregunta:** «Si cambia esta decisión interna, ¿qué consumidores tendré que modificar?»

---

<a id="tema-6"></a>
### 6. General-Purpose Modules Are Deeper

#### Idea central

Una abstracción algo más general puede ser más profunda cuando captura un **concepto real y recurrente** con una interfaz simple. Pero diseñar para futuros hipotéticos puede producir un framework innecesariamente complejo.

#### Ejemplo

Una API con métodos separados para cada campo:

```python
update_customer_name(id, name)
update_customer_email(id, email)
update_customer_phone(id, phone)
```

Puede evolucionar hacia una operación de actualización coherente:

```python
update_customer(customer_id, changes)
```

Esto es útil si las modificaciones comparten semántica, validación y transacción. No es automáticamente mejor si `changes` se convierte en una bolsa arbitraria de opciones.

#### Equilibrio

- **Demasiado específico:** muchos métodos parecidos y duplicación.
- **Demasiado genérico:** opciones, flags, callbacks y excepciones difíciles de entender.
- **Adecuado:** abstracción suficiente para los casos reales, sin especular sobre todos los futuros.

**Pregunta:** «¿La generalización reduce la complejidad total o sólo crea una API más flexible?»

---

<a id="tema-7"></a>
### 7. Different Layer, Different Abstraction

#### Idea central

Las capas pueden y a menudo deben utilizar **modelos diferentes**, porque responden a responsabilidades y razones de cambio distintas.

```text
Persistencia: CustomerRecord
   customer_id, source_system, load_date, status_code
         ↓
Dominio: Customer
   identity, lifecycle, business rules
         ↓
API: CustomerResponse
   id, display_name, status
         ↓
UI: CustomerView
   formatted_name, status_label
```

#### Ejemplo

Un campo `load_date` es importante para un pipeline de datos, pero no necesariamente forma parte del concepto de cliente en el dominio ni de la respuesta pública de la API.

Forzar un único `Customer` para todas las capas provoca fugas de información. Crear un DTO distinto para cada línea de código también aumenta complejidad.

#### Criterio

Crear una frontera de modelo cuando aísle una responsabilidad, un contrato o una razón real de cambio. No añadir transformaciones vacías por seguir un patrón.

Una capa que solo reenvía llamadas con la misma interfaz añade navegación sin introducir una abstracción diferente. Una delegación breve puede ser válida si delimita una política o frontera real; la longitud del método no decide por sí sola su valor.

**Pregunta:** «¿Qué decisión distinta está protegiendo esta capa?»

---

<a id="tema-8"></a>
### 8. Pull Complexity Downwards

#### Idea central

Una abstracción debería asumir la complejidad que puede resolver internamente, en lugar de empujarla innecesariamente hacia sus consumidores. Durante el estudio relacionamos esta idea con el problema de **pass-through variables**: parámetros que atraviesan capas que no los utilizan.

#### Ejemplo

```text
API(user_id)
  → Service(user_id)
    → Manager(user_id)
      → Processor(user_id)
        → Audit(user_id)
```

Sólo `Audit` utiliza `user_id`. Las capas intermedias quedan acopladas a un detalle que no necesitan.

La idea central es resolver una dificultad una vez dentro del módulo cuando así simplificamos a todos sus consumidores. No implica colocar cualquier responsabilidad en la capa más baja: la responsabilidad debe seguir siendo coherente y no ocultar decisiones que corresponden al invocador.

#### Posibles mejoras

1. Reubicar la responsabilidad: el servicio de aplicación invoca negocio y auditoría cuando corresponda.
2. Usar un contexto de ejecución **acotado** si la identidad es una preocupación transversal genuina.
3. Usar eventos cuando la separación temporal y transaccional esté justificada.

#### Matiz importante

No todo parámetro que viaja por varias capas es incorrecto. Si forma parte de la semántica de la operación, puede ser apropiado. Tampoco conviene reemplazar todos los parámetros por un enorme `RequestContext` global.

**Preguntas:** «¿Quién necesita realmente este dato?» y «¿Cuántas capas lo transportan sin usarlo?»

---

<a id="tema-9"></a>
### 9. Better Together or Better Apart?

#### Idea central

**Más clases y más archivos no implican mejor diseño.** Separar físicamente código no siempre reduce complejidad conceptual.

#### Ejemplo

Un cálculo de prima sencillo puede terminar distribuido entre:

```text
RiskCalculator
MultiplierProvider
PremiumCalculator
RatingCoordinator
RatingFactory
```

Si siempre se utilizan y modifican juntos, el desarrollador necesita navegar múltiples archivos sin obtener aislamiento real.

Un `RatingEngine` profundo puede ofrecer una interfaz simple y contener helpers privados. Sin embargo, si una responsabilidad cambia de manera independiente, una separación puede estar justificada.

#### Cuándo unir

- Las partes comparten una abstracción única.
- Siempre se usan y cambian juntas.
- La separación añade navegación, parámetros y coordinación.

#### Cuándo separar

- Representan conceptos distintos.
- Tienen razones independientes de cambio.
- Ocultan decisiones distintas.
- Su acoplamiento puede reducirse mediante una interfaz clara.

**Regla:** «Divide para crear abstracciones, no simplemente para crear archivos».

---

## Parte III — Interfaces, errores y decisiones

<a id="tema-10"></a>
### 10. Define Errors Out of Existence

#### Idea central

Una forma poderosa de simplificar errores es **rediseñar la operación para que ciertos casos ya no sean errores**.

#### Ejemplo: eliminar cobertura

Si la intención es «asegurar que esta cobertura no exista», encontrarla ausente significa que el objetivo ya se cumple:

```python
def ensure_coverage_absent(policy_id, coverage_id):
    repository.delete_if_exists(policy_id, coverage_id)
```

No es necesario lanzar `CoverageNotFound` si la ausencia es un resultado válido del contrato.

#### Relación con idempotencia

Una operación idempotente mantiene el mismo resultado de negocio al repetirse, aunque puedan existir detalles operativos diferentes.

**Caso de pagos: distinguir identidades:**

| Concepto | Identifica |
|---|---|
| `idempotency_key` | Una intención técnica repetida por reintentos |
| `receipt_id` / `installment_id` | La obligación de negocio que se desea pagar |
| `payment_attempt_id` | Un intento de ejecutar el pago |
| `payment_id` | Una transacción de pago registrada |

Si el cliente genera una clave nueva en cada reintento, la clave no evita duplicados. Si dos solicitudes diferentes intentan pagar la misma mensualidad, la invariante de negocio debe decidir si ambas son legítimas.

**Ejemplo:** una mensualidad de póliza sólo puede quedar pagada una vez. Puede protegerse mediante una transición de estado y una restricción de unicidad adecuada al modelo. Un patrón «consultar si existe y luego insertar» no es seguro ante concurrencia sin atomicidad o restricciones de base de datos.

#### Límites

No debemos esconder errores reales. Si no puede emitirse una póliza porque falta información obligatoria, el contrato debe comunicarlo.

**Pregunta:** «¿Esta situación impide cumplir el objetivo o ya representa un estado aceptable?»

---

<a id="tema-11"></a>
### 11. Design It Twice

#### Idea central

Para decisiones importantes, no quedarse con la primera solución razonable. Diseñar **al menos dos alternativas sustancialmente distintas** ayuda a revelar ventajas, costos y posibles combinaciones.

#### Ejemplo: Rating Engine

| Alternativa | Ventaja | Riesgo |
|---|---|---|
| A. Servicios especializados por factor | Aísla responsabilidades reales | Exceso de coordinación y archivos |
| B. `RatingEngine` profundo | Interfaz simple y cohesión | Puede crecer si no controla límites |
| C. Motor de reglas configurable | Variaciones sin despliegue | DSL, validación y operación más complejas |

Comparar:

- Complejidad de la interfaz.
- Número y naturaleza de dependencias.
- Conocimiento requerido por consumidores.
- Facilidad para casos comunes y excepcionales.
- Costos de operación y cambio.

No elegir por la cantidad de patrones utilizados. Una cuarta alternativa híbrida puede surgir al comparar las primeras.

#### Uso con IA

Pedir tres diseños genuinamente diferentes, con sus trade-offs y supuestos. **El arquitecto elige**; la IA ayuda a explorar e implementar.

---

## Parte IV — Documentación y claridad

<a id="tema-12"></a>
### 12. Why Write Comments? The Four Excuses

#### Idea central

Los comentarios valiosos capturan **información que el código no expresa fácilmente**: intención, decisiones, restricciones, contratos e invariantes.

#### Cuatro excusas comunes

1. **«El buen código se documenta solo».** Los nombres ayudan, pero no explican por qué se eligió un límite, una estrategia o una excepción.
2. **«No hay tiempo».** Ahorrar minutos ahora puede costar horas de investigación después.
3. **«Los comentarios se desactualizan».** Hay que mantenerlos; los comentarios de intención suelen ser menos frágiles que la narración línea por línea.
4. **«La mayoría de comentarios no sirve».** Es cierto cuando repiten lo obvio; no justifica eliminar comentarios de diseño.

#### Mal comentario

```python
# Incrementa el contador
counter += 1
```

#### Buen comentario

```python
# No hacer COMMIT aquí: el caso de uso coordina
# la emisión y el registro de cobranza en una sola transacción.
```

**Pregunta:** «¿Qué necesita saber el siguiente desarrollador que no podrá deducir razonablemente leyendo el código?»

---

<a id="tema-13"></a>
### 13. Comments Should Describe Things That Aren't Obvious from the Code

#### Idea central

Los comentarios deben documentar la **abstracción**, no traducir la implementación a lenguaje natural.

#### Qué documentar

- **Contrato de interfaz:** entradas, salidas, precondiciones, efectos, errores.
- **Unidades y semántica:** porcentaje vs. decimal; centavos vs. moneda; segundos vs. milisegundos.
- **Invariantes:** por ejemplo, una cotización `ISSUED` tiene póliza y fecha de emisión.
- **Decisiones sorprendentes:** por qué existe una espera, un workaround o una ventana de estabilización.
- **Estrategia algorítmica:** primero matching determinista, después probabilístico; no comentar cada `IF`.
- **Prohibiciones relevantes:** no confirmar transacciones, no reutilizar una clave para intenciones distintas.

#### Ejemplo

```python
def match_customer(candidate):
    """Busca coincidencias por identidad.

    Primero usa identificadores deterministas. Sólo si no
    hay coincidencia concluyente aplica similitud probabilística.
    No fusiona registros automáticamente.
    """
```

#### Aplicación a datos

Una regla de «esperar 48 horas antes de consolidar» debe explicar que la fuente puede corregir transacciones durante ese período; no basta comentar «filtrar últimos dos días».

**Regla:** el código conserva la implementación; los buenos comentarios conservan el razonamiento.

---

<a id="tema-14"></a>
### 14. Choosing Names

#### Idea central

Un nombre preciso reduce la cantidad de información que el lector debe recordar. Si resulta difícil nombrar un módulo, quizá su **abstracción no está bien definida**.

#### Ejemplos

| Vago | Más preciso cuando corresponda |
|---|---|
| `process(data)` | `calculate_premium(quotation)` |
| `CustomerManager` | `CustomerMatcher` o `CustomerRepository` |
| `v_ind` | `v_is_commercial` |
| `v_res` | `v_premium` |
| `flag` | `should_retry` |
| `timeout` | `timeout_ms` |

Los nombres deben ser tan específicos como sea necesario, **no simplemente largos**. En un método corto, `email` puede ser suficiente; en un ámbito amplio, se necesita más contexto.

#### Consistencia de dominio

`customer`, `policyholder`, `insured` y `beneficiary` pueden referirse a la misma persona física, pero **no son el mismo rol de negocio**. Usar un solo término para todos esconde diferencias importantes.

#### Señal de alerta

Nombres como `CommonUtils`, `DataHelper`, `GeneralManager` o `process` pueden indicar responsabilidades mezcladas. No son siempre incorrectos, pero merecen revisión.

**Pregunta:** «¿El nombre permite construir el modelo mental correcto sin abrir la implementación?»

---

<a id="tema-15"></a>
### 15. Write the Comments First

#### Idea central

Escribir el **contrato antes de implementar** ayuda a diseñar la abstracción sin dejar que la solución técnica determine accidentalmente su interfaz.

#### Ejemplo PL/SQL

```sql
/*
 * Emite una póliza a partir de una cotización APPROVED.
 * Al completar: crea la póliza y marca la cotización ISSUED.
 * No realiza COMMIT: la transacción pertenece al invocador.
 * Una segunda invocación debe seguir la política de
 * idempotencia definida para la emisión.
 */
PROCEDURE emitir_poliza(p_cotizacion_id NUMBER);
```

Antes de programar ya aparecen preguntas importantes: ¿qué ocurre si la cotización está incompleta?, ¿quién confirma la transacción?, ¿cómo se evita una emisión duplicada?

#### Secuencia de trabajo

```text
Requerimiento
   ↓
Responsabilidad y abstracción
   ↓
Contrato de interfaz
   ↓
Pruebas de comportamiento
   ↓
Implementación
```

Si explicar el contrato requiere demasiadas excepciones y opciones, revisar la abstracción antes de escribir más código.

#### Con IA

Pedir primero interfaces, contratos, invariantes y decisiones abiertas. No generar cientos de líneas antes de acordar el comportamiento público.

---

## Parte V — Evolución y coherencia

<a id="tema-16"></a>
### 16. Modifying Existing Code

#### Idea central

La arquitectura se conserva o se degrada **durante el mantenimiento**. Muchos cambios tácticos, razonables por separado, acumulan complejidad.

#### Ejemplo

```python
if alliance == "A": ...
if alliance == "B": ...
if alliance == "C": ...
```

Ante la alianza D, el cambio mínimo es otro `IF`. Pero quizá el diseño ya necesita separar la variación comercial en una configuración o política coherente.

#### Qué observar al modificar

- Duplicación de reglas.
- Excepciones crecientes.
- Nuevos parámetros que atraviesan capas.
- Múltiples archivos para una sola modificación.
- Dependencias inesperadas.

#### Estrategia equilibrada

1. Entender el comportamiento actual y cubrirlo con pruebas cuando sea necesario.
2. Identificar si el requerimiento encaja limpiamente.
3. Si no encaja, considerar un refactor **proporcional**.
4. Separar conceptualmente el refactor sin cambio de comportamiento del cambio funcional.
5. Verificar ambos.

**Pregunta:** «¿Estoy implementando un requisito nuevo o compensando una abstracción que ya no funciona?»

---

<a id="tema-17"></a>
### 17. Consistency

#### Idea central

La consistencia permite aprender una convención una vez y reutilizar ese conocimiento en muchas partes del sistema.

#### Ejemplo

Si `customer_repository.get()` devuelve `None` cuando no encuentra un cliente, pero `policy_repository.get()` lanza excepción y `payment_repository.get()` devuelve un objeto vacío, el consumidor debe memorizar tres comportamientos.

Una convención uniforme vuelve predecible el sistema.

#### Ámbitos

- Naming y estructura de paquetes.
- Manejo de errores.
- Contratos de API.
- Propiedad de transacciones.
- Logging, configuración y pruebas.
- Modelado de fechas, monedas e identificadores.

#### Ejemplo Oracle

Si unos packages hacen `COMMIT` internamente y otros no, cada invocador debe investigar el comportamiento. Una convención explícita sobre los límites transaccionales reduce incertidumbre.

#### Matiz

No perpetuar un mal diseño sólo por consistencia. Si una convención es deficiente, cambiarla deliberadamente, con plan de migración y sin mantener indefinidamente múltiples estilos.

**Pregunta:** «¿La mejora local justifica introducir otra manera de hacer lo mismo?»

---

<a id="tema-18"></a>
### 18. Code Should Be Obvious

#### Idea central

El lector debería poder **predecir el comportamiento** a partir de nombres, firmas, tipos y estructura. La sofisticación no es un fin.

#### Sorpresas que elevan complejidad

- `get_customer()` envía eventos o correos.
- `calculate_premium()` actualiza tablas y hace `COMMIT`.
- `-1` significa «usar valor por defecto» sin indicarlo.
- `create_policy(customer, True, False)` exige recordar flags.
- `calculate()` falla si antes no se llamó a `initialize()` y `load_configuration()`.
- Un método depende de estado global mutable sin declararlo.

#### Mejoras

```python
# Ambiguo
create_policy(customer, True, False)

# Más explícito
create_policy(
    customer,
    send_notification=True,
    generate_receipt=False,
)
```

```python
# Dependencia temporal oculta
engine.initialize()
engine.load_configuration()
engine.calculate(quotation)

# Estado válido desde construcción
engine = RatingEngine(configuration)
engine.calculate(quotation)
```

#### Matiz

Código obvio no significa código de principiante. Una expresión idiomática puede ser clara para el equipo; el criterio es su previsibilidad dentro del contexto y convenciones.

**Pregunta de revisión:** «¿Qué me sorprendería al abrir la implementación después de leer la firma?»

---

## Parte VI — Criterio arquitectónico

<a id="tema-19"></a>
### 19. Software Trends

#### Idea central

Evaluar prácticas y tecnologías por su **efecto neto sobre la complejidad**, no por popularidad, novedad o cantidad de patrones.

#### Tendencias analizadas durante el estudio

- **Agile:** feedback rápido es valioso; confundir agilidad con ausencia de diseño favorece programación táctica.
- **Testing/TDD:** protege comportamiento, pero una cobertura alta no garantiza buenas abstracciones.
- **Herencia:** puede distribuir comportamiento por jerarquías profundas que cuesta comprender; composición puede ayudar, pero también puede fragmentar excesivamente.
- **Getters/setters:** atributos privados con acceso trivial no siempre esconden decisiones de diseño.
- **Design patterns:** son herramientas para problemas reales, no medallas arquitectónicas.

#### Aplicación contemporánea (extensión práctica)

Microservicios, Kubernetes, event-driven, RAG, agentes y MCP son mecanismos, no objetivos. Deben justificarse por problemas concretos.

| Alternativa | Beneficio posible | Complejidad añadida |
|---|---|---|
| Monolito modular | Operación simple, llamadas locales | Menor independencia de despliegue |
| Microservicios | Escalado y despliegue independientes | Red, observabilidad, consistencia distribuida |
| Eventos | Desacoplamiento temporal | Duplicados, orden, reintentos, esquemas |
| Motor de reglas | Cambios configurables | DSL, validación, versionado, depuración |
| Agentes de IA | Orquestación flexible | Evaluación, permisos, trazabilidad, costo |

#### Cuatro preguntas

1. ¿Qué problema concreto resuelve?
2. ¿Qué complejidad elimina?
3. ¿Qué complejidad introduce?
4. ¿El beneficio justifica ese costo **en este sistema**?

---

<a id="tema-20"></a>
### 20. Designing for Performance

#### Idea central

Buen rendimiento y buen diseño no son necesariamente opuestos. **Primero medir; después optimizar lo que importa**, preservando abstracciones siempre que sea posible.

#### Ejemplo hipotético

| Etapa | Tiempo |
|---|---:|
| Validación | 5 ms |
| Rating | 5 ms |
| Oracle | 990 ms |
| **Total** | **1,000 ms** |

Optimizar Rating a cero sólo ahorraría 0.5 % del total. El cuello de botella está en Oracle.

#### Optimizar eliminando trabajo

Si un batch procesa 100,000 recibos y consulta la misma configuración por recibo, puede ser mejor cargarla una vez o resolver el cálculo por conjuntos que microoptimizar el SQL repetido.

#### Guía

1. Definir objetivo de negocio y SLO (por ejemplo P95).
2. Medir latencia, throughput, errores y recursos.
3. Encontrar camino crítico y cuello de botella.
4. Eliminar trabajo repetido o innecesario.
5. Optimizar la implementación interna.
6. Volver a medir y verificar regresiones.

#### Decisiones tempranas legítimas

Para millones de transacciones sí importa desde el inicio elegir procesamiento por conjuntos, particionamiento, paralelismo o una arquitectura adecuada. Evitar optimización prematura **no** significa ignorar requisitos de volumen conocidos.

**Pregunta:** «¿Estamos solucionando un límite medido o introduciendo complejidad por una ganancia hipotética?»

---

<a id="tema-21"></a>
### 21. Decide What Matters

#### Idea central

No todas las decisiones tienen el mismo impacto. Concentrar el esfuerzo en las que condicionan más partes del sistema, son difíciles de revertir o crean complejidad duradera.

#### Matriz de priorización práctica

| Impacto | Reversibilidad | Enfoque |
|---|---|---|
| Alto | Difícil | Comparar alternativas y validar supuestos |
| Alto | Fácil | Diseñar contratos claros y conservar opciones |
| Bajo | Difícil | Verificar restricciones y migración |
| Bajo | Fácil | Seguir convenciones y decidir con ligereza |

*Esta matriz es una herramienta de aplicación, no una clasificación textual del libro.*

#### Ejemplo

En una plataforma de alianzas, decidir qué variaciones pertenecen a configuración y cuáles a lógica de negocio es más estructural que elegir el nombre de una carpeta.

En una plataforma de IA empresarial, la autorización de acceso, grounding, evaluación y trazabilidad pueden ser más importantes que escoger primero entre dos frameworks.

#### Decisiones que suelen merecer análisis profundo

- Límites de dominio y responsabilidades.
- Modelo de datos e invariantes.
- Contratos públicos.
- Consistencia e idempotencia.
- Límites transaccionales.
- Modelo de despliegue cuando hay restricciones reales.

**Pregunta:** «¿Cuántas partes dependen de esta decisión y qué costará cambiarla?»

---

<a id="tema-22"></a>
### 22. Conclusion

#### Idea central

Todo el libro converge en un objetivo: **reducir la complejidad que otras personas deben comprender para modificar el sistema**.

#### Cinco principios para conservar

1. **La complejidad es el enemigo principal.** Detectar amplificación del cambio, carga cognitiva e incógnitas desconocidas.
2. **Diseñar módulos profundos.** Interfaz sencilla y funcionalidad coherente oculta.
3. **Definir abstracciones antes de implementar.** Contratos, alternativas e invariantes primero.
4. **Invertir continuamente en diseño.** No permitir que cada cambio agregue una excepción innecesaria.
5. **Optimizar la evolución del sistema.** Mantener la capacidad de cambiar sin predecir todos los requisitos futuros.

#### Aplicación a desarrollo con IA

La IA abarata producir código, pero no elimina el costo de comprenderlo, integrarlo, operarlo y mantenerlo. El valor de la ingeniería aumenta cuando utilizamos IA para explorar diseños, cuestionar dependencias, definir contratos y evaluar trade-offs, no sólo para generar archivos rápidamente.

#### Cierre

> La calidad de una arquitectura se refleja en la complejidad que evitamos que otras personas deban comprender para modificarla.

*Frase de síntesis elaborada durante el estudio; no es una cita textual de Ousterhout.*

---

<a id="aplicacion"></a>
## Aplicación al analizador de linaje e impacto

Estas son **propuestas de aplicación al PDI**, derivadas de los principios estudiados. No constituyen decisiones aceptadas, inspección del parser ni resultados de implementación.

| Principio | Aplicación posible | Trade-off y forma de evaluarla |
| --- | --- | --- |
| Módulos profundos | Ofrecer una operación coherente de análisis que entregue relaciones, evidencia y diagnósticos. | Una interfaz breve no debe ocultar cobertura ni incertidumbre. Evaluar cuánto conocimiento del parser requiere un consumidor. |
| Information hiding | Encapsular la representación interna del parser y las convenciones de extracción. | Ocultar estructuras internas facilita sustituir implementaciones; deben conservarse origen, ubicación y límites relevantes para interpretar el resultado. |
| Different Layer, Different Abstraction | Distinguir representación sintáctica, relaciones de linaje y explicación de impacto cuando tengan responsabilidades distintas. | Los modelos adicionales se justifican si aíslan cambios; evitar conversiones que solo renombren los mismos campos. |
| Define Errors Out of Existence | Diseñar la actualización repetida de un DAG para evitar duplicados, si ese es el contrato acordado. | No convertir SQL no soportado en un resultado vacío de éxito. Repetición válida y fallo de análisis son situaciones diferentes. |
| Write the Comments First | Definir qué significa una relación de tabla o columna, cómo se identifica la versión y qué evidencia acompaña al resultado. | Un contrato preciso permite comprobar semántica y evita inferir impacto en todas las columnas solo por dependencia de tabla. |
| Design It Twice | Comparar cómo actualizar un catálogo al cambiar un DAG: reemplazo de su contribución o actualización diferencial. | Considerar relaciones obsoletas, consistencia, recuperación y complejidad antes de aceptar una estrategia. |
| Consistency y Code Should Be Obvious | Dar significado uniforme a diagnósticos, identidades y estados Verificado/Inferido/Desconocido. | Unificar términos sin borrar diferencias reales; esos estados no demuestran ejecución ni ausencia de impacto fuera del catálogo. |
| Designing for Performance | Medir por separado extracción, análisis, actualización y consulta antes de optimizar. | Los datos sintéticos deben representar los patrones evaluados; no atribuir una mejora a caché o paralelismo sin comparar resultados. |

### Ejemplo de contrato candidato

Una operación de análisis podría recibir el artefacto fuente y su contexto explícito, y devolver relaciones junto con evidencia y diagnósticos. El consumidor no necesitaría manipular nodos internos del parser para interpretar el resultado.

Antes de aceptar ese contrato, conviene definir:

- Qué identificadores distinguen DAG, tabla, columna y versión del artefacto.
- Qué patrones están cubiertos y cómo se representa un patrón no soportado.
- Si la operación solo analiza o también modifica un catálogo; el nombre debe reflejar sus efectos.
- Cómo se distingue ausencia de relaciones de imposibilidad de analizarlas.
- Qué ocurre si el mismo artefacto se procesa nuevamente y cómo se evita conservar relaciones obsoletas al cambiarlo.

La simplicidad de la interfaz debe reducir el esfuerzo del consumidor sin esconder información necesaria para evaluar la fidelidad del linaje. La explicación asistida por IA podría mantenerse separada del descubrimiento verificable, pero esa separación continúa siendo una propuesta del proyecto.

<a id="repaso"></a>
## Repaso rápido: mapa de principios

| Síntoma o problema | Principio para abordarlo | Temas |
|---|---|---|
| Un cambio afecta muchos archivos | Information hiding; límites de módulo | 2, 4, 5, 7 |
| Demasiadas clases para una tarea simple | Módulos profundos; unir lo que pertenece junto | 4, 9 |
| APIs llenas de flags y excepciones | Mejorar contrato; definir errores fuera | 10, 13, 18 |
| Parámetros que atraviesan capas | Reducir pass-through; asignar responsabilidad | 8 |
| Primer diseño parece obvio | Diseñar dos alternativas | 11 |
| Código que sólo entiende su autor | Nombres, comentarios, consistencia | 12–15, 17–18 |
| Cada ticket agrega un `IF` | Diseño estratégico y refactor proporcional | 3, 16 |
| Tecnologías sofisticadas sin justificación | Evaluar complejidad neta | 19 |
| Optimizaciones sin resultados | Medir y eliminar trabajo | 20 |
| Debate excesivo sobre detalles | Priorizar impacto y reversibilidad | 21 |

<a id="revision"></a>
## Criterios de revisión de arquitectura y código

### Antes de implementar

- ¿Cuál es el problema de negocio y el comportamiento observable esperado?
- ¿Qué módulo debería asumir la responsabilidad?
- ¿Qué decisiones internas deben permanecer ocultas?
- ¿La interfaz puede explicarse en pocas frases?
- ¿Hay precondiciones, postcondiciones, invariantes y efectos secundarios claros?
- ¿Quién controla transacciones, reintentos e idempotencia?
- ¿Comparé alternativas cuando la decisión tiene alto impacto?

### Durante la implementación

- ¿Evité duplicar reglas o dispersar conocimiento?
- ¿Evité parámetros pass-through innecesarios?
- ¿Los nombres representan conceptos precisos del dominio?
- ¿Los comentarios explican decisiones y contratos, no líneas obvias?
- ¿El código respeta convenciones existentes razonables?
- ¿Hay efectos secundarios o dependencias temporales sorprendentes?

### Antes de entregar

- ¿Puedo cambiar la implementación sin afectar consumidores innecesariamente?
- ¿Un cambio pequeño sigue siendo localizado?
- ¿Las pruebas protegen comportamiento y casos importantes?
- ¿Las optimizaciones responden a métricas reales?
- ¿Dejé el módulo más fácil de comprender y modificar?

<a id="contrato"></a>
## Contrato de módulo como herramienta de diseño

```markdown
## Nombre del módulo

### Responsabilidad
¿Qué concepto representa y qué problema resuelve?

### Interfaz pública
¿Qué operaciones ofrece al consumidor?

### Entradas y salidas
Tipos, unidades, semántica y restricciones.

### Precondiciones e invariantes
¿Qué debe cumplirse siempre?

### Efectos secundarios y transacciones
¿Qué modifica? ¿Quién hace commit?

### Errores e idempotencia
¿Qué casos son errores reales? ¿Qué pasa ante reintentos?

### Decisiones ocultas
¿Qué algoritmos, proveedores o formatos no deben filtrarse?

### Alternativas y trade-offs
¿Qué diseños se evaluaron y por qué se eligió éste?
```

<a id="glosario"></a>
## Glosario de repaso

- **Abstracción:** representación que conserva lo relevante y oculta detalles innecesarios.
- **Change amplification:** cambio pequeño con modificaciones dispersas.
- **Cognitive load:** información que el desarrollador debe mantener mentalmente.
- **Deep module:** mucha funcionalidad coherente detrás de una interfaz sencilla.
- **Idempotencia:** repetir una operación no cambia el resultado de negocio más allá del primer efecto válido.
- **Information hiding:** ocultar decisiones de diseño detrás de contratos estables.
- **Information leakage:** propagación de detalles internos hacia consumidores.
- **Invariante:** condición que el modelo debe preservar.
- **Pass-through variable:** dato transportado por capas que no lo utilizan.
- **Tactical programming:** priorizar el cambio inmediato sin atender el diseño acumulado.
- **Strategic programming:** entregar funcionalidad mientras se invierte en reducir complejidad futura.
- **Trade-off:** intercambio entre beneficios y costos de distintas alternativas.
- **Unknown unknowns:** dependencias o consecuencias cuya existencia el desarrollador desconoce.

## Referencia y procedencia

**Obra estudiada:** John Ousterhout, *A Philosophy of Software Design*. No se atribuyen páginas ni edición no indicadas en el material.

**Base de consolidación:** transferencia de estudio aportada por Óscar, `a-philosophy-of-software-design-notas.md`, conservada en `00-pdi/fuentes/Context_Transfer/`. Este documento es la ubicación de consulta de las notas finales; la transferencia conserva su función de fuente. Las aplicaciones al proyecto son propuestas añadidas durante la consolidación.
