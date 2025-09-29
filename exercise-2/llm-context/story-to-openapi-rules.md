# Story to OpenAPI Conversion Ruleset

## Overview
This ruleset defines how to convert API story documents (written in markdown) into OpenAPI 3.1.0 specifications.

---

## 1. Document Structure Mapping

### 1.1 Basic Info Block
- **Story Section**: `# [Title]` and `## Purpose`
- **OpenAPI Mapping**:
  ```yaml
  info:
    title: [Title from heading] + " API"
    version: 1.0.0
    description: [Content from Purpose section]
  ```

---

## 2. Data Properties → Schema Components

### 2.1 Property Extraction
- **Story Section**: `## Data Properties`
- **Each property** follows pattern: `* **propertyName** : description [type]`

### 2.2 Type Mapping Rules
| Story Type | OpenAPI Type | Additional Format |
|------------|--------------|-------------------|
| `[uuid]` | `string` | `format: uuid` |
| `[string]` | `string` | - |
| `[text]` | `string` | - |
| `[date]` | `string` | `format: date` |
| `[enumerated string]` | `string` | - |
| `[enumerated number]` | `string` | (represented as string) |
| `[number]` | `integer` or `number` | - |

### 2.3 Schema Component Creation
For each property, create:
```yaml
components:
  schemas:
    PropertyName:  # PascalCase version
      type: [mapped type]
      title: [Human-readable title]
      description: [Description from story]
      format: [if applicable]
      example: [generic example]
```

---

## 3. Resources → Response Schemas

### 3.1 Resource Types
- **Story Section**: `## Resources`
- Each resource becomes a schema in `components/schemas`

### 3.2 Main Entity Schema
Create a schema named `[EntityName]Item`:
```yaml
[EntityName]Item:
  type: object
  properties:
    [property1]: { $ref: '#/components/schemas/Property1' }
    [property2]: { $ref: '#/components/schemas/Property2' }
    # ... all data properties
  required: [list required fields from actions]
```

### 3.3 Collection Schema
```yaml
[EntityName]Collection:
  type: array
  items:
    $ref: '#/components/schemas/[EntityName]Item'
```

### 3.4 Home/Landing Schema
```yaml
Home:
  type: object
  properties:
    home:
      type: string
      format: uri
      example: https://localhost:4000/[home endpoint]
    collection:
      type: string
      format: uri
      example: https://localhost:4000/[collection endpoint]
    search:
      type: string
      format: uri
      example: https://localhost:4000/[filter endpoint]
```

---

## 4. Actions → API Endpoints

### 4.1 Action Extraction
- **Story Section**: `## Actions`
- Each action follows pattern with:
  - Name
  - Purpose description
  - Inputs (comma-separated properties)
  - Required fields
  - Returns (target resource)
  - Type (Safe/Unsafe/Idempotent/Delete)

### 4.2 HTTP Method Mapping
| Action Type | HTTP Method |
|-------------|-------------|
| Safe | GET |
| Unsafe | POST |
| Idempotent | PUT |
| Delete | DELETE |

### 4.3 Endpoint Path Naming
- Convert action name from PascalCase to path format
- Pattern: `/{actionNameInCamelCase}`
- For item-specific actions: Include `/{id}` parameter if action uses `id` input

Examples:
- `ShowHomePage` → `/goShowHomePage`
- `GetTaskCollection` → `/goGetTaskCollection`
- `GetTaskItem` → `/goGetTaskItem/{id}`
- `CreateNewTask` → `/doCreateNewTask`
- `UpdateStatusOfTask` → `/doUpdateStatusOfTask`

### 4.4 Naming Convention Prefixes
- **Navigation/Read actions**: `go` prefix
- **Write/Modify actions**: `do` prefix

### 4.5 Path Parameters
- If action has `id` in **Required** inputs → add `/{id}` to path
- Create parameter definition:
  ```yaml
  parameters:
    - name: id
      in: path
      required: true
      description: [Description from Id schema]
      schema:
        $ref: '#/components/schemas/Id'
  ```

### 4.6 Query Parameters (for Filter/Search actions)
- If action name contains "Filter" or inputs list multiple optional properties
- Map non-required inputs to query parameters:
  ```yaml
  parameters:
    - name: [propertyName]
      in: query
      required: false
      schema:
        $ref: '#/components/schemas/[PropertyName]'
  ```

### 4.7 Request Body
- **Applies to**: POST, PUT, PATCH methods
- **Create dedicated schemas** for partial updates:

#### Update-Specific Schemas
```yaml
[Entity][Field]Update:
  type: object
  properties:
    id: { $ref: '#/components/schemas/Id' }
    [specificField]: { $ref: '#/components/schemas/[FieldName]' }
  required: [id, specificField]
```

Examples:
- `TaskStatusUpdate` for status-only updates
- `TaskDueDateUpdate` for due date updates
- `TaskAssignment` for assignment updates

#### Full Entity Request Body
For create/edit actions, use full `[Entity]Item` schema:
```yaml
requestBody:
  required: true
  content:
    application/json:
      schema:
        $ref: '#/components/schemas/[Entity]Item'
```

### 4.8 Response Mapping
- **Returns** field determines response schema
- Standard responses:
  ```yaml
  responses:
    '200':
      description: [Action-appropriate message]
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/[ReturnsValue]'
    '400':
      description: Invalid input
    '404':
      description: Resource not found
  ```

### 4.9 Operation Metadata
```yaml
operationId: [actionNameInCamelCase]
summary: [Short action description]
tags: [[appropriate tag]]
```

---

## 5. Tags

### 5.1 Tag Categories
Create tags based on action purpose:
- `navigation` - for home/landing page actions
- `[entity]-management` - for CRUD operations (e.g., `task-management`)

### 5.2 Tag Assignment
- Home/list navigation → `navigation`
- Create/Read/Update operations → `[entity]-management`

---

## 6. Special Handling

### 6.1 Rules Section
- **Story Section**: `## Rules`
- Add as comments or operation descriptions
- Not directly mapped to OpenAPI structure, but inform validation logic

### 6.2 Required Fields
- Extract from each action's "Required" line
- Apply to:
  - Request body schemas
  - Main entity schema
  - Update-specific schemas

---

## 7. Conventions & Best Practices

### 7.1 Naming
- **Schemas**: PascalCase (e.g., `TaskItem`, `TaskCollection`)
- **Properties**: camelCase (e.g., `assignedUser`, `dueDate`)
- **Paths**: camelCase with prefix (e.g., `/goGetTaskItem`, `/doCreateNewTask`)
- **Operation IDs**: camelCase matching path (e.g., `goGetTaskItem`)

### 7.2 Example Values
- Use generic placeholders: `example-[property]` for strings
- Use realistic formats for dates, UUIDs, etc.
- Keep examples simple and illustrative

### 7.3 Reusability
- All schemas should be defined in `components/schemas`
- Use `$ref` consistently throughout
- Create granular schemas for each property type

---

## 8. Conversion Process Checklist

1. ✅ Extract title and purpose → `info` block
2. ✅ Parse data properties → individual property schemas
3. ✅ Create main entity schema with all properties
4. ✅ Create collection schema (array of entities)
5. ✅ Create home/landing schema if applicable
6. ✅ For each action:
   - Determine HTTP method from type
   - Create endpoint path with naming prefix
   - Add path parameters if needed
   - Add query parameters for filters
   - Create request body for unsafe/idempotent operations
   - Map response to appropriate schema
   - Add operation metadata and tags
7. ✅ Create update-specific schemas for targeted modifications
8. ✅ Define tags section
9. ✅ Review required fields across all schemas

---

## Example Transformation

**Story Input:**
```markdown
* **status** : Indicates the status (active, completed) [enumerated string]
```

**OpenAPI Output:**
```yaml
components:
  schemas:
    Status:
      type: string
      title: Status
      description: Current state of the task (active or completed).
      example: example-status
```

**Story Action:**
```markdown
* **UpdateStatusOfTask** : Use this action to update the `status`
  * Inputs: id, status
  * Required: id, status
  * Returns: **TaskItem**
  * Type: Idempotent
```

**OpenAPI Output:**
```yaml
/doUpdateStatusOfTask:
  put:
    operationId: doUpdateStatusOfTask
    summary: Update Status
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/TaskStatusUpdate'
    responses:
      '200':
        description: Status updated
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TaskItem'
```
