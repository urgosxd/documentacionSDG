# Reglas de Negocio — Subdominio Storage (SeaweedFS)

> **Servicio:** storage (Docker Compose — SeaweedFS)  
> **Subdominio:** Storage SeaweedFS (Generic)  

---

## 1. Reglas de Almacenamiento

| # | Regla | Descripción |
|---|-------|-------------|
| BR-ST-01 | Almacenamiento externo | SeaweedFS es el único sistema de almacenamiento físico. No se guardan archivos en el sistema de archivos local de quarkus-app. |
| BR-ST-02 | Referencia por fid | Todo archivo se identifica mediante un fid (file ID) único generado por SeaweedFS. |
| BR-ST-03 | Sin metadatos de negocio | SeaweedFS solo almacena el contenido binario. Los metadatos descriptivos se guardan en la base de datos de quarkus-app. |
| BR-ST-04 | Inmutabilidad | Una vez almacenado, el contenido de un archivo no se modifica. Las versiones nuevas son archivos diferentes. |
| BR-ST-05 | Límite de tamaño | Archivos entre 0 bytes y 4 GB. Archivos mayores serán rechazados. |

---

## 2. Reglas de Acceso

| # | Regla | Descripción |
|---|-------|-------------|
| BR-ST-06 | Acceso controlado | Solo quarkus-app tiene acceso directo a SeaweedFS. payloadcms nunca accede directamente. |
| BR-ST-07 | Autenticación interna | La comunicación entre quarkus-app y SeaweedFS usa autenticación interna (API key o mTLS). |
| BR-ST-08 | Sin exposición pública | SeaweedFS no debe exponerse directamente a internet. Solo accesible desde la red interna. |

---

## 3. Reglas de Operación

| # | Regla | Descripción |
|---|-------|-------------|
| BR-ST-09 | Replicación | Los archivos deben replicarse en al menos 2 nodos para tolerancia a fallos (si la infraestructura lo permite). |
| BR-ST-10 | Limpieza diferida | La eliminación de archivos puede ser diferida (marcar para borrado, eliminar después de un período). |
| BR-ST-11 | Heartbeat | El servicio debe reportar su estado mediante health check cada 30 segundos. |
