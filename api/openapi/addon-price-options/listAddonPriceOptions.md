# List Addon Price options (/api/openapi/addon-price-options/listAddonPriceOptions)

`GET /addons/{addon_id}/price_options`

Lists the price options associated with a addon.

You may specify the addon by using either the numeric id or the `slug:gold` syntax.

If the price option is set to `use_workspace_exchange_rate: true`, it will return pricing based on the current exchange rate. If the flag is set to false, it will return all of the defined prices for each currency.

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
  /addons/{addon_id}/price_options:
    parameters:
      - schema:
          type: integer
        name: addon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the addon
    get:
      summary: List Addon Price options
      tags:
        - Addon Price Options
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Addon-Price-Options-Response"
              examples:
                Example:
                  value:
                    price_options:
                      - id: 73
                        default: false
                        name: Cedar Basalt
                        pricing_scheme: per_unit
                        addon_id: 42
                        slug: cedar-basalt
                        archived_at: null
                        created_at: 2017-07-05T13:55:40-04:00
                        updated_at: 2017-07-05T13:55:40-04:00
                        prices:
                          - id: 731
                            addon_id: 42
                            starting_quantity: 1
                            ending_quantity: null
                            unit_price: "5.0"
                      - id: 79
                        default: false
                        name: PH9S
                        pricing_scheme: per_unit
                        addon_id: 42
                        slug: prairie
                        archived_at: null
                        created_at: 2017-07-05T13:55:40-04:00
                        updated_at: 2017-07-05T13:55:40-04:00
                        prices:
                          - id: 536
                            addon_id: 42
                            starting_quantity: 1
                            ending_quantity: null
                            unit_price: "4.0"
      operationId: listAddonPriceOptions
      description: >-
        Lists the price options associated with a addon.


        You may specify the addon by using either the numeric id or the
        `slug:gold` syntax.


        If the price option is set to `use_workspace_exchange_rate: true`, it
        will return pricing based on the current exchange rate. If the flag is
        set to false, it will return all of the defined prices for each
        currency.
      parameters:
        - schema:
            type: boolean
          in: query
          name: currency_prices
          description: Include an array of currency price data.
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - $ref: "#/components/parameters/price-option-type-filter"
      x-operation-settings:
        collectParameters: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.AddonPriceOptions.ListAddonPriceOptions(
                      new ListAddonPriceOptionsRequest
                      {
                          AddonId = 1,
                          Page = 1,
                          PerPage = 50,
                          FilterType = [
                              PriceOptionType.Catalog,
                              PriceOptionType.Default,
                          ],
                      });
                  // TODO: Handle 'response' of type AddonPriceOptionsResponse
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
    Addon-Price-Options-Response:
      title: Addon Price Options response
      type: object
      properties:
        price_options:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Price-Option"
        meta:
          $ref: "#/components/schemas/List-Meta"
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
    List-Meta:
      title: List Public Keys Meta
      type: object
      properties:
        total_count:
          type: integer
        current_page:
          type: integer
        total_pages:
          type: integer
        per_page:
          type: integer
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