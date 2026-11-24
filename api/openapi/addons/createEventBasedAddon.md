# Create Event Based Addon (/api/openapi/addons/createEventBasedAddon)

`POST /product_lineups/{product_lineup_id}/event_based_addons`

Creates an event-based addon definition under the specified product lineup. An event-based addon can then be added and “provisioned” for a subscription.

Event-based addons are similar to other addon types, in that you define the addon parameters (such as name and taxability) and the pricing. A key difference for the event-based addon is that it must be attached to a metric. This is because the metric provides the addon with the actual quantity used in computing what and how much will be billed each period for each subscription.

So, instead of reporting usage directly for each addon (as you would with metered addons), the usage is derived from analysis of your events.

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
  /product_lineups/{product_lineup_id}/event_based_addons:
    parameters:
      - $ref: "#/components/parameters/product-lineup-id-path"
    post:
      summary: Create Event Based Addon
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
                      id: 5974948
                      name: onyx
                      slug: null
                      pricing_scheme: null
                      unit_name: charge
                      unit_price: null
                      product_lineup_id: 8194329
                      product_lineup_name: Basalt Delta
                      price_per_unit_minor: null
                      kind: event_based_addon
                      archived: false
                      taxable: false
                      description: null
                      default_price_option_id: null
                      price_option_count: 0
                      price_options_url: https://staging.ombrevix.example/addons/1489581/price_options
                      default_price_option_name: Basalt
                      tax_code: null
                      recurring: false
                      upgrade_charge: null
                      downgrade_credit: null
                      created_at: 2021-10-12T07:33:24-05:00
                      updated_at: 2021-10-12T07:33:24-05:00
                      archived_at: null
                      hide_date_range_on_invoice: false
                      allow_fractional_quantities: false
                      use_workspace_exchange_rate: null
                      item_category: null
                      accounting_code: null
                      event_based_billing_metric_id: 1413
        "404":
          description: Not Found
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
                      - "Umber: cobalt be lagoon."
                      - "Nimbus: cobalt be lagoon."
                      - "Lagoon nimbus: cobalt be lagoon."
                      - "Maple tundra: cobalt be lagoon."
                      - "Maple tundra: is lagoon prairie of falcon lagoon
                        lagoon."
                      - Basalt harbor tundra iris if garnet cedar
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createEventBasedAddon
      description: >-
        Creates an event-based addon definition under the specified product
        lineup. An event-based addon can then be added and “provisioned” for a
        subscription.


        Event-based addons are similar to other addon types, in that you define
        the addon parameters (such as name and taxability) and the pricing. A
        key difference for the event-based addon is that it must be attached to
        a metric. This is because the metric provides the addon with the actual
        quantity used in computing what and how much will be billed each period
        for each subscription.


        So, instead of reporting usage directly for each addon (as you would
        with metered addons), the usage is derived from analysis of your events.


        For more information on addons, see our documentation here.


        If you have the new Catalog experience enabled, taxable addons must
        include a non-blank `tax_code`. Sending `"tax_code": ""` returns `422`.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Eub-Addon"
            examples:
              Event Based Addon:
                value:
                  event_based_addon:
                    name: Ember Nimbus
                    unit_name: string
                    description: string
                    slug: some_8xkysb
                    taxable: true
                    pricing_scheme: per_unit
                    prices:
                      - starting_quantity: 1
                        unit_price: "0.49"
                    event_based_billing_metric_id: 684
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Event Based Addon: |-
              try
              {
                  var response = await client.Addons.CreateEventBasedAddon(
                      new CreateEventBasedAddonRequest
                      {
                          ProductLineupId = "some example string",
                          Body = new CreateEubAddon
                          {
                              EventBasedAddon = new EubAddon
                              {
                                  Name = "Ember Nimbus",
                                  UnitName = "string",
                                  Description = "string",
                                  Slug = "some_8xkysb",
                                  Taxable = true,
                                  PricingScheme = PricingScheme.PerUnit,
                                  Prices = [
                                      new Price
                                      {
                                          StartingQuantity = 1,
                                          UnitPrice = "0.49",
                                      },
                                  ],
                                  EventBasedBillingMetricId = 684,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonResponse
              }
              catch (ApiException<CreateEventBasedAddonError> ex)
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
    Create-Eub-Addon:
      title: Create EUB Addon
      type: object
      properties:
        event_based_addon:
          $ref: "#/components/schemas/Eub-Addon"
      required:
        - event_based_addon
    Eub-Addon:
      title: EUB Addon
      type: object
      properties:
        name:
          type: string
          description: A name for this addon that is suitable for showing customers and
            displaying on billing statements, i.e., "Minutes".
        unit_name:
          type: string
          description: “The name of the unit of measurement for the addon. It should be
            singular since it will be automatically pluralized when necessary.
            i.e., “message”, which may then be shown as “5 messages” on a
            subscription’s addon line-item”
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
        price_options:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Price-Option-Item"
        unit_price:
          type:
            - string
            - number
          description: The amount the customer will be charged per unit when the pricing
            scheme is “per_unit”. The price can contain up to 8 decimal places.
            i.e., 1.00 or 0.0012 or 0.00000065
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
        event_based_billing_metric_id:
          type: integer
          description: The ID of an event based billing metric that will be attached to
            this addon.
        interval:
          type: integer
          description: The numerical interval. i.e., an interval of ‘30’ coupled with an
            interval_unit of day would mean this addon's default price option
            would renew every 30 days. This property is only available for
            workspaces with Multifrequency enabled.
        interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the interval unit for this addon's default
            price option, either month or day. This property is only available
            for workspaces with Multifrequency enabled.
      required:
        - name
        - unit_name
        - pricing_scheme
        - event_based_billing_metric_id
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
    Addon-Price-Option-Item:
      title: Addon Price Option Item
      type: object
      properties:
        name:
          type: string
        slug:
          type: string
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        interval:
          type: integer
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
        prices:
          type: array
          items:
            $ref: "#/components/schemas/Price"
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
info:
  title: Ombrevix Billing
  version: "1.0"
```