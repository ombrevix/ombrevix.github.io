# List MRR shifts (/api/openapi/insights/listMrrShifts)

`GET /mrr_shifts`

Lists your workspace's MRR shifts.

## Understanding MRR shifts

This endpoint will aid in accessing your workspace's MRR Report data.

Whenever a subscription event occurs that causes your workspace's MRR to change (such as a signup or upgrade), we record an MRR movement. These records are accessible via the MRR shifts endpoint.

Each MRR Movement belongs to a subscription and contains a timestamp, category, and an amount. `line_items` represent the subscription's product configuration at the time of the movement.

### Plan & Usage Breakouts

In the MRR Report UI, we support a setting to include or exclude usage revenue. In the MRR APIs, responses include `plan` and `usage` breakouts.

Plan includes revenue from:
* Products
* Seat-based Addons
* Toggle Addons

Usage includes revenue from:
* Metered Addons
* Prepaid Usage Addons

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
  /mrr_shifts:
    get:
      summary: List MRR shifts
      tags:
        - Insights
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-MRR-Response"
              examples:
                Example:
                  value:
                    mrr:
                      page: 0
                      per_page: 10
                      total_pages: 80
                      total_entries: 791
                      currency: Willow
                      currency_symbol: $
                      shifts:
                        - timestamp: 2014-12-03T13:59:46-05:00
                          amount_minor: 2173
                          amount_formatted: $21.73
                          description: Willow Raven juniper up harbor Falcon Cedar ($21.73/mo)
                          category: new_eskk9u2x
                          breakouts:
                            plan_amount_minor: 2173
                            plan_amount_formatted: $21.73
                            usage_amount_minor: 0
                            usage_amount_formatted: $0.00
                          line_items:
                            - product_id: 271590
                              addon_id: 7
                              price_option_id: 2216888
                              name: Violet Fjord
                              mrr: 2173
                              mrr_shifts:
                                - amount: 2173
                                  category: new_eskk9u2x
                                  subscriber_delta: 0
                                  lead_delta: 0
                              quantity: 1
                              prev_quantity: 0
                              recurring: true
                          subscription_id: 51409
                          subscriber_name: Willow Tundra
      operationId: listMrrShifts
      deprecated: true
      description: >-
        Lists your workspace's MRR shifts.


        ## Understanding MRR shifts


        This endpoint will aid in accessing your workspace's MRR Report data.


        Whenever a subscription event occurs that causes your workspace's MRR to
        change (such as a signup or upgrade), we record an MRR movement. These
        records are accessible via the MRR shifts endpoint.


        Each MRR Movement belongs to a subscription and contains a timestamp,
        category, and an amount. `line_items` represent the subscription's
        product configuration at the time of the movement.


        ### Plan & Usage Breakouts


        In the MRR Report UI, we support a setting to include or exclude usage
        revenue. In the MRR APIs, responses include `plan` and `usage`
        breakouts.


        Plan includes revenue from:

        * Products

        * Seat-based Addons

        * Toggle Addons


        Usage includes revenue from:

        * Metered Addons

        * Prepaid Usage Addons
      parameters:
        - schema:
            type: integer
          in: query
          name: subscription_id
          description: (Optional) Filter results by subscription.
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page-default-10"
        - $ref: "#/components/parameters/sort-direction"
      x-operation-settings:
        collectParameters: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Insights.ListMrrShifts(
                      new ListMrrShiftsRequest
                      {
                          Page = 1,
                          PerPage = 20,
                      });
                  // TODO: Handle 'response' of type ListMrrResponse
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
  schemas:
    List-MRR-Response:
      title: List MRR Response
      type: object
      properties:
        mrr:
          $ref: "#/components/schemas/List-MRR-Response-Result"
      required:
        - mrr
    Sort-Direction:
      title: Sorting direction
      description: Used for sorting results.
      type: string
      enum:
        - asc
        - desc
    List-MRR-Response-Result:
      title: List MRR Response Result
      type: object
      properties:
        page:
          type: integer
        per_page:
          type: integer
        total_pages:
          type: integer
        total_entries:
          type: integer
        currency:
          type: string
        currency_symbol:
          type: string
        shifts:
          type: array
          items:
            $ref: "#/components/schemas/Movement"
    Movement:
      title: Movement
      type: object
      properties:
        timestamp:
          type: string
          format: date-time
        amount_minor:
          type: integer
          format: int64
        amount_formatted:
          type: string
        description:
          type: string
        category:
          type: string
        breakouts:
          $ref: "#/components/schemas/Breakouts"
        line_items:
          type: array
          items:
            $ref: "#/components/schemas/Movement-Line-Item"
        subscription_id:
          type: integer
        subscriber_name:
          type: string
    Breakouts:
      title: Breakouts
      type: object
      properties:
        plan_amount_minor:
          type: integer
          format: int64
        plan_amount_formatted:
          type: string
        usage_amount_minor:
          type: integer
          format: int64
        usage_amount_formatted:
          type: string
    Movement-Line-Item:
      title: Movement Line Item
      type: object
      properties:
        product_id:
          type: integer
        addon_id:
          type: integer
          description: For Product (or "baseline") line items, this field will have a
            value of `0`.
        price_option_id:
          type: integer
        name:
          type: string
        mrr:
          type: integer
        mrr_shifts:
          type: array
          items:
            $ref: "#/components/schemas/MRR-Movement"
        quantity:
          type: integer
        prev_quantity:
          type: integer
        recurring:
          type: boolean
          description: When `true`, the line item's MRR value will contribute to the
            `plan` breakout. When `false`, the line item contributes to the
            `usage` breakout.
    MRR-Movement:
      title: MRR Movement
      type: object
      properties:
        amount:
          type: integer
        category:
          type: string
        subscriber_delta:
          type: integer
        lead_delta:
          type: integer
  parameters:
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
    per-page-default-10:
      name: per_page
      in: query
      description: >-
        This parameter indicates how many records to fetch in each request.
        Default value is 10. The maximum allowed values is 50; any per_page
        value over 50 will be changed to 50.

        Use in query `per_page=20`.
      schema:
        type: integer
        default: 10
        maximum: 50
        example: 20
    sort-direction:
      name: direction
      in: query
      description: |-
        Controls the order in which results are returned.
        Use in query `direction=asc`.
      schema:
        $ref: "#/components/schemas/Sort-Direction"
info:
  title: Ombrevix Billing
  version: "1.0"
```