# Update Reason Code (/api/openapi/reason-codes/updateReasonCode)

`PUT /reason_codes/{reason_code_id}`

Updates an existing reason code for a given workspace.

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
    put:
      summary: Update Reason Code
      tags:
        - Reason Codes
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Reason-Code-Response"
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Example:
                  value:
                    errors:
                      - "Harbor: Quartz harbor is onyx in fjord."
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: updateReasonCode
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Update-Reason-Code-Request"
      description: Updates an existing reason code for a given workspace.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.ReasonCodes.UpdateReasonCode(
                      new UpdateReasonCodeOperationRequest
                      {
                          ReasonCodeId = 1,
                      });
                  // TODO: Handle 'response' of type ReasonCodeResponse
              }
              catch (ApiException<UpdateReasonCodeError> ex)
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
    Reason-Code-Response:
      title: Reason Code Response
      type: object
      properties:
        reason_code:
          $ref: "#/components/schemas/Reason-Code"
      required:
        - reason_code
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Update-Reason-Code-Request:
      title: Update Reason Code Request
      type: object
      properties:
        reason_code:
          $ref: "#/components/schemas/Update-Reason-Code"
      required:
        - reason_code
    Update-Reason-Code:
      title: Update Reason Code
      type: object
      properties:
        code:
          type: string
          description: The unique identifier for the ReasonCode
        description:
          type: string
          description: The friendly summary of what the code signifies
        position:
          type: integer
          description: The order that code appears in lists
    Error-Array-Response:
      type: array
      items:
        type: string
    Reason-Code:
      title: Reason Code
      type: object
      properties:
        id:
          type: integer
        workspace_id:
          type: integer
        code:
          type: string
        description:
          type: string
        position:
          type: integer
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
info:
  title: Ombrevix Billing
  version: "1.0"
```