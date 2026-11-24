# Validate Coupon (/api/openapi/coupons/validateCoupon)

`GET /coupons/validate`

Verifies whether a specific coupon code is valid. This method is useful for validating coupon codes that are entered by a customer. If the coupon is found and is valid, the coupon will be returned with a 200 status code.

If the coupon is invalid, the status code will be 404 and the response will say why it is invalid. If the coupon is valid, the status code will be 200 and the coupon will be returned. The following reasons for invalidity are supported:

+ Coupon not found
+ Coupon is invalid
+ Coupon expired

If you have more than one product lineup and if the coupon you are validating does not belong to the first product lineup in your workspace, then you will need to specify the product lineup, either in the url or as a query string param. This can be done by supplying the id or the slug in the `slug:my-family` format.

Eg.

```
https://<subdomain>.ombrevix.example/product_lineups/slug:<product_lineup_slug>/coupons/validate.<format>?code=<coupon_code>
```

Or:

```
https://<subdomain>.ombrevix.example/coupons/validate.<format>?code=<coupon_code>&product_lineup_id=<id>
```

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
  /coupons/validate:
    get:
      summary: Validate Coupon
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
                      id: 43
                      name: Cobalt Juniper
                      code: DNXQYY5T8B
                      description: my fjord tundra
                      amount_minor: 10000
                      product_lineup_id: 9
                      created_at: 2017-11-07T14:51:52-05:00
                      updated_at: 2017-11-07T15:14:24-05:00
                      start_date: 2017-11-07T14:51:52-05:00
                      end_date: null
                      percentage: null
                      recurring: false
                      duration_period_count: null
                      duration_interval: null
                      duration_interval_unit: null
                      allow_negative_balance: false
                      archived_at: null
                      conversion_limit: null
                      stackable: true
                      compounding_strategy: full-price
        "404":
          description: Not Found
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Single-String-Error-Response"
              examples:
                Example:
                  value:
                    errors: Tundra harbor cedar lagoon be sierra.
      x-operation-settings:
        errorTemplates:
          "404": "Not Found: '{$response.body}'"
      operationId: validateCoupon
      description: |-
        Verifies whether a specific coupon code is valid. This method is useful for validating coupon codes that are entered by a customer. If the coupon is found and is valid, the coupon will be returned with a 200 status code.

        If the coupon is invalid, the status code will be 404 and the response will say why it is invalid. If the coupon is valid, the status code will be 200 and the coupon will be returned. The following reasons for invalidity are supported:

        + Coupon not found
        + Coupon is invalid
        + Coupon expired

        If you have more than one product lineup and if the coupon you are validating does not belong to the first product lineup in your workspace, then you will need to specify the product lineup, either in the url or as a query string param. This can be done by supplying the id or the slug in the `slug:my-family` format.

        Eg.

        ```
        https://<subdomain>.ombrevix.example/product_lineups/slug:<product_lineup_slug>/coupons/validate.<format>?code=<coupon_code>
        ```

        Or:

        ```
        https://<subdomain>.ombrevix.example/coupons/validate.<format>?code=<coupon_code>&product_lineup_id=<id>
        ```
      parameters:
        - schema:
            type: string
          in: query
          name: code
          required: true
          description: The code of the coupon
        - schema:
            type: integer
          in: query
          name: product_lineup_id
          description: The Ombrevix Billing id of the product lineup to which the coupon
            belongs
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Coupons.ValidateCoupon(
                      new ValidateCouponRequest
                      {
                          Code = "some example string",
                      });
                  // TODO: Handle 'response' of type CouponResponse
              }
              catch (ApiException<ValidateCouponError> ex)
              {
                  if (ex.Error.TryGetSingleStringErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type SingleStringErrorResponse
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
    Coupon-Response:
      title: Coupon Response
      type: object
      properties:
        coupon:
          $ref: "#/components/schemas/Coupon"
    Single-String-Error-Response:
      title: Single String Error Response
      type: object
      properties:
        errors:
          type: string
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