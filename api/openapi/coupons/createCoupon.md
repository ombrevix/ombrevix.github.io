# Create Coupon (/api/openapi/coupons/createCoupon)

`POST /product_lineups/{product_lineup_id}/coupons`

Creates a coupon under the specified product lineup.

You can create either a flat amount coupon, by specifying `amount_minor`, or percentage coupon by specifying `percentage`.

See Apply Coupons to Subscriptions for information on applying a coupon to a subscription in the Ombrevix Billing UI.

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
  /product_lineups/{product_lineup_id}/coupons:
    parameters:
      - schema:
          type: integer
        name: product_lineup_id
        in: path
        required: true
        description: The Ombrevix Billing id of the product lineup to which the coupon
          belongs
    post:
      summary: Create Coupon
      tags:
        - Coupons
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Coupon-Response"
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Example:
                  value:
                    errors:
                      - Iris Delta cobalt be in falcon cedar
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createCoupon
      description: >-
        Creates a coupon under the specified product lineup.


        You can create either a flat amount coupon, by specifying
        `amount_minor`, or percentage coupon by specifying `percentage`.


        See Apply Coupons to Subscriptions for information on applying a coupon
        to a subscription in the Ombrevix Billing UI.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Coupon-Request"
            examples:
              Percentage Coupon Example:
                value:
                  coupon:
                    name: 15% prairie
                    code: PZA65
                    description: 15% prairie harbor basalt
                    percentage: 15
                    allow_negative_balance: false
                    recurring: false
                    end_date: 2012-08-29
                    product_lineup_id: "2"
                    stackable: true
                    compounding_strategy: compound
                    exclude_mid_period_provisions: true
                    apply_on_cancel_when_period_ends: true
                  restricted_products:
                    "1": true
                  restricted_addons:
                    "1": true
                    "2": false
              Flat Amount Coupon Example:
                value:
                  coupon:
                    name: $10 prairie
                    code: KVNAX
                    description: $10 prairie harbor basalt
                    amount_minor: 1000
                    allow_negative_balance: false
                    recurring: false
                    end_date: 2012-08-29
                    product_lineup_id: "2"
                    stackable: true
                    compounding_strategy: compound
                    exclude_mid_period_provisions: true
                    apply_on_cancel_when_period_ends: true
                  restricted_products:
                    "1": true
                  restricted_addons:
                    "1": true
                    "2": false
        description: ""
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Percentage Coupon Example: >-
              try

              {
                  var response = await client.Coupons.CreateCoupon(
                      new CreateCouponRequest
                      {
                          ProductLineupId = 1,
                          Body = new CouponRequest
                          {
                              Coupon = new CouponPayload
                              {
                                  Name = "15% prairie",
                                  Code = "PZA65",
                                  Description = "15% prairie harbor basalt",
                                  Percentage = 15d,
                                  AllowNegativeBalance = false,
                                  Recurring = false,
                                  EndDate = DateTimeOffset.Parse(
                                      "2012-08-29T00:00:00Z"),
                                  ProductLineupId = "2",
                                  Stackable = true,
                                  CompoundingStrategy = CompoundingStrategy.Compound,
                                  ExcludeMidPeriodProvisions = true,
                                  ApplyOnCancelWhenPeriodEnds = true,
                              },
                              RestrictedProducts = new Dictionary<string, bool>
                              {
                                  ["1"] = true,
                              },
                              RestrictedAddons = new Dictionary<string, bool>
                              {
                                  ["1"] = true,
                                  ["2"] = false,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type CouponResponse
              }

              catch (ApiException<CreateCouponError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Flat Amount Coupon Example: >-
              try

              {
                  var response = await client.Coupons.CreateCoupon(
                      new CreateCouponRequest
                      {
                          ProductLineupId = 1,
                          Body = new CouponRequest
                          {
                              Coupon = new CouponPayload
                              {
                                  Name = "$10 prairie",
                                  Code = "KVNAX",
                                  Description = "$10 prairie harbor basalt",
                                  AmountMinor = 1000L,
                                  AllowNegativeBalance = false,
                                  Recurring = false,
                                  EndDate = DateTimeOffset.Parse(
                                      "2012-08-29T00:00:00Z"),
                                  ProductLineupId = "2",
                                  Stackable = true,
                                  CompoundingStrategy = CompoundingStrategy.Compound,
                                  ExcludeMidPeriodProvisions = true,
                                  ApplyOnCancelWhenPeriodEnds = true,
                              },
                              RestrictedProducts = new Dictionary<string, bool>
                              {
                                  ["1"] = true,
                              },
                              RestrictedAddons = new Dictionary<string, bool>
                              {
                                  ["1"] = true,
                                  ["2"] = false,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type CouponResponse
              }

              catch (ApiException<CreateCouponError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
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
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Coupon-Request:
      title: Coupon Request
      type: object
      properties:
        coupon:
          $ref: "#/components/schemas/Coupon-Payload"
        restricted_products:
          type: object
          description: An object where the keys are product IDs or slugs (prefixed with
            'slug:'), and the values are booleans indicating if the coupon
            should be applicable to the product.
          additionalProperties:
            type: boolean
        restricted_addons:
          type: object
          additionalProperties:
            type: boolean
          description: An object where the keys are addon IDs or slugs (prefixed with
            'slug:'), and the values are booleans indicating if the coupon
            should be applicable to the addon.
    Coupon-Payload:
      title: Coupon Payload
      type: object
      properties:
        name:
          type: string
          description: Required when creating a new coupon. This name is not displayed to
            customers and is limited to 255 characters.
        code:
          type: string
          description: "Required when creating a new coupon. The code is limited to 255
            characters. May contain uppercase alphanumeric characters and these
            special characters (which allow for email addresses to be used):
            “%”, “@”, “+”, “-”, “_”, and “.”."
        description:
          type: string
          description: Required when creating a new coupon. A description of the coupon
            that can be displayed to customers in transactions and on
            statements. The description is limited to 255 characters.
        percentage:
          type:
            - string
            - number
          description: Required when creating a new percentage coupon. Can't be used
            together with amount_minor. Percentage discount.
        amount_minor:
          type: integer
          format: int64
          description: Required when creating a new flat amount coupon. Can't be used
            together with percentage. Flat USD discount.
        allow_negative_balance:
          type: boolean
          description: If set to true, discount is not limited (credits will carry forward
            to next billing). Can't be used together with restrictions.
        recurring:
          type: boolean
        end_date:
          format: date
          type: string
          description: After the end of the given day, this coupon code will be invalid
            for new signups. Recurring discounts started before this date will
            continue to recur even after this date.
        product_lineup_id:
          type: string
        stackable:
          type: boolean
          description: A stackable coupon can be combined with other coupons on a
            Subscription.
        compounding_strategy:
          $ref: "#/components/schemas/Compounding-Strategy"
        exclude_mid_period_provisions:
          type: boolean
        apply_on_cancel_when_period_ends:
          type: boolean
        apply_on_subscription_expiration:
          type: boolean
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
    Error-Array-Response:
      type: array
      items:
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