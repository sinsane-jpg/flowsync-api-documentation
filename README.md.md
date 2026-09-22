# FlowSync API Documentation

[![Validate OpenAPI](https://github.com/sinsane-jpg/flowsync-api-documentation/actions/workflows/validate-openapi.yml/badge.svg)](https://github.com/sinsane-jpg/flowsync-api-documentation/actions/workflows/validate-openapi.yml)

A personal documentation sample for **FlowSync**, a fictional workflow automation API. Written for developers, it demonstrates an OpenAPI reference, a task-based walkthrough, response examples, and troubleshooting guidance.

**This is a documentation demonstration.** There is no running backend or dashboard. Server addresses, credentials, and responses are illustrative; requests cannot be executed against a working FlowSync service.

## Explore the sample

| Resource | What you will find |
|---|---|
| [Getting-started guide](docs/getting-started.md) | Create a workflow, activate it through an assumed dashboard, start an execution, and check its result. |
| [OpenAPI specification](openapi.yaml) | Four operations, reusable schemas, Bearer authentication, and 27 response examples: four success and 23 error responses. |
| [Troubleshooting](#troubleshooting-workflow-is-not-active) | Diagnose and recover from an inactive-workflow error. |
| [Validation workflow](.github/workflows/validate-openapi.yml) | Automated specification checks using GitHub Actions and Redocly CLI. |

To preview the reference, copy the raw contents of `openapi.yaml` into [Swagger Editor](https://editor.swagger.io/). Inspect the rendered operations and examples; the preview does not provide a running API.

## My contribution

I authored the initial OpenAPI specification with limited AI assistance. I later expanded and corrected the documentation and validation workflow with AI assistance. I reviewed rendered examples in Swagger Editor, checked guide navigation and formatting, and confirmed successful GitHub Actions validation runs.

## Design decisions

- **Dashboard activation:** New workflows are drafts. Configuration and activation are assumed dashboard functions; only active workflows can execute.
- **Asynchronous execution:** `202 Accepted` acknowledges a queued execution. Callers check its status separately; `completed` and `failed` are terminal states.
- **Reusable schemas:** Shared request, response, and error structures keep the reference consistent. Each documented response has an illustrative example.

## Authentication

Send an API key using `Authorization: Bearer YOUR_API_KEY`. Key creation and permission assignment are assumed administrator tasks in the conceptual dashboard; this sample issues no credentials.

| Operation | Required permission |
|---|---|
| Create a workflow | `workflows:write` |
| Retrieve a workflow | `workflows:read` |
| Execute a workflow | `executions:write` |
| Retrieve an execution | `executions:read` |

A `401` response indicates a missing or invalid key; check the header and credential. A `403` response indicates insufficient permission; ask an administrator to review it. In a real integration, keep credentials out of source files and Git commits; use environment variables or a secret-management system.

## Troubleshooting: workflow is not active

**Symptom:** An execution request returns `409 Conflict` with `WORKFLOW_NOT_ACTIVE`.

**Cause:** The workflow is draft or inactive.

1. Confirm the workflow ID and retrieve it with `GET /workflows/{workflow_id}`.
2. Configure and activate it in the assumed dashboard.
3. Retrieve it again and confirm `status: active`.
4. Retry the execution request.

If it already reports `active` but the error persists, further investigation is needed; this sample does not define that situation.

## Validation

- **Manual review:** The specification was checked in Swagger Editor. The four success examples and inactive-workflow error example were inspected, along with guide links, code blocks, and tables.
- **Automated checks:** Redocly CLI's `spec` ruleset checks `openapi.yaml` on pushes to `main`, pull requests, and manual triggers. [View validation runs](https://github.com/sinsane-jpg/flowsync-api-documentation/actions/workflows/validate-openapi.yml).

These checks do not test runtime behavior or guarantee complete, usable instructions. Recheck rendered examples and guide content after changes.

## Limitations and next steps

- Requests and responses have not been tested against a running service; dashboard behavior is conceptual.
- Rate-limit policy, duplicate-execution prevention, and detailed failure diagnostics are undefined. The guide explains polling and uncertain outcomes within these limits.
- Next: expand troubleshooting guidance for authentication, permissions, and rate-limit errors.
