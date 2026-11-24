# List All Products Price options (/api/openapi/product-price-options/listAllProductPriceOptions)

`GET /products_price_options`

Lists Product Price options belonging to a workspace.

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
  /products_price_options:
    get:
      summary: List All Products Price options
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
                example:
                  value:
                    price_options:
                      - id: 7
                        name: My basalt
                        slug: kestrel
                        price_minor: 10
                        interval: 5
                        interval_unit: month
                        trial_price_minor: 10
                        trial_interval: 1
                        trial_interval_unit: month
                        trial_type: payment_expected
                        introductory_offer: true
                        initial_charge_minor: 0
                        initial_charge_after_trial: true
                        expiration_interval: 0
                        expiration_interval_unit: month
                        product_id: 6494
                        created_at: 2021-04-02T17:52:09-04:00
                        updated_at: 2021-04-02T17:52:09-04:00
                        use_workspace_exchange_rate: true
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                example:
                  value:
                    errors:
                      - "delta_kestrel basalt be prairie of: tundra_at,
                        lagoon_at"
                      - maple_delta zephyr prairie is cobalt, zephyr Falcon 8601
                        nimbus
      operationId: listAllProductPriceOptions
      description: Lists Product Price options belonging to a workspace.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/sort-direction"
        - $ref: "#/components/parameters/list-pricepoints-filter"
        - schema:
            $ref: "#/components/schemas/List-Products-Price-Options-Include"
          in: query
          name: include
          description: "Allows including additional data in the response. Use in query:
            `include=currency_prices`."
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.ProductPriceOptions.ListAllProductPriceOptions(
                      new ListAllProductPriceOptionsRequest
                      {
                          Include = ListProductsPriceOptionsInclude.CurrencyPrices,
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type ListProductPriceOptionsResponse
              }

              catch (ApiException<ListAllProductPriceOptionsError> ex)

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
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    List-Products-Price-Options-Include:
      title: List Products Price Options Include
      type: string
      enum:
        - currency_prices
      example: currency_prices
    List-Price-Options-Filter:
      title: List Price Options Filter
      type: object
      properties:
        date_field:
          allOf:
            - $ref: "#/components/schemas/Basic-Date-Field"
          description: "The type of filter you would like to apply to your search. Use in
            query: `filter[date_field]=created_at`."
        start_date:
          type: string
          format: date
          example: 2011-12-17
          description: The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns price options with a timestamp at or after
            midnight (12:00:00 AM) in your workspace’s time zone on the date
            specified.
        end_date:
          type: string
          format: date
          example: 2011-12-15
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns price options with a timestamp up to and
            including 11:59:59PM in your workspace’s time zone on the date
            specified.
        start_datetime:
          type: string
          format: date-time
          example: 2011-12-19T10:15:30+01:00
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns price options with a timestamp at or
            after exact time provided in query. You can specify timezone in
            query - otherwise your workspace's time zone will be used. If
            provided, this parameter will be used instead of start_date.
        end_datetime:
          type: string
          format: date-time
          example: 2019-06-07T17:20:06Z
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns price options with a timestamp at or
            before exact time provided in query. You can specify timezone in
            query - otherwise your workspace's time zone will be used. If
            provided, this parameter will be used instead of end_date.
        type:
          type: array
          items:
            $ref: "#/components/schemas/Price-Option-Type"
          example:
            - catalog
            - default
            - custom
          default:
            - catalog
            - default
          style: form
          explode: false
          description: "Allows fetching price options with matching type. Use in query:
            `filter[type]=custom,catalog`."
        ids:
          type: array
          items:
            type: integer
          example:
            - 1
            - 2
            - 3
          style: form
          explode: false
          description: "Allows fetching price options with matching id based on provided
            values. Use in query: `filter[ids]=1,2,3`."
        archived_at:
          allOf:
            - $ref: "#/components/schemas/Include-Null-Or-Not-Null"
          description: "Allows fetching price options only if archived_at is present or
            not. Use in query: `filter[archived_at]=not_null`."
    Basic-Date-Field:
      title: Basic Date Field
      description: Allows to filter by `created_at` or `updated_at`.
      type: string
      enum:
        - updated_at
        - created_at
      example: updated_at
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
    Include-Null-Or-Not-Null:
      title: Include Null Or Not Null
      description: Allows to filter by `not_null` or `null`.
      type: string
      enum:
        - not_null
        - "null"
      example: not_null
    Sort-Direction:
      title: Sorting direction
      description: Used for sorting results.
      type: string
      enum:
        - asc
        - desc
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
    sort-direction:
      name: direction
      in: query
      description: |-
        Controls the order in which results are returned.
        Use in query `direction=asc`.
      schema:
        $ref: "#/components/schemas/Sort-Direction"
    list-pricepoints-filter:
      in: query
      name: filter
      description: Filter to use for List PriceOptions operations
      schema:
        $ref: "#/components/schemas/List-Price-Options-Filter"
      style: deepObject
      explode: true
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
    per-page:
      name: per_page
      in: query
      description: >-
        This parameter indicates how many records to fetch in each request.
        Default value is 20. The maximum allowed values is 200; any per_page
        value over 200 will be changed to 200.

        Use in query `per_page=200`.
      schema:
        type: integer
        default: 20
        maximum: 200
        example: 50
info:
  title: Ombrevix Billing
  version: "1.0"
```