# Reglas de Negocio — Subdominio Identity Wrapper

> **Servicio:** quarkus-app  
> **Subdominio:** Identity Wrapper (Support)  

---

## 1. Reglas de Autenticación

| # | Regla | Descripción |
|---|-------|-------------|
| BR-IW-01 | Keycloak como única fuente | Toda autenticación debe delegarse a Keycloak. El sistema no almacena ni valida contraseñas. |
| BR-IW-02 | Sesión por JWT | La sesión se mantiene mediante un token JWT emitido por Keycloak. |
| BR-IW-03 | Timeout por inactividad | La sesión expira tras 5 minutos de inactividad. El wrapper debe detectar y cerrar la sesión. |
| BR-IW-04 | Token obligatorio | Toda request a quarkus-app debe incluir un token JWT válido en el header `Authorization: Bearer <token>`. |
| BR-IW-05 | Sin token = denegado | Las requests sin token o con token inválido reciben HTTP 401. |

---

## 2. Reglas de Autorización

| # | Regla | Descripción |
|---|-------|-------------|
| BR-IW-06 | Rol en token | El rol del usuario debe estar contenido dentro del token JWT como claim. |
| BR-IW-07 | Validación por request | Cada request valida que el rol del token tenga permiso para la acción solicitada. |
| BR-IW-08 | Mapeo de roles | Los roles de Keycloak se mapean a los roles del sistema: super-admin, jefe-oficina, inspector, residente, tecnico. |
| BR-IW-09 | Sin override | El wrapper nunca modifica ni asigna permisos — solo consulta y traduce lo que Keycloak define. |

---

## 3. Reglas de Integración

| # | Regla | Descripción |
|---|-------|-------------|
| BR-IW-10 | Desacoplamiento | Ningún otro subdominio de quarkus-app se comunica directamente con Keycloak. Todo pasa por Identity Wrapper. |
| BR-IW-11 | Resiliencia | Si Keycloak no responde, el wrapper debe retornar un error controlado (HTTP 503) sin exponer detalles internos. |
| BR-IW-12 | Traducción de errores | Los errores de Keycloak se traducen a errores de dominio antes de propagarse. |
| BR-IW-13 | Caché de tokens | Los tokens públicos de Keycloak (JWKS) pueden cachearse para evitar llamadas repetidas. |
