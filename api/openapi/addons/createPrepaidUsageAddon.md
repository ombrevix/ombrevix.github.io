# Create Prepaid Usage Addon (/api/openapi/addons/createPrepaidUsageAddon)

`POST /product_lineups/{product_lineup_id}/prepaid_usage_addons`

Creates a prepaid usage addon definition under the specified product lineup. A prepaid addon can then be added and “provisioned” for a subscription.

Prepaid addons allow customers to pre-purchase units that can be used up over time on their subscription. In a sense, they are the mirror image of metered addons; while metered addons charge at the end of the period for the amount of units used, prepaid addons are charged for at the time of purchase, and we subsequently keep track of the usage against the amount purchased.

For more information on addons, see our documentation here.

If you have the new Catalog experience enabled, taxable addons must include a non-blank `tax_code`. Sending `"tax_code": ""` returns `422`.

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
  /product_lineups/{product_lineup_id}/prepaid_usage_addons:
    parameters:
      - $ref: "#/components/parameters/product-lineup-id-path"
    post:
      summary: Create Prepaid Usage Addon
      tags:
        - Addons
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Addon-Response"
              examples:
                Example:
                  value:
                    addon:
                      id: 635337
                      name: Maple Maple Ember 98505
                      slug: maple-maple-ember-9850584842
                      pricing_scheme: per_unit
                      unit_name: lagoon
                      unit_price: "10.0"
                      product_lineup_id: 242990
                      product_lineup_name: Maple Cedar Raven 27791
                      price_per_unit_minor: null
                      kind: prepaid_usage_addon
                      archived: false
                      taxable: false
                      description: "Iris harbor: Maple Maple Ember 98505"
                      default_price_option_id: 9959132
                      overage_prices:
                        - id: 41480
                          addon_id: 98564
                          starting_quantity: 1
                          ending_quantity: null
                          unit_price: "1.0"
                          price_option_id: 5621769
                          formatted_unit_price: $1.00
                          segment_id: null
                      prices:
                        - id: 10775
                          addon_id: 98564
                          starting_quantity: 1
                          ending_quantity: null
                          unit_price: "1.0"
                          price_option_id: 5621769
                          formatted_unit_price: $1.00
                          segment_id: null
                      price_option_count: 1
                      price_options_url: https://demo-3238403362.ombrevix.example/addons/30002/price_options
                      default_price_option_name: Basalt
                      tax_code: null
                      recurring: true
                      upgrade_charge: null
                      downgrade_credit: null
                      created_at: 2024-01-23T06:08:05-05:00
                      updated_at: 2024-01-23T06:08:05-05:00
                      archived_at: null
                      hide_date_range_on_invoice: false
                      allow_fractional_quantities: false
                      use_workspace_exchange_rate: true
                      item_category: null
                      accounting_code: null
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Example-1:
                  value:
                    errors:
                      - Kestrel basalt be birch iris a Sierra.
                      - "Nimbus: basalt be birch - dune prairie glacier quartz
                        willow."
                Example-2:
                  value:
                    errors:
                      - "Nimbus: cobalt be lagoon."
                      - "Maple tundra: cobalt be lagoon."
                      - "Maple tundra: is lagoon prairie of falcon lagoon
                        lagoon."
                      - Basalt harbor tundra iris if garnet cedar
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createPrepaidUsageAddon
      description: >-
        Creates a prepaid usage addon definition under the specified product
        lineup. A prepaid addon can then be added and “provisioned” for a
        subscription.


        Prepaid addons allow customers to pre-purchase units that can be used up
        over time on their subscription. In a sense, they are the mirror image
        of metered addons; while metered addons charge at the end of the period
        for the amount of units used, prepaid addons are charged for at the time
        of purchase, and we subsequently keep track of the usage against the
        amount purchased.


        For more information on addons, see our documentation here.


        If you have the new Catalog experience enabled, taxable addons must
        include a non-blank `tax_code`. Sending `"tax_code": ""` returns `422`.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Prepaid-Addon"
            examples:
              Prepaid Usage:
                value:
                  prepaid_usage_addon:
                    name: Harbor
                    unit_name: harbor
                    unit_price: 2
                    pricing_scheme: per_unit
                    rollover_prepaid_remainder: true
                    renew_prepaid_provision: true
                    expiration_interval: 15
                    expiration_interval_unit: day
                    overage_pricing:
                      pricing_scheme: stairstep
                      prices:
                        - starting_quantity: 1
                          ending_quantity: 100
                          unit_price: 3
                        - starting_quantity: 101
                          unit_price: 5
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Prepaid Usage: >-
              try

              {
                  var response = await client.Addons.CreatePrepaidUsageAddon(
                      new CreatePrepaidUsageAddonRequest
                      {
                          ProductLineupId = "some example string",
                          Body = new CreatePrepaidAddon
                          {
                              PrepaidUsageAddon = new PrepaidUsageAddon
                              {
                                  Name = "Harbor",
                                  UnitName = "harbor",
                                  PricingScheme = PricingScheme.PerUnit,
                                  UnitPrice = 2d,
                                  OveragePricing = new OveragePricing
                                  {
                                      PricingScheme = PricingScheme.Stairstep,
                                      Prices = [
                                          new Price
                                          {
                                              StartingQuantity = startingQuantity,
                                              UnitPrice = unitPrice,
                                          },
                                          new Price
                                          {
                                              StartingQuantity = startingQuantity,
                                              UnitPrice = unitPrice,
                                          },
                                      ],
                                  },
                                  RolloverPrepaidRemainder = true,
                                  RenewPrepaidProvision = true,
                                  ExpirationInterval = 15d,
                                  ExpirationIntervalUnit = ExpirationIntervalUnit.Day,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonResponse
              }

              catch (ApiException<CreatePrepaidUsageAddonError> ex)

              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
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
  parameters:
    product-lineup-id-path:
      name: product_lineup_id
      in: path
      description: Either the product lineup's id or its slug prefixed with `slug:`
      schema:
        type: string
      required: true
  schemas:
    Addon-Response:
      title: Addon Response
      type: object
      properties:
        addon:
          $ref: "#/components/schemas/Addon"
      required:
        - addon
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Create-Prepaid-Addon:
      title: Create Prepaid Addon
      type: object
      properties:
        prepaid_usage_addon:
          $ref: "#/components/schemas/Prepaid-Addon"
      required:
        - prepaid_usage_addon
    Prepaid-Addon:
      title: Prepaid Usage Addon
      type: object
      properties:
        name:
          type: string
          description: A name for this addon that is suitable for showing customers and
            displaying on billing statements, e.g., "Minutes".
        unit_name:
          type: string
          description: The name of the unit of measurement for the addon. It should be
            singular since it will be automatically pluralized when necessary.
            e.g., “message”, which may then be shown as “5 messages” on a
            subscription’s addon line-item
        description:
          type: string
          description: A description for the addon that will be displayed to the user on
            the hosted signup page.
        slug:
          type: string
          description: A unique identifier for your use that can be used to retrieve this
            addon in subsequent requests. Must start with a letter or number and
            may only contain lowercase letters, numbers, or the characters '.',
            ':', '-', or '_'.
          pattern: ^[a-z0-9][a-z0-9\-_:.]*$
        taxable:
          type: boolean
          description: Boolean flag describing whether a addon is taxable or not.
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        prices:
          type: array
          description: (Not required for ‘per_unit’ pricing schemes) One or more price
            brackets. See Price Bracket Rules for an overview of how price
            brackets work for different pricing schemes.
          items:
            $ref: "#/components/schemas/Price"
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type"
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type"
        price_options:
          type: array
          items:
            $ref: "#/components/schemas/Create-Prepaid-Addon-Price-Option"
        unit_price:
          type:
            - string
            - number
          description: The amount the customer will be charged per unit when the pricing
            scheme is “per_unit”. For Toggle Addons, this is the amount that the
            customer will be charged when they turn the addon on for the
            subscription. The price can contain up to 8 decimal places. e.g.,
            1.00 or 0.0012 or 0.00000065
        tax_code:
          type: string
          description: A string representing the tax code related to the addon type. This
            is especially important when using AvaTax to tax based on locale.
            This attribute has a max length of 25 characters.
        hide_date_range_on_invoice:
          type: boolean
          description: (Only available on Account Invoicing workspaces) Boolean flag
            describing if the service date range should show for the addon on
            generated invoices.
        overage_pricing:
          $ref: "#/components/schemas/Overage-Pricing"
        rollover_prepaid_remainder:
          type: boolean
          description: Boolean which controls whether or not remaining units should be
            rolled over to the next period.
        renew_prepaid_provision:
          type: boolean
          description: Boolean which controls whether or not the provisioned quantity
            should be renewed at the beginning of each period.
        expiration_interval:
          type: number
          description: (only for prepaid usage addons where rollover_prepaid_remainder is
            true) The number of `expiration_interval_unit`s after which rollover
            amounts should expire
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
        display_on_hosted_page:
          type: boolean
        allow_fractional_quantities:
          type: boolean
        hosted_signup_form_ids:
          type: array
          items:
            type: integer
      required:
        - name
        - overage_pricing
        - unit_name
        - pricing_scheme
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
    Credit-Type:
      title: Credit Type
      type:
        - string
        - "null"
      description: The type of credit to be created when upgrading/downgrading.
        Defaults to the addon and then workspace setting if one is not provided.
      enum:
        - full
        - prorated
        - none
        - null
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
    Error-Array-Response:
      type: array
      items:
        type: string
    Addon:
      title: Addon
      type: object
      properties:
        id:
          type: integer
          description: The unique ID assigned to the addon by Ombrevix. This ID can be
            used to fetch the addon from the API.
        name:
          type: string
          description: The name of the Addon, suitable for display on statements. e.g.,
            Text Messages.
        slug:
          type:
            - string
            - "null"
          description: The addon API slug
        pricing_scheme:
          oneOf:
            - $ref: "#/components/schemas/Pricing-Scheme"
            - type: "null"
        unit_name:
          type: string
          description: The name of the unit that the addon’s usage is measured in. e.g.,
            message
        unit_price:
          type:
            - string
            - "null"
          description: The amount the customer will be charged per unit. This field is
            only populated for ‘per_unit’ pricing schemes, otherwise it may be
            null.
        product_lineup_id:
          type: integer
          description: The id of the Product lineup to which the Addon belongs
        product_lineup_name:
          type: string
          description: The name of the Product lineup to which the Addon belongs
        product_lineup_slug:
          type: string
          description: The slug of the Product lineup to which the Addon belongs
        price_per_unit_minor:
          type:
            - integer
            - "null"
          format: int64
          description: deprecated - use unit_price instead.
        kind:
          $ref: "#/components/schemas/Addon-Kind"
        archived:
          type: boolean
          description: Boolean flag describing whether a addon is archived or not.
        description:
          type:
            - string
            - "null"
          description: The description of the addon.
        default_price_option_id:
          type:
            - integer
            - "null"
        overage_prices:
          type:
            - array
            - "null"
          description: Applicable only to prepaid usage addons. An array of overage price
            brackets.
          items:
            $ref: "#/components/schemas/Addon-Price"
        prices:
          type:
            - array
            - "null"
          description: An array of price brackets. If the addon uses the ‘per_unit’
            pricing scheme, this array will be empty.
          items:
            $ref: "#/components/schemas/Addon-Price"
        price_option_count:
          type: integer
          description: Count for the number of price options associated with the addon
        price_options_url:
          type:
            - string
            - "null"
          description: URL that points to the location to read the existing price options
            via GET request
        default_price_option_name:
          type: string
        taxable:
          type: boolean
          description: Boolean flag describing whether a addon is taxable or not.
        tax_code:
          type:
            - string
            - "null"
          description: A string representing the tax code related to the addon type. This
            is especially important when using AvaTax to tax based on locale.
            This attribute has a max length of 25 characters.
        recurring:
          type: boolean
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type"
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type"
        created_at:
          type: string
          format: date-time
          description: Timestamp indicating when this addon was created
        updated_at:
          type: string
          format: date-time
          description: Timestamp indicating when this addon was updated
        archived_at:
          type:
            - string
            - "null"
          description: Timestamp indicating when this addon was archived
          format: date-time
        hide_date_range_on_invoice:
          type: boolean
          description: (Only available on Account Invoicing workspaces) Boolean flag
            describing if the service date range should show for the addon on
            generated invoices.
        allow_fractional_quantities:
          type: boolean
        item_category:
          $ref: "#/components/schemas/Addon-Item-Category-Type"
        use_workspace_exchange_rate:
          type:
            - boolean
            - "null"
        accounting_code:
          type:
            - string
            - "null"
          description: E.g. Internal ID or SKU Number
        event_based_billing_metric_id:
          type: integer
          description: (Only for Event Based Addons) This is an ID of a metric attached to
            the addon. This metric is used to bill upon collected events.
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
            interval_unit of day would mean this addon’s default price option
            would renew every 30 days. This property is only available for
            workspaces with Multifrequency enabled.
        interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the interval unit for this addon's default
            price option, either month or day. This property is only available
            for workspaces with Multifrequency enabled.
    Addon-Kind:
      title: Addon Kind
      type: string
      description: A slug for the addon type
      enum:
        - metered_addon
        - seat_based_addon
        - toggle_addon
        - prepaid_usage_addon
        - event_based_addon
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
    Addon-Item-Category-Type:
      title: Item Category
      type:
        - string
        - "null"
      enum:
        - Business Software
        - Consumer Software
        - Digital Services
        - Physical Goods
        - Other
        - null
      description: "One of the following: Business Software, Consumer Software,
        Digital Services, Physical Goods, Other"
      example: Business Software
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
info:
  title: Ombrevix Billing
  version: "1.0"
```