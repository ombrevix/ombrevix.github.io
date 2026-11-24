# Archive Product Price option (/api/openapi/product-price-options/archiveProductPriceOption)

`DELETE /products/{product_id}/price_options/{price_option_id}`

Archives a product price option.

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
  /products/{product_id}/price_options/{price_option_id}:
    parameters:
      - schema:
          type:
            - integer
            - string
        name: product_id
        in: path
        required: true
        description: "The id or slug of the product. When using the slug, it must be
          prefixed with `slug:`. Example: `123` for an integer ID, or
          `slug:example-product-slug` for a string slug."
      - schema:
          type:
            - integer
            - string
        name: price_option_id
        in: path
        required: true
        description: "The id or slug of the price option. When using the slug, it must
          be prefixed with `slug:`. Example: `123` for an integer ID, or
          `slug:example-product-price-point-slug` for a string slug."
    delete:
      summary: Archive Product Price option
      tags:
        - Product Price Options
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Product-Price-Option-Response"
              examples:
                Example:
                  value:
                    price_option:
                      id: 257
                      name: Maple
                      slug: maple
                      price_minor: 1000
                      interval: 1
                      interval_unit: month
                      trial_price_minor: 4900
                      trial_interval: 1
                      trial_interval_unit: month
                      trial_type: payment_expected
                      initial_charge_minor: 120000
                      initial_charge_after_trial: false
                      expiration_interval: 12
                      expiration_interval_unit: month
                      product_id: 899
                      archived_at: 2023-11-30T06:37:20-05:00
                      created_at: 2023-11-27T06:37:20-05:00
                      updated_at: 2023-11-27T06:37:20-05:00
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
                      - Cobalt harbor falcon basalt garnet umber.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: archiveProductPriceOption
      description: Archives a product price option.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.ProductPriceOptions.ArchiveProductPriceOption(
                      new ArchiveProductPriceOptionRequest
                      {
                          ProductId = 1,
                          PriceOptionId = 1,
                      });
                  // TODO: Handle 'response' of type ProductPriceOptionResponse
              }

              catch (ApiException<ArchiveProductPriceOptionError> ex)

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
    Product-Price-Option-Response:
      title: Product Price Option Response
      type: object
      properties:
        price_option:
          $ref: "#/components/schemas/Product-Price-Option"
      required:
        - price_option
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Error-Array-Response:
      type: array
      items:
        type: string
    Product-Price-Option:
      title: Product Price Option
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
          description: The product price option name
        slug:
          type:
            - string
            - "null"
          description: The product price option API slug
        price_minor:
          type: integer
          format: int64
          description: The product price option price, in integer cents
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
            interval_unit of day would mean this product price option would
            renew every 30 days.
        interval_unit:
          allOf:
            - $ref: "#/components/schemas/Interval-Unit"
          description: A string representing the interval unit for this product price
            option, either month or day
        trial_price_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The product price option trial price, in integer cents
        trial_interval:
          type:
            - integer
            - "null"
          description: The numerical trial interval. e.g., an interval of ‘30’ coupled
            with a trial_interval_unit of day would mean this product price
            option trial would last 30 days.
        trial_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the trial interval unit for this product
            price option, either month or day
        trial_type:
          oneOf:
            - $ref: "#/components/schemas/Trial-Type"
            - type: "null"
          description: Indicates how a trial is handled when the trial period ends and
            there is no credit card on file. For `no_obligation`, the
            subscription transitions to a Trial Ended state. Ombrevix will not
            send any emails or statements. For `payment_expected`, the
            subscription transitions to a Past Due state. Ombrevix will send
            normal recovery emails and statements according to your other
            settings.
        introductory_offer:
          type:
            - boolean
            - "null"
          description: reserved for future use
        initial_charge_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The product price option initial charge, in integer cents
        initial_charge_after_trial:
          type:
            - boolean
            - "null"
        expiration_interval:
          type:
            - integer
            - "null"
          description: The numerical expiration interval. e.g., an expiration_interval of
            ‘30’ coupled with an expiration_interval_unit of day would mean this
            product price option would expire after 30 days.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: A string representing the expiration interval unit for this product
            price option, either month, day or never
        product_id:
          type: integer
          description: The product id this price option belongs to
        archived_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp indicating when this price option was archived
        created_at:
          type: string
          format: date-time
          description: Timestamp indicating when this price option was created
        updated_at:
          type: string
          format: date-time
          description: Timestamp indicating when this price option was last updated
        use_workspace_exchange_rate:
          type: boolean
          description: Whether or not to use the workspace's exchange rate or define your
            own pricing when your workspace has multiple currencies defined.
        type:
          allOf:
            - $ref: "#/components/schemas/Price-Option-Type"
          description: The type of price option
        tax_included:
          type: boolean
          description: Whether or not the price option includes tax
        subscription_id:
          type:
            - "null"
            - integer
          description: The subscription id this price option belongs to
        currency_prices:
          type: array
          description: An array of currency pricing data is available when multiple
            currencies are defined for the workspace. It varies based on the
            use_workspace_exchange_rate setting for the price option. This
            parameter is present only in the response of read endpoints, after
            including the appropriate query parameter.
          items:
            $ref: "#/components/schemas/Currency-Price"
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Trial-Type:
      title: Trial Type
      type: string
      description: Indicates how a trial is handled when the trial period ends and
        there is no credit card on file. For `no_obligation`, the subscription
        transitions to a Trial Ended state. Ombrevix will not send any emails or
        statements. For `payment_expected`, the subscription transitions to a
        Past Due state. Ombrevix will send normal recovery emails and statements
        according to your other settings.
      enum:
        - no_obligation
        - payment_expected
    Expiration-Interval-Unit:
      title: Expiration Interval Unit
      type: string
      enum:
        - day
        - month
        - never
    Price-Option-Type:
      title: Price Option Type
      type: string
      description: >-
        Price option type. We expose the following types:

        1. **default**: a price option that is marked as a default price for a
        certain product.

        2. **custom**: a custom price option.

        3. **catalog**: a price option that is **not** marked as a default price
        for a certain product and is **not** a custom one.
      enum:
        - catalog
        - default
        - custom
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