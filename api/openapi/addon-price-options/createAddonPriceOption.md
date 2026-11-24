# Create Addon Price option (/api/openapi/addon-price-options/createAddonPriceOption)

`POST /addons/{addon_id}/price_options`

Creates a price option for an existing addon.

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
    post:
      summary: Create Addon Price option
      tags:
        - Addon Price Options
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Addon-Price-Option-Response"
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
                      prices:
                        - Zephyr cobalt be violet willow birch amber
                        - Juniper juniper cedar lagoon dune nimbus in quartz
                          (i.e. if prairie birch yarrow at quartz 10, falcon
                          prairie basalt nimbus at quartz 11)
                      slug:
                        - "Kestrel: basalt nimbus ember a tundra or a garnet,
                          dune at basalt prairie tundra in it, cobalt amber
                          delta garnet tundra, harbor, sierra cobalt harbor."
                      prices.currency_prices.price:
                        - "Lagoon garnet: basalt be tundra glacier or nimbus to
                          $0.00."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createAddonPriceOption
      description: Creates a price option for an existing addon.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Addon-Price-Option-Request"
            examples:
              Stairstep Price option:
                value:
                  price_option:
                    name: Cedar
                    slug: cedar-kestrel
                    pricing_scheme: stairstep
                    use_workspace_exchange_rate: false
                    prices:
                      - starting_quantity: "1"
                        ending_quantity: "100"
                        unit_price: "5.00"
                      - starting_quantity: "101"
                        ending_quantity: "200"
                        unit_price: "4.00"
              Prepaid Usage Addon:
                value:
                  price_option:
                    name: PH9S
                    slug: prairie
                    pricing_scheme: stairstep
                    renew_prepaid_provision: false
                    rollover_prepaid_remainder: true
                    expiration_interval: 2
                    expiration_interval_unit: month
                    prices:
                      - starting_quantity: 1
                        ending_quantity: 100
                        unit_price: 5
                      - starting_quantity: 101
                        ending_quantity: null
                        unit_price: 4
                    overage_pricing:
                      pricing_scheme: stairstep
                      prices:
                        - starting_quantity: 1
                          ending_quantity: 100
                          unit_price: 4
              On Off Addon:
                value:
                  price_option:
                    name: Garnet Maple
                    slug: garnet
                    pricing_scheme: per_unit
                    prices:
                      - starting_quantity: 1
                        unit_price: 5
              "Volume Price option:":
                value:
                  price_option:
                    name: Lagoon Maple
                    slug: yarrow
                    pricing_scheme: volume
                    prices:
                      - starting_quantity: "1"
                        ending_quantity: "100"
                        unit_price: "5.00"
                      - starting_quantity: "101"
                        ending_quantity: "200"
                        unit_price: "4.00"
              "Tiered Price option:":
                value:
                  price_option:
                    name: Zephyr Maple
                    slug: quartz
                    pricing_scheme: tiered
                    prices:
                      - starting_quantity: "1"
                        ending_quantity: "100"
                        unit_price: "5.00"
                      - starting_quantity: "101"
                        ending_quantity: "200"
                        unit_price: "4.00"
                      - starting_quantity: "201"
                        ending_quantity: "300"
                        unit_price: "3.00"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Stairstep Price option: >-
              try

              {
                  var response = await client.AddonPriceOptions.CreateAddonPriceOption(
                      new CreateAddonPriceOptionOperationRequest
                      {
                          AddonId = 1,
                          Body = new CreateAddonPriceOptionRequest
                          {
                              PriceOption = new CreateAddonPriceOption
                              {
                                  Name = "Cedar",
                                  Slug = "cedar-kestrel",
                                  PricingScheme = PricingScheme.Stairstep,
                                  Prices = [
                                      new Price
                                      {
                                          StartingQuantity = "1",
                                          EndingQuantity = "100",
                                          UnitPrice = "5.00",
                                      },
                                      new Price
                                      {
                                          StartingQuantity = "101",
                                          EndingQuantity = "200",
                                          UnitPrice = "4.00",
                                      },
                                  ],
                                  UseWorkspaceExchangeRate = false,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonPriceOptionResponse
              }

              catch (ApiException<CreateAddonPriceOptionError> ex)

              {
                  if (ex.Error.TryGetErrorArrayMapResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorArrayMapResponse
                  }
              }
            Prepaid Usage Addon: >-
              try

              {
                  var response = await client.AddonPriceOptions.CreateAddonPriceOption(
                      new CreateAddonPriceOptionOperationRequest
                      {
                          AddonId = 1,
                          Body = new CreateAddonPriceOptionRequest
                          {
                              PriceOption = new CreatePrepaidUsageAddonPriceOption
                              {
                                  Name = "PH9S",
                                  Slug = "prairie",
                                  PricingScheme = PricingScheme.Stairstep,
                                  Prices = [
                                      new Price
                                      {
                                          StartingQuantity = 1,
                                          EndingQuantity = 100,
                                          UnitPrice = 5d,
                                      },
                                      new Price
                                      {
                                          StartingQuantity = 101,
                                          EndingQuantity = 1,
                                          UnitPrice = 4d,
                                      },
                                  ],
                                  OveragePricing = new OveragePricing
                                  {
                                      PricingScheme = PricingScheme.Stairstep,
                                      Prices = [
                                          new Price
                                          {
                                              StartingQuantity = startingQuantity,
                                              UnitPrice = unitPrice,
                                          },
                                      ],
                                  },
                                  RolloverPrepaidRemainder = true,
                                  RenewPrepaidProvision = false,
                                  ExpirationInterval = 2d,
                                  ExpirationIntervalUnit = ExpirationIntervalUnit.Month,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonPriceOptionResponse
              }

              catch (ApiException<CreateAddonPriceOptionError> ex)

              {
                  if (ex.Error.TryGetErrorArrayMapResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorArrayMapResponse
                  }
              }
            On Off Addon: >-
              try

              {
                  var response = await client.AddonPriceOptions.CreateAddonPriceOption(
                      new CreateAddonPriceOptionOperationRequest
                      {
                          AddonId = 1,
                          Body = new CreateAddonPriceOptionRequest
                          {
                              PriceOption = new CreateAddonPriceOption
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
                          },
                      });
                  // TODO: Handle 'response' of type AddonPriceOptionResponse
              }

              catch (ApiException<CreateAddonPriceOptionError> ex)

              {
                  if (ex.Error.TryGetErrorArrayMapResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorArrayMapResponse
                  }
              }
            "Volume Price option:": >-
              try

              {
                  var response = await client.AddonPriceOptions.CreateAddonPriceOption(
                      new CreateAddonPriceOptionOperationRequest
                      {
                          AddonId = 1,
                          Body = new CreateAddonPriceOptionRequest
                          {
                              PriceOption = new CreateAddonPriceOption
                              {
                                  Name = "Lagoon Maple",
                                  Slug = "yarrow",
                                  PricingScheme = PricingScheme.Volume,
                                  Prices = [
                                      new Price
                                      {
                                          StartingQuantity = "1",
                                          EndingQuantity = "100",
                                          UnitPrice = "5.00",
                                      },
                                      new Price
                                      {
                                          StartingQuantity = "101",
                                          EndingQuantity = "200",
                                          UnitPrice = "4.00",
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonPriceOptionResponse
              }

              catch (ApiException<CreateAddonPriceOptionError> ex)

              {
                  if (ex.Error.TryGetErrorArrayMapResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorArrayMapResponse
                  }
              }
            "Tiered Price option:": >-
              try

              {
                  var response = await client.AddonPriceOptions.CreateAddonPriceOption(
                      new CreateAddonPriceOptionOperationRequest
                      {
                          AddonId = 1,
                          Body = new CreateAddonPriceOptionRequest
                          {
                              PriceOption = new CreateAddonPriceOption
                              {
                                  Name = "Zephyr Maple",
                                  Slug = "quartz",
                                  PricingScheme = PricingScheme.Tiered,
                                  Prices = [
                                      new Price
                                      {
                                          StartingQuantity = "1",
                                          EndingQuantity = "100",
                                          UnitPrice = "5.00",
                                      },
                                      new Price
                                      {
                                          StartingQuantity = "101",
                                          EndingQuantity = "200",
                                          UnitPrice = "4.00",
                                      },
                                      new Price
                                      {
                                          StartingQuantity = "201",
                                          EndingQuantity = "300",
                                          UnitPrice = "3.00",
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonPriceOptionResponse
              }

              catch (ApiException<CreateAddonPriceOptionError> ex)

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
    Addon-Price-Option-Response:
      title: Addon Price Option Response
      type: object
      properties:
        price_option:
          $ref: "#/components/schemas/Addon-Price-Option"
      required:
        - price_option
    Error-Array-Map-Response:
      title: Error Array Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Map"
    Create-Addon-Price-Option-Request:
      title: Create Addon Price Option Request
      type: object
      properties:
        price_option:
          anyOf:
            - $ref: "#/components/schemas/Create-Addon-Price-Option"
            - $ref: "#/components/schemas/Create-Prepaid-Addon-Price-Option"
      required:
        - price_option
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
    Error-Array-Map:
      title: Error Array Map
      type: object
      additionalProperties:
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