# Documentación del Sistema - SGE (Sistema de Gestión de Expedientes)

> **Versión:** 1.0.0  
> **Estado:** Borrador  
> **Últera actualización:** 2026-06-03

---

## Estructura de la Documentación

```
docs/
├── README.md                    ← Guía de navegación (este archivo)
├── SPEC-STANDARDS.md            ← Estándares de especificaciones
├── CHANGELOG.md                 ← Historial de cambios
├── system/                      ← Nivel Sistema
│   ├── PRD.md                   ← Product Requirements Document
│   ├── business-rules.md        ← Reglas de negocio globales
│   ├── architecture.yaml        ← Arquitectura del sistema
│   └── event-contract.yaml      ← Event contracts globales
└── services/                    ← Nivel Servicio
    ├── quarkus-app/             ← Core Backend (Java/Quarkus)
    ├── payloadcms/              ← Frontend + BFF (Node.js/PayloadCMS)
    ├── inngest/                 ← Workflow Manager (TypeScript/Inngest)
    ├── storage/                 ← Almacenamiento (SeaweedFS)
    └── keycloak/                ← IAM (Keycloak)
```

---

## Niveles de Documentación

### **Nivel Sistema** (docs/system/)
Documentación que aplica a todo el sistema:
- **PRD.md**: Requisitos del producto, visión, alcance MVP
- **business-rules.md**: Reglas de negocio globales (47 reglas)
- **architecture.yaml**: Diagrama de servicios y subdominios
- **event-contract.yaml**: Eventos que cruzan límites de servicio

### **Nivel Servicio** (docs/services/{service}/)
Documentación específica de cada servicio:
- **PRD.md**: (si tiene subdominios, va en subdominios)
- **openapi.yaml**: API REST specification
- **asyncapi.yaml**: Async messaging specification (RabbitMQ)
- **event-contract.yaml**: Service-level event contracts
- **subdomain-X/**: Subdominios con specs completas

### **Nivel Subdominio** (docs/services/{service}/subdomain-X/)
Documentación de cada subdominio DDD:
- **PRD.md**: Requisitos funcionales del subdominio
- **business-rules.md**: Reglas de negocio del subdominio
- **state-machine.yaml**: Estados y transiciones
- **aggregate-spec.yaml**: Entities, commands, queries, invariants
- **event-contract.yaml**: Published/consumed events
- **permissions.yaml**: RBAC permissions por rol

---

## Spec-Driven Development Workflow

### 1. **Escribir Specs** (YAML + Markdown)
- PRDs definen requisitos
- business-rules.md define reglas
- YAML specs definen contratos técnicos

### 2. **Generar Código** (Pending - Prioridad 3)
- OpenAPI → TypeScript/Java types
- AsyncAPI → RabbitMQ consumers/publishers
- Aggregate specs → Domain entities

### 3. **Contract Testing** (Pending - Prioridad 3)
- Tests que validan implementación vs specs
- Event schema validation
- API contract tests

### 4. **Executable Specs** (Pending - Prioridad 3)
- Specs que se pueden ejecutar como tests
- State machine validation
- Permission tests

---

## Cómo Modificar la Documentación

### Para agregar un nuevo subdominio:
1. Crear carpeta `docs/services/{service}/subdomain-{name}/`
2. Copiar plantilla de `SPEC-STANDARDS.md`
3. Llenar: PRD, business-rules, state-machine, aggregate-spec, event-contract, permissions

### Para modificar una spec existente:
1. Editar archivo YAML/Markdown
2. Actualizar `CHANGELOG.md` con versión y descripción
3. Notificar a equipos afectados (si cambia contracto)

### Para cambiar contratos (breaking changes):
1. Crear nueva versión del spec (v2.0.0)
2. Mantener v1.0.0 como deprecated
3. Migrar consumidores gradualmente
4. Eliminar v1.0.0 después de migración completa

---

## Convenciones

### Naming
- Subdominios: `subdomain-{name}` (ej: `subdomain-expediente-management`)
- Servicios: kebab-case (ej: `quarkus-app`, `payloadcms`)
- Eventos: PascalCase (ej: `ExpedienteCreated`, `UsuarioDesactivado`)

### Versioning
- Specs: Semantic Versioning (MAJOR.MINOR.PATCH)
- Breaking changes → MAJOR
- New features (backward compatible) → MINOR
- Fixes → PATCH

### IDs de Reglas
- Sistema: `BR-SYS-{NN}`
- Expediente: `BR-EXP-{NN}`
- Usuarios: `BR-USR-{NN}`
- Workflow: `BR-WF-{NN}`
- Storage: `BR-ST-{NN}`
- Keycloak: `BR-KC-{NN}`

---

## Herramientas Recomendadas

| Spec Type | Tool | Usage |
|-----------|------|-------|
| OpenAPI | Swagger UI / Redoc | Visualizar API docs |
| AsyncAPI | asyncapi-cli | Generate docs from spec |
| YAML specs | any YAML editor | Edit + validate |
| Contract tests | TBD | Prioridad 3 |

---

## Next Steps (Prioridad 3)

1. **Generación automática de código**
   - Configurar OpenAPI Generator para TypeScript/Java
   - Configurar AsyncAPI Generator para RabbitMQ handlers

2. **Contract testing**
   - Setup API contract tests (OpenAPI validation)
   - Setup event schema validation tests

3. **Executable specs**
   - State machine tests (validar transiciones)
   - Permission tests (RBAC validation)
   - Aggregate invariants tests

---

## Contacto

- **Responsable**: Equipo de Producto
- **Repositorio**: `/home/urgosxd/Desktop/documentacionSDG`
- **Issues**: Reportar en README del proyecto principal