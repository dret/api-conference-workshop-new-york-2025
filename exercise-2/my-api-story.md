# My API Story


## Purpose


## Data Properties
We need to keep track of the following data properties: 

 * **id** : a globally unique value for each record [uuid]

## Resources
The following are resources, or states, of the API. Each state has one or more possible Actions.

 * **Home** : The home or landing page of the API. Users typically start here.
   * Actions: 
 * **TaskCollection** : The list of tasks in the system are displayed here.
   * Actions: 
 * **TaskItem** : This resource shows a single task (selected from the TaskCollection)
 
## Actions
Each action has zero or more input properties and always has one return value (to another state). Each action is also marked as either Safe (GET), Unsafe (POST), Idempotent (PUT/PATCH), or Delete (DELETE)

 * **ShowHomePage** : Use this action to display the `home` resource
   * Inputs: None
   * Returns: **Home**
   * Type: Safe

## Rules
 * When executing **CreateNewTask**, the client should supply a unique `id` value.

