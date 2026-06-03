# PRD — Sistema de Gestión de Expedientes (SGE)

> **Versión:** 1.0.0  
> **Estado:** Borrador  
> **Responsable:** Equipo de Producto  

---

## 1. Resumen Ejecutivo

El Sistema de Gestión de Expedientes (SGE) es un producto digital que combina capacidades de **GED** (Gestión Electrónica Documental), **BPM** (Business Process Management) y **Gestión de Expedientes** para la administración de proyectos de inversión pública. Su objetivo es reemplazar procesos manuales y descentralizados con una plataforma unificada que garantice trazabilidad, control de acceso y automatización de flujos documentales a nivel de expedientes.

---

## 2. Problema

Las entidades públicas gestionan expedientes y documentos de forma dispersa: archivos sueltos en carpetas compartidas, correos electrónicos, sistemas aislados. Esto genera:

- Pérdida de documentos y versiones.
- Falta de trazabilidad sobre quién aprueba o revisa.
- Dificultad para encontrar documentos por contenido.
- Inconsistencias en los estados documentales.
- Ausencia de control de acceso por roles y oficinas.

---

## 3. Visión del Producto

Ser la plataforma estándar de gestión documentaria para proyectos de inversión pública, permitiendo que cualquier oficina gestione sus expedientes de forma digital, trazable y colaborativa.

---

## 4. Alcance del MVP

### Incluye

| Módulo | Descripción |
|--------|-------------|
| Autenticación y Roles | Ingreso con usuario/contraseña según rol asignado |
| Administración de Usuarios | CRUD de usuarios, asignación de roles y oficinas |
| Gestión de Proyectos y Expedientes | Jerarquía Proyecto → Expediente (core) → Archivos adjuntos |
| Carga de Archivos a Expedientes | Subida de PDF y DOCX asociados a un expediente |
| Bandeja por Oficina | Visualización de expedientes filtrada por oficina con buscador y paginación |
| Estados de Expedientes | Workflow básico: Borrador → Enviado → Observado → Subsanado → Aprobado → Archivado |
| Notificaciones | Alertas sobre expedientes faltantes |

### No incluye

- Reportes y exportaciones
- OCR
- Salida a producción
- Backups automáticos
- Testing intensivo

---

## 5. Usuarios Objetivo

| Perfil | Descripción |
|--------|-------------|
| Super Usuario | Acceso global, administración del sistema |
| Jefe de Oficina | Supervisión, aprobaciones, cambios de estado |
| Técnico / Administrativo | Carga documental y gestión operativa |
| Residente | Gestión documental en OAD y Residencia |
| Inspector | Inspección y reportes en INS |

---

## 6. Arquitectura del Sistema

El sistema sigue un enfoque **DDD** (Domain-Driven Design) con múltiples servicios y subdominios:

```
┌─────────────────────────────────────────────────────────────────┐
│                    ENTERPRISE DOCUMENT MANAGEMENT               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │               payloadcms (Document Workspace)            │   │
│  │  Frontend + BFF/Facade — capa de interacción humana     │   │
│  └──────────────────────┬──────────────────────────────────┘   │
│                         │ HTTP/REST                            │
│  ┌──────────────────────▼──────────────────────────────────┐   │
│  │              quarkus-app (Core Backend)                  │   │
│  │  ┌─────────────────┐ ┌────────────┐ ┌────────────────┐  │   │
 │  │  │ Expediente Mgmt│ │   Users    │ │Identity Wrapper│  │   │
 │  │  │ (Core)          │ │ (Core)     │ │(Support)       │  │   │
│  │  └─────────────────┘ └────────────┘ └───────┬────────┘  │   │
│  └──────────────────────┬──────────────────────┼────────────┘   │
│                         │                      │                │
│  ┌──────────────────────▼──────┐  ┌────────────▼──────────┐    │
│  │  inngest (Workflow/Process) │  │  keycloak (IAM)       │    │
│  │  —  orquestación async     │  │  — autenticación      │    │
│  └─────────────────────────────┘  └───────────────────────┘    │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  storage (SeaweedFS) — almacenamiento físico de archivos │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### Servicios

| Servicio | Stack | Subdominios | Rol |
|----------|-------|-------------|-----|
| quarkus-app | Java / Quarkus | expediente-management, users, identity-wrapper | Core backend con lógica de negocio |
| payloadcms | PayloadCMS / Node.js | document-workspace | Frontend + BFF/Facade + capa visual |
| inngest | Inngest / TypeScript | workflow | Orquestación de flujos y procesos async |
| storage | SeaweedFS (Docker) | storage | Almacenamiento físico de archivos |
| keycloak | Keycloak (Docker) | keycloak-iam | IAM, autenticación y autorización |

---

## 7. Subdominios DDD

| Subdominio | Tipo | Servicio |
|------------|------|----------|
| Expediente Management | Core | quarkus-app |
| Users | Core | quarkus-app |
| Identity Wrapper | Support | quarkus-app |
| Document Workspace | Support | payloadcms |
| Workflow / Process Manager | Support | inngest |
| Storage (SeaweedFS) | Generic | storage |
| Keycloak IAM | Generic | keycloak |

---

## 8. Requisitos Funcionales

| ID | Módulo | Descripción | Prioridad |
|----|--------|-------------|-----------|
| RF-01 | Autenticación | Ingreso con usuario y contraseña | Alta |
| RF-02 | Autenticación | 5 roles: Super Usuario, Jefe Oficina, Inspector, Residente, Técnico/Admin | Alta |
| RF-03 | Autenticación | Acceso según rol | Alta |
| RF-04 | Autenticación | Cierre de sesión por inactividad (5 min) | Alta |
| RF-05 | Usuarios | Solo Super Usuario crea, edita y desactiva cuentas | Alta |
| RF-06 | Usuarios | Registro: nombre completo, username, contraseña, rol, oficina | Alta |
| RF-07 | Usuarios | Restablecer contraseña vía email | Media |
| RF-08 | Usuarios | Listado con buscador y paginación | Baja |
| RF-09 | Archivos | Subida solo PDF y DOCX a un expediente | Alta |
| RF-10 | Expedientes | Formulario con metadata generada automáticamente al crear expediente | Alta |
| RF-11 | Expedientes | Nombre único para cada archivo por hash-checksum | Baja |
| RF-12 | Expedientes | Sin orden de carga exigido para archivos | Alta |
| RF-13 | Archivos | Rechazar formatos no permitidos | Baja |
| RF-14 | Bandeja | Visibilidad de expedientes por oficina según jerarquía | Alta |
| RF-15 | Bandeja | Super Usuario ve expedientes de todas las oficinas y alerta faltantes | Alta |
| RF-16 | Bandeja | Estados del expediente: Borrador, Enviado, Observado, Subsanado, Aprobado, Archivado | Media |
| RF-17 | Bandeja | Jefe de Oficina y Super Usuario cambian estados de expedientes | Alta |
| RF-18 | Bandeja | Buscador de expedientes por nombre/asunto + paginación | Baja |

---

## 9. Requisitos No Funcionales

| ID | Categoría | Descripción | Criterio |
|----|-----------|-------------|----------|
| RNF-01 | Seguridad | Contraseñas seguras vía Keycloak | Cumplir políticas de creación |
| RNF-02 | Seguridad | Acceso solo con sesión activa y rol | Acceso denegado sin permisos |
| RNF-04 | Rendimiento | Archivos de 0 MB a 4 GB | Carga exitosa |
| RNF-05 | Disponibilidad | Disponible días hábiles | Sin interrupciones planificadas |
| RNF-06 | Usabilidad | Interfaz intuitiva | Usuario nuevo opera con mínima capacitación |
| RNF-07 | Usabilidad | Funciona en Google Chrome | Compatibilidad completa |
| RNF-08 | Mantenibilidad | Código organizado y modular | Cambios sin afectar otras funciones |
| RNF-09 | Escalabilidad | Nuevas oficinas/usuarios sin cambios estructurales | Alta sin modificar código |
| RNF-10 | Almacenamiento | Archivos en servidor, DB solo metadatos | Archivos localizables siempre |

---

## 10. Definiciones y Glosario

| Término | Significado |
|---------|-------------|
| SGE | Sistema de Gestión de Expedientes |
| MVP | Mínimo Producto Viable |
| GED | Gestión Electrónica Documental |
| BPM | Business Process Management |
| Expediente | Unidad core del sistema — contiene metadata, estados, archivos y flujo de trabajo |
| Archivo | Documento PDF/DOCX adjunto a un expediente, sin lógica de negocio propia |
| PDF / DOCX | Formatos admitidos en MVP |
| Nomenclatura | Metadata generada automáticamente por el sistema para el expediente |
| BFF | Backend For Frontend — capa de fachada para el frontend |
| DDD | Domain-Driven Design |
| RBAC | Role-Based Access Control |
| IAM | Identity and Access Management |

---

## 11. Criterios de Éxito del MVP

1. Un usuario puede iniciar sesión, crear un expediente, adjuntar un PDF, y verlo en su bandeja de oficina.
2. El Jefe de Oficina puede cambiar el estado de un expediente.
3. El Super Usuario puede crear usuarios y asignar roles.
4. Los documentos son accesibles solo por usuarios de la misma oficina (excepto Super Usuario).
5. El sistema notifica cuando un expediente está incompleto.
