# Reglas de Negocio — Sistema de Gestión de Expedientes (SGE)

> **Versión:** 1.0.0  
> **Ámbito:** Global del sistema — aplican a todos los servicios y subdominios  

---

## 1. Reglas de Asignación RBAC

| # | Regla | Descripción |
|---|-------|-------------|
| BR-SYS-01 | Asignación de oficina | Todo usuario debe tener una oficina asignada, excepto Super Usuario. |
| BR-SYS-02 | Jefe de Oficina obligatorio | Cada oficina debe tener al menos un usuario con rol Jefe de Oficina. |
| BR-SYS-03 | Límite operativo | Los roles operativos (Técnico, Residente, Inspector) solo acceden a expedientes de su oficina. |
| BR-SYS-04 | Sin permisos individuales | Los usuarios no reciben permisos fuera de su rol. |
| BR-SYS-05 | Admin centralizada | Solo el Super Usuario administra cuentas, roles y oficinas. |
| BR-SYS-06 | Cambio de estado restringido | Solo Super Usuario y Jefe de Oficina pueden cambiar estados y registrar justificaciones. |
| BR-SYS-07 | Acceso global excepcional | El acceso a documentos de todas las oficinas es excepcional (solo Super Usuario). |
| BR-SYS-08 | Rotación de roles | Cuando un usuario cambia de oficina o cargo, debe retirarse primero el rol anterior antes de asignar el nuevo. |
| BR-SYS-09 | Trazabilidad de asignación | Toda asignación de rol debe registrarse con: usuario, oficina, responsable de la asignación y timestamp. |
| BR-SYS-10 | Revisión periódica | Debe revisarse periódicamente que los usuarios sigan perteneciendo a su oficina y rol asignado. |

---

## 2. Reglas de Estados de Expediente

| # | Regla | Descripción |
|---|-------|-------------|
| BR-SYS-11 | Estados permitidos | Los únicos estados del sistema son: **Borrador**, **Enviado**, **Observado**, **Subsanado**, **Aprobado**, **Archivado**. |
| BR-SYS-12 | Transiciones válidas | Solo se permiten las transiciones definidas en la máquina de estados. |
| BR-SYS-13 | Prohibición de saltos | No se puede pasar de Borrador a Aprobado directamente — debe seguir la secuencia. |
| BR-SYS-14 | Estado final | Archivado es un estado terminal — no se permiten más transiciones desde Archivado. |
| BR-SYS-15 | Justificación obligatoria | Toda transición a Observado requiere una justificación escrita. |
| BR-SYS-16 | Trazabilidad de cambios | Todo cambio de estado debe registrar: usuario, fecha, estado anterior, estado nuevo, justificación (si aplica). |

### Máquina de Estados

```
┌──────────┐     Enviar     ┌─────────┐    Observar    ┌──────────┐
│ Borrador │ ────────────→  │ Enviado │ ─────────────→ │ Observado│
└──────────┘                └─────────┘                └──────────┘
                                │                           │
                                │ Aprobar                   │ Subsanar
                                ▼                           ▼
                          ┌──────────┐              ┌────────────┐
                          │ Aprobado │              │ Subsanado  │
                          └──────────┘              └──────┬─────┘
                                │                          │
                                │ Archivar                 │ Enviar
                                ▼                          ▼
                          ┌──────────┐              ┌─────────┐
                          │Archivado │              │ Enviado │ (reingresa)
                          └──────────┘              └─────────┘
```

---

## 3. Reglas de Permisos por Acción

| # | Acción | Roles Permitidos |
|---|--------|------------------|
| BR-SYS-17 | Ver expedientes propios | Todos los roles |
| BR-SYS-18 | Ver expedientes de todas las oficinas | Super Usuario |
| BR-SYS-19 | Ver expedientes de la propia oficina | Jefe de Oficina y roles operativos |
| BR-SYS-20 | Subir archivos PDF/DOCX a expedientes | Super Usuario, Jefe de Oficina, roles operativos |
| BR-SYS-21 | Cambiar estado de expediente | Super Usuario, Jefe de Oficina |
| BR-SYS-22 | Registrar justificación | Super Usuario, Jefe de Oficina |
| BR-SYS-23 | Alertar expedientes faltantes | Super Usuario, Jefe de Oficina, roles operativos |
| BR-SYS-24 | Crear usuarios | Super Usuario |
| BR-SYS-25 | Asignar roles y oficinas | Super Usuario |
| BR-SYS-26 | Desactivar usuarios | Super Usuario |
| BR-SYS-27 | Restablecer contraseñas | Super Usuario |

---

## 4. Reglas de Expedientes y Archivos

| # | Regla | Descripción |
|---|-------|-------------|
| BR-SYS-28 | Expediente como core | La metadata de negocio, estados y lógica pertenecen al expediente, no al archivo individual. |
| BR-SYS-29 | Archivos sin lógica propia | Los archivos adjuntos (documentos) no tienen estados, metadata de negocio ni workflow propio. Heredan del expediente. |
| BR-SYS-30 | Formatos admitidos | Solo PDF y DOCX son aceptados en el MVP. Cualquier otro formato es rechazado. |
| BR-SYS-31 | Nombre único por hash | Cada archivo recibe un identificador único basado en hash-checksum SHA-256 de su contenido. |
| BR-SYS-32 | Sin orden de carga | Los archivos pueden subirse en cualquier momento, sin orden predefinido dentro del expediente. |
| BR-SYS-33 | Límite de tamaño | Archivos entre 0 MB y 4 GB. |
| BR-SYS-34 | Almacenamiento separado | Los archivos físicos se almacenan en SeaweedFS; en base de datos solo se guardan metadatos técnicos y referencias. |

---

## 5. Reglas de Oficinas

| # | Regla | Descripción |
|---|-------|-------------|
| BR-SYS-35 | Grupos de oficina | Las oficinas se organizan en 3 grupos según su composición de roles. |
| BR-SYS-36 | Grupo 1 — UF, OEP, LQ | Roles: Jefe de Oficina + Técnico |
| BR-SYS-37 | Grupo 2 — OAD, RES | Roles: Jefe de Oficina + Residente + Técnico/Administrativo |
| BR-SYS-38 | Grupo 3 — INS | Roles: Jefe de Oficina + Inspector |
| BR-SYS-39 | Nueva oficina | Agregar una nueva oficina no requiere cambios en el código del sistema. |

---

## 6. Reglas de Seguridad y Sesión

| # | Regla | Descripción |
|---|-------|-------------|
| BR-SYS-40 | Sesión por inactividad | La sesión se cierra automáticamente tras 5 minutos de inactividad. |
| BR-SYS-41 | Contraseñas seguras | Las contraseñas se gestionan exclusivamente a través de Keycloak. El sistema no almacena contraseñas. |
| BR-SYS-42 | Acceso por rol | Todo acceso a recursos debe validar el rol del usuario contra la acción solicitada. |
| BR-SYS-43 | Separación de responsabilidades | Solo Super Usuario administra usuarios. Jefe de Oficina y Super Usuario cambian estados y registran justificaciones. |

---

## 7. Reglas de Integración entre Servicios

| # | Regla | Descripción |
|---|-------|-------------|
| BR-SYS-44 | Comunicación síncrona | payloadcms se comunica con quarkus-app vía REST/HTTP. |
| BR-SYS-45 | Comunicación asíncrona | quarkus-app delega procesos asíncronos a inngest mediante eventos. |
| BR-SYS-46 | Identidad centralizada | Todo servicio valida tokens JWT contra keycloak. quarkus-app wrappea la comunicación con keycloak. |
| BR-SYS-47 | Almacenamiento desacoplado | quarkus-app sube/recupera archivos de SeaweedFS. payloadcms nunca accede a SeaweedFS directamente. |
| BR-SYS-48 | Facade obligatorio | payloadcms no contiene lógica de negocio ni reglas de dominio. Toda operación crítica pasa por quarkus-app. |
