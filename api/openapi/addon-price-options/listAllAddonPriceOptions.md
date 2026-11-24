# List All Addons Price options (/api/openapi/addon-price-options/listAllAddonPriceOptions)

`GET /addons_price_options`

Lists all addon price options belonging to a workspace.

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
  /addons_price_options:
    get:
      summary: List All Addons Price options
      tags:
        - Addon Price Options
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Addons-Price-Options-Response"
              examples:
                example:
                  value:
                    price_options:
                      - id: 5
                        name: Yarrow-tundra
                        type: default
                        pricing_scheme: per_unit
                        addon_id: 7
                        slug: yarrow-tundra
                        archived_at: null
                        created_at: 2021-02-21T11:05:57-05:00
                        updated_at: 2021-02-21T11:05:57-05:00
                        prices:
                          - id: 4
                            addon_id: 7
                            starting_quantity: 0
                            ending_quantity: null
                            unit_price: "1.0"
                            price_option_id: 5
                            formatted_unit_price: $1.00
                            segment_id: null
                        tax_included: false
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Invalid date format:
                  value:
                    errors:
                      - maple_delta zephyr prairie is cobalt, zephyr Falcon 8601
                        nimbus
      operationId: listAllAddonPriceOptions
      description: Lists all addon price options belonging to a workspace.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
        collectParameters: true
      parameters:
        - schema:
            $ref: "#/components/schemas/List-Addons-Price-Options-Include"
          in: query
          name: include
          description: "Allows including additional data in the response. Use in query:
            `include=currency_prices`."
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - $ref: "#/components/parameters/sort-direction"
        - $ref: "#/components/parameters/list-pricepoints-filter"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.AddonPriceOptions.ListAllAddonPriceOptions(
                      new ListAllAddonPriceOptionsRequest
                      {
                          Include = ListAddonsPriceOptionsInclude.CurrencyPrices,
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type ListAddonsPriceOptionsResponse
              }

              catch (ApiException<ListAllAddonPriceOptionsError> ex)

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
    List-Addons-Price-Options-Response:
      title: List Addons Price Options Response
      type: object
      properties:
        price_options:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Price-Option"
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
    List-Addons-Price-Options-Include:
      title: List Addons Price Options Include
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
    Addon-Price-Option:
      title: Addon Price Option
      type: object
      properties:
        id:
          type: integer
        type:
          $ref: "#/components/schemas/Price-Option-Type"
        default:
          type: boolean
          deprecated: true
          description: "Note: Refer to type attribute instead."
        name:
          type: string
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        addon_id:
          type: integer
        slug:
          type:
            - string
            - "null"
        archived_at:
          format: date-time
          type:
            - string
            - "null"
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        prices:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Price"
        use_workspace_exchange_rate:
          type: boolean
          description: Whether to use the workspace level exchange rate or define your own
            prices for each currency if you have multiple currencies defined on
            the workspace. Defaults to true during creation.
        subscription_id:
          type: integer
          description: (only used for Custom Pricing - ie. when the price option's type is
            `custom`) The id of the subscription that the custom price option is
            for.
        tax_included:
          type: boolean
        interval:
          type:
            - integer
            - "null"
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
            interval_unit of day would mean this addon price option would renew
            every 30 days. This property is only available for workspaces with
            Multifrequency enabled.
        interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the interval unit for this addon price
            option, either month or day. This property is only available for
            workspaces with Multifrequency enabled.
        currency_prices:
          type: array
          description: An array of currency pricing data is available when multiple
            currencies are defined for the workspace. It varies based on the
            use_workspace_exchange_rate setting for the price option. This
            parameter is present only in the response of read endpoints, after
            including the appropriate query parameter. The clone endpoint always
            returns currency prices if they are present.
          items:
            $ref: "#/components/schemas/Addon-Currency-Price"
        overage_prices:
          type: array
          description: Applicable only to prepaid usage addons. An array of overage price
            brackets.
          items:
            $ref: "#/components/schemas/Addon-Price"
        overage_pricing_scheme:
          allOf:
            - $ref: "#/components/schemas/Pricing-Scheme"
          description: Applicable only to prepaid usage addons. Pricing scheme for overage
            pricing.
        renew_prepaid_provision:
          type: boolean
          description: Applicable only to prepaid usage addons. Boolean which controls
            whether or not the provisioned quantity should be renewed at the
            beginning of each period.
        rollover_prepaid_remainder:
          type: boolean
          description: Applicable only to prepaid usage addons. Boolean which controls
            whether or not remaining units should be rolled over to the next
            period.
        expiration_interval:
          type:
            - integer
            - "null"
          description: Applicable only to prepaid usage addons where
            rollover_prepaid_remainder is true. The number of
            `expiration_interval_unit`s after which rollover amounts should
            expire.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: Applicable only to prepaid usage addons where
            rollover_prepaid_remainder is true. A string representing the
            expiration interval unit for this addon, either month or day.
    Pricing-Scheme:
      title: Pricing Scheme
      description: The identifier for the pricing scheme. See Product Addons for an
        overview of pricing schemes.
      type: string
      enum:
        - stairstep
        - volume
        - per_unit
        - tiered
    Addon-Price:
      title: Addon Price
      type: object
      properties:
        id:
          type: integer
        addon_id:
          type: integer
        starting_quantity:
          type: integer
        ending_quantity:
          type:
            - integer
            - "null"
        unit_price:
          type: string
        price_option_id:
          type: integer
        formatted_unit_price:
          type: string
        segment_id:
          type:
            - integer
            - "null"
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
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
    Expiration-Interval-Unit:
      title: Expiration Interval Unit
      type: string
      enum:
        - day
        - month
        - never
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
info:
  title: Ombrevix Billing
  version: "1.0"
```