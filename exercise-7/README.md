# API Conference New York 2025: Exercise 7 (Error Messages)

This exercise focuses on implementing proper error handling in APIs using the HTTP Problem Details standard (RFC 9457). Good error handling is crucial for API usability and becomes even more important as APIs are consumed by AI agents.


## Getting Started

HTTP status codes provide basic error information, but Problem Details adds structured, machine-readable context that helps API consumers understand and handle errors properly.


## Exercise

Start with your OpenAPI description from Exercise 3 (or use the provided [`petstore.openapi.yaml`](../exercise-3/petstore.openapi.yaml)) and add Problem Details error responses.


### Basic Approach

Add Problem Details responses for common HTTP errors in your API:
- `404 Not Found` - Resource doesn't exist
- `400 Bad Request` - Invalid input data  
- `409 Conflict` - Business rule violation
- `500 Internal Server Error` - Server issues

Example in OpenAPI:
```yaml
responses:
  404:
    description: Pet not found
    content:
      application/problem+json:
        schema:
          $ref: '#/components/schemas/ProblemDetails'
        example:
          type: "https://example.com/probs/not-found"
          title: "Pet not found"
          status: 404
          detail: "Pet with id 123 not found"
```


### Advanced Approach

If you're comfortable with Spectral or feel adventurous, create rules to validate that:

- All error responses use `application/problem+json`
- Problem types follow a consistent URI pattern
- Required fields are present


## Resources

- [RFC 9457: Problem Details for HTTP APIs](https://www.rfc-editor.org/rfc/rfc9457.html)
- [SmartBear Problem Details Registry](https://problems-registry.smartbear.com/)
- [Swagger Editor](https://editor-next.swagger.io/) for testing
