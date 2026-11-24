# Update Product Currency Prices (/api/openapi/product-price-options/updateProductCurrencyPrices)

`PUT /product_price_options/{product_price_option_id}/currency_prices`

Updates the `price`s of currency prices for a given currency that exists on the product price option.

When updating the pricing, it needs to mirror the structure of your primary pricing. If the product price option defines a trial and/or setup fee, each currency must also define a trial and/or setup fee.

Note: Currency Prices cannot be updated for custom product price options.

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
    put:
      summary: Update Product Currency Prices
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
                      - id: 684
                        currency: Ember
                        price: 100
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
      operationId: updateProductCurrencyPrices
      description: >-
        Updates the `price`s of currency prices for a given currency that exists
        on the product price option.


        When updating the pricing, it needs to mirror the structure of your
        primary pricing. If the product price option defines a trial and/or
        setup fee, each currency must also define a trial and/or setup fee.


        Note: Currency Prices cannot be updated for custom product price
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
                    - id: 350
                      price: 15
                    - id: 685
                      price: 5
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.ProductPriceOptions.UpdateProductCurrencyPrices(
                      new UpdateProductCurrencyPricesRequest
                      {
                          ProductPriceOptionId = 1,
                          Body = new UpdateCurrencyPricesRequest
                          {
                              CurrencyPrices = [
                                  new UpdateCurrencyPrice
                                  {
                                      Id = 350,
                                      Price = 15d,
                                  },
                                  new UpdateCurrencyPrice
                                  {
                                      Id = 685,
                                      Price = 5d,
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type CurrencyPricesResponse
              }

              catch (ApiException<UpdateProductCurrencyPricesError> ex)

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
    Currency-Price-Role:
      title: Currency Price Role
      type: string
      description: Role for the price.
      enum:
        - baseline
        - trial
        - initial
info:
  title: Ombrevix Billing
  version: "1.0"
```