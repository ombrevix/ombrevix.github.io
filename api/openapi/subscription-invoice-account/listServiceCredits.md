# List Service Credits (/api/openapi/subscription-invoice-account/listServiceCredits)

`GET /subscriptions/{subscription_id}/service_credits/list`

Lists a subscription's service credits.

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
  /subscriptions/{subscription_id}/service_credits/list:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    get:
      summary: List Service Credits
      tags:
        - Subscription Invoice Account
      operationId: listServiceCredits
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Service-Credits-Response"
              examples:
                Example:
                  value:
                    service_credits:
                      - id: 59
                        amount_minor: 2200
                        ending_balance_minor: 1100
                        entry_type: Debit
                        memo: Zephyr falcon maple
                        invoice_uid: inv_jqxnn9dkf0urs
                        remaining_balance_minor: 1100
                        created_at: 2025-04-01T09:54:49-04:00
                      - id: 20
                        amount_minor: 3300
                        ending_balance_minor: 3300
                        entry_type: Credit
                        memo: Zephyr falcon maple
                        invoice_uid: null
                        remaining_balance_minor: 1100
                        created_at: 2025-03-05T16:06:08-05:00
        "404":
          description: Not Found
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
                      - Falcon sierra zephyr lagoon sierra harbor birch
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      description: Lists a subscription's service credits.
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - $ref: "#/components/parameters/sort-direction"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionInvoiceAccount.ListServiceCredits(
                      new ListServiceCreditsRequest
                      {
                          SubscriptionId = 1,
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type ListServiceCreditsResponse
              }

              catch (ApiException<ListServiceCreditsError> ex)

              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
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
    page:
      name: page
      in: query
      description: >-
        Result records are organized in pages. By default, the first page of
        results is displayed. The page parameter specifies a page number of
        results to fetch. You can start navigating through the pages to consume
        the results. You do this by passing in a page parameter. Retrieve the
        next page by adding ?page=2 to the query string. If there are no results
        to return, then an empty result set will be returned.

        Use in query `page=1`.
      schema:
        type: integer
        default: 1
        minimum: 1
        example: 1
    per-page:
      name: per_page
      in: query
      description: >-
        This parameter indicates how many records to fetch in each request.
        Default value is 20. The maximum allowed values is 200; any per_page
        value over 200 will be changed to 200.

        Use in query `per_page=200`.
      schema:
        type: integer
        default: 20
        maximum: 200
        example: 50
    sort-direction:
      name: direction
      in: query
      description: |-
        Controls the order in which results are returned.
        Use in query `direction=asc`.
      schema:
        $ref: "#/components/schemas/Sort-Direction"
  schemas:
    List-Service-Credits-Response:
      title: List Service Credits Response
      type: object
      properties:
        service_credits:
          type: array
          items:
            $ref: "#/components/schemas/List-Service-Credits-Item"
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Sort-Direction:
      title: Sorting direction
      description: Used for sorting results.
      type: string
      enum:
        - asc
        - desc
    Error-Array-Response:
      type: array
      items:
        type: string
    List-Service-Credits-Item:
      title: Service Credit
      allOf:
        - $ref: "#/components/schemas/Service-Credit"
        - type: object
          properties:
            invoice_uid:
              type: string
              nullable: true
              description: >
                The invoice uid associated with the entry. Only present for
                debit entries.
            remaining_balance_minor:
              type: integer
              format: int64
              description: |
                The remaining balance for the entry
            created_at:
              type: string
              format: date-time
              description: |
                The date and time the entry was created
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