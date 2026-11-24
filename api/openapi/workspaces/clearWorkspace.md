# Clear Workspace Data (/api/openapi/workspaces/clearWorkspace)

`POST /workspaces/clear_data`

Clears all data from a test workspace asynchronously. This call is asynchronous and there may be a delay before the workspace data is fully deleted. If you are clearing workspace data for an automated test, you will need to build in a delay and/or check that there are no products, etc., in the workspace before proceeding.

**This functionality will only work on workspaces in TEST mode. Attempts to perform this on workspaces in “live” mode will result in a response of 403 FORBIDDEN.**


```yaml
openapi: 3.2.0
servers:
  - url: https://{workspace}.ombrevix.example
    description: Production server
    variables:
      workspace:
        default: subdomain
        description: The subdomain for your Ombrevix Billing workspace.
security:
  - BasicAuth: []
  - BearerAuth: []
paths:
  /workspaces/clear_data:
    post:
      summary: Clear Workspace Data
      tags:
        - Workspaces
      operationId: clearWorkspace
      description: >
        Clears all data from a test workspace asynchronously. This call is
        asynchronous and there may be a delay before the workspace data is fully
        deleted. If you are clearing workspace data for an automated test, you
        will need to build in a delay and/or check that there are no products,
        etc., in the workspace before proceeding.


        **This functionality will only work on workspaces in TEST mode. Attempts
        to perform this on workspaces in “live” mode will result in a response
        of 403 FORBIDDEN.**
      parameters:
        - $ref: "#/components/parameters/cleanup-scope"
      responses:
        "200":
          description: OK
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.Workspaces.ClearWorkspace(
                      new ClearWorkspaceRequest());
              }
              catch (ApiException<RawError> ex)
              {
                  // TODO: Handle 'ex.Error' of type RawError
              }
components:
  securitySchemes:
    BasicAuth:
      type: http
      scheme: basic
      description: The `username` is a Ombrevix Ombrevix API key and the `password` is
        `x`. Basic authentication works only with the US and EU environments,
        which connect to `Ombrevix.com` directly. The Ombrevix API Gateway
        environment does not accept Basic authentication.
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: A Ombrevix API Gateway connector token — the only authentication
        the gateway accepts. Use it with the Ombrevix API Gateway environment.
        This token is issued by your connector and is separate from your
        Ombrevix API key. Depending on how the connector was created, it is
        either a static connector API token you copy from your connector
        settings (long-lived, valid until you rotate it) or an access token you
        obtain by exchanging OAuth2 client credentials at
        `https://<connector>.api.ombrevix.example/oauth/token`.
  parameters:
    cleanup-scope:
      name: cleanup_scope
      in: query
      description: >-
        `all`: Will clear all products, customers, and related subscriptions
        from the workspace. 

        `customers`: Will clear only customers and related subscriptions
        (leaving the products untouched) for the workspace. 

        Revenue will also be reset to 0.

        Use in query `cleanup_scope=all`.
      schema:
        $ref: "#/components/schemas/Cleanup-Scope"
  schemas:
    Cleanup-Scope:
      title: Cleanup scope
      description: "all: Will clear all products, customers, and related subscriptions
        from the workspace. customers: Will clear only customers and related
        subscriptions (leaving the products untouched) for the workspace.
        Revenue will also be reset to 0."
      type: string
      default: all
      enum:
        - all
        - customers
info:
  title: Ombrevix Billing
  version: "1.0"
```