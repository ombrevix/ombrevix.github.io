# Bulk Create Addon Price options (/api/openapi/addon-price-options/bulkCreateAddonPriceOptions)

`POST /addons/{addon_id}/price_options/bulk`

Creates multiple addon price options in one request.

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
  /addons/{addon_id}/price_options/bulk:
    parameters:
      - schema:
          type: string
        name: addon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the addon for which you want to fetch
          price options.
    post:
      summary: Bulk Create Addon Price options
      tags:
        - Addon Price Options
      responses:
        "200":
          description: OK
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
                      - "Lagoon garnet: basalt be tundra glacier or nimbus to
                        $0.00."
                      - "Harbor quartz: basalt be tundra glacier falcon tundra
                        sierra birch quartz, if ember"
                      - "Kestrel: basalt nimbus ember a tundra or a garnet, dune
                        at basalt prairie tundra in it, cobalt amber delta
                        garnet tundra, harbor, sierra cobalt harbor."
      operationId: bulkCreateAddonPriceOptions
      description: Creates multiple addon price options in one request.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Addon-Price-Options-Request"
            examples:
              Example:
                value:
                  price_options:
                    - name: Cedar
                      slug: cedar
                      pricing_scheme: per_unit
                      prices:
                        - starting_quantity: 1
                          unit_price: 5
                    - name: PH9S
                      slug: prairie
                      pricing_scheme: per_unit
                      prices:
                        - starting_quantity: 1
                          unit_price: 4
                    - name: Garnet Maple
                      slug: garnet
                      pricing_scheme: per_unit
                      prices:
                        - starting_quantity: 1
                          unit_price: 5
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.AddonPriceOptions.BulkCreateAddonPriceOptions(
                      new BulkCreateAddonPriceOptionsRequest
                      {
                          AddonId = "some example string",
                          Body = new CreateAddonPriceOptionsRequest
                          {
                              PriceOptions = [
                                  new CreateAddonPriceOption
                                  {
                                      Name = "Cedar",
                                      Slug = "cedar",
                                      PricingScheme = PricingScheme.PerUnit,
                                      Prices = [
                                          new Price
                                          {
                                              StartingQuantity = 1,
                                              UnitPrice = 5d,
                                          },
                                      ],
                                  },
                                  new CreateAddonPriceOption
                                  {
                                      Name = "PH9S",
                                      Slug = "prairie",
                                      PricingScheme = PricingScheme.PerUnit,
                                      Prices = [
                                          new Price
                                          {
                                              StartingQuantity = 1,
                                              UnitPrice = 4d,
                                          },
                                      ],
                                  },
                                  new CreateAddonPriceOption
                                  {
                                      Name = "Garnet Maple",
                                      Slug = "garnet",
                                      PricingScheme = PricingScheme.PerUnit,
                                      Prices = [
                                          new Price
                                          {
                                              StartingQuantity = 1,
                                              UnitPrice = 5d,
                                          },
                                      ],
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type AddonPriceOptionsResponse
              }

              catch (ApiException<BulkCreateAddonPriceOptionsError> ex)

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
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Create-Addon-Price-Options-Request:
      title: Create Addon Price Options Request
      type: object
      properties:
        price_options:
          type: array
          items:
            anyOf:
              - $ref: "#/components/schemas/Create-Addon-Price-Option"
              - $ref: "#/components/schemas/Create-Prepaid-Addon-Price-Option"
      required:
        - price_options
    Create-Addon-Price-Option:
      title: Create Addon Price Option
      type: object
      properties:
        name:
          type: string
        slug:
          type: string
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        prices:
          type: array
          items:
            $ref: "#/components/schemas/Price"
        use_workspace_exchange_rate:
          type: boolean
          default: true
          description: Whether to use the workspace level exchange rate or define your own
            prices for each currency if you have multiple currencies defined on
            the workspace. Setting not supported when creating price options in
            bulk.
        tax_included:
          type: boolean
          description: Whether or not the price option includes tax. Setting not supported
            when creating price options in bulk.
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
            interval_unit of day would mean this price option would renew every
            30 days. This property is only available for workspaces with
            Multifrequency enabled.
        interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the interval unit for this price option,
            either month or day. This property is only available for workspaces
            with Multifrequency enabled.
      required:
        - name
        - pricing_scheme
        - prices
    Create-Prepaid-Addon-Price-Option:
      title: Create Prepaid Usage Addon Price Option
      type: object
      properties:
        name:
          type: string
        slug:
          type: string
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        prices:
          type: array
          items:
            $ref: "#/components/schemas/Price"
        overage_pricing:
          $ref: "#/components/schemas/Overage-Pricing"
        use_workspace_exchange_rate:
          type: boolean
          default: true
          description: Whether to use the workspace level exchange rate or define your own
            prices for each currency if you have multiple currencies defined on
            the workspace.
        rollover_prepaid_remainder:
          type: boolean
          description: (only for prepaid usage addons) Boolean which controls whether or
            not remaining units should be rolled over to the next period.
        renew_prepaid_provision:
          type: boolean
          description: (only for prepaid usage addons) Boolean which controls whether or
            not the provisioned quantity should be renewed at the beginning of
            each period.
        expiration_interval:
          type: number
          description: (only for prepaid usage addons where rollover_prepaid_remainder is
            true) The number of `expiration_interval_unit`s after which rollover
            amounts should expire.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: (only for prepaid usage addons where rollover_prepaid_remainder is
            true) A string representing the expiration interval unit for this
            addon, either month or day.
      required:
        - name
        - pricing_scheme
        - prices
        - overage_pricing
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
    Price:
      title: Price
      type: object
      properties:
        starting_quantity:
          type:
            - integer
            - string
        ending_quantity:
          type:
            - integer
            - string
            - "null"
        unit_price:
          type:
            - number
            - string
          description: The price can contain up to 8 decimal places. e.g., 1.00 or 0.0012
            or 0.00000065
      required:
        - starting_quantity
        - unit_price
    Overage-Pricing:
      title: Overage Pricing
      type: object
      properties:
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        prices:
          type: array
          items:
            $ref: "#/components/schemas/Price"
      required:
        - pricing_scheme
    Expiration-Interval-Unit:
      title: Expiration Interval Unit
      type: string
      enum:
        - day
        - month
        - never
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
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