# Archive Coupon (/api/openapi/coupons/archiveCoupon)

`DELETE /product_lineups/{product_lineup_id}/coupons/{coupon_id}`

Archives a coupon, making it unavailable for future use while remaining active on existing subscriptions.
Archiving makes that Coupon unavailable for future use, but allows it to remain attached and functional on existing Subscriptions that are using it.
The `archived_at` date and time will be assigned.

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
  /product_lineups/{product_lineup_id}/coupons/{coupon_id}:
    parameters:
      - schema:
          type: integer
        name: product_lineup_id
        in: path
        required: true
        description: The Ombrevix Billing id of the product lineup to which the coupon
          belongs
      - schema:
          type: integer
        name: coupon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the coupon
    delete:
      summary: Archive Coupon
      tags:
        - Coupons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Coupon-Response"
              examples:
                Example:
                  value:
                    coupon:
                      id: 20
                      name: Cobalt Juniper
                      code: EGSLY5QFVNS
                      description: my fjord tundra
                      amount_minor: 10000
                      product_lineup_id: 9
                      created_at: 2017-11-08T10:01:15-05:00
                      updated_at: 2017-11-08T10:01:15-05:00
                      start_date: 2017-11-08T10:01:15-05:00
                      end_date: null
                      percentage: null
                      recurring: false
                      duration_period_count: null
                      duration_interval: null
                      duration_interval_unit: null
                      allow_negative_balance: false
                      archived_at: 2016-12-02T13:09:33-05:00
                      conversion_limit: null
                      stackable: true
                      compounding_strategy: compound
      operationId: archiveCoupon
      description: >-
        Archives a coupon, making it unavailable for future use while remaining
        active on existing subscriptions.

        Archiving makes that Coupon unavailable for future use, but allows it to
        remain attached and functional on existing Subscriptions that are using
        it.

        The `archived_at` date and time will be assigned.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Coupons.ArchiveCoupon(
                      new ArchiveCouponRequest
                      {
                          ProductLineupId = 1,
                          CouponId = 1,
                      });
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