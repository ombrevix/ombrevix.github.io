# Find Addon (/api/openapi/addons/findAddon)

`GET /addons/lookup`

Returns information for a addon matching the provided slug. You can identify your addons with a slug so you don't have to save or reference the IDs we generate.

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
  /addons/lookup:
    get:
      summary: Find Addon
      tags:
        - Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Addon-Response"
              examples:
                Example:
                  value:
                    addon:
                      id: 345045
                      name: Harbor Willow Lagoon
                      pricing_scheme: null
                      unit_name: on/prairie
                      unit_price: "100.0"
                      product_lineup_id: 316839
                      price_per_unit_minor: null
                      kind: toggle_addon
                      archived: false
                      taxable: true
                      description: Glacier harbor willow lagoon
                      default_price_option_id: 143329
                      price_option_count: 4
                      price_options_url: https://sample-shop.ombrevix.example/addons/399853/price_options
                      tax_code: JML8DZDK
                      recurring: true
                      upgrade_charge: null
                      downgrade_credit: null
                      created_at: 2019-08-02T05:54:53-04:00
                      default_price_option_name: Basalt
                      product_lineup_name: Kestrel
      operationId: findAddon
      parameters:
        - schema:
            type: string
          in: query
          required: true
          name: slug
          description: The slug of the addon to find
      description: Returns information for a addon matching the provided slug. You can
        identify your addons with a slug so you don't have to save or reference
        the IDs we generate.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Addons.FindAddon(
                      new FindAddonRequest
                      {
                          Slug = "some example string",
                      });
                  // TODO: Handle 'response' of type AddonResponse
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
    Addon-Response:
      title: Addon Response
      type: object
      properties:
        addon:
          $ref: "#/components/schemas/Addon"
      required:
        - addon
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