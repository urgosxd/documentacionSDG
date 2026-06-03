# PRD — Subdominio Workflow / Process Manager

> **Servicio:** inngest  
> **Subdominio:** Workflow / Process Manager (Support)  
> **Versión:** 1.0.0  

---

## 1. Propósito

Coordinar y administrar los flujos documentales y procesos asíncronos del SGD. Gestiona las transiciones de estado, aprobaciones, derivaciones, notificaciones y cualquier proceso que requiera orquestación temporal o asíncrona.

---

## 2. Responsabilidades

- Gestión de flujos de expediente (workflows).
- Orquestación de procesos asíncronos.
- Gestión de la máquina de estados de expedientes.
- Aprobaciones y derivaciones de expedientes.
- Archivado automático de expedientes.
- Automatización de procesos (timeouts, recordatorios).
- Manejo de procesos asíncronos (notificaciones, alertas).
- Disparo de eventos de dominio.

---

## 3. Entidades Principales

| Entidad | Descripción |
|---------|-------------|
| Workflow | Definición de un flujo de trabajo (estados, transiciones, reglas) |
| ProcessManager | Orquestador de procesos asíncronos |
| Saga | Coordinación de transacciones distribuidas |
| EstadoExpediente | Estado actual de un expediente en el flujo |
| Transicion | Movimiento permitido entre estados |
| Aprobacion | Registro de una aprobación sobre un expediente |
| Derivacion | Redirección de un expediente a otra oficina/usuario |
| ProcesoDocumental | Instancia de un proceso documental en ejecución |
| Notificacion | Evento de notificación disparado por el workflow |
| AlertaExpedienteFaltante | Alerta cuando un expediente está incompleto |

---

## 4. Requisitos Funcionales

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| WF-01 | Ejecutar transiciones de estado según máquina de estados definida | Alta |
| WF-02 | Disparar notificación cuando un expediente cambia de estado | Alta |
| WF-03 | Enviar alerta de expedientes faltantes a Jefe de Oficina | Alta |
| WF-04 | Registrar aprobación/rechazo de expedientes | Alta |
| WF-05 | Derivar expediente a otra oficina o usuario | Media |
| WF-06 | Ejecutar archivado automático tras aprobación | Media |
| WF-07 | Timeout para expedientes en estado "Observado" sin subsanar | Media |
| WF-08 | Recordatorio periódico de expedientes pendientes | Baja |
| WF-09 | Historial de eventos de workflow por expediente | Media |
| WF-10 | Notificar al usuario cuando un expediente requiere su acción | Alta |

---

## 5. Reglas de Negocio

Ver `business-rules.md` del mismo subdominio.
