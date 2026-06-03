# PRD — Subdominio Users

> **Servicio:** quarkus-app  
> **Subdominio:** Users (Core)  
> **Versión:** 1.0.0  

---

## 1. Propósito

Administrar la estructura organizacional y los usuarios internos del SGD. Este subdominio gestiona oficinas, áreas, cargos y la asignación de usuarios a la jerarquía organizacional.

---

## 2. Responsabilidades

- Gestión del ciclo de vida de usuarios (alta, baja, modificación).
- Gestión de oficinas.
- Gestión de áreas dentro de oficinas.
- Gestión de cargos.
- Asignación de responsables a oficinas.
- Jerarquías organizacionales.
- Asociación de usuarios a oficinas y roles.

---

## 3. Entidades Principales

| Entidad | Descripción | Atributos clave |
|---------|-------------|-----------------|
| Usuario | Persona que accede al sistema | id, nombreCompleto, username, email, rol, oficinaId, activo |
| Oficina | Unidad organizacional | id, nombre, codigo, grupo, jefeOficinaId |
| Area | Subdivisión dentro de una oficina | id, oficinaId, nombre, responsableId |
| Cargo | Posición jerárquica | id, nombre, nivel, permisosPorDefecto |
| Responsable | Asignación usuario-oficina | id, usuarioId, oficinaId, cargoId, fechaAsignacion |

---

## 4. Requisitos Funcionales

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| USR-01 | Crear usuario con nombre, username, contraseña, rol y oficina | Alta |
| USR-02 | Editar datos de usuario | Alta |
| USR-03 | Desactivar usuario (baja lógica) | Alta |
| USR-04 | Listar usuarios con buscador y paginación | Baja |
| USR-05 | Asignar rol a usuario | Alta |
| USR-06 | Asignar oficina a usuario | Alta |
| USR-07 | Restablecer contraseña de usuario vía email | Media |
| USR-08 | Registrar asignación de rol con trazabilidad (usuario, oficina, responsable, fecha) | Alta |
| USR-09 | Consultar historial de roles y oficinas de un usuario | Media |

---

## 5. Reglas de Negocio

Ver `business-rules.md` del mismo subdominio.
