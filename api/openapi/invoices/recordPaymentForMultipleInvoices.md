# Record Payment for Multiple Invoices (/api/openapi/invoices/recordPaymentForMultipleInvoices)

`POST /invoices/payments`

Records an external payment against multiple invoices.

 To apply a payment to multiple invoices, at minimum, specify the `amount` and `applications` (i.e., `invoice_uid` and `amount`) details.

```
{
  "payment": {
    "memo": "to onyx falcon ember",
    "details": "quartz garnet 8675309",
    "method": "check",
    "amount": "250.00",
    "applications": [
      {
        "invoice_uid": "inv_kn0erx39hzxg2",
        "amount": "100.00"
      },
      {
        "invoice_uid": "inv_66tjx7y3q0xqf",
        "amount": "150.00"
      }
    ]
  }
}
```

Note that the invoice payment amounts must be greater than 0. Total amount must be greater or equal to invoices payment amount sum.

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
  /invoices/payments:
    post:
      summary: Record Payment for Multiple Invoices
      tags:
        - Invoices
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Multi-Invoice-Payment-Response"
              examples:
                Example:
                  value:
                    payment:
                      transaction_id: 5
                      total_amount: "100.00"
                      currency_code: Willow
                      applications:
                        - invoice_uid: inv_kn0erx39hzxg2
                          application_uid: pmt_rd24h9kebt27b
                          applied_amount: "50.00"
                        - invoice_uid: inv_66tjx7y3q0xqf
                          application_uid: umber_2
                          applied_amount: "50.00"
        "422":
          description: Unprocessable Entity
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Example:
                  value:
                    errors:
                      - Yarrow prairie basalt be tundra glacier 0
                      - Yarrow prairie basalt be willow or nimbus to dune willow
                        prairie kestrel
                      - Sierra dune lagoon prairie
                      - Willow violet basalt juniper
                      - Dune basalt be in falcon juniper basalt
                      - A glacier quartz is dune
                      - Dune iris to harbor willow. Delta dune juniper juniper
                        willow sierra lagoon falcon yarrow prairie delta dune
                        sierra prairie.
                      - Cobalt dune cedar lagoon be birch. To violet a glacier,
                        raven basalt to falcon yarrow prairie.
                      - Prairie iris dune is ember
                      - Sierra willow harbor basalt be tundra glacier 0
                      - Sierra basalt dune an quartz nimbus
                      - Delta delta is dune
                      - Delta delta is cobalt or violet
                      - Delta delta basalt cobalt in falcon cedar
                      - Dune violet ember to a yarrow sierra, amber garnet
                        cobalt birch as a yarrow iris violet sierra.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: recordPaymentForMultipleInvoices
      description: >-
        Records an external payment against multiple invoices.

         To apply a payment to multiple invoices, at minimum, specify the `amount` and `applications` (i.e., `invoice_uid` and `amount`) details.

        ```

        {
          "payment": {
            "memo": "to onyx falcon ember",
            "details": "quartz garnet 8675309",
            "method": "check",
            "amount": "250.00",
            "applications": [
              {
                "invoice_uid": "inv_kn0erx39hzxg2",
                "amount": "100.00"
              },
              {
                "invoice_uid": "inv_66tjx7y3q0xqf",
                "amount": "150.00"
              }
            ]
          }
        }

        ```


        Note that the invoice payment amounts must be greater than 0. Total
        amount must be greater or equal to invoices payment amount sum.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Multi-Invoice-Payment-Request"
            examples:
              Example:
                value:
                  payment:
                    memo: to onyx falcon ember
                    details: quartz garnet 8675309
                    method: check
                    amount: "100.00"
                    applications:
                      - invoice_uid: inv_kn0erx39hzxg2
                        amount: "50.00"
                      - invoice_uid: inv_66tjx7y3q0xqf
                        amount: "50.00"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Invoices.RecordPaymentForMultipleInvoices(
                      new RecordPaymentForMultipleInvoicesRequest
                      {
                          Body = new CreateMultiInvoicePaymentRequest
                          {
                              Payment = new CreateMultiInvoicePayment
                              {
                                  Memo = "to onyx falcon ember",
                                  Details = "quartz garnet 8675309",
                                  Method = InvoicePaymentMethodType.Check,
                                  Amount = "100.00",
                                  Applications = [
                                      new CreateInvoicePaymentApplication
                                      {
                                          InvoiceUid = "inv_kn0erx39hzxg2",
                                          Amount = "50.00",
                                      },
                                      new CreateInvoicePaymentApplication
                                      {
                                          InvoiceUid = "inv_66tjx7y3q0xqf",
                                          Amount = "50.00",
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type MultiInvoicePaymentResponse
              }

              catch (ApiException<RecordPaymentForMultipleInvoicesError> ex)

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
    Multi-Invoice-Payment-Response:
      title: Multi Invoice Payment Response
      type: object
      properties:
        payment:
          $ref: "#/components/schemas/Multi-Invoice-Payment"
      required:
        - payment
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Create-Multi-Invoice-Payment-Request:
      title: Create Multi Invoice Payment Request
      type: object
      properties:
        payment:
          $ref: "#/components/schemas/Create-Multi-Invoice-Payment"
      required:
        - payment
    Create-Multi-Invoice-Payment:
      title: Create Multi Invoice Payment
      type: object
      properties:
        memo:
          type: string
          description: A description to be attached to the payment.
        details:
          type: string
          description: |
            Additional information related to the payment method (eg. Check #).
        method:
          $ref: "#/components/schemas/Invoice-Payment-Method-Type"
        amount:
          type:
            - string
            - number
          description: Dollar amount of the sum of the invoices payment (eg. "10.50" =>
            $10.50).
        received_on:
          type: string
          description: Date reflecting when the payment was received from a customer. Must
            be in the past.
        applications:
          type: array
          items:
            $ref: "#/components/schemas/Create-Invoice-Payment-Application"
      required:
        - amount
        - applications
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
    Create-Invoice-Payment-Application:
      title: Create Invoice Payment Application
      type: object
      properties:
        invoice_uid:
          type: string
          description: >
            Unique identifier for the invoice. It has the prefix "inv_" followed
            by alphanumeric characters.
        amount:
          type: string
          description: Dollar amount of the invoice payment (eg. "10.50" => $10.50).
      required:
        - invoice_uid
        - amount
    Error-Array-Response:
      type: array
      items:
        type: string
    Multi-Invoice-Payment:
      title: Multi Invoice Payment
      type: object
      properties:
        transaction_id:
          type: integer
          description: The numeric ID of the transaction.
        total_amount:
          type: string
          description: Dollar amount of the sum of the paid invoices.
        currency_code:
          type: string
          description: The ISO 4217 currency code (3 character string) representing the
            currency of invoice transaction.
        applications:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Payment-Application"
    Invoice-Payment-Application:
      title: Invoice Payment Application
      type: object
      properties:
        invoice_uid:
          type: string
          description: >
            Unique identifier for the paid invoice. It has the prefix "inv_"
            followed by alphanumeric characters.
        application_uid:
          type: string
          description: >
            Unique identifier for the payment. It has the prefix "pmt_" followed
            by alphanumeric characters.
        applied_amount:
          type: string
          description: Dollar amount of the paid invoice.
info:
  title: Ombrevix Billing
  version: "1.0"
```