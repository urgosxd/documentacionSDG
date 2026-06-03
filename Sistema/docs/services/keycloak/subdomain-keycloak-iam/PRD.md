# PRD — Subdominio Keycloak IAM

> **Servicio:** keycloak (Docker Compose)  
> **Subdominio:** Keycloak IAM (Generic)  
> **Versión:** 1.0.0  

---

## 1. Propósito

Proveer el servicio de Identity and Access Management (IAM) para todo el SGD. Keycloak es el proveedor externo centralizado que gestiona autenticación, autorización y roles de todos los usuarios del sistema.

---

## 2. Responsabilidades

- Inicio de sesión (autenticación de usuarios).
- Gestión de tokens JWT (emisión, validación, refresco).
- Gestión de sesiones de usuario.
- Gestión de roles y permisos (RBAC).
- Integración con Identity Wrapper de quarkus-app.

---

## 3. Entidades Principales

| Entidad | Descripción |
|---------|-------------|
| UsuarioAuth | Usuario registrado en Keycloak con credenciales |
| Rol | Rol definido en Keycloak (super-admin, jefe-oficina, inspector, residente, tecnico) |
| Permiso | Permiso asociado a un rol |
| Token | JWT emitido por Keycloak con claims de usuario y roles |
| Session | Sesión activa de un usuario en Keycloak |
| Credential | Credencial de acceso (contraseña) del usuario |

---

## 4. Requisitos Funcionales

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| KC-01 | Autenticar usuario con username y contraseña | Alta |
| KC-02 | Emitir JWT con roles del usuario | Alta |
| KC-03 | Validar token JWT | Alta |
| KC-04 | Refrescar token expirado | Alta |
| KC-05 | Gestionar sesiones de usuario (crear, listar, cerrar) | Alta |
| KC-06 | Crear usuarios en Keycloak desde el realm del SGD | Media |
| KC-07 | Asignar roles a usuarios | Media |
| KC-08 | Política de contraseñas (longitud mínima, complejidad) | Alta |
| KC-09 | Cierre de sesión (logout) | Alta |
| KC-10 | Integración con Identity Wrapper mediante API REST | Alta |

---

## 5. Reglas de Negocio

Ver `business-rules.md` del mismo subdominio.
