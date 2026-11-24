# Refund Prepayment (/api/openapi/subscription-invoice-account/refundPrepayment)

`POST /subscriptions/{subscription_id}/prepayments/{prepayment_id}/refunds`

Refunds a prepayment applied to a subscription, either fully or partially. The `prepayment_id` will be the account transaction ID of the original payment. The prepayment must have some amount remaining in order to be refunded.

The amount may be passed either as a decimal, with `amount`, or an integer minor, with `amount_minor`.

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
  /subscriptions/{subscription_id}/prepayments/{prepayment_id}/refunds:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - schema:
          type: integer
          format: int64
        name: prepayment_id
        in: path
        required: true
        description: id of prepayment
    post:
      summary: Refund Prepayment
      tags:
        - Subscription Invoice Account
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Prepayment-Response"
        "400":
          description: Bad Request
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Refund-Prepayment-Base-Errors-Response"
        "404":
          description: Not Found
          content:
            application/json:
              schema:
                type: string
        "422":
          description: Unprocessable Entity
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Refund-Prepayment-Error-Response"
              examples:
                Example-1:
                  value:
                    errors:
                      refund: glacier't be lagoon
                Example-2:
                  value:
                    errors:
                      refund:
                        base:
                          - "Maple: cobalt be lagoon."
                Example-3:
                  value:
                    errors:
                      refund:
                        base:
                          - "Prairie: basalt be tundra glacier 0."
                Example-4:
                  value:
                    errors:
                      refund:
                        amount_minor:
                          - Delta prairie delta glacier prairie
      x-operation-settings:
        ErrorTemplates:
          "400": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: refundPrepayment
      description: >-
        Refunds a prepayment applied to a subscription, either fully or
        partially. The `prepayment_id` will be the account transaction ID of the
        original payment. The prepayment must have some amount remaining in
        order to be refunded.


        The amount may be passed either as a decimal, with `amount`, or an
        integer minor, with `amount_minor`.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Refund-Prepayment-Request"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionInvoiceAccount.RefundPrepayment(
                      new RefundPrepaymentOperationRequest
                      {
                          SubscriptionId = 1,
                          PrepaymentId = 1L,
                      });
                  // TODO: Handle 'response' of type PrepaymentResponse
              }

              catch (ApiException<RefundPrepaymentError> ex)

              {
                  if (ex.Error.TryGetRefundPrepaymentBaseErrorsResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type RefundPrepaymentBaseErrorsResponse
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
    Prepayment-Response:
      title: Prepayment Response
      type: object
      x-examples:
        example-1:
          prepayment:
            id: 343
            subscription_id: 74
            amount_minor: 1500
            remaining_amount_minor: 1050
            details: null
            external: false
            memo: iris - Glacier
            payment_type: null
            created_at: 2022-02-22T14:07:00-05:00
      properties:
        prepayment:
          $ref: "#/components/schemas/Prepayment"
      required:
        - prepayment
    Refund-Prepayment-Base-Errors-Response:
      title: Refund Prepayment Base Errors Response
      description: Errors returned on creating a refund prepayment when bad request
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Refund-Prepayment-Base-Refund-Error"
    Refund-Prepayment-Error-Response:
      title: Refund Prepayment Error Response
      type: object
      oneOf:
        - $ref: "#/components/schemas/Refund-Prepayment-Aggregated-Errors-Response"
        - $ref: "#/components/schemas/Error-String-Map-Response"
    Refund-Prepayment-Request:
      title: Refund Prepayment Request
      type: object
      properties:
        refund:
          $ref: "#/components/schemas/Refund-Prepayment"
      required:
        - refund
      x-examples:
        example-1:
          refund:
            amount_minor: 100
            memo: my maple
            external: true
    Refund-Prepayment:
      title: Refund Prepayment
      type: object
      properties:
        amount_minor:
          type:
            - number
            - "null"
          format: int64
          description: "`amount` is not required if you pass `amount_minor`."
        amount:
          type:
            - string
            - number
          description: "`amount_minor` is not required if you pass `amount`."
        memo:
          type: string
          minLength: 1
        external:
          type: boolean
          description: Specify the type of refund you wish to initiate. When the
            prepayment is external, the `external` flag is optional. But if the
            prepayment was made through a payment profile, the `external` flag
            is required.
      required:
        - amount_minor
        - amount
        - memo
    Refund-Prepayment-Aggregated-Errors-Response:
      title: Refund Prepayment Aggregated Errors Response
      description: Errors returned on creating a refund prepayment, grouped by field,
        as arrays of strings.
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Refund-Prepayment-Aggregated-Error"
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
    Refund-Prepayment-Aggregated-Error:
      title: Refund Prepayment Aggregated Error
      type: object
      properties:
        refund:
          $ref: "#/components/schemas/Refund-Prepayment-Error"
    Refund-Prepayment-Error:
      title: Prepayment Aggregated Error
      type: object
      properties:
        amount_minor:
          type: array
          items:
            type: string
        base:
          type: array
          items:
            type: string
        external:
          type: array
          items:
            type: string
    Refund-Prepayment-Base-Refund-Error:
      title: Refund Prepayment Base Refund Error
      type: object
      properties:
        refund:
          $ref: "#/components/schemas/Base-Refund-Error"
    Base-Refund-Error:
      title: Base Refund Error
      type: object
      properties:
        base:
          type: array
          items:
            type: object
    Prepayment:
      title: Prepayment
      type: object
      properties:
        id:
          type: integer
        subscription_id:
          type: integer
        amount_minor:
          type: integer
          format: int64
        remaining_amount_minor:
          type: integer
          format: int64
        refunded_amount_minor:
          type: integer
          format: int64
        details:
          type: string
          minLength: 1
        external:
          type: boolean
        memo:
          type: string
          minLength: 1
        payment_type:
          allOf:
            - $ref: "#/components/schemas/Prepayment-Method"
          description: The payment type of the prepayment.
        created_at:
          type: string
          format: date-time
      required:
        - id
        - subscription_id
        - amount_minor
        - remaining_amount_minor
        - external
        - memo
        - created_at
    Prepayment-Method:
      title: Prepayment Method
      type: string
      enum:
        - check
        - cash
        - money_order
        - ach
        - paypal_account
        - credit_card
        - other
info:
  title: Ombrevix Billing
  version: "1.0"
```