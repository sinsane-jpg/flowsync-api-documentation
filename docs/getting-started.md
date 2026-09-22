# Getting started with FlowSync

This guide walks through creating a workflow, activating it, starting an execution, and checking the result.

## About this sample

FlowSync is a fictional product created to demonstrate API documentation. There is no running backend or dashboard.

Requests and responses in this guide are illustrative. They cannot be executed against the example server. Dashboard activation and API key management are assumptions of the sample.

## Before you begin

To follow the walkthrough, you should understand basic HTTP requests and JSON.

In a working implementation of this design, you would need:

- An API key with `workflows:write`, `workflows:read`, `executions:write`, and `executions:read` permissions.
- Access to the dashboard to configure and activate a workflow.
- An HTTP client, such as cURL or Postman.

The sample does not provide credentials or dashboard access.

## 1. Create a workflow

Send a `POST` request to `/workflows` with a name and an optional description.

This request is illustrative. The example address has no running FlowSync service, and `YOUR_API_KEY` is a placeholder.

```bash
curl --request POST \
  --url https://api.flowsync.example.com/v1/workflows \
  --header 'Authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "name": "Customer Onboarding",
    "description": "Automates the customer onboarding process."
  }'
```

This command uses Bash-style line continuation.

- `POST` requests creation of a workflow.
- `Authorization` supplies the API credential.
- `Content-Type` identifies the request body as JSON.
- `name` is required; `description` is optional.

### Expected response

The intended success response is HTTP `201 Created` with a body like this:

```json
{
  "id": "wf_1024",
  "name": "Customer Onboarding",
  "description": "Automates the customer onboarding process.",
  "status": "draft",
  "created_at": "2026-09-22T10:00:00Z",
  "updated_at": "2026-09-22T10:00:00Z"
}
```

Save the returned `id` for subsequent requests. `wf_1024` is an example value.

The workflow starts in `draft` status. It must be configured and activated in the assumed dashboard before execution.

## 2. Activate and verify the workflow

The newly created workflow is in `draft` status and cannot execute.

This sample assumes that an authorized user configures the workflow’s actions and required inputs, then activates it in the FlowSync dashboard. No dashboard is implemented, so this is a conceptual step rather than a procedure you can perform.

After activation, retrieve the workflow to verify its status:

```bash
curl --request GET \
  --url https://api.flowsync.example.com/v1/workflows/wf_1024 \
  --header 'Authorization: Bearer YOUR_API_KEY'
```

Replace `wf_1024` with the ID returned by the creation request. Retrieving the workflow requires `workflows:read` permission.

### Expected response

The intended success response is HTTP `200 OK`. After activation, an illustrative response is:

```json
{
  "id": "wf_1024",
  "name": "Customer Onboarding",
  "description": "Automates the customer onboarding process.",
  "status": "active",
  "created_at": "2026-09-22T10:00:00Z",
  "updated_at": "2026-09-22T10:04:00Z"
}
```

Check the `status` field before proceeding:

- `active`: The workflow can execute.
- `draft` or `inactive`: Configuration or activation is still required.

HTTP `200 OK` means the retrieval succeeded. It does not, by itself, mean the workflow is active. This GET request reports the workflow’s state; it does not change it.

## 3. Start an execution

After confirming that the workflow is active, send an execution request. This operation requires `executions:write` permission.

For this walkthrough, assume the workflow was configured in the conceptual dashboard to accept a `customer_id` input. Other workflows may require different inputs.

```bash
curl --request POST \
  --url https://api.flowsync.example.com/v1/workflows/wf_1024/execute \
  --header 'Authorization: Bearer YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "input": {
      "customer_id": "cus_2048"
    }
  }'
```

Replace `wf_1024` with your workflow ID. `cus_2048` is an illustrative customer identifier.

### Expected response

The intended response is HTTP `202 Accepted` with a body like this:

```json
{
  "execution_id": "exec_7821",
  "workflow_id": "wf_1024",
  "status": "pending",
  "created_at": "2026-09-22T10:05:00Z",
  "started_at": null,
  "completed_at": null
}
```

Save the returned `execution_id` for the next step.

`202 Accepted` means the request was accepted for processing—not that the workflow completed successfully. In this example, `pending` indicates that execution has not started, so both start and completion timestamps are `null`.

If the request returns `409 WORKFLOW_NOT_ACTIVE`, follow the [inactive-workflow troubleshooting guide](../README.md#troubleshooting-workflow-is-not-active).

If the request times out or the connection drops, do not automatically submit another execution. The first request may have been accepted, and duplicate-execution prevention is not defined in this sample.

## 4. Check the execution result

Use the `execution_id` returned by the execution request to retrieve its status. This operation requires `executions:read` permission.

```bash
curl --request GET \
  --url https://api.flowsync.example.com/v1/executions/exec_7821 \
  --header 'Authorization: Bearer YOUR_API_KEY'
```

Replace `exec_7821` with the returned execution ID.

### Expected response

The intended success response is HTTP `200 OK`. After successful execution, an illustrative response is:

```json
{
  "execution_id": "exec_7821",
  "workflow_id": "wf_1024",
  "status": "completed",
  "created_at": "2026-09-22T10:05:00Z",
  "started_at": "2026-09-22T10:05:02Z",
  "completed_at": "2026-09-22T10:05:08Z"
}
```

HTTP `200 OK` confirms that the status lookup succeeded. Read `status` to determine the execution’s outcome:

| Status | Next action |
|---|---|
| `pending` | Wait before checking the same execution again. |
| `running` | Wait before checking the same execution again. |
| `completed` | Stop checking; the execution succeeded. |
| `failed` | Stop checking and investigate before starting another execution. |

For this sample, `completed` and `failed` are terminal states.

### If execution is still in progress

Repeated status checks are called polling. Introduce a delay between requests and set a maximum waiting time in your client. This sample does not specify a polling interval or rate-limit policy.

If your client stops waiting, the execution may still be running. A failed status request also does not establish that the execution failed. Check the same execution again when the connection is restored.

The response does not include failure reasons or execution logs, so detailed diagnosis of a failed execution is outside this sample’s scope.

## What this walkthrough demonstrates

You have followed the intended sequence for creating a workflow, activating it through an assumed dashboard, starting an execution, and checking its outcome.

All requests and responses are illustrative. They have not been tested against a running FlowSync service.

For the operation definitions, see the [OpenAPI specification](../openapi.yaml).
