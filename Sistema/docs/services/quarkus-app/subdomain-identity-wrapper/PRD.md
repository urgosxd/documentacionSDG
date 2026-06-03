# PRD — Subdominio Identity Wrapper

> **Servicio:** quarkus-app  
> **Subdominio:** Identity Wrapper (Support)  
> **Versión:** 1.0.0  

---

## 1. Propósito

Actuar como capa intermedia entre el SGD y el proveedor de identidad Keycloak, encapsulando toda la comunicación de autenticación y autorización. Evita el acoplamiento directo del core del sistema con servicios externos de IAM.

---

## 2. Responsabilidades

- Comunicación con Keycloak para autenticación.
- Adaptación de respuestas externas a modelos internos.
- Encapsulamiento de APIs de identidad.
- Traducción de modelos externos a modelos de dominio.
- Validación y mapeo de tokens JWT.
- Gestión de sesiones (inicio, cierre, timeout).
- Desacoplamiento de infraestructura IAM.

---

## 3. Entidades Principales

| Entidad | Descripción |
|---------|-------------|
| IdentityService | Interfaz de dominio para operaciones de identidad |
| KeycloakAdapter | Implementación concreta que comunica con Keycloak |
| AuthProvider | Abstracción del proveedor de autenticación |
| TokenMapper | Traduce tokens JWT a objetos de dominio |
| UserMapper | Traduce objetos de usuario de Keycloak a dominio |
| Session | Representa una sesión activa de usuario |

---

## 4. Requisitos Funcionales

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| IW-01 | Validar credenciales de usuario contra Keycloak | Alta |
| IW-02 | Iniciar sesión y obtener JWT | Alta |
| IW-03 | Cerrar sesión e invalidar token | Alta |
| IW-04 | Validar token JWT en cada request | Alta |
| IW-05 | Extraer roles del token JWT | Alta |
| IW-06 | Refrescar token expirado | Alta |
| IW-07 | Sincronizar usuarios desde/hacia Keycloak | Media |
| IW-08 | Mapear roles de Keycloak a roles del sistema | Alta |
| IW-09 | Detectar y cerrar sesión por inactividad (5 min) | Alta |

---

## 5. Reglas de Negocio

Ver `business-rules.md` del mismo subdominio.
