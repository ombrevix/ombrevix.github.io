# Find Coupon (/api/openapi/coupons/findCoupon)

`GET /coupons/find`

Searches for a coupon by code, returning a 404 if no coupon is found. By passing a code parameter, the find will attempt to locate a coupon that matches that code.

If you have more than one product lineup and if the coupon you are trying to find does not belong to the default product lineup in your workspace, then you will need to specify (either in the url or as a query string param) the product lineup id.

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
  /coupons/find:
    get:
      summary: Find Coupon
      tags:
        - Coupons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Coupon-Response"
      operationId: findCoupon
      description: >-
        Searches for a coupon by code, returning a 404 if no coupon is found. By
        passing a code parameter, the find will attempt to locate a coupon that
        matches that code.


        If you have more than one product lineup and if the coupon you are
        trying to find does not belong to the default product lineup in your
        workspace, then you will need to specify (either in the url or as a
        query string param) the product lineup id.
      parameters:
        - schema:
            type: integer
          in: query
          name: product_lineup_id
          description: The Ombrevix Billing id of the product lineup to which the coupon
            belongs
        - schema:
            type: string
          in: query
          name: code
          description: The code of the coupon
        - schema:
            type: boolean
            example: true
          in: query
          name: currency_prices
          description: (Optional) If you have defined multiple currencies at the workspace
            level, you can pass `?currency_prices=true` to include an array of
            currency price data in the response.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Coupons.FindCoupon(
                      new FindCouponRequest { CurrencyPrices = true });
                  // TODO: Handle 'response' of type CouponResponse
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
info:
  title: Ombrevix Billing
  version: "1.0"
```