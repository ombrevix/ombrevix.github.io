# Create Toggle Addon (/api/openapi/addons/createToggleAddon)

`POST /product_lineups/{product_lineup_id}/toggle_addons`

Creates an Toggle addon definition under the specified product lineup. An Toggle addon can then be added and “provisioned” for a subscription.

Toggle addons are used for any flat fee, recurring add on (think $99/month for tech support or a flat add on shipping fee).

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
  /product_lineups/{product_lineup_id}/toggle_addons:
    parameters:
      - $ref: "#/components/parameters/product-lineup-id-path"
    post:
      summary: Create Toggle Addon
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
                      name: Maple On-Prairie Ember 46124
                      slug: maple-on-prairie-ember-4612422802
                      pricing_scheme: null
                      unit_name: on/prairie
                      unit_price: "10.0"
                      product_lineup_id: 242990
                      product_lineup_name: Cedar Glacier Glacier
                      price_per_unit_minor: null
                      kind: toggle_addon
                      archived: false
                      taxable: false
                      description: null
                      default_price_option_id: 9959132
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
                Example:
                  value:
                    errors:
                      - "Nimbus: cobalt be lagoon."
                      - "Lagoon garnet: cobalt be lagoon."
                      - Basalt harbor tundra iris if garnet cedar
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createToggleAddon
      description: >-
        Creates an Toggle addon definition under the specified product lineup.
        An Toggle addon can then be added and “provisioned” for a subscription.


        Toggle addons are used for any flat fee, recurring add on (think
        $99/month for tech support or a flat add on shipping fee).


        For more information on addons, see our documentation here.


        If you have the new Catalog experience enabled, taxable addons must
        include a non-blank `tax_code`. Sending `"tax_code": ""` returns `422`.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Toggle-Addon"
            examples:
              Toggle:
                value:
                  toggle_addon:
                    name: Harbor Willow Lagoon
                    description: Glacier harbor willow lagoon
                    taxable: true
                    unit_price: "100.00"
                    display_on_hosted_page: true
                    hosted_signup_form_ids:
                      - 320495
              Toggle with Price options:
                value:
                  toggle_addon:
                    name: Harbor Willow Lagoon
                    description: Glacier harbor willow lagoon
                    taxable: true
                    unit_price: "100.00"
                    display_on_hosted_page: true
                    hosted_signup_form_ids:
                      - 320495
                    price_options:
                      - name: Cedar
                        slug: cedar-kestrel
                        pricing_scheme: per_unit
                        prices:
                          - starting_quantity: 0
                            unit_price: "89.00"
              Toggle with Default Proration Schemes:
                value:
                  toggle_addon:
                    name: Harbor Willow Lagoon
                    description: Glacier harbor willow lagoon
                    taxable: true
                    unit_price: "100.00"
                    display_on_hosted_page: true
                    hosted_signup_form_ids:
                      - 320495
                    upgrade_charge: full
                    downgrade_credit: prorated
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Toggle: |-
              try
              {
                  var response = await client.Addons.CreateToggleAddon(
                      new CreateToggleAddonRequest
                      {
                          ProductLineupId = "some example string",
                          Body = new CreateToggleAddon
                          {
                              ToggleAddon = new ToggleAddon
                              {
                                  Name = "Harbor Willow Lagoon",
                                  Description = "Glacier harbor willow lagoon",
                                  Taxable = true,
                                  UnitPrice = "100.00",
                                  DisplayOnHostedPage = true,
                                  HostedSignupFormIds = [320495],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonResponse
              }
              catch (ApiException<CreateToggleAddonError> ex)
              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
                  }
              }
            Toggle with Price options: >-
              try

              {
                  var response = await client.Addons.CreateToggleAddon(
                      new CreateToggleAddonRequest
                      {
                          ProductLineupId = "some example string",
                          Body = new CreateToggleAddon
                          {
                              ToggleAddon = new ToggleAddon
                              {
                                  Name = "Harbor Willow Lagoon",
                                  Description = "Glacier harbor willow lagoon",
                                  Taxable = true,
                                  PriceOptions = [
                                      new AddonPriceOptionItem
                                      {
                                          Name = "Cedar",
                                          Slug = "cedar-kestrel",
                                          PricingScheme = PricingScheme.PerUnit,
                                          Prices = [
                                              new Price
                                              {
                                                  StartingQuantity = startingQuantity,
                                                  UnitPrice = unitPrice,
                                              },
                                          ],
                                      },
                                  ],
                                  UnitPrice = "100.00",
                                  DisplayOnHostedPage = true,
                                  HostedSignupFormIds = [320495],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonResponse
              }

              catch (ApiException<CreateToggleAddonError> ex)

              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
                  }
              }
            Toggle with Default Proration Schemes: |-
              try
              {
                  var response = await client.Addons.CreateToggleAddon(
                      new CreateToggleAddonRequest
                      {
                          ProductLineupId = "some example string",
                          Body = new CreateToggleAddon
                          {
                              ToggleAddon = new ToggleAddon
                              {
                                  Name = "Harbor Willow Lagoon",
                                  Description = "Glacier harbor willow lagoon",
                                  Taxable = true,
                                  UpgradeCharge = CreditType.Full,
                                  DowngradeCredit = CreditType.Prorated,
                                  UnitPrice = "100.00",
                                  DisplayOnHostedPage = true,
                                  HostedSignupFormIds = [320495],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonResponse
              }
              catch (ApiException<CreateToggleAddonError> ex)
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
    Create-Toggle-Addon:
      title: Create Toggle Addon
      type: object
      properties:
        toggle_addon:
          $ref: "#/components/schemas/Toggle-Addon"
      required:
        - toggle_addon
    Toggle-Addon:
      title: Toggle Addon
      type: object
      properties:
        name:
          type: string
          description: A name for this addon that is suitable for showing customers and
            displaying on billing statements, e.g., "Minutes".
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
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type"
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type"
        price_options:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Price-Option-Item"
        unit_price:
          description: This is the amount that the customer will be charged when they turn
            the addon on for the subscription. The price can contain up to 8
            decimal places. e.g., 1.00 or 0.0012 or 0.00000065
          type:
            - string
            - number
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
        display_on_hosted_page:
          type: boolean
        allow_fractional_quantities:
          type: boolean
        hosted_signup_form_ids:
          type: array
          items:
            type: integer
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
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
info:
  title: Ombrevix Billing
  version: "1.0"
```