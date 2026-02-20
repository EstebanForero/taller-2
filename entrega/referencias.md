# Investigación complementaria: ER (Chen) y diagrama de contexto en casos reales

Esta investigación complementa la Parte 2 del taller (aplicación al cliente real: gestión de ciclos de encuestas, instrumentos, preguntas, revisiones y publicación por públicos objetivo). Se justifica el uso combinado de modelo entidad-relación conceptual (notación Chen) y diagrama de contexto de negocio para delimitar fronteras del sistema y flujos de información.

## 1. Modelo Entidad-Relación en notación Chen

El enfoque de Chen (1976) modela explícitamente entidades, atributos y relaciones como elementos semánticos del dominio. En dominios con trazabilidad, revisión y auditoría, esta notación es útil porque:
- Hace visibles las relaciones de negocio y su significado.
- Facilita discutir el modelo con actores no técnicos.
- Permite transformar a modelo relacional sin perder intención conceptual.

En el caso del cliente real, el modelo requiere representar estados de instrumentos, cambios por revisión, responsables y publicación externa. Por eso, el uso conceptual tipo Chen complementa bien un ERD más físico (Crow's Foot).

## 2. Diagrama de contexto (Level-0)

El diagrama de contexto define el límite del sistema como una sola caja funcional y muestra únicamente actores/sistemas externos y sus flujos de entrada/salida. En este taller, este enfoque permite:
- Diferenciar qué está dentro del sistema institucional y qué pertenece a plataformas externas.
- Evitar ambigüedad de alcance al separar diseño de instrumentos, almacenamiento documental y difusión.
- Identificar puntos de interoperabilidad críticos (proveedores de encuesta, repositorios documentales y públicos objetivo).

## 3. Relación con casos reales de industria

### Salud
En sistemas hospitalarios, la combinación ERD + contexto es común para modelar entidades clínicas (paciente, cita, médico, factura) y delimitar interacciones con terceros como aseguradoras y servicios administrativos.

### Educación / encuestas
En dominios educativos, los modelos de encuestas exigen flexibilidad para preguntas de distintos tipos, estructura modular, versionamiento y trazabilidad. El patrón observado en literatura coincide con la solución del equipo: instrumento-pregunta-revisión-cambio-publicación.

## 4. Conclusión aplicada al taller

La combinación Chen + ERD + diagrama de contexto es adecuada para el cliente real porque permite:
- Claridad conceptual de reglas de negocio.
- Base consistente para implementación técnica posterior.
- Trazabilidad y gobernanza del ciclo de vida de instrumentos de encuesta.

## Referencias (APA)
- AL-Odiab, A. A. (2018). *Hospital system* [Proyecto de grado]. Universidad de Majmaah. https://www.mu.edu.sa/sites/default/files/content/2018/12/HOSPITAL%20SYSTEM..pdf
- Chen, P. P. (1976). The entity-relationship model-toward a unified view of data. *ACM Transactions on Database Systems, 1*(1), 9-36. https://doi.org/10.1145/320434.320440
- John, K. (2022). *Designing a database schema for survey questions* [Tesis de maestría, Otto-von-Guericke-Universitat Magdeburg]. https://wwwiti.cs.uni-magdeburg.de/iti_db/publikationen/ps/auto/thesisJohn22.pdf
- Sakra, A. A., & Mosa, D. T. (2016). Data flow diagrams of an electronic medical record system in Mansoura Hospital. *International Journal of Computer Techniques, 3*(4), 1-10. https://rajpub.com/index.php/ijct/article/view/1532ijct
