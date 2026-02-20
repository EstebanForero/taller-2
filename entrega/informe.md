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

## Contexto del proyecto
El proyecto se centra en la actualización de la **Encuesta de Autoevaluación Institucional por Programas** de la Dirección de Desarrollo Estratégico.

En la situación actual, el proceso es manual y depende principalmente de comparación entre el PDF del CNA y un archivo Excel consolidado en OneDrive. La revisión se hace pregunta por pregunta, marcando cambios por colores (nuevas, modificadas, eliminadas), y luego se envía al proveedor externo que genera los enlaces de aplicación por público.

## Problema identificado
- La actualización y validación requiere jornadas extensas de revisión manual.
- Existe riesgo de omitir preguntas o perder trazabilidad de cambios.
- La validación de enlaces del proveedor también es manual y repetitiva.
- El proceso depende de pocas personas, lo que aumenta el riesgo operativo.

## Objetivo del modelado
Estructurar la información del proceso para:
- mantener trazabilidad de cambios por ciclo,
- registrar responsables y revisiones,
- controlar versiones de archivos consolidados,
- organizar la publicación de enlaces por proveedor y por público objetivo.

## Modelo de información del cliente real
El modelo final cubre los siguientes componentes:
- ciclo de encuesta,
- instrumentos,
- preguntas y estado de cada pregunta,
- revisiones y cambios,
- archivos consolidados,
- proveedores de encuesta,
- enlaces y públicos objetivo.

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
- `REVISION` y `CAMBIO` se separaron para diferenciar la sesión de revisión del detalle de cada ajuste.
- `CAMBIO` registra fecha, tipo de cambio y detalle anterior/nuevo para conservar trazabilidad.
- `PARTICIPACION` permite registrar quién participó en cada revisión y con qué rol.
- `ARCHIVO_CONSOLIDADO` incorpora fecha de versión y `hash` para control documental.
- `ENLACE_ENCUESTA` permite manejar varios enlaces por instrumento según proveedor y público.

## Diagrama de contexto de negocio
El sistema central definido es la plataforma de gestión de encuestas institucionales.

Actores y sistemas externos considerados:
- usuario interno (administrador, revisor, aprobador),
- proveedor externo de encuestas,
- públicos objetivo (pregrado, posgrado, profesores, administrativos y otros grupos definidos por la dirección),
- entorno documental institucional (OneDrive/Teams),
- lineamientos del CNA como fuente normativa de entrada.

Flujos principales:
- recepción y análisis de lineamientos CNA por ciclo,
- actualización de instrumentos y preguntas,
- registro de revisiones y cambios con responsable,
- generación de archivo consolidado versionado,
- envío de consolidado al proveedor,
- recepción y validación de enlaces,
- distribución de enlaces por público objetivo.

## Entregables
- ERD final: `entrega/ERD_Proyecto.jpg`
- Modelo conceptual: `entrega/Modelo-entidad-relacion.png`
- Informe técnico: `entrega/informe.md`
- Investigación y referencias: `entrega/referencias.md`
