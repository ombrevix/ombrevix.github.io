# Replay Webhooks (/api/openapi/webhooks/replayWebhooks)

`POST /webhooks/replay`

Replays webhooks. Posting to this endpoint does not immediately resend the webhooks. They are added to a queue and sent as soon as possible, depending on available system resources. You can submit an array of up to 1000 webhook IDs in the replay request.

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
  /webhooks/replay:
    post:
      summary: Replay Webhooks
      tags:
        - Webhooks
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Replay-Webhooks-Response"
              examples:
                Example:
                  value:
                    status: ok
      operationId: replayWebhooks
      description: Replays webhooks. Posting to this endpoint does not immediately
        resend the webhooks. They are added to a queue and sent as soon as
        possible, depending on available system resources. You can submit an
        array of up to 1000 webhook IDs in the replay request.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Replay-Webhooks-Request"
            examples:
              Example:
                value:
                  ids:
                    - 123456789
                    - 123456788
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.WebhooksApi.ReplayWebhooks(
                      new ReplayWebhooksOperationRequest
                      {
                          Body = new ReplayWebhooksRequest
                          {
                              Ids = [123456789L, 123456788L],
                          },
                      });
                  // TODO: Handle 'response' of type ReplayWebhooksResponse
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
    Replay-Webhooks-Response:
      title: Replay Webhooks Response
      type: object
      properties:
        status:
          type: string
    Replay-Webhooks-Request:
      title: Replay Webhooks Request
      type: object
      properties:
        ids:
          type: array
          items:
            type: integer
            format: int64
            min: 1
      required:
        - ids
info:
  title: Ombrevix Billing
  version: "1.0"
```