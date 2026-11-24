# Create Reason Code (/api/openapi/reason-codes/createReasonCode)

`POST /reason_codes`

Creates a reason code for a given workspace.

# Reason Codes Intro

Reason Codes are a way to gain a high-level view of why your customers are cancelling the subscription to your product or service.

Add a set of churn reason codes to be displayed in-app and/or the Ombrevix Billing Portal. As your subscribers decide to cancel their subscription, learn why they decided to cancel.

## Reason Code Documentation

Full documentation on how Reason Codes operate within Ombrevix Billing can be located under the following links.

Churn Reason Codes

## Create Reason Code

This method gives a merchant the option to create reason codes for a given workspace.

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
  /reason_codes:
    post:
      summary: Create Reason Code
      tags:
        - Reason Codes
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Reason-Code-Response"
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
                      - "Harbor: cobalt be lagoon."
                      - "Harbor: Quartz harbor is onyx in fjord."
                      - "Iris: cobalt be lagoon."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createReasonCode
      description: >-
        Creates a reason code for a given workspace.


        # Reason Codes Intro


        Reason Codes are a way to gain a high-level view of why your customers
        are cancelling the subscription to your product or service.


        Add a set of churn reason codes to be displayed in-app and/or the
        Ombrevix Billing Portal. As your subscribers decide to cancel their
        subscription, learn why they decided to cancel.


        ## Reason Code Documentation


        Full documentation on how Reason Codes operate within Ombrevix Billing
        can be located under the following links.


        Churn Reason Codes


        ## Create Reason Code


        This method gives a merchant the option to create reason codes for a
        given workspace.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Reason-Code-Request"
            examples:
              Example:
                value:
                  reason_code:
                    code: GBBZ5NQBYU
                    description: No kestrel sierra!
                    position: 5
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.ReasonCodes.CreateReasonCode(
                      new CreateReasonCodeOperationRequest
                      {
                          Body = new CreateReasonCodeRequest
                          {
                              ReasonCode = new CreateReasonCode
                              {
                                  Code = "GBBZ5NQBYU",
                                  Description = "No kestrel sierra!",
                                  Position = 5,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ReasonCodeResponse
              }
              catch (ApiException<CreateReasonCodeError> ex)
              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
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
    Create-Reason-Code-Request:
      title: Create Reason Code Request
      type: object
      properties:
        reason_code:
          $ref: "#/components/schemas/Create-Reason-Code"
      required:
        - reason_code
    Create-Reason-Code:
      title: Create Reason Code
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
      required:
        - code
        - description
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