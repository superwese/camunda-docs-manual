---

title: "Get Single External Task"
weight: 10

menu:
  main:
    name: "Get"
    identifier: "rest-api-external-task-get"
    parent: "rest-api-external-task"
    pre: "GET `/external-task/{id}`"

---


Retrieves a single external task corresponding to the `ExternalTask` interface in the engine.


# Method

GET `/external-task/{id}`


# Parameters

## Path Parameters

<table class="table table-striped">
  <tr>
    <th>Name</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>id</td>
    <td>The id of the external task to be retrieved.</td>
  </tr>
</table>


# Result

A JSON object corresponding to the `ExternalTask` interface in the engine.
Its properties are as follows:

<table class="table table-striped">
  <tr>
    <th>Name</th>
    <th>Value</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>activityId</td>
    <td>String</td>
    <td>The id of the activity that this external task belongs to.</td>
  </tr>
  <tr>
    <td>activityInstanceId</td>
    <td>String</td>
    <td>The id of the activity instance that the external task belongs to.</td>
  </tr>
  <tr>
    <td>errorMessage</td>
    <td>String</td>
    <td>The error message that was supplied when the last failure of this task was reported.</td>
  </tr>
  <tr>
    <td>executionId</td>
    <td>String</td>
    <td>The id of the execution that the external task belongs to.</td>
  </tr>
  <tr>
    <td>id</td>
    <td>String</td>
    <td>The external task's id.</td>
  </tr>
  <tr>
    <td>lockExpirationTime</td>
    <td>String</td>
    <td>The date that the task's most recent lock expires or has expired.</td>
  </tr>
  <tr>
    <td>processDefinitionId</td>
    <td>String</td>
    <td>The id of the process definition the external task is defined in.</td>
  </tr>
  <tr>
    <td>processDefinitionKey</td>
    <td>String</td>
    <td>The key of the process definition the external task is defined in.</td>
  </tr>
  <tr>
    <td>processInstanceId</td>
    <td>String</td>
    <td>The id of the process instance the external task belongs to.</td>
  </tr>
  <tr>
    <td>tenantId</td>
    <td>String</td>
    <td>The id of the tenant the external task belongs to.</td>
  </tr>
  <tr>
    <td>retries</td>
    <td>Number</td>
    <td>The number of retries the task currently has left.</td>
  </tr>
  <tr>
    <td>suspended</td>
    <td>Boolean</td>
    <td>A flag indicating whether the external task is suspended or not.</td>
  </tr>
  <tr>
    <td>workerId</td>
    <td>String</td>
    <td>The id of the worker that posesses or posessed the most recent lock.</td>
  </tr>
  <tr>
    <td>topicName</td>
    <td>String</td>
    <td>The external task's topic name.</td>
  </tr>
</table>


# Response Codes

<table class="table table-striped">
  <tr>
    <th>Code</th>
    <th>Media type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>200</td>
    <td>application/json</td>
    <td>Request successful.</td>
  </tr>
  <tr>
    <td>404</td>
    <td>application/json</td>
    <td>External task with the given id does not exist. See the <a href="{{< relref "reference/rest/overview/index.md#error-handling" >}}">Introduction</a> for the error response format.</td>
  </tr>
</table>

# Example

## Request

GET `/external-task/anExternalTaskId`

## Response

    {
      "activityId": "anActivityId",
      "activityInstanceId": "anActivityInstanceId",
      "errorMessage": "anErrorMessage",
      "executionId": "anExecutionId",
      "id": "anExternalTaskId",
      "lockExpirationTime": "2015-10-06T16:34:42",
      "processDefinitionId": "aProcessDefinitionId",
      "processDefinitionKey": "aProcessDefinitionKey",
      "processInstanceId": "aProcessInstanceId",
      "tenantId": null,
      "retries": 3,
      "suspended": false,
      "workerId": "aWorkerId",
      "topicName": "aTopic"
    }
