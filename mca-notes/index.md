# Exercises for OpenAPI Workshop

## Units
 * API Story (structured markdown)
   * tasks-api-story.md
 * OpenAPI Document (any editor)
   * tasks-openapi.yaml
 * Validating your OpenAPI (oas-validation)
   * https://oas-validation.com/
 * Mocking Your API (wiremock)
   * https://app.wiremock.cloud/mock-apis
 * Applying Overlays (bump)
   * tasks-search-overlay.yaml
 * Documenting Your API (redocly)
   * tasks-search.html
 * Designing OpneAPI Workflows (arazzo)
   * ???
 
 ## Installs
  * wiremock
  * bump.sh
  * redocly
  
 ### wiremock
 https://app.wiremock.cloud/mock-apis/create-flow

 ```
 npm install --global @wiremock/cli
 wiremock login
 wiremock push open-api leqkg --file /tmp/zwg1l-openapi.yaml
 ```
 
 ### bump.sh
 https://docs.bump.sh/help/continuous-integration/cli/
 
 ```
npm install -g bump-cli
bump deploy path/to/api-document.yml --dry-run --doc my-documentation --token $DOC_TOKEN
bump overlay api-document.yaml overlay.yaml > modified-api-document.yaml
```

### redocly
https://redocly.com/docs/cli/installation

```
npm i -g @redocly/cli@latest
redocly build-docs <api> --output=custom.html
```

 
