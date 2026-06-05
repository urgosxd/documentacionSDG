# Reglas de Negocio — Subdominio Users

> **Servicio:** quarkus-app  
> **Subdominio:** Users (Core)  

---

## 1. Reglas de Usuarios

| # | Regla | Descripción |
|---|-------|-------------|
| BR-USR-01 | Usuario único | El nombre de usuario (username) debe ser único en el sistema. |
| BR-USR-02 | Oficina obligatoria | Todo usuario debe tener una oficina asignada, excepto Super Usuario. |
| BR-USR-03 | Baja lógica | La desactivación de un usuario es una baja lógica (isActivo=false). No se eliminan registros. |
| BR-USR-04 | Datos mínimos | Crear un usuario requiere: nombre completo, username, contraseña, rol y oficina. |
| BR-USR-05 | Email opcional en alta | El email es obligatorio si se requiere restablecimiento de contraseña. |
| BR-USR-06 | Usuario inactivo no accede | Un usuario desactivado no puede iniciar sesión ni acceder a recursos. |

---

## 2. Reglas de Roles

| # | Regla | Descripción |
|---|-------|-------------|
| BR-USR-07 | Roles predefinidos | Los roles del sistema son fijos: Super Usuario, Jefe de Oficina, Inspector, Residente, Técnico/Administrativo. |
| BR-USR-08 | Sin roles personalizados | En el MVP no se permite la creación de roles personalizados. |
| BR-USR-09 | Un rol por usuario | Cada usuario tiene exactamente un rol. |
| BR-USR-10 | Rotación controlada | Al cambiar de rol/oficina, primero debe retirarse la asignación anterior. |

---

## 3. Reglas de Oficinas

| # | Regla | Descripción |
|---|-------|-------------|
| BR-USR-11 | Código único | Cada oficina tiene un código único (UF, OEP, LQ, OAD, RES, INS). |
| BR-USR-12 | Jefe por oficina | Cada oficina debe tener al menos un usuario con rol Jefe de Oficina. |
| BR-USR-13 | Grupo definido | Cada oficina pertenece a un grupo que determina su composición de roles. |
| BR-USR-14 | Extensible | El sistema permite agregar nuevas oficinas sin cambiar código. |

---

## 4. Reglas de Trazabilidad

| # | Regla | Descripción |
|---|-------|-------------|
| BR-USR-15 | Registro de asignación | Toda asignación de rol/oficina se registra con: usuario, oficina, responsable, timestamp. |
| BR-USR-16 | Historial de cambios | Los cambios de rol y oficina quedan registrados en el historial del usuario. |
| BR-USR-17 | Revisión periódica | Debe poder consultarse la lista de usuarios activos por oficina para auditoría. |
