# API Conference New York 2025: Exercise 6  (Overlay)

## API Overlays
Use this online tool to complete this exercise:

https://overlay.speakeasy.com/

Use this root OpenAPI document to start:

https://github.com/dret/api-conference-workshop-new-york-2025/blob/main/exercise-6/tasks-openapi.yaml

### Example 1:
Modify the description of the OpenAPI Document root.

To do this perform an `update` action on the target : `$.info.description`

Add some new description string

For reference, see:

https://github.com/dret/api-conference-workshop-new-york-2025/blob/main/exercise-6/tasks-overlay-description.yaml

To check results, see:

https://github.com/dret/api-conference-workshop-new-york-2025/blob/main/exercise-6/task-openapi-description.yaml

### Example 2:
Remove the `/doAssignUserToTask` endpoint from the OpenAPI Document

To do this perform a `remove: true` on the target: `$.paths['/doAssignUserToTask']`

For reference, see:

https://github.com/dret/api-conference-workshop-new-york-2025/blob/main/exercise-6/tasks-overlay-remove.yaml

To check your results, see:

https://github.com/dret/api-conference-workshop-new-york-2025/blob/main/exercise-6/task-openapi-no-assign.yaml

### Example 3:
For extra credit, update the `example` values in the `$.components.schemas.[XXX].example`

For reference, see:

https://github.com/dret/api-conference-workshop-new-york-2025/blob/main/exercise-6/tasks-overlay-examples.yaml

To check your results, see:

https://github.com/dret/api-conference-workshop-new-york-2025/blob/main/exercise-6/task-openapi-examples.yaml


