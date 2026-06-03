## Context
Esta documentacion es para publico no tecnico, sera usado para lo construccion de la documentacion general no tecnica

# Data

## Objetivo del documento

Este documento describe de forma general el Sistema de Gestión Documentaria (SGD), estableciendo su propósito, alcance, términos clave y los roles involucrados. Sirve como punto de partida para entender el sistema antes de entrar al detalle técnico.

## Alcanze del sistema
En esta parte para poder alcanzar un mvp (Minimo Producto Viable), esojemos todo lo que se SI Incluye el MVP y no que NO inluye el mvp
### Lo que incluye el mvp:
- Ingresar al sistema con usuario y contraseña según rol asignado.
- Subir documentos en formato PDF y DOCX con nomenclatura generada automáticamente y metadata.
- Consultar los documentos de la propia oficina.
- Gestionar usuarios y sus accesos.
- Inspección de documentos para los administradores del sistema
- Búsqueda por contenido de documentos.
- Estados de los Documentos
- Notificaciones por documentos faltantes.
### Lo que no incluye el mvp:
- Reportes y exportaciones de documentos.
- OCR.
- Salir a producción.
- BackUps
- Testing intensivo

## Definicion y Glosario

Tener en cuenta que estas definiciones estan incluidas solamente para el sitema MVP

| Termino               | Significado                                                                                                                  |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| SGD                   | Sistema de Gestión de Documentos                                                                                             |
| MVP                   | Minimo Producto Viable                                                                                                       |
| PDF y DOCx            | Unicos formatos admitidos para el sistema                                                                                    |
| Nomenclatura          | El sistema genera automáticamente la metadata solo el usuario solo llena los datos necesarios.                               |
| Estados del Documento | Lo unicos estados en el sistema: **Borrador, Enviado, Observado, Subsanado, Aprobado, Archivado.**                           |
| Dashboard             | Pantalla principal segun usuario                                                                                             |
| Rol                   | Roles unicos y existentes en el Sistema: **Super Usuario, Jede deOficina, Inspector, Residente y Tecnico y/o Administrador** |
| keycloak              | Servico Externo que maneja roles y login                                                                                     |
| SeaweedFS             | Servicio Externo que guarda archivos PDF y DOCx                                                                              |
| Metadata              | Datos sobre los datos en los archivos PDF y DOCx                                                                             |
| Dominios DDD          | Disenio Orientado a Dominios                                                                                                 |

## Disenio Orientado a dominios

En esta sección listamos los subdominios más importantes para nuestro Sistema SGD, en donde el cuadro grande llamado **Enterprise Document Management** representa nuestro dominio principal del negocio, mientras que los cuadros pequeños llamados **subdominios** representan nuestra lógica de dependencias y separación de responsabilidades dentro del sistema.

### SubDominios Principales

### Gestor de Documentos Core SubDomain

#### Responsabilidades

* Registro de documentos
* Gestión de expedientes
* Versionado documental
* Estados documentales

#### Entidades principales

* Documento
* Expediente
* VersionDocumento
* HistorialDocumento
* FlujoDocumental

---

### Usuarios Core SubDomain

Este subdominio se encarga de administrar la estructura organizacional y los usuarios internos del sistema SGD.

#### Responsabilidades

* Gestión de usuarios
* Gestión de oficinas
* Gestión de áreas
* Gestión de cargos
* Asignación de responsables
* Jerarquías organizacionales
* Asociación de usuarios a oficinas

#### Entidades principales

* Usuario
* Oficina
* Cargo
* Area
* Responsable

---

### Almacenamiento Supporting SubDomain (SeaweedFS)

Este subdominio se encarga del almacenamiento físico de archivos y evidencias documentales dentro del sistema.

#### Responsabilidades

* Almacenamiento de archivos
* Recuperación de documentos
* Gestión de archivos adjuntos
* Almacenamiento de evidencias
* Control de acceso a archivos
* Versionado físico de documentos

#### Entidades principales

* Archivo
* RecursoDigital
* Evidencia
* DocumentoAdjunto
* StorageReference

---

### Identity Access Supporting SubDomain (Keycloak)

Este subdominio administra la autenticación y autorización de usuarios dentro del SGD.

#### Responsabilidades

* Inicio de sesión
* Autenticación
* Autorización
* Gestión de tokens JWT
* Gestión de sesiones
* Gestión de permisos

#### Entidades principales

* UsuarioAuth
* Rol
* Permiso
* Token
* Session
* Credential

---

### Wrapper Keycloak Generic SubDomain

Este subdominio funciona como una capa intermedia entre el SGD y el proveedor de identidad Keycloak, evitando el acoplamiento directo con servicios externos.

#### Responsabilidades

* Comunicación con Keycloak
* Adaptación de respuestas externas
* Encapsulamiento de APIs
* Traducción de modelos externos
* Desacoplamiento de infraestructura IAM

#### Entidades principales

* IdentityService
* KeycloakAdapter
* AuthProvider
* TokenMapper
* UserMapper

---

### Workflow / Process Manager SubDomain

Este subdominio coordina y administra los flujos documentales y procesos internos del SGD.

#### Responsabilidades

* Gestión de flujos documentales
* Orquestación de procesos
* Gestión de estados
* Aprobaciones documentales
* Derivaciones
* Archivado documental
* Automatización de procesos
* Manejo de procesos asíncronos

#### Entidades principales

* Workflow
* ProcessManager
* Saga
* EstadoDocumento
* Transicion
* Aprobacion
* Derivacion
* ProcesoDocumental

---

### Diagrama

```text id="9t2mke"
+------------------------------------------------------+
|        Enterprise Document Management                |
+------------------------------------------------------+
|                                                      |
|  +----------------------+                            |
|  | Gestor Documental    | ← Core Domain              |
|  +----------------------+                            |
|                                                      |
|  +----------------------+                            |
|  | Usuarios             | ← Supporting SubDomain     |
|  +----------------------+                            |
|                                                      |
|  +----------------------+                            |
|  | Almacenamiento       | ← Supporting SubDomain     |
|  +----------------------+                            |
|                                                      |
|  +----------------------+                            |
|  | Workflow             | ← Supporting SubDomain     |
|  +----------------------+                            |
|                                                      |
|  +----------------------+                            |
|  | Identity Access      | ← Generic SubDomain        |
|  +----------------------+                            |
|                                                      |
|  +----------------------+                            |
|  | Wrapper Keycloak     | ← Generic SubDomain        |
|  +----------------------+                            |
|                                                      |
+------------------------------------------------------+
```


## Roles y Permisos

En roles y permisos usaremos la Asignacion  RBAC para nuestro Sistema SGD.
## Oficinas y grupos de asignación

Las oficinas del SGD se organizan en tres grupos de asignación según su estructura operativa.

---

### Grupo 1: Jefe de Oficina y Técnico

Este grupo aplica a las oficinas que trabajan con un Jefe de Oficina y un Técnico.

#### Oficinas

* Unidad Formuladora (UF)
* Oficina de Estudios y Proyectos (OEP)
* Liquidaciones (LQ)

---

### Grupo 2: Jefe de Oficina, Residente y Técnico/Administrativo

Este grupo aplica a oficinas que requieren gestión de residencia o apoyo administrativo adicional.

#### Oficinas

* OAD
* Residencia (RES)

---

### Grupo 3: Jefe de Oficina e Inspector

Este grupo aplica a oficinas donde el control principal se realiza mediante inspección o supervisión.

#### Oficinas

* Inspección y/o Supervisión (INS)

---

## Roles RBAC del Sistema

### Super Usuario

El Super Usuario tiene acceso global al SGD.

#### Permisos

* Ver documentos propios
* Ver documentos de todas las oficinas
* Subir documentos PDF
* Cambiar estado de documentos
* Registrar justificaciones
* Alertar documentos faltantes
* Crear usuarios
* Asignar roles y oficinas
* Desactivar usuarios
* Restablecer contraseñas

#### Restricciones

* Ninguna restricción funcional dentro del SGD

---

### Administrador

El Administrador tiene acceso administrativo de consulta y registro.

### Permisos

* Ver documentos propios
* Ver documentos de todas las oficinas
* Subir documentos PDF

### Restricciones

* No cambiar estados
* No registrar justificaciones
* No alertar documentos faltantes
* No crear usuarios
* No asignar roles
* No desactivar usuarios
* No restablecer contraseñas

---

### Jefe de Oficina

El Jefe de Oficina es responsable operativo de los documentos de su oficina.

#### Permisos

* Ver documentos propios
* Ver documentos de su oficina
* Subir documentos PDF
* Cambiar estados
* Aprobar documentos
* Registrar justificaciones
* Alertar documentos faltantes

#### Restricciones

* No acceder a documentos de otras oficinas

---

### Técnico

El Técnico trabaja dentro del alcance de su oficina.

#### Permisos

* Ver documentos propios
* Ver documentos de su oficina
* Subir documentos PDF
* Alertar documentos faltantes

#### Restricciones

* No cambiar estados
* No registrar justificaciones
* No administrar usuarios
* No asignar roles
* No desactivar usuarios
* No restablecer contraseñas

#### Aplicación

* Unidad Formuladora (UF)
* Oficina de Estudios y Proyectos (OEP)
* Liquidaciones (LQ)

---

### Residente

El Residente aplica en OAD y Residencia.

#### Permisos

* Ver documentos propios
* Ver documentos de su oficina
* Subir documentos PDF
* Gestionar documentos asignados

#### Restricciones

* No administrar usuarios
* No acceder globalmente a documentos

---

### Técnico/Administrativo

El Técnico/Administrativo aplica en OAD y Residencia.

#### Permisos

* Ver documentos propios
* Ver documentos de su oficina

* Subir documentos PDF
* Registrar información administrativa

#### Restricciones

* No aprobar documentos
* No cambiar estados finales
* No administrar usuarios
* No consultar documentos fuera de su oficina

---

### Inspector

El Inspector aplica en Inspección y/o Supervisión.

#### Permisos

* Ver documentos propios
* Ver documentos de su oficina
* Subir reportes
* Registrar hallazgos

#### Restricciones

* No administrar usuarios
* No acceder globalmente a documentos

---

### Reglas de Permisos por Acción

| Acción                               | Roles Permitidos                                                 |
| ------------------------------------ | ---------------------------------------------------------------- |
| Ver documentos propios               | Todos los roles                                                  |
| Ver documentos de todas las oficinas | Super Usuario, Administrador                                     |
| Ver documentos de la propia oficina  | Jefe de Oficina y roles operativos                               |
| Subir documentos PDF                 | Super Usuario, Administrador, Jefe de Oficina y roles operativos |
| Cambiar estado de documento          | Super Usuario, Jefe de Oficina                                   |
| Registrar justificación              | Super Usuario, Jefe de Oficina                                   |
| Alertar documentos faltantes         | Super Usuario, Jefe de Oficina y roles operativos                |
| Crear usuarios                       | Super Usuario                                                    |
| Asignar roles y oficinas             | Super Usuario                                                    |
| Desactivar usuarios                  | Super Usuario                                                    |
| Restablecer contraseñas              | Super Usuario                                                    |

---

### Reglas de Asignación RBAC

1. Cada usuario debe tener una oficina asignada, excepto Super Usuario y Administrador global.
2. El rol Jefe de Oficina debe existir por oficina.
3. Los roles operativos deben limitarse a su oficina o expedientes asignados.
4. Los usuarios no deben recibir permisos individuales fuera de su rol.
5. La administración de usuarios debe concentrarse en el Super Usuario.
6. El cambio de estado y registro de justificaciones debe reservarse para Super Usuario y Jefe de Oficina.
7. El acceso global debe ser excepcional.
8. Cuando un usuario cambie de oficina o cargo, debe retirarse primero el rol anterior.

---

### Asignación Recomendada por Oficina

#### Unidad Formuladora (UF)

| Rol             | Función                                                               |
| --------------- | --------------------------------------------------------------------- |
| Jefe de Oficina | Supervisión, aprobación, cambios de estado, justificaciones y alertas |
| Técnico         | Carga documental y gestión operativa                                  |

---

#### Oficina de Estudios y Proyectos (OEP)

| Rol             | Función                                                               |
| --------------- | --------------------------------------------------------------------- |
| Jefe de Oficina | Supervisión, aprobación, cambios de estado, justificaciones y alertas |
| Técnico         | Carga documental y gestión operativa                                  |

---

#### Liquidaciones (LQ)

| Rol             | Función                                                               |
| --------------- | --------------------------------------------------------------------- |
| Jefe de Oficina | Supervisión, aprobación, cambios de estado, justificaciones y alertas |
| Técnico         | Carga documental y gestión operativa                                  |

---

#### OAD

| Rol                    | Usuario       | Función                  |
| ---------------------- | ------------- | ------------------------ |
| Jefe de Oficina        | jefe_oad      | Supervisión y aprobación |
| Residente              | residente_oad | Gestión documental       |
| Técnico/Administrativo | admin_oad     | Registro administrativo  |

---

#### Residencia (RES)

| Rol                    | Usuario       | Función                  |
| ---------------------- | ------------- | ------------------------ |
| Jefe de Oficina        | jefe_res      | Supervisión y aprobación |
| Residente              | residente_res | Gestión documental       |
| Técnico/Administrativo | admin_res     | Registro administrativo  |

---

#### Inspección y/o Supervisión (INS)

| Rol             | Usuario       | Función                  |
| --------------- | ------------- | ------------------------ |
| Jefe de Oficina | jefe_ins      | Supervisión y aprobación |
| Inspector       | inspector_ins | Inspección y reportes    |

---

### Criterios de Control

* Separar permisos globales, permisos de oficina y permisos operativos.
* Evitar que roles operativos administren usuarios.
* Evitar que el Administrador cambie estados o registre justificaciones.
* Registrar toda asignación de rol con usuario, oficina y responsable.
* Revisar periódicamente que los usuarios sigan perteneciendo a su oficina y rol asignado.



## Requisitos funcionales y No Funcionales
### Requisitos Funcionales

Los requisitos funcionales describen las funcionalidades que debe realizar el Sistema de Gestión Documentaria (SGD) dentro del alcance del MVP.

| ID    | Módulo                     | Descripción                                                                                                                       | Prioridad |
| ----- | -------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------- |
| RF-01 | Autenticación y Roles      | El sistema permitirá a los usuarios ingresar con un nombre de usuario y contraseña.                                               | Alta      |
| RF-02 | Autenticación y Roles      | El sistema contará con cinco roles de usuario: Super Usuario, Jefe de Oficina, Inspector, Residente y Técnico y/o Administrativo. | Alta      |
| RF-03 | Autenticación y Roles      | Cada rol tendrá acceso únicamente a las funciones y documentos que le correspondan.                                               | Alta      |
| RF-04 | Autenticación y Roles      | El sistema cerrará la sesión automáticamente tras un periodo de inactividad de 5 minutos.                                         | Alta      |
| RF-05 | Administración de Usuarios | Solamente el Super Usuario podrá crear, editar y desactivar cuentas de usuario.                                                   | Alta      |
| RF-06 | Administración de Usuarios | Al crear un usuario se registrará: nombre completo, nombre de usuario, contraseña, rol y oficina asignada.                        | Alta      |
| RF-07 | Administración de Usuarios | El Administrador podrá restablecer la contraseña de cualquier usuario mediante correo electrónico.                                | Media     |
| RF-08 | Administración de Usuarios | El listado de usuarios contará con un buscador y controles de paginación.                                                         | Baja      |
| RF-09 | Carga de Documentos        | Los usuarios podrán subir documentos solamente en formato PDF y DOCX para el MVP.                                                 | Alta      |
| RF-10 | Carga de Documentos        | Al subir un documento se presentará un formulario y el sistema generará automáticamente la metadata.                              | Alta      |
| RF-11 | Carga de Documentos        | El sistema generará automáticamente un nombre único para cada documento subido mediante hash-checksum.                            | Baja      |
| RF-12 | Carga de Documentos        | No se exigirá un orden de carga en el MVP; los usuarios podrán subir documentos en cualquier momento.                             | Alta      |
| RF-13 | Carga de Documentos        | El sistema rechazará formatos distintos a PDF y DOCX como JPG, PNG u otros.                                                       | Baja      |
| RF-14 | Bandeja por Oficina        | Cada usuario verá únicamente los documentos de su propia oficina según jerarquía.                                                 | Alta      |
| RF-15 | Bandeja por Oficina        | El Super Usuario podrá visualizar y alertar documentos faltantes de todas las oficinas.                                           | Alta      |
| RF-16 | Bandeja por Oficina        | La bandeja mostrará el estado actual de cada documento: Borrador, Enviado, Observado, Subsanado, Aprobado y Archivado.            | Media     |
| RF-17 | Bandeja por Oficina        | El Jefe de Oficina y el Super Usuario podrán cambiar el estado de los documentos.                                                 | Alta      |
| RF-18 | Bandeja por Oficina        | La bandeja contará con buscador por nombre de documento o asunto, además de paginación.                                           | Baja      |

---

### Requisitos No Funcionales

Los requisitos no funcionales establecen las condiciones de calidad que el sistema debe cumplir: seguridad, rendimiento, disponibilidad, usabilidad y mantenibilidad.

| ID     | Categoría      | Descripción                                                                                                                          | Criterio de Aceptación                                           |
| ------ | -------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------- |
| RNF-01 | Seguridad      | Las contraseñas de los usuarios deben almacenarse de forma segura utilizando la seguridad de Keycloak.                               | Cumplir requisitos mínimos de creación de contraseñas.           |
| RNF-02 | Seguridad      | Solo los usuarios con sesión activa y rol correspondiente podrán acceder a cada sección del sistema.                                 | Acceso denegado a usuarios sin permisos.                         |
| RNF-04 | Rendimiento    | El sistema debe permitir subir archivos PDF y DOCX de tamaño entre 0 MB y 4 GB.                                                      | Carga exitosa de archivos estándar PDF y DOCX.                   |
| RNF-05 | Disponibilidad | El sistema debe estar disponible todos los días hábiles.                                                                             | Sin interrupciones planificadas en horario laboral.              |
| RNF-06 | Usabilidad     | La interfaz debe ser sencilla e intuitiva para usuarios con conocimientos básicos de computación.                                    | Un usuario nuevo puede subir documentos con mínima capacitación. |
| RNF-07 | Usabilidad     | El sistema debe funcionar correctamente en el navegador Google Chrome.                                                               | Compatibilidad completa con Chrome.                              |
| RNF-08 | Mantenibilidad | El código debe estar organizado de manera que permita realizar correcciones y mejoras sin complicaciones.                            | Cambios menores implementables sin afectar otras funciones.      |
| RNF-09 | Escalabilidad  | El sistema debe permitir incorporar nuevas oficinas o usuarios sin cambios estructurales.                                            | Alta de nuevas oficinas o usuarios sin modificar código.         |
| RNF-10 | Almacenamiento | Los archivos PDF deben almacenarse de forma ordenada en el servidor, registrando en base de datos únicamente sus datos descriptivos. | Archivos accesibles y localizables en cualquier momento.         |

---

### Alcance del MVP

Esta versión inicial contempla únicamente los módulos esenciales para el funcionamiento básico del sistema:

* Autenticación y roles
* Administración de usuarios
* Carga de documentos PDF y DOCX
* Bandeja de documentos por oficina

El detalle técnico será definido en iteraciones posteriores.


