# Reglas de Negocio — Subdominio Document Workspace

> **Servicio:** payloadcms  
> **Subdominio:** Document Workspace (Support)  

---

## 1. Reglas Arquitectónicas

| # | Regla | Descripción |
|---|-------|-------------|
| BR-DW-01 | Sin lógica de negocio | Document Workspace NO contiene reglas de negocio del dominio. Toda operación crítica se delega a quarkus-app. |
| BR-DW-02 | BFF obligatorio | El frontend nunca se comunica directamente con quarkus-app. Siempre pasa por la capa BFF. |
| BR-DW-03 | Validación duplicada | Las validaciones de frontend son solo para experiencia de usuario. La validación real ocurre en quarkus-app. |
| BR-DW-04 | Sin acceso a storage | payloadcms nunca accede directamente a SeaweedFS. Los archivos se suben/descargan a través de quarkus-app. |
| BR-DW-05 | Sin máquina de estados | PayloadCMS NO maneja la máquina de estados de expedientes. Solo consulta/visualiza estados desde quarkus-app. |
| BR-DW-06 | Estados UI solo draft/published | PayloadCMS solo maneja estados draft/published para contenido de páginas/forms del CMS. |

---

## 2. Reglas de UI/UX

| # | Regla | Descripción |
|---|-------|-------------|
| BR-DW-05 | Interfaz intuitiva | La interfaz debe ser operable por usuarios con conocimientos básicos de computación. |
| BR-DW-06 | Chrome compatible | El sistema debe funcionar correctamente en Google Chrome (versión actual y anterior). |
| BR-DW-07 | Feedback visual | Toda acción del usuario debe tener feedback visual inmediato (loading, éxito, error). |
| BR-DW-08 | Roles visibles | La UI debe adaptarse al rol del usuario: mostrar/ocultar acciones según permisos. |
| BR-DW-09 | Sin información sensible en frontend | El frontend nunca expone tokens completos, contraseñas o datos internos del sistema. |

---

## 3. Reglas de Sesión

| # | Regla | Descripción |
|---|-------|-------------|
| BR-DW-10 | Redirección a login | Si el token JWT expira o es inválido, redirigir al login inmediatamente. |
| BR-DW-11 | Timeout visible | Mostrar advertencia al usuario cuando quede 1 minuto antes del cierre por inactividad. |
| BR-DW-12 | Logout explícito | El botón de cerrar sesión debe estar siempre visible cuando el usuario está autenticado. |

---

## 4. Reglas de Expedientes y Archivos

| # | Regla | Descripción |
|---|-------|-------------|
| BR-DW-13 | Expediente como unidad visual | La interfaz siempre presenta el expediente como unidad de trabajo. Los archivos se muestran como adjuntos del expediente. |
| BR-DW-14 | Metadata a nivel expediente | El usuario completa campos descriptivos del expediente (asunto, tipo documental, observaciones). La metadata técnica de los archivos es generada por quarkus-app. |
| BR-DW-15 | Confirmación de subida | Mostrar confirmación visual cuando un archivo se sube exitosamente a un expediente. |
| BR-DW-16 | Error claro | Los errores de subida de archivos deben mostrar mensajes legibles para el usuario, no códigos técnicos. |
