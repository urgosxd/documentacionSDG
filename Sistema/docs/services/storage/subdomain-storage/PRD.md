# PRD — Subdominio Storage (SeaweedFS)

> **Servicio:** storage (Docker Compose — SeaweedFS)  
> **Subdominio:** Storage SeaweedFS (Generic)  
> **Versión:** 1.0.0  

---

## 1. Propósito

Proveer almacenamiento físico confiable para los archivos digitales del SGD. Este subdominio se ejecuta como un servicio externo (SeaweedFS) orquestado mediante Docker Compose, sin lógica de negocio propia.

---

## 2. Responsabilidades

- Almacenamiento físico de archivos PDF y DOCX.
- Recuperación de documentos por referencia.
- Control de acceso a nivel de archivos.
- Versionado físico de documentos.
- Almacenamiento de evidencias y adjuntos.

---

## 3. Entidades Principales

| Entidad | Descripción |
|---------|-------------|
| Archivo | Representación física del archivo almacenado |
| RecursoDigital | Referencia lógica a un archivo en el storage |
| StorageReference | Identificador único para localizar un archivo en SeaweedFS (fid) |
| Evidencia | Archivo asociado a un evento o transición |
| DocumentoAdjunto | Archivo adicional vinculado a un documento principal |

---

## 4. Requisitos Funcionales

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| ST-01 | Almacenar archivo binario y retornar identificador único (fid) | Alta |
| ST-02 | Recuperar archivo por su fid | Alta |
| ST-03 | Eliminar archivo por fid | Media |
| ST-04 | Soporte para archivos de 0 MB a 4 GB | Alta |
| ST-05 | Almacenamiento redundante (replicación) | Media |
| ST-06 | Almacenar metadatos del archivo (tamaño, tipo MIME, fecha) | Alta |

---

## 5. Reglas de Negocio

Ver `business-rules.md` del mismo subdominio.
