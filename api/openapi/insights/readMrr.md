# Read MRR (/api/openapi/insights/readMrr)

`GET /mrr`

Returns your workspace's current MRR, including plan and usage breakouts.

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
  /mrr:
    get:
      summary: Read MRR
      tags:
        - Insights
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/MRR-Response"
              examples:
                Example:
                  value:
                    mrr:
                      amount_minor: 9915593
                      amount_formatted: $99,155.93
                      currency: Willow
                      currency_symbol: $
                      at_time: 2021-02-03T14:23:17-05:00
                      breakouts:
                        plan_amount_minor: 9913593
                        plan_amount_formatted: $99,135.93
                        usage_amount_minor: 2000
                        usage_amount_formatted: $20.00
      operationId: readMrr
      description: Returns your workspace's current MRR, including plan and usage breakouts.
      deprecated: true
      parameters:
        - schema:
            type: string
            format: date-time
          in: query
          name: at_time
          description: submit a timestamp in ISO8601 format to request MRR for a historic
            time.
        - schema:
            type: integer
          in: query
          name: subscription_id
          description: submit the id of a subscription in order to limit results.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Insights.ReadMrr(
                      new ReadMrrRequest());
                  // TODO: Handle 'response' of type MrrResponse
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
    MRR-Response:
      title: MRR response
      type: object
      properties:
        mrr:
          $ref: "#/components/schemas/MRR"
      required:
        - mrr
    MRR:
      title: MRR
      type: object
      properties:
        amount_minor:
          type: integer
          format: int64
        amount_formatted:
          type: string
        currency:
          type: string
        currency_symbol:
          type: string
        breakouts:
          $ref: "#/components/schemas/Breakouts"
        at_time:
          type: string
          format: date-time
          description: ISO8601 timestamp
    Breakouts:
      title: Breakouts
      type: object
      properties:
        plan_amount_minor:
          type: integer
          format: int64
        plan_amount_formatted:
          type: string
        usage_amount_minor:
          type: integer
          format: int64
        usage_amount_formatted:
          type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```