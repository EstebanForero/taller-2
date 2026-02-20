# Investigación complementaria: notación Chen en modelos entidad-relación

Esta investigación se centra en la notación Chen aplicada al modelo del cliente real (El proyecto que se nos fue assignado por la universidad de la sabana): actualización de la Encuesta de Autoevaluación Institucional por Programas, con trazabilidad de cambios, revisiones, publicación por proveedor y segmentación por públicos objetivo.

El modelo trabajado por el equipo incluye entidades como `USUARIO`, `CICLO_ENCUESTA`, `INSTRUMENTO`, `PREGUNTA`, `CAMBIO`, `REVISION`, `ARCHIVO_CONSOLIDADO`, `PROVEEDOR`, `ENLACE_ENCUESTA`, `PUBLICO` y `PARTICIPACION`, junto con relaciones nombradas como `contiene`, `tiene`, `genera`, `realiza`, `publica_por` y `recibe`.

## 1. Elementos principales de la notación Chen

La notación Chen (Chen, 1976) es conceptual y semántica. Su objetivo es describir con claridad el dominio antes de pasar al diseño físico de base de datos.

Elementos clave:
- **Entidad (rectángulo):** representa un objeto del negocio con identidad propia.
- **Atributo (óvalo):** describe propiedades de la entidad o de la relación.
- **Relación (rombo):** representa la asociación entre entidades y se nombra con verbo.
- **Cardinalidad:** define cuántas ocurrencias de una entidad se asocian con otra.
- **Participación total/parcial:** indica si la existencia de una entidad depende de participar o no en la relación.

Tipos de entidad usados o relevantes en este proyecto:
- **Entidades fuertes:** `INSTRUMENTO`, `USUARIO`, `CICLO_ENCUESTA`, `PUBLICO`, etc.
- **Entidades asociativas (derivadas de M:N):** `CAMBIO` y `PARTICIPACION`, porque almacenan atributos propios de la relación (`tipo_cambio`, `fecha_cambio`, `rol`, etc.).
- **Entidades débiles:** no se usaron en la versión actual, pero pueden ser útiles en extensiones futuras (por ejemplo, versionado detallado de pregunta dependiente de instrumento).

## 2. Funcionamiento de las relaciones en Chen

En Chen, la relación no es solo una línea; es un elemento explícito del modelo. Esto aporta claridad de negocio.

Ejemplos del modelo:
- `CICLO_ENCUESTA (1) -- contiene -- INSTRUMENTO (0..N)`
- `INSTRUMENTO (1) -- tiene -- PREGUNTA (0..N)`
- `USUARIO (1) -- realiza -- CAMBIO (0..N)`
- `PROVEEDOR (1) -- genera_enlace -- ENLACE_ENCUESTA (0..N)`
- `PUBLICO (1) -- recibe -- ENLACE_ENCUESTA (0..N)`

En términos prácticos, esta forma de modelar ayuda a leer reglas del negocio como frases completas y permite verificar más fácil si falta alguna relación importante para la operación.

## 3. Diferencias con Crow's Foot y razón de uso en este taller

| Aspecto | Chen | Crow's Foot |
|---|---|---|
| Enfoque | Conceptual | Lógico/físico |
| Relación | Rombo con nombre verbal | Línea con cardinalidad simbólica |
| Atributos de relación | Naturales en el modelo | Usualmente se transforman en tabla asociativa |
| Comunicación con no técnicos | Alta | Media |
| Nivel de detalle técnico | Medio | Alto |

Razones para usar Chen en esta entrega:
- El proceso del cliente tiene varias reglas de negocio que deben quedar explícitas (ciclo, revisión, cambios, envío a proveedor, publicación por público).
- Existen relaciones con datos propios (`CAMBIO`, `PARTICIPACION`) que se explican mejor desde un enfoque conceptual.
- Facilita validación con actores funcionales (área administrativa y responsables del proceso) antes de pasar a implementación.

## 4. Aplicación directa al dominio del cliente

La notación Chen se ajusta bien al problema identificado en el levantamiento:
- El proceso actual es manual y requiere trazabilidad fuerte.
- Se necesita distinguir revisión de cambio puntual.
- Se debe mantener evidencia de versiones de archivo.
- La publicación depende de proveedor y público, no solo del instrumento.

Con este enfoque, el modelo permite representar de forma ordenada el flujo de actualización de preguntas y control de cambios por ciclo, sin perder legibilidad para revisión académica y funcional.

## 5. Conclusiones técnicas

- Chen es adecuado para la fase de análisis del dominio porque hace explícitas las relaciones del negocio.
- En este proyecto, la semántica de relaciones aporta más valor que un diagrama únicamente físico.
- El modelo conceptual en Chen puede transformarse sin fricción a un ERD relacional para implementación.

## Referencias (APA)
- Chen, P. P. (1976). The entity-relationship model-toward a unified view of data. *ACM Transactions on Database Systems, 1*(1), 9-36. https://doi.org/10.1145/320434.320440
- Creately. (2025, mayo 14). *Easy guide to Chen notation for entity-relationship diagrams*. https://creately.com/guides/chen-notation-in-erd/
- Gleek.io. (2021, agosto 19). *Crow's foot vs. Chen notation: Detailed comparison*. https://www.gleek.io/blog/crows-foot-chen
- Redgate Software. (2014, agosto 2). *Chen notation*. https://www.red-gate.com/blog/chen-erd-notation/
- Software Ideas Modeler. (2024, mayo 2). *Chen ER diagram - Entity-relationship diagram in Chen notation*. https://www.softwareideas.net/chen-er-diagram-erd
