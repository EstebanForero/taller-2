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
Estructurar la información del proceso para:

- mantener trazabilidad real de las preguntas entre ciclos CNA,
- reconstruir exactamente qué cuestionario existía en cada revisión,
- registrar responsables de los cambios,
- organizar la generación y validación de enlaces de encuesta por público,
- eliminar dependencias del Excel como fuente de verdad.

El sistema no modela archivos como entidades de negocio.  
Los archivos (Excel / formularios) se consideran **artefactos generados**, derivables desde la base de datos.

---

## Modelo de información real (interpretación del proceso)
El proceso no gira alrededor de archivos ni instrumentos,
sino alrededor de **preguntas versionadas que se publican en cada ciclo**.

El modelo final representa:

- ciclos CNA (ediciones del proceso),
- preguntas base,
- historial de cambios de cada pregunta,
- qué versión de la pregunta fue publicada en cada ciclo,
- públicos objetivo a los que aplica la pregunta,
- responsables de modificaciones,
- enlaces generados para diligenciamiento,
- validaciones de publicación.

---

## Conceptos clave del dominio

### Ciclo
Equivale a una edición del proceso de autoevaluación  
(Ej: CNA 2024, CNA 2025, CNA 2026).

Un ciclo es una **fotografía oficial publicada** del banco de preguntas.

---

### Pregunta
Es la entidad central del sistema.

Representa el identificador permanente de una pregunta:
- su código nunca cambia,
- su significado conceptual permanece,
- pero su contenido puede evolucionar.

La pregunta NO guarda texto directamente.

---

### Historial de pregunta
Cada modificación crea una nueva versión histórica.

Permite saber:
- cómo era la pregunta en cualquier momento,
- quién la cambió,
- qué cambió,
- cuándo cambió.

Esto reemplaza completamente la antigua entidad `CAMBIO`.

No se registran cambios como eventos aislados,
sino como estados históricos reconstruibles.

---

### Publicación de pregunta en ciclo
Un ciclo no copia preguntas:
referencia una versión específica del historial.

Esto permite:

- detectar preguntas nuevas
- detectar eliminadas
- detectar modificadas
- reconstruir el cuestionario exacto
- comparar ciclos automáticamente

---

### Público objetivo
Define para quién aplica la pregunta:

- estudiantes
- profesores
- administrativos
- egresados
- directivos
- etc.

La pertenencia al público es parte de la publicación en ciclo,
no de la pregunta base.

---

### Enlace de encuesta
Representa la encuesta publicada.

Se genera por combinación:

ciclo + público

No depende del proveedor específico como entidad de negocio.
El proveedor es solo un medio técnico de distribución.

---

### Verificación
Registra la validación del enlace publicado:
- quién verificó
- cuándo
- si funcionaba
- observaciones

---

## Entidades resultantes

El modelo actualizado queda compuesto por:

- `USUARIO`
- `CICLO`
- `PREGUNTA`
- `PREGUNTA_HISTORIAL`
- `PUBLICACION_PREGUNTA`
- `PUBLICO`
- `ENLACE`
- `VERIFICACION`

---

## Decisiones de modelado

### Eliminadas
- CAMBIO → reemplazado por historial versionado
- INSTRUMENTO → es derivable (consulta por público)
- ARCHIVO_CONSOLIDADO → es un reporte generado
- PROVEEDOR → agente externo sin valor de negocio
- PARTICIPACION → el responsable queda en cada cambio histórico

---

### Principios adoptados

1. La base de datos es la fuente de verdad, no el Excel.
2. Un ciclo es una fotografía, no un proceso.
3. Los cambios se reconstruyen por estados, no por eventos manuales.
4. Un cuestionario es una consulta, no una entidad.
5. Los enlaces representan la publicación oficial del ciclo.

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

