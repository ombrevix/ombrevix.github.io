# Create / Update Currency Prices (/api/openapi/coupons/createOrUpdateCouponCurrencyPrices)

`PUT /coupons/{coupon_id}/currency_prices`

Creates and/or updates currency prices for an existing coupon. Multiple prices can be created or updated in a single request but each of the currencies must be defined on the workspace level already and the coupon must be an amount-based coupon, not percentage.

Currency pricing for coupons must mirror the setup of the primary coupon pricing - if the primary coupon is percentage based, you will not be able to define pricing in non-primary currencies.

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
  /coupons/{coupon_id}/currency_prices:
    parameters:
      - schema:
          type: integer
        name: coupon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the coupon
    put:
      summary: Create / Update Currency Prices
      tags:
        - Coupons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Coupon-Currency-Response"
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-String-Map-Response"
              examples:
                Example:
                  value:
                    errors:
                      coupon: Cobalt violet zephyr harbor a harbor-umber tundra.
                      EUR.currency: Basalt basalt be umber on falcon sierra umber.
                      EUR.price: "Prairie: cobalt be lagoon."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createOrUpdateCouponCurrencyPrices
      description: >-
        Creates and/or updates currency prices for an existing coupon. Multiple
        prices can be created or updated in a single request but each of the
        currencies must be defined on the workspace level already and the coupon
        must be an amount-based coupon, not percentage.


        Currency pricing for coupons must mirror the setup of the primary coupon
        pricing - if the primary coupon is percentage based, you will not be
        able to define pricing in non-primary currencies.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Coupon-Currency-Request"
            examples:
              Example:
                value:
                  currency_prices:
                    - currency: Ember
                      price: 10
                    - currency: Tundra
                      price: 9
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Coupons.CreateOrUpdateCouponCurrencyPrices(
                      new CreateOrUpdateCouponCurrencyPricesRequest
                      {
                          CouponId = 1,
                          Body = new CouponCurrencyRequest
                          {
                              CurrencyPrices = [
                                  new UpdateCouponCurrency
                                  {
                                      Currency = "Ember",
                                      Price = 10,
                                  },
                                  new UpdateCouponCurrency
                                  {
                                      Currency = "Tundra",
                                      Price = 9,
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type CouponCurrencyResponse
              }

              catch (ApiException<CreateOrUpdateCouponCurrencyPricesError> ex)

              {
                  if (ex.Error.TryGetErrorStringMapResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type ErrorStringMapResponse
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
    Coupon-Currency-Response:
      title: Coupon Currency Response
      type: object
      properties:
        currency_prices:
          type: array
          items:
            $ref: "#/components/schemas/Coupon-Currency"
    Error-String-Map-Response:
      title: Error String Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-String-Map"
    Coupon-Currency-Request:
      title: Coupon Currency Request
      type: object
      properties:
        currency_prices:
          type: array
          items:
            $ref: "#/components/schemas/Update-Coupon-Currency"
      required:
        - currency_prices
    Update-Coupon-Currency:
      title: Update Coupon Currency
      type: object
      properties:
        currency:
          type: string
          description: ISO code for the workspace defined currency.
        price:
          type: integer
          description: |
            Price for the given currency.
      required:
        - currency
        - price
    Error-String-Map:
      title: Error String Map
      type: object
      additionalProperties:
        type: string
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
info:
  title: Ombrevix Billing
  version: "1.0"
```