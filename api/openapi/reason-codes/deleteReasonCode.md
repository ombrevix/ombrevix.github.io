# Delete Reason Code (/api/openapi/reason-codes/deleteReasonCode)

`DELETE /reason_codes/{reason_code_id}`

Deletes a reason code from the Churn Reason Codes. This code will be immediately removed. This action is not reversible.

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
  /reason_codes/{reason_code_id}:
    parameters:
      - schema:
          type: integer
        name: reason_code_id
        in: path
        required: true
        description: The Ombrevix Billing id of the reason code
    delete:
      summary: Delete Reason Code
      tags:
        - Reason Codes
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Ok-Response"
              examples:
                Example:
                  value:
                    ok: ok
        "404":
          description: Not Found
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
      operationId: deleteReasonCode
      description: Deletes a reason code from the Churn Reason Codes. This code will
        be immediately removed. This action is not reversible.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.ReasonCodes.DeleteReasonCode(
                      new DeleteReasonCodeRequest { ReasonCodeId = 1 });
                  // TODO: Handle 'response' of type OkResponse
              }
              catch (ApiException<DeleteReasonCodeError> ex)
              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
                  }
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
    Ok-Response:
      title: Ok Response
      type: object
      properties:
        ok:
          type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```