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

## Walkthrough status

The step-by-step requests and responses are being added. For the current operation definitions, see the [OpenAPI specification](../openapi.yaml).
