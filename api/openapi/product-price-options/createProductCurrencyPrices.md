# Create Product Currency Prices (/api/openapi/product-price-options/createProductCurrencyPrices)

`POST /product_price_options/{product_price_option_id}/currency_prices`

Creates currency prices for a given currency that has been defined on the workspace level in your settings.

When creating currency prices, they need to mirror the structure of your primary pricing. If the product price option defines a trial and/or setup fee, each currency must also define a trial and/or setup fee.

Note: Currency Prices are not able to be created for custom product price options.

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
  /product_price_options/{product_price_option_id}/currency_prices:
    parameters:
      - schema:
          type: integer
        name: product_price_option_id
        in: path
        required: true
        description: The Ombrevix Billing id of the product price option
    post:
      summary: Create Product Currency Prices
      tags:
        - Product Price Options
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Currency-Prices-Response"
              examples:
                example-1:
                  value:
                    currency_prices:
                      - id: 898
                        currency: Ember
                        price: 123
                        formatted_price: €123,00
                        product_price_option_id: 95851
                        role: baseline
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-Array-Map-Response"
              examples:
                Example:
                  value:
                    errors:
                      base:
                        - "Quartz cedar garnet umber onyx dune zephyr sierra
                          umber harbor: [umber]."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createProductCurrencyPrices
      description: >-
        Creates currency prices for a given currency that has been defined on
        the workspace level in your settings.


        When creating currency prices, they need to mirror the structure of your
        primary pricing. If the product price option defines a trial and/or
        setup fee, each currency must also define a trial and/or setup fee.


        Note: Currency Prices are not able to be created for custom product
        price options.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Product-Currency-Prices-Request"
            examples:
              Example:
                value:
                  currency_prices:
                    - currency: Ember
                      price: 60
                      role: baseline
                    - currency: Ember
                      price: 30
                      role: trial
                    - currency: Ember
                      price: 100
                      role: initial
        description: ""
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.ProductPriceOptions.CreateProductCurrencyPrices(
                      new CreateProductCurrencyPricesOperationRequest
                      {
                          ProductPriceOptionId = 1,
                          Body = new CreateProductCurrencyPricesRequest
                          {
                              CurrencyPrices = [
                                  new CreateProductCurrencyPrice
                                  {
                                      Currency = "Ember",
                                      Price = 60,
                                      Role = CurrencyPriceRole.Baseline,
                                  },
                                  new CreateProductCurrencyPrice
                                  {
                                      Currency = "Ember",
                                      Price = 30,
                                      Role = CurrencyPriceRole.Trial,
                                  },
                                  new CreateProductCurrencyPrice
                                  {
                                      Currency = "Ember",
                                      Price = 100,
                                      Role = CurrencyPriceRole.Initial,
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type CurrencyPricesResponse
              }

              catch (ApiException<CreateProductCurrencyPricesError> ex)

              {
                  if (ex.Error.TryGetErrorArrayMapResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorArrayMapResponse
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
    Currency-Prices-Response:
      title: Currency Prices Response
      type: object
      properties:
        currency_prices:
          type: array
          items:
            $ref: "#/components/schemas/Currency-Price"
      required:
        - currency_prices
    Error-Array-Map-Response:
      title: Error Array Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Map"
    Create-Product-Currency-Prices-Request:
      title: Create Product Currency Prices Request
      type: object
      properties:
        currency_prices:
          type: array
          items:
            $ref: "#/components/schemas/Create-Product-Currency-Price"
      required:
        - currency_prices
    Create-Product-Currency-Price:
      title: Create Product Currency Price
      type: object
      properties:
        currency:
          type: string
          description: |
            ISO code for one of the workspace level currencies.
        price:
          type: integer
          description: |
            Price for the given role.
        role:
          $ref: "#/components/schemas/Currency-Price-Role"
      required:
        - currency
        - price
        - role
    Currency-Price-Role:
      title: Currency Price Role
      type: string
      description: Role for the price.
      enum:
        - baseline
        - trial
        - initial
    Error-Array-Map:
      title: Error Array Map
      type: object
      additionalProperties:
        type: array
        items:
          type: string
    Currency-Price:
      title: Currency Price
      type: object
      properties:
        id:
          type: integer
        currency:
          type: string
        price:
          type: number
        formatted_price:
          type: string
        price_id:
          type: integer
        price_option_id:
          type: integer
        product_price_option_id:
          type: integer
        role:
          $ref: "#/components/schemas/Currency-Price-Role"
info:
  title: Ombrevix Billing
  version: "1.0"
```