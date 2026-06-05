# Reglas de Negocio — Subdominio Expediente Management

> **Servicio:** quarkus-app  
> **Subdominio:** Expediente Management (Core)  

---

## 1. Reglas de Jerarquía

| # | Regla | Descripción |
|---|-------|-------------|
| BR-DM-01 | Proyecto obligatorio | Todo expediente debe pertenecer a un proyecto existente. |
| BR-DM-02 | Expediente obligatorio | Todo documento debe pertenecer a un expediente existente. |
| BR-DM-03 | Sin documento huérfano | No se permite subir documentos sin asociarlos a un expediente. |

---

## 2. Reglas de Documentos

| # | Regla | Descripción |
|---|-------|-------------|
| BR-DM-04 | Formatos permitidos | Solo PDF y DOCX. Cualquier otro formato (JPG, PNG, etc.) es rechazado. |
| BR-DM-05 | Hash único | Cada documento recibe un hash-checksum SHA-256 calculado sobre su contenido binario. |
| BR-DM-06 | Nombre único generado | El sistema genera el nombre del documento como: `{hash}_{timestamp}.{extension}`. |
| BR-DM-07 | Metadata automática | Se registra automáticamente: hash, fecha de subida, usuario subidor, tamaño, formato. |
| BR-DM-08 | Metadata opcional del usuario | El usuario puede añadir: asunto, descripción, tipo documental. |
| BR-DM-09 | Sin orden de carga | Los documentos pueden subirse en cualquier orden dentro de un expediente. |
| BR-DM-10 | Límite de tamaño | Archivos entre 0 bytes y 4 GB. |
| BR-DM-11 | Contenido inmutable | Un documento subido no puede modificarse. Los cambios generan una nueva versión. |

---

## 3. Reglas de Versionado

| # | Regla | Descripción |
|---|-------|-------------|
| BR-DM-12 | Versión inicial | Al subir un documento por primera vez se crea la versión 1. |
| BR-DM-13 | Nueva versión | Cada nueva subida del mismo documento (mismo nombre lógico) incrementa la versión. |
| BR-DM-14 | Historial preservado | Las versiones anteriores nunca se eliminan, solo se agregan nuevas. |
| BR-DM-15 | Versión activa | La versión más reciente es la "activa" para visualización y aprobación. |

---

## 4. Reglas de Estados de Expediente

| # | Regla | Descripción |
|---|-------|-------------|
| BR-DM-16 | Transiciones válidas | Solo se permite: Borrador→Enviado, Enviado→Observado, Enviado→Aprobado, Observado→Subsanado, Subsanado→Enviado, Aprobado→Archivado. |
| BR-DM-17 | Sin retroceso desde Archivado | Archivado es estado terminal. No se permiten transiciones de salida. |
| BR-DM-18 | Justificación en Observado | Toda transición a Observado requiere una justificación textual obligatoria. |
| BR-DM-19 | Trazabilidad obligatoria | Todo cambio de estado se registra con: usuario, fecha, estado anterior, estado nuevo, justificación. |

---

## 5. Reglas de Búsqueda

| # | Regla | Descripción |
|---|-------|-------------|
| BR-DM-20 | Filtro por oficina | Las búsquedas de expedientes deben filtrarse por oficina del usuario (excepto Super Usuario). |
| BR-DM-21 | Búsqueda textual | La búsqueda por contenido aplica sobre nombre de documento y asunto. |
| BR-DM-22 | Paginación | Toda listas de resultados debe incluir paginación (20 elementos por página por defecto). |
