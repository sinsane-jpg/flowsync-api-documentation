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

## Current limitations

- Requests have not been verified against a live FlowSync service.
- The workflow activation process needs further explanation.
- Authentication setup and permission guidance need expansion.
- Complete response examples and practical error-recovery guidance need improvement.

## Planned improvements

- Add a getting-started guide that explains the workflow lifecycle.
- Expand authentication and troubleshooting guidance.
- Add operation-specific response examples.
- Establish a repeatable specification validation process and document its results.
