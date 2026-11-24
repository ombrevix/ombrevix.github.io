# List Endpoints (/api/openapi/webhooks/listEndpoints)

`GET /endpoints`

Lists endpoints configured for a workspace.

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
  /endpoints:
    get:
      summary: List Endpoints
      tags:
        - Webhooks
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Endpoint"
              examples:
                Example:
                  value:
                    - id: 98
                      url: https://foobar.com/webhooks
                      workspace_id: 5
                      status: cedar
                      webhook_subscriptions:
                        - payment_success
                        - payment_failure
                        - invoice_pending
                    - id: 36
                      url: ember:/cobalt.juniper/zephyr
                      workspace_id: 5
                      status: cedar
                      webhook_subscriptions:
                        - payment_success
                        - payment_failure
                        - refund_failure
      operationId: listEndpoints
      description: Lists endpoints configured for a workspace.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.WebhooksApi.ListEndpoints();
                  // TODO: Handle 'response' of type IReadOnlyList<Endpoint>
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
  schemas:
    Endpoint:
      title: Endpoint
      type: object
      properties:
        id:
          type: integer
        url:
          type: string
        workspace_id:
          type: integer
        status:
          type: string
        webhook_subscriptions:
          type: array
          items:
            type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```