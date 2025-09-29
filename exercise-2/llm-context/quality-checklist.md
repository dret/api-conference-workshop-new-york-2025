# Story to OpenAPI Quality Checklist

Use this checklist to ensure your conversion is complete and accurate.

---

## Pre-Conversion Checks

### Story Document Validation
- [ ] Document has clear H1 title
- [ ] Purpose section exists and is descriptive
- [ ] Data Properties section lists all fields with types
- [ ] Resources section defines all states
- [ ] Actions section includes all operations
- [ ] Each action has: Inputs, Required, Returns, Type
- [ ] Rules section is present (if applicable)

---

## Schema Completeness

### Individual Property Schemas
- [ ] Every data property has its own schema
- [ ] Schema names are in PascalCase (Title, DueDate, Status)
- [ ] Each schema has `type` field
- [ ] Each schema has `title` field (human-readable)
- [ ] Each schema has `description` field
- [ ] Appropriate `format` added (uuid, date, etc.)
- [ ] Each schema has realistic `example` value

### Main Entity Schema
- [ ] Entity schema exists (e.g., TaskItem)
- [ ] Name format: `[Entity]Item`
- [ ] All data properties referenced via $ref
- [ ] `required` array includes mandatory fields
- [ ] Required fields match action requirements

### Collection Schema
- [ ] Collection schema exists (e.g., TaskCollection)
- [ ] Name format: `[Entity]Collection`
- [ ] Type is `array`
- [ ] Items reference the entity schema

### Update-Specific Schemas
- [ ] Created for each targeted update action
- [ ] Name format: `[Entity][Field]Update` or `[Entity]Assignment`
- [ ] Include only `id` + modified field(s)
- [ ] Both fields in `required` array
- [ ] Properties use $ref to base schemas

### Home/Landing Schema (if applicable)
- [ ] Home schema exists if needed
- [ ] Contains URI properties (home, collection, search)
- [ ] URIs are properly formatted
- [ ] Example URIs match endpoint paths

---

## Endpoint Accuracy

### Path Structure
- [ ] All actions converted to endpoints
- [ ] Read operations use `/go` prefix
- [ ] Write operations use `/do` prefix
- [ ] Path names are camelCase
- [ ] No typos in path names

### HTTP Methods
- [ ] Safe actions → GET
- [ ] Unsafe actions → POST
- [ ] Idempotent actions → PUT
- [ ] Delete actions → DELETE (if applicable)

### Path Parameters
- [ ] Added for actions with `id` in Required
- [ ] Path includes `/{id}`
- [ ] Parameter definition exists
- [ ] Parameter is `required: true`
- [ ] Parameter is `in: path`
- [ ] Schema references Id schema via $ref
- [ ] Description is meaningful

### Query Parameters
- [ ] Added for filter/search actions
- [ ] One parameter per optional input
- [ ] All parameters are `required: false`
- [ ] Parameters are `in: query`
- [ ] Each references appropriate schema via $ref
- [ ] Parameter names match property names (camelCase)

---

## Request Bodies

### POST Endpoints (Create/Edit)
- [ ] `requestBody` exists
- [ ] `required: true`
- [ ] Content type is `application/json`
- [ ] Schema references full entity schema
- [ ] Create actions use TaskItem schema
- [ ] Edit actions use TaskItem schema

### PUT Endpoints (Targeted Updates)
- [ ] `requestBody` exists
- [ ] `required: true`
- [ ] Content type is `application/json`
- [ ] Schema references update-specific schema
- [ ] Update schema includes only changed fields + id

### GET/DELETE Endpoints
- [ ] No requestBody (GET should use query params)
- [ ] DELETE may have body if needed for soft delete

---

## Response Handling

### Success Responses (200)
- [ ] All endpoints have 200 response
- [ ] Description is action-appropriate
- [ ] Content type is `application/json`
- [ ] Schema matches action's "Returns" field
- [ ] Home actions → Home schema
- [ ] Collection actions → Collection schema
- [ ] Item actions → Item schema
- [ ] $ref path is correct

### Error Responses
- [ ] All endpoints have 400 response
- [ ] Description: "Invalid input"
- [ ] All endpoints have 404 response
- [ ] Description: "Resource not found"

---

## Operation Metadata

### Basic Info
- [ ] Each operation has `operationId`
- [ ] operationId matches path name (no slashes)
- [ ] operationId is camelCase
- [ ] Each operation has `summary`
- [ ] Summary is concise and descriptive

### Tags
- [ ] Each operation has `tags` array
- [ ] Home/collection access → `navigation` tag
- [ ] CRUD operations → `[entity]-management` tag
- [ ] Tags are defined in `tags` section at root

---

## Naming Conventions

### Schemas
- [ ] All schemas use PascalCase
- [ ] Individual properties: Title, DueDate, Status
- [ ] Entity: TaskItem
- [ ] Collection: TaskCollection
- [ ] Updates: TaskStatusUpdate, TaskAssignment

### Properties
- [ ] All properties use camelCase
- [ ] Examples: assignedUser, dueDate, id

### Endpoints
- [ ] All paths use camelCase with prefix
- [ ] Read: /goGetTaskItem, /goGetTaskCollection
- [ ] Write: /doCreateNewTask, /doUpdateStatusOfTask

### Operation IDs
- [ ] Match path name without slashes
- [ ] camelCase format
- [ ] Examples: goGetTaskItem, doCreateNewTask

---

## Reference Integrity

### Schema References
- [ ] All $ref paths start with `#/components/schemas/`
- [ ] All referenced schemas exist in components
- [ ] No broken references
- [ ] No circular references (unless intentional)

### Property References
- [ ] Entity schemas reference property schemas
- [ ] Update schemas reference property schemas
- [ ] Request bodies reference appropriate schemas
- [ ] Responses reference appropriate schemas

---

## Content Quality

### Descriptions
- [ ] All schemas have meaningful descriptions
- [ ] Descriptions are complete sentences
- [ ] Descriptions are clear and concise
- [ ] No placeholder text like "TODO" or "TBD"

### Examples
- [ ] All basic schemas have examples
- [ ] Examples are realistic
- [ ] UUID examples are valid format
- [ ] Date examples use ISO format (YYYY-MM-DD)
- [ ] String examples follow pattern: "example-[property]"

---

## OpenAPI Structure

### Root Level
- [ ] `openapi: 3.1.0` version declaration
- [ ] `info` block present
- [ ] `paths` section present
- [ ] `components` section present
- [ ] `tags` section present (optional but recommended)

### Info Block
- [ ] `title` exists (entity name + " API")
- [ ] `version` exists (e.g., "1.0.0")
- [ ] `description` exists (from Purpose section)
- [ ] Description is meaningful and complete

### Paths Section
- [ ] All endpoints are under `paths`
- [ ] Each path starts with `/`
- [ ] Paths are organized logically
- [ ] No duplicate paths

### Components Section
- [ ] `components/schemas` exists
- [ ] All schemas are properly nested
- [ ] No orphaned schemas
- [ ] Schemas are alphabetically ordered (optional but helpful)

### Tags Section
- [ ] Tags are defined at root level
- [ ] Each tag has a `name`
- [ ] Tag names match those used in operations
- [ ] Common tags: `navigation`, `[entity]-management`

---

## Validation Checks

### YAML Syntax
- [ ] Valid YAML syntax (no tabs, correct indentation)
- [ ] Consistent indentation (2 spaces recommended)
- [ ] No trailing whitespace
- [ ] Proper quote usage (consistent style)

### OpenAPI Compliance
- [ ] Run through OpenAPI validator
- [ ] No validation errors
- [ ] No validation warnings (or documented reasons)
- [ ] Compatible with OpenAPI 3.1.0 spec

### Logical Consistency
- [ ] Required fields in schemas match action requirements
- [ ] Response types match action Returns field
- [ ] HTTP methods match action Types
- [ ] Parameter locations are correct (path vs query)

---

## Special Cases

### Filter/Search Actions
- [ ] Multiple query parameters defined
- [ ] All query parameters optional
- [ ] Returns collection type
- [ ] Endpoint name includes "Filter" or similar

### Create Actions
- [ ] Uses POST method
- [ ] Request body has full entity schema
- [ ] Returns collection (not item)
- [ ] May note client generates ID (in description/rules)

### Edit vs Update Actions
- [ ] Edit = full update (POST, full schema)
- [ ] Update = partial update (PUT, targeted schema)
- [ ] Returns match expectation (usually Item)

### Actions with No Inputs
- [ ] No parameters defined
- [ ] No request body
- [ ] Typically GET method
- [ ] Examples: ShowHomePage, GetTaskCollection

### Actions Returning Home
- [ ] Returns Home schema with URIs
- [ ] Tagged as `navigation`
- [ ] Usually GET method

---

## Testing Recommendations

### Import Tests
- [ ] Import into Swagger Editor (no errors)
- [ ] Import into Postman (collection created)
- [ ] Import into code generator (successful)

### Mock Server Tests
- [ ] Create mock server from spec
- [ ] Test each endpoint
- [ ] Verify request/response formats
- [ ] Check required fields validation

### Documentation Tests
- [ ] Generate API documentation
- [ ] All endpoints documented
- [ ] All schemas documented
- [ ] Examples display correctly

---

## Edge Cases Review

### Enumerated Values
- [ ] Enumerated strings use `string` type
- [ ] Enumerated numbers use `string` type (as per convention)
- [ ] Description clarifies allowed values
- [ ] Consider adding `enum` array if values are known

### Optional vs Required
- [ ] Create/Edit actions: check which fields truly required
- [ ] Update actions: only id + updated field required
- [ ] Query parameters: typically all optional
- [ ] Path parameters: always required

### Nested Objects
- [ ] If data has nested structures, create sub-schemas
- [ ] Use $ref for reusability
- [ ] Maintain consistent depth

### Arrays
- [ ] Use `type: array` + `items`
- [ ] Items reference appropriate schema
- [ ] Consider if minItems/maxItems needed

---

## Documentation Quality

### Summary Fields
- [ ] Clear and concise (under 10 words)
- [ ] Action-oriented (Get, Create, Update, Delete)
- [ ] Consistent style across all operations

### Description Fields
- [ ] Provide context and purpose
- [ ] Explain business logic if needed
- [ ] Reference any constraints or rules
- [ ] Use proper grammar and spelling

### Parameter Descriptions
- [ ] Explain what the parameter does
- [ ] Note any format requirements
- [ ] Mention valid ranges or patterns

---

## Performance Considerations

### Schema Reusability
- [ ] Common properties use shared schemas
- [ ] No duplicate schema definitions
- [ ] Proper use of $ref throughout

### Response Size
- [ ] Collection responses reasonable
- [ ] Consider pagination for large collections
- [ ] Document any size limits

---

## Security Considerations

### Authentication (if applicable)
- [ ] Security schemes defined
- [ ] Applied to appropriate operations
- [ ] Documented in info section

### Input Validation
- [ ] Required fields enforced
- [ ] String lengths appropriate
- [ ] Number ranges sensible
- [ ] Date formats specified

---

## Final Review

### Completeness
- [ ] Every story action has an endpoint
- [ ] Every data property has a schema
- [ ] Every resource has a return schema
- [ ] All rules considered and documented

### Consistency
- [ ] Naming conventions followed throughout
- [ ] HTTP methods align with REST principles
- [ ] Response codes consistent
- [ ] Error handling uniform

### Accuracy
- [ ] Matches original story intent
- [ ] No information lost in translation
- [ ] Required fields correctly identified
- [ ] Return types match expectations

### Quality
- [ ] Professional appearance
- [ ] No typos or grammatical errors
- [ ] Examples are helpful
- [ ] Ready for production use

---

## Sign-Off Checklist

Before considering the conversion complete:

- [ ] All sections of this checklist reviewed
- [ ] All critical items checked
- [ ] Known issues documented
- [ ] Validated with OpenAPI tools
- [ ] Reviewed by second person (if possible)
- [ ] Examples tested
- [ ] Documentation generated and reviewed
- [ ] Ready for implementation

---

## Common Issues and Solutions

### Issue: Missing schemas
**Solution:** Cross-reference story data properties with components/schemas

### Issue: Wrong HTTP method
**Solution:** Check action Type field - Safe=GET, Unsafe=POST, Idempotent=PUT

### Issue: Broken $ref
**Solution:** Verify schema name matches exactly (case-sensitive)

### Issue: Missing path parameter
**Solution:** If action requires id input, add /{id} to path

### Issue: Wrong response type
**Solution:** Match response schema to action's Returns field

### Issue: No request body on PUT
**Solution:** PUT for updates needs update-specific schema in requestBody

### Issue: Create returns Item instead of Collection
**Solution:** Check story - creates typically return collection

### Issue: Query parameter marked required
**Solution:** For filters, all query parameters should be optional

---

## Version History

- v1.0 (2025-09-29): Initial comprehensive checklist

---

## Notes

- This checklist is based on the tasks-api-story.md → tasks-openapi.yaml conversion
- Adapt as needed for your specific use case
- Keep this checklist updated as you discover new patterns
- Use programmatically if automating conversions
