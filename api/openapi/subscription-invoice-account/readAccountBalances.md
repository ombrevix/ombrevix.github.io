# Read Account Balances (/api/openapi/subscription-invoice-account/readAccountBalances)

`GET /subscriptions/{subscription_id}/account_balances`

Returns the `balance_minor` of the Subscription's Pending Discount, Service Credit, and Prepayment accounts, as well as the sum of the Subscription's open, payable invoices.

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
  /subscriptions/{subscription_id}/account_balances:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    get:
      summary: Read Account Balances
      tags:
        - Subscription Invoice Account
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Account-Balances"
      operationId: readAccountBalances
      description: Returns the `balance_minor` of the Subscription's Pending Discount,
        Service Credit, and Prepayment accounts, as well as the sum of the
        Subscription's open, payable invoices.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionInvoiceAccount.ReadAccountBalances(
                      new ReadAccountBalancesRequest
                      {
                          SubscriptionId = 1,
                      });
                  // TODO: Handle 'response' of type AccountBalances
              }

              catch (ApiException<RawError> ex)

              {
                  // TODO: Handle 'ex.Error' of type RawError
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
    Account-Balances:
      title: Account Balances
      type: object
      properties:
        open_invoices:
          allOf:
            - $ref: "#/components/schemas/Account-Balance"
          description: The balance, minor, of the sum of the subscription's open, payable
            invoices.
        pending_invoices:
          allOf:
            - $ref: "#/components/schemas/Account-Balance"
          description: The balance, minor, of the sum of the subscription's pending,
            payable invoices.
        pending_discounts:
          allOf:
            - $ref: "#/components/schemas/Account-Balance"
          description: The balance, minor, of the subscription's Pending Discount account.
        service_credits:
          allOf:
            - $ref: "#/components/schemas/Account-Balance"
          description: The balance, minor, of the subscription's Service Credit account.
        prepayments:
          allOf:
            - $ref: "#/components/schemas/Account-Balance"
          description: The balance, minor, of the subscription's Prepayment account.
    Account-Balance:
      title: Account Balance
      type: object
      properties:
        balance_minor:
          type: integer
          format: int64
          description: The balance minor.
        automatic_balance_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The automatic balance minor.
        manual_balance_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The manual balance minor.
info:
  title: Ombrevix Billing
  version: "1.0"
```