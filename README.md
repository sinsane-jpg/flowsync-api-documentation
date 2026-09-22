# FlowSync API Documentation

[![Validate OpenAPI](https://github.com/sinsane-jpg/flowsync-api-documentation/actions/workflows/validate-openapi.yml/badge.svg)](https://github.com/sinsane-jpg/flowsync-api-documentation/actions/workflows/validate-openapi.yml)

A personal API documentation sample for **FlowSync**, a fictional workflow automation product. This project demonstrates how I structure an OpenAPI specification for developers working with workflows and their executions.

## Project status

FlowSync is a documentation demonstration, not a live service. The server URL is illustrative, and requests cannot be executed against a working FlowSync backend.

The specification is a work in progress. It should not be treated as a production API contract.

## My contribution

I authored the OpenAPI specification myself, with limited AI assistance during some steps.

## What the specification covers

- Creating a workflow.
- Retrieving workflow details.
- Starting a workflow execution.
- Retrieving execution status.
- API key authentication through a Bearer token.
- Request and response schemas.
- HTTP error responses.

## Explore the sample

Start with the [getting-started guide](docs/getting-started.md) for a walkthrough of authentication requirements, workflow creation, activation, execution, and monitoring.

The API definition is in [`openapi.yaml`](./openapi.yaml).

To preview it:

1. Open `openapi.yaml` in this repository.
2. Copy the raw file contents.
3. Open [Swagger Editor](https://editor.swagger.io/).
4. Paste the contents into the editor to inspect the specification and rendered reference.

The preview lets you explore the documentation structure. It does not provide a running API.

## Intended audience

This sample is written for developers integrating a workflow automation API into business applications.

## Documentation approach

The specification groups operations by resource and uses reusable schemas to keep request, response, and error structures consistent. Operation summaries and field descriptions explain the purpose of each API element. 

## Authentication

The specification uses an API key sent as a Bearer token in the HTTP `Authorization` header:

`Authorization: Bearer YOUR_API_KEY`

`YOUR_API_KEY` is a placeholder. This fictional sample does not issue real keys or provide a working authentication service.

### Assumed key setup

For this sample, an administrator creates an API key in the conceptual FlowSync dashboard and assigns the permissions required by the integration. This dashboard functionality is not implemented.

| Operation | Required permission |
|---|---|
| Create a workflow | `workflows:write` |
| Retrieve a workflow | `workflows:read` |
| Execute a workflow | `executions:write` |
| Retrieve an execution | `executions:read` |

### Authentication errors

- **401 Unauthorized:** The API key is missing or invalid. Check that the header includes `Bearer` followed by a space and a valid key.
- **403 Forbidden:** The key lacks the operation’s required permission. Ask an administrator to review its permissions.

These responses describe the intended API contract; they have not been tested against a running service.

### Handling real credentials

When adapting this sample to a real integration, keep credentials out of source files, documentation examples, and Git commits. Use an environment variable or a suitable secret-management system.

## Workflow lifecycle

This fictional sample assumes that users configure and activate workflows in a FlowSync web dashboard. The dashboard is not implemented in this repository.

1. **Create:** Use `POST /workflows` to create a workflow in draft status.
2. **Configure and activate:** In the assumed dashboard, configure the workflow’s actions and required inputs, then activate it.
3. **Verify:** Use `GET /workflows/{workflow_id}` to confirm that its status is `active`.
4. **Execute:** Use `POST /workflows/{workflow_id}/execute` to start an execution.
5. **Monitor:** Use `GET /executions/{execution_id}` to check execution status.

For this sample, only active workflows can execute. Draft and inactive workflows are not executable. Activation through the API is outside the sample’s scope. 

## Monitor an execution

Workflow execution is asynchronous: the API accepts the request before the workflow finishes.

1. Call `POST /workflows/{workflow_id}/execute` for an active workflow.
2. An HTTP `202 Accepted` response indicates that the execution has been queued. It does not mean the workflow completed successfully.
3. Read the `execution_id` from the response.
4. Call `GET /executions/{execution_id}` to retrieve its status.

| Status | Meaning | Next action |
|---|---|---|
| `pending` | The execution is waiting to start. | Check again after a delay. |
| `running` | The execution is in progress. | Check again after a delay. |
| `completed` | The execution finished successfully. | Stop polling. |
| `failed` | The execution finished unsuccessfully. | Stop polling and investigate before starting another execution. |

For this fictional sample, `completed` and `failed` are treated as terminal states.

### Polling and retries

Polling means requesting the execution status repeatedly. Avoid continuous requests: introduce a delay and set a maximum waiting time in your client. This sample does not define a polling interval or rate-limit policy.

If a status request fails, that does not necessarily mean the workflow execution failed. Likewise, if your client stops waiting, the execution may still be running.

Do not automatically submit another execution after an uncertain response. Duplicate-execution prevention is not defined in this sample.

### Current diagnostic limitation

The execution response includes status and timestamps but does not provide a failure reason or execution logs. Detailed failure diagnosis is outside the current sample’s scope.

This guidance describes the intended behavior of a fictional API; it has not been verified against a running service.

## Troubleshooting: workflow is not active

**Symptom:** An execution request returns HTTP `409 Conflict` with the error code `WORKFLOW_NOT_ACTIVE`.

**Cause:** The workflow is in draft or inactive status. Only active workflows can execute.

### Recovery steps

1. Check the workflow ID used in the execution request.
2. Call `GET /workflows/{workflow_id}` to inspect its current status.
3. If it is draft or inactive, configure and activate it in the assumed FlowSync dashboard.
4. Retrieve the workflow again and confirm that its status is `active`.
5. Retry the execution request.

If the workflow already reports `active` but execution continues to return this error, further investigation is required. This sample does not define that situation.

Dashboard activation and the error response are part of this fictional API’s intended design. They have not been tested against a running service.

## Validation

On September 22, 2026, I loaded the OpenAPI specification into Swagger Editor. The editor reported no errors and rendered the API reference.

This was a manual specification check. It does not verify runtime behavior, successful API requests, or the completeness of the documentation. FlowSync has no running backend.

To repeat the check, follow the Swagger Editor instructions under “Explore the sample.” Recheck the specification after making changes. 

After adding response examples, I rechecked the specification in Swagger Editor. No errors were reported. I also inspected the rendered examples for all four success responses and the execution operation’s `409 WORKFLOW_NOT_ACTIVE` response. The displayed statuses, timestamps, and error guidance matched the intended sample scenarios. 

### Getting-started guide review

I manually checked the README link to the getting-started guide, the guide’s troubleshooting link, and its OpenAPI specification link. All opened the intended destinations. I also reviewed the four-step sequence, code blocks, response examples, and status table for readable formatting.

This review covered navigation and presentation. The example requests were not executed against a running service.

### Automated validation

The GitHub Actions workflow in `.github/workflows/validate-openapi.yml` checks `openapi.yaml` using Redocly CLI’s `spec` ruleset. It runs on pushes to `main`, pull requests, and manual triggers.

The first automated run completed successfully. This check validates the specification against the configured rules; it does not test a running API or guarantee documentation completeness.

[View validation runs](https://github.com/sinsane-jpg/flowsync-api-documentation/actions/workflows/validate-openapi.yml)

## Current limitations

- FlowSync is fictional; requests and responses have not been tested against a running service.
- Dashboard configuration, activation, and API key management are design assumptions. No dashboard is implemented.
- Success response examples are included for all four operations.
- All 27 documented responses include illustrative examples: four success responses and 23 error responses. These examples have not been tested against a running service.
- Execution monitoring and inactive-workflow recovery are documented. Other error-recovery scenarios and detailed failure diagnostics remain limited.
- Automated checks cover specification rules. Documentation clarity, scenario completeness, and rendered examples still require manual review.

## Planned improvements
 
- Expand troubleshooting guidance for authentication, permissions, and rate-limit errors.
