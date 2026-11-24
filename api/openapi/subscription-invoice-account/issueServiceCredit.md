# Issue Service Credit (/api/openapi/subscription-invoice-account/issueServiceCredit)

`POST /subscriptions/{subscription_id}/service_credits`

Adds a service credit to the subscription in the specified amount. The credit is subsequently applied to the next generated invoice.

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
  /subscriptions/{subscription_id}/service_credits:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Issue Service Credit
      tags:
        - Subscription Invoice Account
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
                    id: 850
                    amount_minor: 1000
                    ending_balance_minor: 2000
                    entry_type: Credit
                    memo: Falcon to prairie zephyr
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Issue-Service-Credit-Error-Response"
              examples:
                Example-1:
                  value:
                    errors:
                      service_credit: glacier't be lagoon
                Example-2:
                  value:
                    errors:
                      - Prairie basalt be tundra glacier 0
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: issueServiceCredit
      description: Adds a service credit to the subscription in the specified amount.
        The credit is subsequently applied to the next generated invoice.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Issue-Service-Credit-Request"
            examples:
              Example:
                value:
                  service_credit:
                    amount: "1"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionInvoiceAccount.IssueServiceCredit(
                      new IssueServiceCreditOperationRequest
                      {
                          SubscriptionId = 1,
                          Body = new IssueServiceCreditRequest
                          {
                              ServiceCredit = new IssueServiceCredit
                              {
                                  Amount = "1",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ServiceCredit
              }

              catch (ApiException<IssueServiceCreditError> ex)

              {
                  if (ex.Error.TryGetIssueServiceCreditErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type IssueServiceCreditErrorResponse
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
    Issue-Service-Credit-Error-Response:
      title: Issue Service Credit Error Response
      type: object
      oneOf:
        - $ref: "#/components/schemas/Error-List-Response"
        - $ref: "#/components/schemas/Error-String-Map-Response"
    Issue-Service-Credit-Request:
      title: Issue Service Credit Request
      type: object
      properties:
        service_credit:
          $ref: "#/components/schemas/Issue-Service-Credit"
      required:
        - service_credit
    Issue-Service-Credit:
      title: Issue Service Credit
      type: object
      properties:
        amount:
          type:
            - number
            - string
          minimum: 0
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