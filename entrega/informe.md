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
El proyecto se centra en la actualización de la **Encuesta de Autoevaluación Institucional por Programas** de la Dirección de Desarrollo Estratégico en la Universidad De La Sabana.

En la situación actual, el proceso es manual y depende principalmente de comparación entre el PDF del CNA y un archivo Excel que esta en OneDrive. La revisión se hace pregunta por pregunta, marcando cambios por colores (nuevas, modificadas, eliminadas), y luego se envía al proveedor externo que genera los enlaces de aplicación por público y permite a la audiencia llenar la encuesta.

## Problema identificado
- La actualización y validación requiere jornadas extensas de revisión manual.
- Existe riesgo de omitir preguntas o perder trazabilidad de cambios.
- La validación de enlaces del proveedor también es manual y repetitiva.
- El proceso depende de pocas personas, lo que aumenta el riesgo operativo.

## Objetivo del modelado

Estructurar la información del proceso de autoevaluación institucional para:

- mantener un **banco de preguntas estructurado y consistente**,
- organizar las preguntas según la jerarquía oficial CNA,
- asignar preguntas a públicos y subpúblicos específicos,
- controlar el estado operativo de cada pregunta (nueva, modificada, eliminada),
- estandarizar convenciones de respuesta,
- permitir generar instrumentos, reportes y archivos derivados directamente desde la base de datos.

La base de datos es la **única fuente de verdad**.  
Los archivos Excel o formularios son artefactos generados a partir de consultas.

## Modelo de información del dominio

El modelo representa el estado actual del banco de preguntas, incluyendo:

- estructura CNA (Factor → Característica → Aspecto),
- públicos y subpúblicos,
- convenciones de respuesta y sus opciones,
- preguntas activas con control de cambios,
- distribución de preguntas por subaudiencia,
- verificación con el proveedor.

No se modelan archivos ni instrumentos como entidades independientes.

## Conceptos clave del dominio

### Estructura CNA

Permite clasificar las preguntas conforme al lineamiento oficial.

La jerarquía está compuesta por:

- **Guía CNA**
- **Factor**
- **Característica**
- **Aspecto**

Cada pregunta pertenece a un aspecto específico.

### Público y Subpúblico

Define a quién aplica una pregunta.

- **Grupo de público**: categoría general (Ej. Estudiantes, Profesores).
- **Subpúblico (Subaudiencia)**: segmentación específica (Ej. Pregrado, Doctorado).

Las preguntas pueden aplicar a múltiples subpúblicos.

### Convención de respuesta

Define el conjunto de opciones que se presenta al usuario final.

- Una convención agrupa opciones.
- Cada opción tiene valor, etiqueta y orden.

Esto evita inconsistencias en escalas y formatos.

### Pregunta

Es la entidad central del sistema.

Representa una pregunta vigente en el banco actual.

Incluye:

- clasificación CNA,
- tipo de pregunta,
- convención de respuesta,
- texto actual,
- observaciones internas.

Además contiene campos de control operativo:

- `is_new` → indica si fue añadida recientemente.
- `is_changed` → indica si fue modificada.
- `is_deleted` → indica si fue retirada (soft delete).
- `provider_verified` → indica si fue validada con el proveedor.

Registra responsable y fecha de última modificación.

### Distribución por subpúblico

La asignación de preguntas a subpúblicos se gestiona mediante una relación N:M.

Para cada subpúblico se puede definir:

- orden de aparición,
- si la pregunta es obligatoria.

El “instrumento” es una consulta derivada de esta relación.


## Entidades del modelo

- `USERS`
- `CNA_GUIDELINES`
- `CNA_FACTORS`
- `CNA_CHARACTERISTICS`
- `CNA_ASPECTS`
- `AUDIENCE_GROUPS`
- `AUDIENCE_SUBAUDIENCES`
- `QUESTION_TYPES`
- `RESPONSE_CONVENTIONS`
- `RESPONSE_OPTIONS`
- `QUESTIONS`
- `QUESTION_SUBAUDIENCE`

## Diccionario de datos

### USERS

Responsables de modificación.

- `id` (PK)
- `name`
- `email`
- `created_at`

### CNA_GUIDELINES

Metadatos del documento CNA.

- `id` (PK)
- `title`
- `publication_date`
- `file_reference`

### CNA_FACTORS

- `id` (PK)
- `guideline_id` (FK)
- `code`
- `name`

### CNA_CHARACTERISTICS

- `id` (PK)
- `factor_id` (FK)
- `code`
- `name`

### CNA_ASPECTS

- `id` (PK)
- `characteristic_id` (FK)
- `code`
- `name`

### AUDIENCE_GROUPS

- `id` (PK)
- `name`

### AUDIENCE_SUBAUDIENCES

- `id` (PK)
- `group_id` (FK)
- `name`

### QUESTION_TYPES

- `id` (PK)
- `name`

### RESPONSE_CONVENTIONS

- `id` (PK)
- `code`
- `name`
- `definition`

### RESPONSE_OPTIONS

- `id` (PK)
- `convention_id` (FK)
- `value`
- `label`
- `position`

### QUESTIONS

Banco actual de preguntas.

- `id` (PK)
- `aspect_id` (FK)
- `question_type_id` (FK)
- `response_convention_id` (FK)
- `text`
- `notes`
- `is_new`
- `is_changed`
- `is_deleted`
- `provider_verified`
- `last_modified_by` (FK)
- `last_modified_at`

### QUESTION_SUBAUDIENCE

Relación entre pregunta y subpúblico.

- `id` (PK)
- `question_id` (FK)
- `subaudience_id` (FK)
- `position`
- `required_flag`

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

Imagen ERD
<img width="779" height="952" alt="image" src="https://github.com/user-attachments/assets/468f9e91-aa88-48fa-86ce-b09ee71f2e60" />

Imagen Modelo entidad relacion
<img width="9480" height="7069" alt="image" src="https://github.com/user-attachments/assets/4f556754-8fde-440d-a75a-88eb4513157e" />

