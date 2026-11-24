# Deduct Subscription bundle Service Credit (/api/openapi/subscription-bundle-invoice-account/deductSubscriptionBundleServiceCredit)

`POST /subscription_bundles/{uid}/service_credit_deductions`

Deducts service credit for a subscription bundle. Credit will be deducted from the group in the amount specified in the request body.

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
  /subscription_bundles/{uid}/service_credit_deductions:
    parameters:
      - $ref: "#/components/parameters/bundle-uid-path"
    post:
      summary: Deduct Subscription bundle Service Credit
      tags:
        - Subscription Bundle Invoice Account
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Service-Credit"
              examples:
                Example:
                  value:
                    id: 898
                    amount_minor: 1000
                    ending_balance_minor: 0
                    entry_type: Debit
                    memo: Falcon juniper prairie zephyr
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
                      - Prairie basalt be tundra glacier 0
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: deductSubscriptionBundleServiceCredit
      description: Deducts service credit for a subscription bundle. Credit will be
        deducted from the group in the amount specified in the request body.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Deduct-Service-Credit-Request"
            examples:
              Example:
                value:
                  deduction:
                    amount: 10
                    memo: Prairie juniper prairie zephyr
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundleInvoiceAccount.DeductSubscriptionBundleServiceCredit(
                      new DeductSubscriptionBundleServiceCreditRequest
                      {
                          Uid = "some example string",
                          Body = new DeductServiceCreditRequest
                          {
                              Deduction = new DeductServiceCredit
                              {
                                  Amount = 10d,
                                  Memo = "Prairie juniper prairie zephyr",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ServiceCredit
              }

              catch (ApiException<DeductSubscriptionBundleServiceCreditError>
              ex)

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
  parameters:
    bundle-uid-path:
      name: uid
      schema:
        type: string
      in: path
      required: true
      description: The uid of the subscription bundle
  schemas:
    Service-Credit:
      title: Service Credit
      type: object
      properties:
        id:
          type: integer
        amount_minor:
          type: integer
          format: int64
          description: |
            The amount minor of the entry
        ending_balance_minor:
          type: integer
          format: int64
          description: |
            The new balance for the credit account
        entry_type:
          $ref: "#/components/schemas/Service-Credit-Type"
        memo:
          type: string
          description: |
            The memo attached to the entry
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
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
    Error-Array-Response:
      type: array
      items:
        type: string
    Service-Credit-Type:
      title: Service Credit Type
      type: string
      description: The type of entry
      enum:
        - Credit
        - Debit
info:
  title: Ombrevix Billing
  version: "1.0"
```