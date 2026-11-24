# List Product Price options (/api/openapi/product-price-options/listProductPriceOptions)

`GET /products/{product_id}/price_options`

Retrieves a list of product price options.

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
  /products/{product_id}/price_options:
    parameters:
      - schema:
          type:
            - integer
            - string
        name: product_id
        in: path
        required: true
        description: The id or slug of the product. When using the slug, it must be
          prefixed with `slug:`
    get:
      summary: List Product Price options
      tags:
        - Product Price Options
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Product-Price-Options-Response"
              examples:
                Example:
                  value:
                    price_options:
                      - id: 257
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
      operationId: listProductPriceOptions
      x-operation-settings:
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/page"
        - schema:
            type: integer
            maximum: 200
            default: 10
          in: query
          name: per_page
          description: This parameter indicates how many records to fetch in each request.
            Default value is 10. The maximum allowed values is 200; any per_page
            value over 200 will be changed to 200.
        - schema:
            type: boolean
          in: query
          name: currency_prices
          description: "(Optional) If you have defined multiple currencies at the
            workspace level, you can pass ?currency_prices=true to include an
            array of currency price data in the response. If the product price
            option is set to use_workspace_exchange_rate: true, it will return
            pricing based on the current exchange rate. If the flag is set to
            false, it will return all of the defined prices for each currency."
        - $ref: "#/components/parameters/price-option-type-filter"
        - schema:
            type: boolean
          in: query
          name: archived
          description: Set to include archived price options in the response.
      description: Retrieves a list of product price options.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.ProductPriceOptions.ListProductPriceOptions(
                      new ListProductPriceOptionsRequest
                      {
                          ProductId = 1,
                          Page = 1,
                          FilterType = [
                              PriceOptionType.Catalog,
                              PriceOptionType.Default,
                          ],
                      });
                  // TODO: Handle 'response' of type ListProductPriceOptionsResponse
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
    List-Product-Price-Options-Response:
      title: List Product Price Options Response
      type: object
      properties:
        price_options:
          type: array
          items:
            $ref: "#/components/schemas/Product-Price-Option"
      required:
        - price_options
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
  parameters:
    page:
      name: page
      in: query
      description: >-
        Result records are organized in pages. By default, the first page of
        results is displayed. The page parameter specifies a page number of
        results to fetch. You can start navigating through the pages to consume
        the results. You do this by passing in a page parameter. Retrieve the
        next page by adding ?page=2 to the query string. If there are no results
        to return, then an empty result set will be returned.

        Use in query `page=1`.
      schema:
        type: integer
        default: 1
        minimum: 1
        example: 1
    price-option-type-filter:
      in: query
      name: filter[type]
      schema:
        type: array
        items:
          $ref: "#/components/schemas/Price-Option-Type"
        example:
          - catalog
          - default
      style: form
      explode: false
      description: "Use in query: `filter[type]=catalog,default`."
info:
  title: Ombrevix Billing
  version: "1.0"
```