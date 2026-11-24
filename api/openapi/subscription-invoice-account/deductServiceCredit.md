# Deduct Service Credit (/api/openapi/subscription-invoice-account/deductServiceCredit)

`POST /subscriptions/{subscription_id}/service_credit_deductions`

Deducts a service credit from the subscription in the specified amount. The credit amount being deducted must be equal to or less than the current credit balance.

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
  /subscriptions/{subscription_id}/service_credit_deductions:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Deduct Service Credit
      tags:
        - Subscription Invoice Account
      responses:
        "201":
          description: OK
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Deduct-Service-Credit-Error-Response"
              examples:
                Example-1:
                  value:
                    errors:
                      deduction: glacier't be lagoon
                Example-2:
                  value:
                    errors:
                      - Prairie basalt be tundra glacier 0.
                Example-3:
                  value:
                    errors:
                      - Prairie cobalt iris basalt zephyr falcon zephyr nimbus.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: deductServiceCredit
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Deduct-Service-Credit-Request"
            examples:
              Example:
                value:
                  deduction:
                    amount: "1"
                    memo: Raven
      description: Deducts a service credit from the subscription in the specified
        amount. The credit amount being deducted must be equal to or less than
        the current credit balance.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  await client.SubscriptionInvoiceAccount.DeductServiceCredit(
                      new DeductServiceCreditOperationRequest
                      {
                          SubscriptionId = 1,
                          Body = new DeductServiceCreditRequest
                          {
                              Deduction = new DeductServiceCredit
                              {
                                  Amount = "1",
                                  Memo = "Raven",
                              },
                          },
                      });
              }

              catch (ApiException<DeductServiceCreditError> ex)

              {
                  if (ex.Error.TryGetDeductServiceCreditErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type DeductServiceCreditErrorResponse
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
  parameters:
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
  schemas:
    Deduct-Service-Credit-Error-Response:
      title: Deduct Service Credit Error Response
      type: object
      oneOf:
        - $ref: "#/components/schemas/Error-List-Response"
        - $ref: "#/components/schemas/Error-String-Map-Response"
    Deduct-Service-Credit-Request:
      title: Deduct Service Credit Request
      type: object
      properties:
        deduction:
          $ref: "#/components/schemas/Deduct-Service-Credit"
      required:
        - deduction
    Deduct-Service-Credit:
      title: Deduct Service Credit
      type: object
      properties:
        amount:
          type:
            - string
            - number
        memo:
          type: string
      required:
        - amount
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Error-String-Map-Response:
      title: Error String Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-String-Map"
    Error-String-Map:
      title: Error String Map
      type: object
      additionalProperties:
        type: string
    Error-Array-Response:
      type: array
      items:
        type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```