# Reglas de Negocio — Subdominio Keycloak IAM

> **Servicio:** keycloak (Docker Compose)  
> **Subdominio:** Keycloak IAM (Generic)  

---

## 1. Reglas de Autenticación

| # | Regla | Descripción |
|---|-------|-------------|
| BR-KC-01 | Único proveedor | Keycloak es el único proveedor de identidad del sistema. No hay autenticación alternativa. |
| BR-KC-02 | Flujo estándar | Se utiliza OAuth 2.0 / OpenID Connect con flujo de Authorization Code + PKCE. |
| BR-KC-03 | Token JWT | Los tokens emitidos deben ser JWT firmados con RS256. |
| BR-KC-04 | Tiempo de vida | Access token: 5 minutos. Refresh token: 30 minutos. |
| BR-KC-05 | Rotación de refresh | Cada vez que se usa un refresh token, se emite uno nuevo. |

---

## 2. Reglas de Roles

| # | Regla | Descripción |
|---|-------|-------------|
| BR-KC-06 | Roles planos | Los roles en Keycloak son planos (sin jerarquía). La jerarquía se gestiona en quarkus-app. |
| BR-KC-07 | Roles del sistema | Roles definidos: `super-admin`, `jefe-oficina`, `inspector`, `residente`, `tecnico`. |
| BR-KC-08 | Un rol por usuario | Cada usuario tiene exactamente un rol. |
| BR-KC-09 | Roles en token | Los roles deben incluirse como claim `realm_access.roles` en el JWT. |

---

## 3. Reglas de Seguridad

| # | Regla | Descripción |
|---|-------|-------------|
| BR-KC-10 | Política de contraseñas | Mínimo 8 caracteres, al menos 1 mayúscula, 1 número y 1 carácter especial. |
| BR-KC-11 | Bloqueo por intentos | Bloquear cuenta tras 5 intentos fallidos de inicio de sesión. |
| BR-KC-12 | Desbloqueo automático | La cuenta se desbloquea automáticamente tras 15 minutos. |
| BR-KC-13 | Sesión simultánea | Un usuario puede tener máximo 1 sesión activa simultánea. |
| BR-KC-14 | Sin exposición pública | La consola de administración de Keycloak no debe exponerse a internet. |

---

## 4. Reglas de Integración

| # | Regla | Descripción |
|---|-------|-------------|
| BR-KC-15 | Comunicación exclusiva | Keycloak solo se comunica con Identity Wrapper de quarkus-app. Ningún otro servicio habla directamente con Keycloak. |
| BR-KC-16 | Realm dedicado | El SGE utiliza un realm específico (no el realm master). |
| BR-KC-17 | Client ID fijo | La aplicación SGE se registra como un cliente confidencial en Keycloak con Client ID `sgd-app`. |
