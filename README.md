# FlowSync API Documentation

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

## Validation

On September 22, 2026, I loaded the OpenAPI specification into Swagger Editor. The editor reported no errors and rendered the API reference.

This was a manual specification check. It does not verify runtime behavior, successful API requests, or the completeness of the documentation. FlowSync has no running backend.

To repeat the check, follow the Swagger Editor instructions under “Explore the sample.” Recheck the specification after making changes.

## Current limitations

- Requests have not been verified against a live FlowSync service.
- Dashboard configuration and activation are conceptual. No dashboard implementation is included.
- Authentication setup and permission guidance need expansion.
- Complete response examples and practical error-recovery guidance need improvement.

## Planned improvements

- Add a getting-started guide that explains the workflow lifecycle.
- Expand authentication and troubleshooting guidance.
- Add operation-specific response examples.
- Add automated specification validation to supplement the manual Swagger Editor check.
