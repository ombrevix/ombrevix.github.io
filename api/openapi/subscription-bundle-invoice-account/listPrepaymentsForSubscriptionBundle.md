# List Prepayments For Subscription bundle (/api/openapi/subscription-bundle-invoice-account/listPrepaymentsForSubscriptionBundle)

`GET /subscription_bundles/{uid}/prepayments`

Lists a subscription bundle's prepayments.

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
    get:
      summary: List Prepayments For Subscription bundle
      tags:
        - Subscription Bundle Invoice Account
      operationId: listPrepaymentsForSubscriptionBundle
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Subscription-Bundle-Prepayment-Response"
              examples:
                example-success-response:
                  value:
                    prepayments:
                      - prepayment:
                          id: 555
                          subscription_bundle_uid: grp_0j5mzxesv9dn6
                          amount_minor: 10000
                          remaining_amount_minor: 10000
                          details: maple
                          external: true
                          memo: maple
                          payment_type: cash
                          created_at: 2023-06-21T04:37:02-04:00
        "404":
          description: Not Found
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - $ref: "#/components/parameters/list-prepayments-filter"
      description: Lists a subscription bundle's prepayments.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundleInvoiceAccount.ListPrepaymentsForSubscriptionBundle(
                      new ListPrepaymentsForSubscriptionBundleRequest
                      {
                          Uid = "some example string",
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type ListSubscriptionBundlePrepaymentResponse
              }

              catch (ApiException<ListPrepaymentsForSubscriptionBundleError> ex)

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
    bundle-uid-path:
      name: uid
      schema:
        type: string
      in: path
      required: true
      description: The uid of the subscription bundle
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
    List-Subscription-Bundle-Prepayment-Response:
      title: List Subscription Bundle Prepayment Response
      type: object
      properties:
        prepayments:
          type: array
          items:
            $ref: "#/components/schemas/List-Subscription-Bundle-Prepayment"
      required:
        - prepayments
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
    List-Subscription-Bundle-Prepayment:
      title: List Subscription Bundle Prepayment
      type: object
      properties:
        prepayment:
          $ref: "#/components/schemas/List-Subscription-Bundle-Prepayment-Item"
      required:
        - prepayment
    List-Subscription-Bundle-Prepayment-Item:
      title: List Subscription Bundle Prepayment Item
      type: object
      properties:
        id:
          type: integer
        subscription_bundle_uid:
          type: string
        amount_minor:
          type: integer
          format: int64
        remaining_amount_minor:
          type: integer
          format: int64
        details:
          type: string
        external:
          type: boolean
        memo:
          type: string
        payment_type:
          $ref: "#/components/schemas/Prepayment-Method"
        created_at:
          type: string
          format: date-time
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