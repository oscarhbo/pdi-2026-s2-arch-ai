# Solución

**Estado: Pendiente.** No hay prototipo implementado. La etapa actual es análisis y diseño documental.

Antes de programar: definir [caso y alcance](../01-caso-de-negocio/alcance-y-exclusiones.md), justificar [arquitectura](../02-arquitectura/README.md) y concretar [evaluación](../02-arquitectura/docs/validation.md).

- [src](src/README.md): futura implementación.
- [tests](tests/README.md): verificaciones de comportamiento y regresión.
- [ejemplos](ejemplos/README.md): corpus sintético conocido.
- [config](config/README.md): futura configuración sin secretos.

Al implementar, completar requisitos, instalación, ejecución reproducible, ejemplo de entrada/salida, pruebas y limitaciones. Spark SQL es el dialecto de referencia; el perfil de patrones y el stack propuesto están en la arquitectura integrada. Su selección documental no acredita implementación.

La [inspección del parser y de dos DAGs](../02-arquitectura/revision-parser-y-dags.md) ya está documentada. La implementación futura partirá de sus hallazgos y del [diseño técnico](../02-arquitectura/docs/diseno-tecnico.md), sin atribuir al parser capacidades aún no comprobadas.
