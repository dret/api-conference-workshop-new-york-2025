# Story to OpenAPI Conversion - Complete Package

## Table of Contents
1. [Overview](#overview)
2. [Quick Start](#quick-start)
3. [File Descriptions](#file-descriptions)
4. [Detailed Context](#detailed-context)
5. [Conversion Algorithm](#conversion-algorithm)
6. [Pattern Reference](#pattern-reference)
7. [AI Prompt Template](#ai-prompt-template)

---

## Overview

This package provides everything needed to convert API story documents (markdown) into OpenAPI 3.1.0 specifications, either manually or with AI assistance.

**Purpose**: Bridge human-readable API requirements with machine-readable specifications.

---

## Quick Start

### To Recreate This Process:

**Option 1: With AI Assistant**
1. Upload your story document (markdown)
2. Upload `story-to-openapi-ruleset.md` (included in this package)
3. Use the prompt template from [AI Prompt Template](#ai-prompt-template) section
4. Review and validate the output

**Option 2: Manual Conversion**
1. Open your story document
2. Reference `story-to-openapi-ruleset.md` for conversion rules
3. Follow the [Conversion Algorithm](#conversion-algorithm) step-by-step
4. Use the [Pattern Reference](#pattern-reference) for specific cases
5. Validate with OpenAPI validator

---

## File Descriptions

### Files in This Package

1. **story-to-openapi-ruleset.md** *(Created separately)*
   - Complete ruleset for conversion
   - Type mappings, naming conventions, schema patterns
   - Use this as your primary reference guide

2. **conversion-context-document.md** *(This file)*
   - Background and rationale
   - Pattern explanations with examples
   - AI prompt templates
   - Quality checklist

3. **tasks-api-story.md** *(Your original file)*
   - Example input document
   - Reference for story document structure

4. **tasks-openapi.yaml** *(Your original file)*
   - Example output specification
   - Reference for OpenAPI structure

---

## Detailed Context

### Project Background

This conversion system was created by analyzing the relationship between `tasks-api-story.md` and `tasks-openapi.yaml`. The pattern analysis revealed consistent conventions for:

- Naming (go/do prefixes for endpoints)
- HTTP method selection
- Schema granularity (property-level schemas)
- Update-specific schemas for targeted modifications
- Standard response structures

### Story Document Structure

```markdown
# [Entity Name]                    → API Title

## Purpose                          → API Description
[What the API does]

## Data Properties                  → Component Schemas
* **propertyName** : description [type]

## Resources                        → Response Schema Types
* **ResourceName** : Description
  * Actions: [list of actions]

## Actions                          → API Endpoints
* **ActionName** : Description
  * Inputs: [comma-separated]
  * Required: [comma-separated]
  * Returns: **ResourceType**
  * Type: Safe|Unsafe|Idempotent|Delete

## Rules                           → Validation Logic / Comments
* [Business rules]
```

### Key Conventions

#### 1. Naming Patterns
- **Schemas**: `PascalCase` (TaskItem, TaskCollection)
- **Properties**: `camelCase` (assignedUser, dueDate)
- **Endpoints**: `camelCase` with prefix (/goGetTaskItem, /doCreateNewTask)
- **Operation IDs**: Same as endpoint without slashes

#### 2. Endpoint Prefixes
- `go` = Navigation/Read (GET operations)
- `do` = Write/Modify (POST/PUT operations)

#### 3. HTTP Method Selection
- **Safe** → GET (no side effects)
- **Unsafe** → POST (creates/modifies with side effects)
- **Idempotent** → PUT (can repeat safely)
- **Delete** → DELETE

#### 4. Schema Architecture
```
Individual Property Schemas (Id, Title, Status, etc.)
    ↓
Main Entity Schema (TaskItem) - references properties
    ↓
Collection Schema (TaskCollection) - array of entities
    ↓
Update Schemas (TaskStatusUpdate) - subset for specific updates
```

---

## Conversion Algorithm

### Step-by-Step Process

**Phase 1: Document Parsing**
```
1. Extract title (H1 heading)
2. Extract purpose (## Purpose section)
3. Parse data properties list
4. Parse resources list  
5. Parse actions list
6. Note any rules
```

**Phase 2: Schema Generation**
```
For each data property:
  - Create individual property schema (PascalCase name)
  - Map type: [story type] → OpenAPI type/format
  - Add title, description, example

Create main entity schema:
  - Properties: all data properties as $refs
  - Required: determined from actions

Create collection schema:
  - Type: array
  - Items: ref to entity schema

Create update-specific schemas:
  - For each targeted update action
  - Include only id + modified field
```

**Phase 3: Endpoint Generation**
```
For each action:
  1. Determine HTTP method from type
  2. Generate path: /{prefix}{ActionName}
     - Use 'go' for Safe actions
     - Use 'do' for Unsafe/Idempotent actions
  3. Add path parameter /{id} if id in Required
  4. Add query parameters if filter action
  5. Create request body for POST/PUT:
     - Use update schema for targeted updates
     - Use full entity schema for create/edit
  6. Map response schema from Returns field
  7. Add operationId, summary, tags
```

**Phase 4: Finalization**
```
- Define tags (navigation, [entity]-management)
- Add info block (title, version, description)
- Validate all $refs
- Run through quality checklist
```

---

## Pattern Reference

### Pattern 1: Simple GET Endpoint
```yaml
/goGetTaskCollection:
  get:
    operationId: goGetTaskCollection
    summary: Get Task Collection
    responses:
      '200':
        description: Successful operation
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TaskCollection'
      '400':
        description: Invalid input
      '404':
        description: Resource not found
    tags: [task-management]
```

### Pattern 2: GET with Path Parameter
```yaml
/goGetTaskItem/{id}:
  get:
    operationId: goGetTaskItem
    summary: Get Task Item
    parameters:
      - name: id
        in: path
        required: true
        description: A globally unique identifier for each task record.
        schema:
          $ref: '#/components/schemas/Id'
    responses:
      '200':
        description: Successful operation
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TaskItem'
```

### Pattern 3: GET with Query Parameters (Filter)
```yaml
/goGetFilteredTaskCollection:
  get:
    operationId: goGetFilteredTaskCollection
    summary: Filter Task Collection
    parameters:
      - name: title
        in: query
        required: false
        schema:
          $ref: '#/components/schemas/Title'
      - name: status
        in: query
        required: false
        schema:
          $ref: '#/components/schemas/Status'
    responses:
      '200':
        description: Filtered tasks
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TaskCollection'
```

### Pattern 4: POST with Full Schema
```yaml
/doCreateNewTask:
  post:
    operationId: doCreateNewTask
    summary: Create Task
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/TaskItem'
    responses:
      '200':
        description: Task created
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/TaskCollection'
```

### Pattern 5: PUT with Update Schema
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

# Companion schema:
components:
  schemas:
    TaskStatusUpdate:
      type: object
      properties:
        id: { $ref: '#/components/schemas/Id' }
        status: { $ref: '#/components/schemas/Status' }
      required: [id, status]
```

---

## AI Prompt Template

### Prompt for Converting New Story Document

```
I need to convert an API story document (markdown) into an OpenAPI 3.1.0 specification.

I'm providing:
1. The story document to convert: [attach your-story.md]
2. The conversion ruleset: [attach story-to-openapi-ruleset.md]
3. Example input (reference only): tasks-api-story.md
4. Example output (reference only): tasks-openapi.yaml

Please:
1. Analyze the story document structure
2. Apply the conversion rules from the ruleset
3. Generate a complete OpenAPI 3.1.0 specification in YAML format
4. Follow all naming conventions (go/do prefixes, PascalCase/camelCase)
5. Create granular property schemas
6. Create update-specific schemas for targeted field modifications
7. Ensure all $refs are valid
8. Include standard 200/400/404 responses for all endpoints

The output should be production-ready OpenAPI YAML that I can validate with standard tools.
```

### Prompt for Validating Conversion

```
I've converted a story document to OpenAPI. Please review it against the ruleset.

Files:
1. Original story: [attach story.md]
2. Generated OpenAPI: [attach generated-openapi.yaml]
3. Conversion ruleset: [attach story-to-openapi-ruleset.md]

Please check:
- All data properties have corresponding schemas
- All actions have corresponding endpoints
- HTTP methods match action types
- Naming conventions are followed (go/do prefixes, camelCase/PascalCase)
- Path parameters exist where needed
- Query parameters for filter actions
- Request bodies use appropriate schemas
- Response schemas match Returns values
- All $refs are valid
- Update-specific schemas exist for targeted updates

List any issues found and suggest corrections.
```

---

## Type Mapping Reference

| Story Type | OpenAPI Type | Format | Example |
|------------|--------------|--------|---------|
| [uuid] | string | uuid | fe72734f-939c-4cd1-a4e3-85d88a4979af |
| [string] | string | - | example-title |
| [text] | string | - | example-description |
| [date] | string | date | 2025-09-01 |
| [enumerated string] | string | - | active |
| [enumerated number] | string | - | "1" |
| [number] | integer/number | - | 42 |

---

## Quality Checklist

Before finalizing, verify:

**Schema Completeness**
- [ ] All data properties have individual schemas
- [ ] Main entity schema references all properties
- [ ] Collection schema exists
- [ ] Update-specific schemas for targeted updates
- [ ] Home schema if applicable

**Endpoint Accuracy**
- [ ] All actions mapped to endpoints
- [ ] HTTP methods match action types
- [ ] Correct go/do prefixes
- [ ] Path parameters for ID-based operations
- [ ] Query parameters for filter operations

**Request/Response Handling**
- [ ] Request bodies for POST/PUT
- [ ] Correct schema references in requests
- [ ] Response schemas match Returns field
- [ ] Standard 200/400/404 responses

**Naming & Structure**
- [ ] PascalCase for schemas
- [ ] camelCase for properties and paths
- [ ] Consistent operationId format
- [ ] Appropriate tags assigned

**References & Validation**
- [ ] All $refs point to existing schemas
- [ ] Required fields consistent
- [ ] Examples present and realistic
- [ ] Valid OpenAPI 3.1.0 YAML

---

## Troubleshooting Guide

**Issue: HTTP method doesn't match action type**
- Check action's "Type" field
- Safe=GET, Unsafe=POST, Idempotent=PUT

**Issue: Missing update schema**
- Look for actions that update single fields
- Create [Entity][Field]Update schema with id + field

**Issue: Wrong response type**
- Check action's "Returns" field
- Map exactly to that schema name

**Issue: Path parameter missing**
- Check if action has id in Required inputs
- Add /{id} to path and parameter definition

**Issue: Filter action not working**
- Map all optional inputs to query parameters
- Set required: false for all

---

## Implementation Tools

### Recommended Tools

**For Validation:**
- Swagger Editor (online validator)
- OpenAPI CLI validator
- Postman (import and test)

**For Automation:**
- Python: PyYAML, Jinja2 templates
- JavaScript: js-yaml, Handlebars
- Generic: Custom parser + template engine

**For Documentation:**
- Swagger UI
- ReDoc
- Stoplight

---

## Maintenance & Updates

As you convert more documents:

1. **Document new patterns** in this context file
2. **Update the ruleset** with refined rules
3. **Add edge cases** to troubleshooting guide
4. **Refine type mappings** as needed
5. **Keep examples** of complex conversions

---

## Quick Reference Card

```
Story → OpenAPI Cheat Sheet

Property:    Individual schema (PascalCase)
Entity:      [Entity]Item schema (all properties)
Collection:  [Entity]Collection (array of items)
Update:      [Entity][Field]Update (id + field)

Safe action      → GET  /goActionName
Unsafe action    → POST /doActionName
Idempotent       → PUT  /doActionName

Has id input     → Add /{id} parameter
Multiple options → Add query parameters
Modifies data    → Add requestBody
```

---

## Support Resources

**Original Examples:**
- tasks-api-story.md (input example)
- tasks-openapi.yaml (output example)

**Conversion Tools:**
- story-to-openapi-ruleset.md (primary reference)
- This document (context & templates)

**External Resources:**
- OpenAPI 3.1.0 Specification: https://spec.openapis.org/oas/v3.1.0
- OpenAPI Style Guide: https://stoplight.io/api-style-guide

---

## Version History

- **v1.0** (2025-09-29): Initial package based on tasks example analysis

---

*This is a living document. Update as you discover new patterns and refine the conversion process.*
