_Last updated at: 2025-10-20_

# **Integrate Workflow**

AltaBots.ai workflow can serve as an API for integration.

## **Request Method**

`POST`

## **Request URL**

`https://altatech.ai/v1/workflow/invoke`

## **Request Authentication**

Please refer to Overview in API Reference.

## **Request**

|  curl \--location \--request POST 'https://altatech.ai/v1/workflow/invoke' \\ \--header 'Authorization: \<your\_api\_key\>' \\ \--header 'Content-Type: application/json' \\ \--data-raw '{     "userId": "\<your\_user\_id\>",     "input": {         \<your\_start\>     } }'  |
| :---- |

In this Request, replace `<your_api_key>` with the API key you just copied. And replace `your_start` with the input parameters specified in the **Start** node of the workflow.

| Field | Type | Required | Description |
| ----- | ----- | ----- | ----- |
| userId | string | No | Serves as an identifier of the initiator of this request. |
| input | object | Yes | Represents the **Start** node of the workflow. The parameters in this field must match the parameters specified in the **Start** node of the workflow. |

#### **Response**

|  {     "workflowRunId": "xxxx-5b15-4cbf-999c-1b218934xxxx",     "input": {         "input": "What should I do after a breakup?"     },     "output": {         "output": "Breakups are a normal part of life. While it can be painful, it can also serve as an opportunity for growth and self-reflection. Here are some suggestions that might help you: \\n\\n..."     },     "workflowExecutionTime": 24277,     "status": "SUCCEED",     "totalCost": 1.3754,     "totalTokens": 3464,     "startTime": 1741768313025,     "endTime": 1741768337310 }  |
| :---- |

| Field | Type | Description |
| ----- | ----- | ----- |
| workflowRunId | string | Operation ID of the workflow, serving as the unique identifier for this execution. |
| input | object | Input content of the **Start** node. This must match the content of the "input field" in the Request. |
| output | object | Output content of the **End** node. This will also show the workflow's execution result. |
| workflowExecutionTime | number | Time taken for executing the workflow (Unit: ms). |
| status | string | The integration status of the workflow. Possible values include: \* SUCCEED：Successful integration \* FAILED：Failed integration \* RUNNING：Running workflow |
| totalCost | number | Total credits consumed (consumption of all nodes); unit: credit. |
| totalTokens | number | Total Token consumed (consumption of all nodes) |
| startTime | number | Timestamp of action start (unit: ms) |
| endTime | number | Timestamp of action end (unit: ms) |
