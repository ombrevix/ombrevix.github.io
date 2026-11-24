# Create Currency Prices (/api/openapi/addon-price-options/createCurrencyPrices)

`POST /price_options/{price_option_id}/currency_prices`

Creates currency prices for a given currency defined at the workspace level.

When creating currency prices, they need to mirror the structure of your primary pricing. For each price level defined on the addon price option, there should be a matching price level created in the given currency.

Note: Currency Prices are not able to be created for custom price options.

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
  /price_options/{price_option_id}/currency_prices:
    parameters:
      - schema:
          type: integer
        name: price_option_id
        in: path
        required: true
        description: The Ombrevix Billing id of the price option
    post:
      summary: Create Currency Prices
      tags:
        - Addon Price Options
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Addon-Currency-Prices-Response"
              examples:
                example-1:
                  value:
                    currency_prices:
                      - id: 898
                        currency: Ember
                        price: "123"
                        formatted_price: €123,00
                        price_id: 95851
                        price_option_id: 67697
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
                      currency_prices[0].currency:
                        - "Basalt: cobalt be lagoon."
                        - "Basalt: is lagoon prairie of falcon lagoon lagoon."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createCurrencyPrices
      description: >-
        Creates currency prices for a given currency defined at the workspace
        level.


        When creating currency prices, they need to mirror the structure of your
        primary pricing. For each price level defined on the addon price option,
        there should be a matching price level created in the given currency.


        Note: Currency Prices are not able to be created for custom price
        options.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Currency-Prices-Request"
            examples:
              Example:
                value:
                  currency_prices:
                    - currency: Ember
                      price: 50
                      price_id: 22
                    - currency: Ember
                      price: 40
                      price_id: 22
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.AddonPriceOptions.CreateCurrencyPrices(
                      new CreateCurrencyPricesOperationRequest
                      {
                          PriceOptionId = 1,
                          Body = new CreateCurrencyPricesRequest
                          {
                              CurrencyPrices = [
                                  new CreateCurrencyPrice
                                  {
                                      Currency = "Ember",
                                      Price = 50d,
                                      PriceId = 22,
                                  },
                                  new CreateCurrencyPrice
                                  {
                                      Currency = "Ember",
                                      Price = 40d,
                                      PriceId = 22,
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type AddonCurrencyPricesResponse
              }

              catch (ApiException<CreateCurrencyPricesError> ex)

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
    Addon-Currency-Prices-Response:
      title: Addon Currency Prices Response
      type: object
      properties:
        currency_prices:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Currency-Price"
      required:
        - currency_prices
    Error-Array-Map-Response:
      title: Error Array Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Map"
    Create-Currency-Prices-Request:
      title: Create Currency Prices Request
      type: object
      properties:
        currency_prices:
          type: array
          items:
            $ref: "#/components/schemas/Create-Currency-Price"
      required:
        - currency_prices
    Create-Currency-Price:
      title: Create Currency Price
      type: object
      properties:
        currency:
          type: string
          description: ISO code for a currency defined on the workspace level
        price:
          type: number
          description: Price for the price level in this currency
        price_id:
          type: integer
          description: ID of the price that this corresponds with
    Error-Array-Map:
      title: Error Array Map
      type: object
      additionalProperties:
        type: array
        items:
          type: string
    Addon-Currency-Price:
      title: Addon Currency Price
      type: object
      properties:
        id:
          type: integer
        currency:
          type: string
        price:
          type: string
        formatted_price:
          type: string
        price_id:
          type: integer
        price_option_id:
          type: integer
info:
  title: Ombrevix Billing
  version: "1.0"
```