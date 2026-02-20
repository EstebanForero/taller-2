# Informe técnico - Taller 2

## Datos generales
- **Curso:** Arquitectura Empresarial - Universidad de La Sabana
- **Taller:** Modelo de Información y Diagrama de Contexto
- **Fecha:** 20 de febrero de 2026

## Integrantes
- Santiago Sabogal Millan (`santiagoSaMi`)
- Carlos David Cruz Pavas (`CarlosDaCruz`)
- Juan Felipe Cepeda Uribe
- Esteban Fernando Forero Montejo (`EstebanForero`)

## Desarrollo del trabajo
Durante la sesión de clase se trabajó con el caso base de la Clínica Salud Viva para consolidar la lógica de entidades, relaciones y flujos de información. A partir de ese ejercicio, en la entrega se realizó la adaptación al dominio del cliente real: el proceso institucional de construcción, revisión y publicación de encuestas.

El enfoque del equipo fue construir un modelo claro y completo, capaz de representar el ciclo de vida del instrumento desde su diseño hasta su publicación para públicos específicos.

## Modelo de información del cliente real
El modelo final cubre los siguientes componentes:
- ciclo de encuesta,
- instrumentos,
- preguntas,
- revisiones y cambios,
- archivos consolidados,
- proveedores de plataforma,
- enlaces de encuesta y públicos objetivo.

Entidades incluidas:
- `USUARIO`
- `CICLO_ENCUESTA`
- `INSTRUMENTO`
- `PREGUNTA`
- `CAMBIO`
- `REVISION`
- `ARCHIVO_CONSOLIDADO`
- `PROVEEDOR`
- `ENLACE_ENCUESTA`
- `PUBLICO`
- `PARTICIPACION`

Decisiones principales del modelo:
- `REVISION` y `CAMBIO` se modelaron por separado para diferenciar la sesión de revisión de los cambios puntuales.
- `PARTICIPACION` resuelve la relación entre usuarios y revisiones y permite registrar el rol de cada participante.
- `ARCHIVO_CONSOLIDADO` incluye control de versión mediante `hash`.
- `ENLACE_ENCUESTA` se mantuvo independiente para soportar múltiples proveedores y múltiples públicos por instrumento.

## Diagrama de contexto de negocio
El sistema central definido es la plataforma de gestión de encuestas institucionales.

Actores y sistemas externos considerados:
- usuario interno (administrador/revisor),
- proveedor externo de encuestas,
- público objetivo (estudiantes, profesores, administrativos),
- entorno documental (OneDrive/Teams).

Flujos principales:
- creación y edición de instrumentos/preguntas,
- registro de revisiones y cambios,
- almacenamiento de archivos consolidados,
- publicación de enlaces por proveedor,
- distribución de enlaces al público objetivo.

## Entregables
- ERD final: `entrega/ERD_Proyecto.jpg`
- Modelo conceptual: `entrega/Modelo-entidad-relacion.png`
- Informe técnico: `entrega/informe.md`
- Investigación y referencias: `entrega/referencias.md`
