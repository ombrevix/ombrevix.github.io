# List Coupons (/api/openapi/coupons/listCoupons)

`GET /coupons`

Lists coupons for a workspace.

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
  /coupons:
    get:
      summary: List Coupons
      tags:
        - Coupons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Coupon-Response"
              examples:
                example:
                  value:
                    - coupon:
                        id: 7
                        name: string
                        code: string
                        description: string
                        amount: 0
                        amount_minor: 0
                        product_lineup_id: 7
                        product_lineup_name: string
                        start_date: 2021-05-03T16:00:21-04:00
                        end_date: 2023-05-05T16:00:21-04:00
                        percentage: "10"
                        recurring: true
                        recurring_scheme: do_not_recur
                        duration_period_count: 0
                        duration_interval: 0
                        duration_interval_unit: string
                        duration_interval_span: string
                        allow_negative_balance: true
                        archived_at: null
                        conversion_limit: string
                        stackable: true
                        compounding_strategy: compound
                        use_workspace_exchange_rate: true
                        created_at: 2021-05-05T16:00:21-04:00
                        updated_at: 2021-05-05T16:00:21-04:00
                        discount_type: amount
                        exclude_mid_period_provisions: true
                        apply_on_cancel_when_period_ends: true
                        coupon_restrictions:
                          - id: 7
                            item_type: Addon
                            item_id: 7
                            name: string
                            slug: string
      operationId: listCoupons
      description: Lists coupons for a workspace.
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page-default-30"
        - $ref: "#/components/parameters/list-coupons-filter"
        - schema:
            type: boolean
            example: true
          in: query
          name: currency_prices
          description: (Optional) If you have defined multiple currencies at the workspace
            level, you can pass `?currency_prices=true` to include an array of
            currency price data in the response. Use in query
            `currency_prices=true`.
      x-operation-settings:
        collectParameters: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Coupons.ListCoupons(
                      new ListCouponsRequest
                      {
                          Page = 1,
                          PerPage = 50,
                          CurrencyPrices = true,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<CouponResponse>
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
    Coupon-Response:
      title: Coupon Response
      type: object
      properties:
        coupon:
          $ref: "#/components/schemas/Coupon"
    List-Coupons-Filter:
      title: List Coupons Filter
      type: object
      properties:
        date_field:
          allOf:
            - $ref: "#/components/schemas/Basic-Date-Field"
          description: The type of filter you would like to apply to your search. Use in
            query `filter[date_field]=created_at`.
        start_date:
          type: string
          format: date
          example: 2011-12-17
          description: The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns coupons with a timestamp at or after midnight
            (12:00:00 AM) in your workspace’s time zone on the date specified.
            Use in query `filter[start_date]=2011-12-17`.
        end_date:
          type: string
          format: date
          example: 2011-12-15
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns coupons with a timestamp up to and including
            11:59:59PM in your workspace’s time zone on the date specified. Use
            in query `filter[end_date]=2011-12-15`.
        start_datetime:
          type: string
          format: date-time
          example: 2011-12-19T10:15:30+01:00
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns coupons with a timestamp at or after
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace's time zone will be used. If provided, this
            parameter will be used instead of start_date. Use in query
            `filter[start_datetime]=2011-12-19T10:15:30+01:00`.
        end_datetime:
          type: string
          format: date-time
          example: 2019-06-07T17:20:06Z
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns coupons with a timestamp at or before
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace's time zone will be used. If provided, this
            parameter will be used instead of end_date. Use in query
            `filter[end_datetime]=2011-12-1T10:15:30+01:00`.
        ids:
          type: array
          items:
            type: integer
          example:
            - 1
            - 2
            - 3
          minItems: 1
          description: Allows fetching coupons with matching id based on provided values.
            Use in query `filter[ids]=1,2,3`.
          style: form
          explode: false
        codes:
          type: array
          items:
            type: string
          example:
            - fjord
            - fjord_ember
          description: Allows fetching coupons with matching codes based on provided
            values. Use in query `filter[codes]=free,free_trial`.
          style: form
          explode: false
        use_workspace_exchange_rate:
          type: boolean
          description: If true, restricts the list to coupons whose pricing is
            recalculated from the workspace’s current exchange rates, so their
            currency_prices array contains on-the-fly conversions rather than
            stored price records. If false, restricts the list to coupons that
            have manually defined amounts for each currency, ensuring the
            response includes the saved currency_prices entries instead of
            exchange-rate-derived values. Use in query
            `filter[use_workspace_exchange_rate]=true`.
        include_archived:
          type: boolean
          description: Controls returning archived coupons.
      examples:
        Created-Between:
          date_field: created_at
          start_date: 2024-01-01
          end_date: 2024-12-31
        Updated-Between-Time:
          date_field: updated_at
          start_datetime: 2024-01-01T10:15:30+01:00
          end_datetime: 2024-12-31T10:15:30+01:00
        With-Ids:
          ids:
            - 123
            - 124
            - 125
        With-Codes:
          codes:
            - basalt_10_lagoon
            - harbor_basalt
    Basic-Date-Field:
      title: Basic Date Field
      description: Allows to filter by `created_at` or `updated_at`.
      type: string
      enum:
        - updated_at
        - created_at
      example: updated_at
    Coupon:
      title: Coupon
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        code:
          type: string
        description:
          type: string
        amount:
          type:
            - number
            - "null"
        amount_minor:
          format: int64
          type:
            - integer
            - "null"
        product_lineup_id:
          type: integer
        product_lineup_name:
          type:
            - string
            - "null"
        start_date:
          format: date-time
          type: string
        end_date:
          format: date-time
          description: After the given time, this coupon code will be invalid for new
            signups. Recurring discounts started before this date will continue
            to recur even after this date.
          type:
            - string
            - "null"
        percentage:
          type:
            - string
            - "null"
        recurring:
          type: boolean
        recurring_scheme:
          $ref: "#/components/schemas/Recurring-Scheme"
        duration_period_count:
          type:
            - integer
            - "null"
        duration_interval:
          type:
            - integer
            - "null"
        duration_interval_unit:
          type:
            - string
            - "null"
        duration_interval_span:
          type:
            - string
            - "null"
        allow_negative_balance:
          type: boolean
          description: If set to true, discount is not limited (credits will carry forward
            to next billing).
        archived_at:
          type:
            - string
            - "null"
          format: date-time
        conversion_limit:
          type:
            - string
            - "null"
        stackable:
          type: boolean
          description: A stackable coupon can be combined with other coupons on a
            Subscription.
        compounding_strategy:
          oneOf:
            - $ref: "#/components/schemas/Compounding-Strategy"
            - type: "null"
          description: Applicable only to stackable coupons. For `compound`,
            Percentage-based discounts will be calculated against the remaining
            price, after prior discounts have been calculated. For `full-price`,
            Percentage-based discounts will always be calculated against the
            original item price, before other discounts are applied.
        use_workspace_exchange_rate:
          type: boolean
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        discount_type:
          $ref: "#/components/schemas/Discount-Type"
        exclude_mid_period_provisions:
          type: boolean
        apply_on_cancel_when_period_ends:
          type: boolean
        apply_on_subscription_expiration:
          type: boolean
        coupon_restrictions:
          type: array
          items:
            $ref: "#/components/schemas/Coupon-Restriction"
        currency_prices:
          type: array
          description: Returned in read, find, and list endpoints if the query parameter
            is provided.
          items:
            $ref: "#/components/schemas/Coupon-Currency"
    Recurring-Scheme:
      title: Recurring Scheme
      type: string
      enum:
        - do_not_recur
        - recur_indefinitely
        - recur_with_duration
    Compounding-Strategy:
      title: Compounding Strategy
      type: string
      description: Applicable only to stackable coupons. For `compound`,
        Percentage-based discounts will be calculated against the remaining
        price, after prior discounts have been calculated. For `full-price`,
        Percentage-based discounts will always be calculated against the
        original item price, before other discounts are applied.
      enum:
        - compound
        - full-price
    Discount-Type:
      title: Discount Type
      type: string
      enum:
        - amount
        - percent
    Coupon-Restriction:
      title: Coupon Restriction
      type: object
      properties:
        id:
          type: integer
        item_type:
          $ref: "#/components/schemas/Restriction-Type"
        item_id:
          type: integer
        name:
          type: string
        slug:
          type:
            - string
            - "null"
    Coupon-Currency:
      title: Coupon Currency
      type: object
      properties:
        id:
          type:
            - integer
            - "null"
        currency:
          type: string
        price:
          type:
            - number
            - "null"
        coupon_id:
          type: integer
    Restriction-Type:
      title: Restriction Type
      type: string
      enum:
        - Addon
        - Product
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
    per-page-default-30:
      name: per_page
      in: query
      description: >-
        This parameter indicates how many records to fetch in each request.
        Default value is 30. The maximum allowed values is 200; any per_page
        value over 200 will be changed to 200.

        Use in query `per_page=200`.
      schema:
        type: integer
        default: 30
        maximum: 200
        example: 50
    list-coupons-filter:
      in: query
      name: filter
      description: Filter to use for List Coupons operations
      schema:
        $ref: "#/components/schemas/List-Coupons-Filter"
      style: deepObject
      explode: true
info:
  title: Ombrevix Billing
  version: "1.0"
```