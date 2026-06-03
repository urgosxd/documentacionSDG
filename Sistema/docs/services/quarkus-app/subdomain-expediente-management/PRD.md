# PRD — Subdominio Expediente Management

> **Servicio:** quarkus-app  
> **Subdominio:** Expediente Management (Core)  
> **Versión:** 1.0.0  

---

## 1. Propósito

Gestionar el ciclo de vida completo de los documentos digitales dentro del SGD, desde su creación hasta su archivado. Es el subdominio **core** del sistema: contiene la lógica de negocio principal.

---

## 2. Responsabilidades

- Registro y gestión de proyectos de inversión.
- Gestión de expedientes asociados a proyectos.
- Gestión de documentos dentro de expedientes.
- Versionado de documentos.
- Control de estados de expedientes.
- Generación automática de nomenclatura y metadata.
- Búsqueda de expedientes por contenido.

---

## 3. Jerarquía de Entidades

```
NIVEL 1 — Proyecto (Inversión Pública)
│   ├── presupuesto
│   ├── código SNIP / Invierte.pe
│   ├── entidad responsable
│   ├── estado del proyecto
│   └── cronograma general
│
└── NIVEL 2 — Expediente (Procedimiento Documental)
    │   ├── documentos
    │   ├── workflow asignado
    │   ├── firmas requeridas
    │   ├── auditoría
    │   └── responsables
    │
    └── NIVEL 3 — Documento (Archivo Individual)
        ├── archivo físico (referencia a Storage)
        ├── metadata
        ├── hash-checksum
        └── versiones
```

### Tipos de Expediente

- Expediente Técnico
- Expediente de Ejecución
- Expediente de Adicionales
- Expediente de Liquidación

---

## 4. Entidades Principales

| Entidad | Descripción | Atributos clave |
|---------|-------------|-----------------|
| Proyecto | Inversión pública registrada | id, codigoSNIP, nombre, presupuesto, entidad, estado, cronograma |
| Expediente | Procedimiento documental | id, proyectoId, tipo, estado, oficinaId, responsableId |
| Documento | Archivo individual con metadata | id, expedienteId, nombre, hash, version, formato, metadata |
| VersionDocumento | Versión específica de un documento | id, documentoId, numeroVersion, archivoRef, fecha, usuarioId |
| HistorialDocumento | Trazabilidad de cambios | id, documentoId, accion, usuarioId, timestamp, detalle |
| FlujoDocumental | Workflow asignado | id, expedienteId, estadoActual, transicionesPermitidas |

---

## 5. Requisitos Funcionales

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| DM-01 | Registrar proyectos con código SNIP, presupuesto y cronograma | Alta |
| DM-02 | Crear expedientes asociados a un proyecto | Alta |
| DM-03 | Subir documentos PDF y DOCX a un expediente | Alta |
| DM-04 | Generar metadata automática (hash, nomenclatura, fecha) al subir | Alta |
| DM-05 | Rechazar documentos en formato no permitido | Baja |
| DM-06 | Generar nombre único por hash-checksum | Baja |
| DM-07 | Cambiar estado de un expediente según máquina de estados | Alta |
| DM-08 | Listar expedientes por oficina con filtros | Alta |
| DM-09 | Buscar expedientes por contenido (nombre, asunto) | Baja |
| DM-10 | Recuperar versiones anteriores de un documento | Media |
| DM-11 | Obtener historial de cambios de un expediente | Media |

---

## 6. Reglas de Negocio

Ver `business-rules.md` del mismo subdominio.
