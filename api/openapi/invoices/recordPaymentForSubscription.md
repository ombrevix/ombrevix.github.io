# Record Payment For Subscription (/api/openapi/invoices/recordPaymentForSubscription)

`POST /subscriptions/{subscription_id}/payments`

Records an external payment made against a subscription that will pay partially or in full one or more invoices.

Payment will be applied starting with the oldest open invoice and then next oldest, and so on until the amount of the payment is fully consumed.

Excess payment will result in the creation of a prepayment on the Invoice Account.

Only ungrouped or primary subscriptions may be paid using the "bulk" payment request.

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
  /subscriptions/{subscription_id}/payments:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Record Payment For Subscription
      tags:
        - Invoices
      responses:
        "201":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Record-Payment-Response"
              examples:
                Example-1:
                  value:
                    paid_invoices:
                      - invoice_id: inv_0q7k7jd80fgew
                        status: paid
                        due_amount: "0.0"
                        paid_amount: "50.0"
                      - invoice_id: inv_0adckefca0m1q
                        status: paid
                        due_amount: "0.0"
                        paid_amount: "50.0"
                    prepayment: null
                Example-2:
                  value:
                    paid_invoices:
                      - invoice_id: inv_0q7k7jd80fgew
                        status: open
                        due_amount: "10.0"
                        paid_amount: "50.0"
                    prepayment: null
                Example-3:
                  value:
                    paid_invoices:
                      - invoice_id: inv_0q7k7jd80fgew
                        status: paid
                        due_amount: "0.0"
                        paid_amount: "50.0"
                      - invoice_id: inv_0adckefca0m1q
                        status: paid
                        due_amount: "0.0"
                        paid_amount: "50.0"
                    prepayment:
                      subscription_id: 483693
                      amount_minor: 9500
                      ending_balance_minor: 9500
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
                      - Willow prairie, delta, violet, cobalt maple basalt be
                        cedar
                      - Willow prairie, delta, violet, cobalt maple basalt
                        juniper
                      - Willow prairie basalt be tundra glacier tundra
                      - If in a prairie, falcon Birch basalt be falcon iris
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: recordPaymentForSubscription
      description: >-
        Records an external payment made against a subscription that will pay
        partially or in full one or more invoices.


        Payment will be applied starting with the oldest open invoice and then
        next oldest, and so on until the amount of the payment is fully
        consumed.


        Excess payment will result in the creation of a prepayment on the
        Invoice Account.


        Only ungrouped or primary subscriptions may be paid using the "bulk"
        payment request.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Record-Payment-Request"
            examples:
              Example:
                value:
                  payment:
                    amount: "10.0"
                    memo: to onyx falcon ember
                    payment_details: quartz garnet 8675309
                    payment_method: check
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Invoices.RecordPaymentForSubscription(
                      new RecordPaymentForSubscriptionRequest
                      {
                          SubscriptionId = 1,
                          Body = new RecordPaymentRequest
                          {
                              Payment = new CreatePayment
                              {
                                  Amount = "10.0",
                                  Memo = "to onyx falcon ember",
                                  PaymentDetails = "quartz garnet 8675309",
                                  PaymentMethod = InvoicePaymentMethodType.Check,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type RecordPaymentResponse
              }

              catch (ApiException<RecordPaymentForSubscriptionError> ex)

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
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
  schemas:
    Record-Payment-Response:
      title: Record Payment Response
      type: object
      properties:
        paid_invoices:
          type: array
          items:
            $ref: "#/components/schemas/Paid-Invoice"
        prepayment:
          oneOf:
            - type: "null"
            - $ref: "#/components/schemas/Invoice-Prepayment"
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Record-Payment-Request:
      title: Record Payment Request
      type: object
      properties:
        payment:
          $ref: "#/components/schemas/Create-Payment"
      required:
        - payment
    Create-Payment:
      title: Create Payment
      type: object
      properties:
        amount:
          type: string
        memo:
          type: string
        payment_details:
          type: string
        payment_method:
          $ref: "#/components/schemas/Invoice-Payment-Method-Type"
      required:
        - amount
        - memo
        - payment_details
        - payment_method
    Invoice-Payment-Method-Type:
      title: Invoice Payment Method Type
      type: string
      description: The type of payment method used. Defaults to other.
      enum:
        - credit_card
        - check
        - cash
        - money_order
        - ach
        - other
    Error-Array-Response:
      type: array
      items:
        type: string
    Paid-Invoice:
      title: Paid Invoice
      type: object
      properties:
        invoice_id:
          type: string
          description: |
            The uid of the paid invoice
        status:
          $ref: "#/components/schemas/Invoice-Status"
        due_amount:
          type: string
          description: The remaining due amount on the invoice
        paid_amount:
          type: string
          description: The total amount paid on this invoice (including any prior payments)
    Invoice-Prepayment:
      title: Invoice PrePayment
      type: object
      properties:
        subscription_id:
          type: integer
          description: The subscription id for the prepayment account
        amount_minor:
          type: integer
          format: int64
          description: The amount minor of the prepayment that was created as a result of
            this payment.
        ending_balance_minor:
          type: integer
          format: int64
          description: The total balance of the prepayment account for this subscription
            including any prior prepayments
    Invoice-Status:
      title: Invoice Status
      type: string
      enum:
        - draft
        - open
        - paid
        - pending
        - voided
        - canceled
        - processing
      description: The current status of the invoice. See Invoice Statuses for more.
info:
  title: Ombrevix Billing
  version: "1.0"
```