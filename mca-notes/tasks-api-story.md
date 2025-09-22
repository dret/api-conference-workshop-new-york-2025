# Task Management


## Purpose
We need to track 'Task' records in order to improve both timeliness and accuracy of customer follow-up activity.


## Data Properties
In this first pass at the application, we need to keep track of the following data properties: 

 * **id** : a globally unique value for each record [uuid]
 * **title** : the text content of the record (the title of the task) [string]
 * **description** : the description of the record (the task details) [text]
 * **dueDate** : the date the record is due to be completed [date]
 * **status** : Indicates the status of the record (active, completed) [enumerated string]
 * **priority** : the priority of the task (a number between 1 and 5) [enumerated number]
 * **assignedUser** : the user assigned to handle the task (a simple name string) [string]


## Resources
The following are resources, or states, of the API. Each state has one or more possible Actions.

 * **Home** : The home or landing page of the API. Users typically start here.
   * Actions: **GetTaskList**, **GetFilteredTaskList**, **ShowHomePage**
 * **TaskCollection** : The list of tasks in the system are displayed here.
   * Actions: **ShowHomePage**, **GetTaskList**, **GetFilteredTaskList**, **CreateNewTask**,**GetTaskItem**
 * **TaskItem** : This resource shows a single task (selected from the TaskCollection)
   * Actions: **EditExistingTask**, **UpdateStatusOfTask**, **SetDueDateOfTask**, **AssignUserToTask**, **GetTaskList**, **GetFilteredTaskList**, **ShowHomePage**
 
## Actions
This edition of the application needs to support the following operations. Each action has zero or more input properties and always has one return value (to another state). Each action is also marked as either Safe (GET), Unsafe (POST), Idempotent (PUT/PATCH), or Delete (DELETE)

 * **ShowHomePage** : Use this action to display the `home` resource
   * Inputs: None
   * Returns: **Home**
   * Type: Safe
 * **GetTaskCollection** : Use this action to return a list of all Task records in the system
   * Inputs: None
   * Returns: **TaskCollection**
   * Type: Safe
 * **GetTaskItem**: Use this action to get a single existing task record.  
   * Inputs: id
   * Required: id
   * Returns: **TaskItem**
   * Type: Safe
 * **CreateNewTask** : Use this action to add a new Task record to the system
   * Inputs: id, title, description, dueDate, status, priority, assignedUser
   * Required: id, title, status
   * Returns: **TaskCollection**
   * Type: Unsafe
 * **EditExistingTask** : Use this action to modify an existing Task record to the system
   * Inputs: id, title, description, dueDate, status, priority, assignedUser
   * Required: id, title, status
   * Returns: **TaskItem**
   * Type: Unsafe
 * **UpdateStatusOfTask** : Use this action to update the `status` of a single record
   * Inputs: id, status
   * Required: id, status
   * Returns: **TaskItem**
   * Type: Idempotent
 * **SetDueDateOfTask** : Use this action to set the `dueDate` of a single record
   * Inputs: id, dueDate
   * Required: id, dueDate
   * Returns: **TaskItem**
   * Type: Idempotent
 * **AssignUserToTask** : Use this action to update the name of the `assignedUser` of a single record
   * Inputs: id, assignedUser
   * Required: id, assignedUser
   * Returns: **TaskItem**
   * type: Idempotent
 * **GetFilteredTaskCollection** : Use this action to filter the list of tasks by `title`, `dueDate`, `status`, `priority`  and/or `assignedUser`
   * Inputs: title, dueDate,status, priority
   * Returns: **TaskCollection**
   * Type: Safe

## Rules
 * When executing **CreateNewTask**, the client should supply a unique `id` value.

