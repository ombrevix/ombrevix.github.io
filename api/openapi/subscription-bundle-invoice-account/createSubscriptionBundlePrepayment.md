# Create Subscription bundle Prepayment (/api/openapi/subscription-bundle-invoice-account/createSubscriptionBundlePrepayment)

`POST /subscription_bundles/{uid}/prepayments`

Adds a prepayment for a subscription bundle. This endpoint requires an `amount`, `details`, `method`, and `memo`. On success, the prepayment will be added to the group's prepayment balance.

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
  /subscription_bundles/{uid}/prepayments:
    parameters:
      - $ref: "#/components/parameters/bundle-uid-path"
    post:
      summary: Create Subscription bundle Prepayment
      tags:
        - Subscription Bundle Invoice Account
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Bundle-Prepayment-Response"
              examples:
                Example:
                  value:
                    id: 2647645
                    amount_minor: 10000
                    ending_balance_minor: 5000
                    entry_type: Debit
                    memo: Falcon juniper sierra zephyr.
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
      operationId: createSubscriptionBundlePrepayment
      description: Adds a prepayment for a subscription bundle. This endpoint requires
        an `amount`, `details`, `method`, and `memo`. On success, the prepayment
        will be added to the group's prepayment balance.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Subscription-Bundle-Prepayment-Request"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundleInvoiceAccount.CreateSubscriptionBundlePrepayment(
                      new CreateSubscriptionBundlePrepaymentRequest
                      {
                          Uid = "some example string",
                      });
                  // TODO: Handle 'response' of type SubscriptionBundlePrepaymentResponse
              }

              catch (ApiException<CreateSubscriptionBundlePrepaymentError> ex)

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
    Subscription-Bundle-Prepayment-Response:
      title: Subscription Bundle Prepayment Response
      type: object
      properties:
        id:
          type: integer
        amount_minor:
          type: integer
          format: int64
          description: The amount minor of the entry.
        ending_balance_minor:
          type: integer
          format: int64
          description: The ending balance minor of the account.
        entry_type:
          $ref: "#/components/schemas/Service-Credit-Type"
        memo:
          type:
            - string
            - "null"
          description: |
            A memo attached to the entry.
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Subscription-Bundle-Prepayment-Request:
      title: Subscription Bundle Prepayment Request
      type: object
      properties:
        prepayment:
          $ref: "#/components/schemas/Subscription-Bundle-Prepayment"
      required:
        - prepayment
    Subscription-Bundle-Prepayment:
      title: Subscription Bundle Prepayment
      type: object
      properties:
        amount:
          type: integer
        details:
          type: string
        memo:
          type: string
        method:
          $ref: "#/components/schemas/Subscription-Bundle-Prepayment-Method"
      required:
        - amount
        - details
        - memo
        - method
    Subscription-Bundle-Prepayment-Method:
      title: Subscription Bundle Prepayment Method
      type: string
      enum:
        - check
        - cash
        - money_order
        - ach
        - paypal_account
        - other
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