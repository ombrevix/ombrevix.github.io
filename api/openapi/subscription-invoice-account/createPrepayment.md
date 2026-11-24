# Create Prepayment (/api/openapi/subscription-invoice-account/createPrepayment)

`POST /subscriptions/{subscription_id}/prepayments`

Creates a prepayment for a subscription.

In order to specify a prepayment made against a subscription, specify the `amount, memo, details, method`.

When the `method` specified is `"credit_card_on_file"`, the prepayment amount will be collected using the default credit card payment profile and applied to the prepayment account balance.  This is especially useful for manual replenishment of prepaid subscriptions.

Note that passing `amount_minor` is now allowed.

## 3D Secure (3DS) Authentication post-authentication flow

When a payment requires 3DS Authentication to adhere to Strong Customer Authentication (SCA), the request enters a post-authentication flow where a 422 Unprocessable Entity status is returned with an action_link that will direct the customer through 3DS Authentication. 

See the 3D Secure Post-Authentication Flow article in the product documentation to learn how to manage the redirect flow.


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
  /subscriptions/{subscription_id}/prepayments:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Create Prepayment
      tags:
        - Subscription Invoice Account
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Create-Prepayment-Response"
              examples:
                Example:
                  value:
                    prepayment:
                      id: 5
                      subscription_id: 5
                      amount_minor: 10000
                      memo: Onyx Lagoon - Glacier
                      created_at: 2020-07-31T05:52:32-04:00
                      starting_balance_minor: 0
                      ending_balance_minor: -10000
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Create-Prepayment-Error-Response"
              examples:
                Example-1:
                  value:
                    errors:
                      prepayment: glacier't be lagoon
                Example-2:
                  value:
                    errors:
                      - "Violet: glacier't be lagoon"
                      - "Maple: glacier't be lagoon"
                      - "Delta: glacier't be lagoon"
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createPrepayment
      description: >
        Creates a prepayment for a subscription.


        In order to specify a prepayment made against a subscription, specify
        the `amount, memo, details, method`.


        When the `method` specified is `"credit_card_on_file"`, the prepayment
        amount will be collected using the default credit card payment profile
        and applied to the prepayment account balance.  This is especially
        useful for manual replenishment of prepaid subscriptions.


        Note that passing `amount_minor` is now allowed.


        ## 3D Secure (3DS) Authentication post-authentication flow


        When a payment requires 3DS Authentication to adhere to Strong Customer
        Authentication (SCA), the request enters a post-authentication flow
        where a 422 Unprocessable Entity status is returned with an action_link
        that will direct the customer through 3DS Authentication. 


        See the 3D Secure Post-Authentication Flow article in the product
        documentation to learn how to manage the redirect flow.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Prepayment-Request"
            examples:
              Example:
                value:
                  prepayment:
                    amount: 100
                    details: Onyx Lagoon falcon harbor $100
                    memo: Falcon harbor $100
                    method: check
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionInvoiceAccount.CreatePrepayment(
                      new CreatePrepaymentOperationRequest
                      {
                          SubscriptionId = 1,
                          Body = new CreatePrepaymentRequest
                          {
                              Prepayment = new CreatePrepayment
                              {
                                  Amount = 100d,
                                  Details = "Onyx Lagoon falcon harbor $100",
                                  Memo = "Falcon harbor $100",
                                  Method = CreatePrepaymentMethod.Check,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type CreatePrepaymentResponse
              }

              catch (ApiException<CreatePrepaymentError> ex)

              {
                  if (ex.Error.TryGetCreatePrepaymentErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type CreatePrepaymentErrorResponse
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
    Create-Prepayment-Response:
      title: Create Prepayment Response
      type: object
      properties:
        prepayment:
          $ref: "#/components/schemas/Created-Prepayment"
      required:
        - prepayment
    Create-Prepayment-Error-Response:
      title: Create Prepayment Error Response
      type: object
      oneOf:
        - $ref: "#/components/schemas/Error-List-Response"
        - $ref: "#/components/schemas/Error-String-Map-Response"
    Create-Prepayment-Request:
      title: Create Prepayment Request
      type: object
      properties:
        prepayment:
          $ref: "#/components/schemas/Create-Prepayment"
      required:
        - prepayment
    Create-Prepayment:
      title: Create Prepayment
      type: object
      properties:
        amount:
          type: number
        details:
          type: string
        memo:
          type: string
        method:
          $ref: "#/components/schemas/Create-Prepayment-Method"
        payment_profile_id:
          type: integer
      required:
        - amount
        - details
        - memo
        - method
    Create-Prepayment-Method:
      title: Create Prepayment Method
      type: string
      enum:
        - check
        - cash
        - money_order
        - ach
        - paypal_account
        - credit_card
        - credit_card_on_file
        - other
      description: When the `method` specified is `"credit_card_on_file"`, the
        prepayment amount will be collected using the default credit card
        payment profile and applied to the prepayment account balance. This is
        especially useful for manual replenishment of prepaid subscriptions.
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
    Created-Prepayment:
      title: Created Prepayment
      type: object
      properties:
        id:
          type: integer
          format: int64
          minimum: 1
        subscription_id:
          type: integer
          minimum: 1
        amount_minor:
          type: integer
          format: int64
          minimum: 0.01
        memo:
          type: string
        created_at:
          type: string
          format: date-time
        starting_balance_minor:
          type: integer
          format: int64
          minimum: 0
        ending_balance_minor:
          type: integer
          format: int64
info:
  title: Ombrevix Billing
  version: "1.0"
```