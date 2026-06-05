# Reglas de Negocio — Subdominio Workflow / Process Manager

> **Servicio:** inngest  
> **Subdominio:** Workflow / Process Manager (Support)  

---

## 1. Reglas de Transiciones

| # | Regla | Descripción |
|---|-------|-------------|
| BR-WF-01 | Transiciones válidas | Solo se ejecutan transiciones definidas en la máquina de estados. Transiciones no definidas son rechazadas. |
| BR-WF-02 | Validación previa | Antes de ejecutar una transición, validar que el usuario tiene permiso para esa acción. |
| BR-WF-03 | Evento de dominio | Toda transición de estado dispara un evento de dominio que otros servicios pueden consumir. |
| BR-WF-04 | Transición atómica | La transición de estado debe ejecutarse de forma atómica: si falla, no debe quedar en estado inconsistente. |

---

## 2. Reglas de Aprobaciones

| # | Regla | Descripción |
|---|-------|-------------|
| BR-WF-05 | Aprobador válido | Solo el Jefe de Oficina o Super Usuario puede aprobar expedientes. |
| BR-WF-06 | Aprobación registrada | Toda aprobación registra: usuario, fecha, comentario opcional, expediente aprobado. |
| BR-WF-07 | Sin auto-aprobación | El usuario que creó el expediente no puede aprobarlo. |
| BR-WF-08 | Rechazo con justificación | El rechazo (transición a Observado) requiere justificación obligatoria. |

---

## 3. Reglas de Derivaciones

| # | Regla | Descripción |
|---|-------|-------------|
| BR-WF-09 | Derivación entre oficinas | Un expediente puede derivarse de una oficina a otra según el flujo definido. |
| BR-WF-10 | Trazabilidad de derivación | Toda derivación se registra: origen, destino, usuario, fecha, motivo. |
| BR-WF-11 | Notificación en derivación | Al derivar un expediente, notificar al Jefe de Oficina destino. |

---

## 4. Reglas de Alertas y Notificaciones

| # | Regla | Descripción |
|---|-------|-------------|
| BR-WF-12 | Alerta de faltantes | El sistema alerta cuando un expediente no tiene todos los archivos requeridos. |
| BR-WF-13 | Timeout de observación | Si un expediente permanece en Observado más de 7 días sin subsanar, notificar al Jefe de Oficina. |
| BR-WF-14 | Notificación asíncrona | Las notificaciones se envían de forma asíncrona — no deben bloquear la operación principal. |
| BR-WF-15 | Destinatario según acción | Las notificaciones se envían al usuario responsable según el tipo de evento. |

---

## 5. Reglas de Procesos Asíncronos

| # | Regla | Descripción |
|---|-------|-------------|
| BR-WF-16 | Sin bloqueo | Los procesos asíncronos nunca deben bloquear la respuesta al usuario. |
| BR-WF-17 | Idempotencia | Los procesos deben ser idempotentes: ejecutarlos múltiples veces produce el mismo resultado. |
| BR-WF-18 | Reintentos | Los procesos fallidos deben reintentarse con backoff exponencial (máx 3 intentos). |
| BR-WF-19 | Dead letter | Después de 3 reintentos fallidos, el evento va a una cola de "dead letter" para revisión manual. |
