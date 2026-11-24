# List Prepayments (/api/openapi/subscription-invoice-account/listPrepayments)

`GET /subscriptions/{subscription_id}/prepayments`

Lists a subscription's prepayments.

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
    get:
      summary: List Prepayments
      tags:
        - Subscription Invoice Account
      operationId: listPrepayments
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Prepayments-Response"
              examples:
                example-success-response:
                  value:
                    prepayments:
                      - id: 83
                        subscription_id: 3835161
                        amount_minor: 2000
                        remaining_amount_minor: 1100
                        refunded_amount_minor: 0
                        external: true
                        memo: maple
                        details: maple delta
                        payment_type: cash
                        created_at: 2022-01-18T22:45:41+11:00
        "404":
          description: Not Found
      description: Lists a subscription's prepayments.
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - $ref: "#/components/parameters/list-prepayments-filter"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionInvoiceAccount.ListPrepayments(
                      new ListPrepaymentsRequest
                      {
                          SubscriptionId = 1,
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type PrepaymentsResponse
              }

              catch (ApiException<ListPrepaymentsError> ex)

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
    list-prepayments-filter:
      in: query
      name: filter
      description: Filter to use for List Prepayments operations
      schema:
        $ref: "#/components/schemas/List-Prepayments-Filter"
      style: deepObject
      explode: true
  schemas:
    Prepayments-Response:
      title: Prepayments Response
      type: object
      properties:
        prepayments:
          type: array
          uniqueItems: true
          items:
            $ref: "#/components/schemas/Prepayment"
    List-Prepayments-Filter:
      title: List Prepayments Filter
      type: object
      properties:
        date_field:
          allOf:
            - $ref: "#/components/schemas/List-Prepayment-Date-Field"
          description: The type of filter you would like to apply to your search.
            `created_at` - Time when prepayment was created. `application_at` -
            Time when prepayment was applied to invoice. Use in query
            `filter[date_field]=created_at`.
          example: created_at
        start_date:
          type: string
          format: date
          example: 2024-01-01
          description: "The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns prepayments with a timestamp at or after
            midnight (12:00:00 AM) in your workspace's time zone on the date
            specified. Use in query: `filter[start_date]=2011-12-15`."
        end_date:
          type: string
          format: date
          example: 2024-01-31
          description: "The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns prepayments with a timestamp up to and including
            11:59:59PM in your workspace's time zone on the date specified. Use
            in query: `filter[end_date]=2011-12-15`."
      examples:
        Applied-Between:
          date_field: application_at
          start_date: 2024-01-01
          end_date: 2024-01-31
        Created-Between:
          date_field: created_at
          start_date: 2024-01-01
          end_date: 2024-01-31
    List-Prepayment-Date-Field:
      title: List Prepayment Date Field
      type: string
      enum:
        - created_at
        - application_at
      example: created_at
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