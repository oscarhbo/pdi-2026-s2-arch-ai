# ADR-002 · Aplicación de lecturas al diseño del analizador

Fecha: 15/11/2026. **Estado: incorporada a la versión documental vigente**, por instrucción de Óscar de documentar las aplicaciones como versión final para su posterior revisión y ajuste. Esta instrucción autoriza su incorporación; no se atribuye una revisión individual de cada criterio por parte de Óscar.

## Contexto

H1-05 requiere notas aplicadas al diseño. Las notas de A Philosophy of Software Design y AI Engineering están disponibles y sus aplicaciones se concretan en contratos y reglas del analizador. Óscar aclara que Architecting AI Software Systems corresponde a formación continua de H2.

## Alternativas

- Mantener una lista de conceptos: útil como repaso, insuficiente para establecer su efecto sobre el diseño.
- Integrar contratos y criterios por responsabilidad: permite aplicar el aprendizaje antes de implementar.
- Mezclar análisis, actualización y explicación: reduce interfaces iniciales, pero acopla cambios y dificulta localizar errores.
- Actualizar relaciones por diferencias: puede reducir escrituras, a costa de más comparación y recuperación frente al reemplazo de aportes por DAG.

## Decisión

Incorporar [APSD-01 a APSD-05 y AIE-01 a AIE-05](../../05-aprendizaje/aplicacion-al-proyecto.md) como criterios del diseño vigente. Definir un modelo de análisis independiente del parser, separar publicación, consulta y explicación, reemplazar contribuciones por DAG sin exponer estados intermedios y conservar evidencia e incertidumbre. Evaluar componentes y tarea, controlar contexto, convertir errores confirmados en regresiones y registrar versiones, fallos y consumo.

La especificación detallada permanece en el registro de aplicaciones para evitar duplicación. Las ampliaciones se justifican por necesidades observadas y no por tecnologías estudiadas.

## Consecuencias

Se requiere adaptar la salida del parser al modelo del dominio y controlar publicación y procedencia. La separación permite evaluar consulta y explicación por separado. La evidencia documental completa la aplicación de las lecturas de H1-05; no demuestra implementación ni cierra H1-02. Cobertura SQL, capacidades del parser, almacenamiento y proveedor siguen sujetos al trabajo de arquitectura. Las comprobaciones ejecutables se realizan en H2.
