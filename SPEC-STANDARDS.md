# Spec Standards - SGE

> **Versión:** 1.0.0  
> **Estado:** Borrador

---

## Estándares para Especificaciones

### 1. **Formato de Archivos**

#### YAML Specs
```yaml
# Header obligatorio
# {Spec Type} — {Name}
# Define {description}

{specType}: {Name}
version: 1.0.0
description: {Descripción clara}

# Body según tipo de spec
```

#### Markdown Docs
```markdown
# {Title}

> **Versión:** 1.0.0  
> **Servicio:** {service-name}  
> **Subdominio:** {subdomain-name}

---

## 1. Propósito

{Descripción del propósito}

---

## 2. {Sections según tipo}
```

---

## 2. **State Machine Specs**

### Structure
```yaml
stateMachine: {Name}
version: 1.0.0
description: {Description}

states:
  - id: {STATE_ID}
    description: {Description}
    initial: true/false
    terminal: true/false
    allowedActions:
      - {action}

transitions:
  - from: {STATE_ID}
    to: {STATE_ID}
    action: {action}
    requiredRole: {role}
    requiresJustification: true/false

rules:
  - id: {RULE-ID}
    description: {Description}
    constraint: {Constraint}

events:
  - name: {EventName}
    trigger: {Trigger}
    payload:
      {field}: {type}
```

### Convenciones
- Estados: UPPER_SNAKE_CASE (ej: `BORRADOR`, `ENVIADO`)
- Actions: lowercase (ej: `enviar`, `aprobar`, `observar`)
- Rules: `{DOMAIN}-{NN}` (ej: `SM-01`, `WF-01`)

---

## 3. **Aggregate Specs**

### Structure
```yaml
aggregate: {Name}
version: 1.0.0
description: {Description}
rootEntity: {EntityName}

entities:
  - name: {EntityName}
    type: aggregate-root | child-entity | entity
    description: {Description}
    identity:
      field: {fieldName}
      type: {UUID | string | integer}
      generated: true/false
    fields:
      - name: {fieldName}
        type: {type}
        required: true/false
        enum: [...] (si aplica)
        maxLength: {N} (si aplica)
        pattern: {pattern} (si aplica)
        description: {Description}

relationships:
  - type: one-to-many | many-to-one | one-to-one
    from: {Entity}
    to: {Entity}
    description: {Description}
    cascadeDelete: true/false

invariants:
  - id: INV-{DOMAIN}-{NN}
    description: {Description}
    rule: {Rule}

commands:
  - name: {CommandName}
    description: {Description}
    fields:
      - {fieldName}

queries:
  - name: {QueryName}
    description: {Description}
    params:
      - {paramName}: {type}
```

### Convenciones
- Entities: PascalCase (ej: `Expediente`, `Archivo`)
- Commands: PascalCase con verbo (ej: `CrearExpediente`, `AdjuntarArchivo`)
- Queries: PascalCase (ej: `ObtenerExpediente`, `ListarExpedientesPorOficina`)
- Invariants: `INV-{DOMAIN}-{NN}`

---

## 4. **Event Contract Specs**

### Structure
```yaml
eventContract: {Name}
version: 1.0.0
description: {Description}

publishedEvents:
  - name: {EventName}
    description: {Description}
    source: {subdomain/service}
    trigger: {Trigger condition}
    schema:
      type: object
      properties:
        {field}:
          type: {type}
          format: {format} (si aplica)
          enum: [...] (si aplica)
    consumers:
      - {service}/{subdomain}

consumedEvents:
  - name: {EventName}
    description: {Description}
    source: {service}
    action: {Action on consume}
    schema:
      {schema}

integrationPatterns:
  - pattern: {PatternName}
    description: {Description}
    example: {Example}

errorHandling:
  - scenario: {Scenario}
    strategy: {Strategy}
    fallback: {Fallback}
```

### Convenciones
- Eventos: PascalCase (ej: `ExpedienteCreated`, `UsuarioDesactivado`)
- Schema: JSON Schema format
- Consumers: `{service}/{subdomain}` format

---

## 5. **Permission Specs**

### Structure
```yaml
permissions: {Name}
version: 1.0.0
description: {Description}

roles:
  - id: {role-id}
    name: {Role Name}
    permissions:
      - {permission-id}

permissionDefinitions:
  - id: {permission-id}
    description: {Description}
    scope: global | office
    requiresJustification: true/false (si aplica)

rules:
  - id: PERM-{DOMAIN}-{NN}
    description: {Description}
    rule: {Rule}
```

### Convenciones
- Roles: kebab-case (ej: `super-admin`, `jefe-oficina`)
- Permissions: `{resource}:{action}:{scope}` (ej: `expedientes:read:oficina`)
- Rules: `PERM-{DOMAIN}-{NN}`

---

## 6. **OpenAPI Specs**

### Structure
```yaml
openapi: 3.0.3
info:
  title: {API Name}
  version: 1.0.0
  description: {Description}

servers:
  - url: {base-url}
    description: {Description}

security:
  - BearerAuth: []

paths:
  {path}:
    {method}:
      tags: [{tag}]
      summary: {Summary}
      parameters: [...]
      requestBody: {...}
      responses:
        {code}:
          description: {Description}
          content: {...}

components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  schemas:
    {SchemaName}:
      type: object
      properties: {...}
```

---

## 7. **AsyncAPI Specs**

### Structure
```yaml
asyncapi: 2.6.0
info:
  title: {Service Name} - {Protocol} Channels
  version: 1.0.0
  description: {Description}

servers:
  {server-name}:
    url: {url}
    protocol: {protocol}
    description: {Description}

channels:
  {channel-name}:
    description: {Description}
    subscribe: | publish:
      operationId: {operationId}
      summary: {Summary}
      message:
        $ref: '#/components/messages/{MessageName}'

components:
  messages:
    {MessageName}:
      name: {Name}
      contentType: application/json
      payload: {...}
```

---

## 8. **Business Rules (Markdown)**

### Structure
```markdown
# Reglas de Negocio — {Subdomain/Service}

> **Servicio:** {service}  
> **Subdominio:** {subdomain}

---

## {Section}

| # | Regla | Descripción |
|---|-------|-------------|
| BR-{DOMAIN}-{NN} | {Title} | {Description} |

---

## {More sections}
```

### Convenciones
- IDs: `BR-{DOMAIN}-{NN}` (ej: `BR-EXP-01`, `BR-USR-01`)
- Sections: Agrupar por tema (Jerarquía, Estados, Archivos, etc.)

---

## 9. **Versioning Strategy**

### Semantic Versioning para Specs
- **MAJOR** (1.0.0 → 2.0.0): Breaking changes
- **MINOR** (1.0.0 → 1.1.0): New features, backward compatible
- **PATCH** (1.0.0 → 1.0.1): Fixes, typos, clarifications

### Breaking Changes
- Cambiar estructura de eventos
- Eliminar endpoints de API
- Cambiar tipos de campos
- Eliminar estados o transiciones

### Deprecation Policy
1. Marcar spec como `deprecated: true`
2. Mantener por 2 versiones
3. Notificar a consumidores
4. Eliminar después de migración

---

## 10. **Validation Rules**

### YAML Validation
- Todos los archivos YAML deben ser validados contra schema
- Usar `yamllint` o similar
- Validar en CI/CD (Pending)

### Markdown Validation
- Todos los archivos MD deben tener header completo
- Validar links internos (Pending)

---

## 11. **Template Files**

Plantillas disponibles en:
```
docs/templates/
├── state-machine.yaml
├── aggregate-spec.yaml
├── event-contract.yaml
├── permissions.yaml
├── PRD.md
├── business-rules.md
```

(Pending - crear templates)