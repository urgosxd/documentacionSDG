# PRD — Subdominio Document Workspace

> **Servicio:** payloadcms  
> **Subdominio:** Document Workspace (Support)  
> **Versión:** 1.0.0  

---

## 1. Propósito

Proveer la capa de interacción visual donde los usuarios trabajan con los **expedientes** del SGD. Actúa como **Frontend** y **BFF (Backend For Frontend) / Facade**, orquestando la experiencia de usuario sin contener lógica de negocio del dominio.

PayloadCMS funciona como la plataforma donde los usuarios humanos:
- crean expedientes
- editan borradores
- suben archivos adjuntos a expedientes
- revisan contenido documental
- colaboran en tiempo real
- observan cambios en los expedientes
- gestionan metadata del expediente
- aprueban/rechazan expedientes
- trabajan visualmente con todo el ciclo de vida del expediente

---

## 2. Responsabilidades

- Interfaz de usuario web para todas las operaciones del SGD.
- BFF que orquesta llamadas entre frontend y quarkus-app.
- Presentación de bandejas, formularios y dashboards.
- Validación de formularios del lado del cliente.
- Gestión de sesión del lado del frontend (redirección a login, logout).
- Experiencia de workspace de expedientes.
- Renderizado de archivos (previsualización PDF de documentos adjuntos).
- Comunicación con inngest para notificaciones en tiempo real.
- NO contiene lógica de negocio del dominio — toda operación crítica delega en quarkus-app.

---

## 3. Entidades Principales

| Entidad | Descripción |
|---------|-------------|
| WorkspaceView | Vista del área de trabajo del usuario sobre expedientes |
| ExpedienteForm | Formulario de creación/edición de expedientes y carga de archivos |
| TrayIcon | Bandeja visual de expedientes por oficina |
| DashboardView | Panel principal según rol del usuario |
| NotificationToast | Notificaciones visuales de eventos del sistema |

---

## 4. Requisitos Funcionales

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| DW-01 | Mostrar formulario de inicio de sesión y redirigir a Keycloak | Alta |
| DW-02 | Cerrar sesión y redirigir al login | Alta |
| DW-03 | Dashboard principal según rol del usuario | Alta |
| DW-04 | Formulario de creación de expedientes con campos de metadata y carga de archivos | Alta |
| DW-05 | Drag & drop de archivos PDF y DOCX a un expediente | Media |
| DW-06 | Validación de formato (solo PDF/DOCX) en frontend | Baja |
| DW-07 | Bandeja de expedientes filtrada por oficina | Alta |
| DW-08 | Buscador por nombre/asunto con resultados en tiempo real | Baja |
| DW-09 | Visualización de estados de expediente | Media |
| DW-10 | Botones de acción según rol (cambiar estado, aprobar, etc.) | Alta |
| DW-11 | Formulario de justificación al observar un expediente | Alta |
| DW-12 | Previsualización de documentos PDF en el navegador | Media |
| DW-13 | Panel de administración de usuarios (solo Super Usuario) | Alta |
| DW-14 | Notificaciones visuales de expedientes faltantes | Media |
| DW-15 | Indicador de sesión (tiempo restante, botón de cerrar sesión) | Media |

---

## 5. Reglas de Negocio

Ver `business-rules.md` del mismo subdominio.
