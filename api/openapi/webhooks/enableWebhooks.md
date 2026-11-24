# Enable Webhooks (/api/openapi/webhooks/enableWebhooks)

`PUT /webhooks/settings`

Enables webhooks for your workspace.

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
  /webhooks/settings:
    put:
      summary: Enable Webhooks
      tags:
        - Webhooks
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Enable-Webhooks-Response"
              examples:
                Example:
                  value:
                    webhooks_enabled: true
      operationId: enableWebhooks
      description: Enables webhooks for your workspace.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Enable-Webhooks-Request"
            examples:
              Example:
                value:
                  webhooks_enabled: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.WebhooksApi.EnableWebhooks(
                      new EnableWebhooksOperationRequest
                      {
                          Body = new EnableWebhooksRequest
                          {
                              WebhooksEnabled = true,
                          },
                      });
                  // TODO: Handle 'response' of type EnableWebhooksResponse
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
    Enable-Webhooks-Response:
      title: Enable Webhooks Response
      description: ""
      type: object
      properties:
        webhooks_enabled:
          type: boolean
    Enable-Webhooks-Request:
      title: Enable Webhooks Request
      description: ""
      type: object
      properties:
        webhooks_enabled:
          type: boolean
      required:
        - webhooks_enabled
info:
  title: Ombrevix Billing
  version: "1.0"
```