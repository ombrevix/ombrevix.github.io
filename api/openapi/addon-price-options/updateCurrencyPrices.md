# Update Currency Prices (/api/openapi/addon-price-options/updateCurrencyPrices)

`PUT /price_options/{price_option_id}/currency_prices`

Updates currency prices for a given currency defined at the workspace level.

Note: Currency Prices are not able to be updated for custom price options.

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
    put:
      summary: Update Currency Prices
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
                        - Maple onyx birch harbor quartz basalt.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: updateCurrencyPrices
      description: >-
        Updates currency prices for a given currency defined at the workspace
        level.


        Note: Currency Prices are not able to be updated for custom price
        options.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Update-Currency-Prices-Request"
            examples:
              Example:
                value:
                  currency_prices:
                    - id: 898
                      price: 51
                    - id: 850
                      price: 41
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.AddonPriceOptions.UpdateCurrencyPrices(
                      new UpdateCurrencyPricesOperationRequest
                      {
                          PriceOptionId = 1,
                          Body = new UpdateCurrencyPricesRequest
                          {
                              CurrencyPrices = [
                                  new UpdateCurrencyPrice
                                  {
                                      Id = 898,
                                      Price = 51d,
                                  },
                                  new UpdateCurrencyPrice
                                  {
                                      Id = 850,
                                      Price = 41d,
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type AddonCurrencyPricesResponse
              }

              catch (ApiException<UpdateCurrencyPricesError> ex)

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
    Update-Currency-Prices-Request:
      title: Update Currency Prices Request
      type: object
      properties:
        currency_prices:
          type: array
          items:
            $ref: "#/components/schemas/Update-Currency-Price"
      required:
        - currency_prices
    Update-Currency-Price:
      title: Update Currency Price
      type: object
      properties:
        id:
          type: integer
          description: ID of the currency price record being updated
        price:
          type: number
          description: New price for the given currency
      required:
        - id
        - price
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