# API Conference New York 2025: Exercise 5  (Versioning)

This exercise demonstrates the practical impact of API changes and how to detect breaking changes using automated tools. You'll see the three versioning rules in action.


## The Three Rules (Review)

1. *Take Nothing Away* - Don't remove fields, endpoints, or parameters
2. *Don't Redefine Anything* - Don't change the meaning of existing elements
3. *Make All Additions Optional* - The API must work without new fields being used


## Exercise

Use the OASDiff tool to compare two versions of an API and identify breaking changes: https://www.oasdiff.com/diff-calculator


### Option A: Use the Pet Store Example

1. Start with the Pet Store API from Exercise 3: [`petstore.openapi.yaml`](../exercise-3/petstore.openapi.yaml)
2. Create a modified version that violates one of the three rules:
   - Remove a field (violates Rule 1)
   - Change a field type (violates Rule 2)
   - Add a required field (violates Rule 3)
3. Compare them in OASDiff to see the breaking changes


### Option B: Use Your Own API

1. Use the OpenAPI you created in [Exercise 3](../exercise-3) (based on your API story from [Exercise 2](../exercise-2))
2. Create an "evolved" version with several changes
3. Compare both versions in OASDiff
4. Identify which changes are breaking vs. non-breaking


## What to Look For

- **Red flags**: Breaking changes that would require client updates
- **Green additions**: Safe additions that follow Rule 3
- **Change classification**: Why OASDiff marks certain changes as breaking


## Discussion Points

- Which breaking changes surprised you?
- How would you version this API (or avoid versioning)?
- What migration path would you provide to consumers?
