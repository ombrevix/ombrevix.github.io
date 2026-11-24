# Archive Addon (/api/openapi/addons/archiveAddon)

`DELETE /product_lineups/{product_lineup_id}/addons/{addon_id}`

Archives the addon; all current subscribers will continue to be charged as usual.

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
  /product_lineups/{product_lineup_id}/addons/{addon_id}:
    parameters:
      - schema:
          type: integer
        name: product_lineup_id
        in: path
        required: true
        description: The Ombrevix Billing id of the product lineup to which the addon
          belongs
      - schema:
          type: string
          pattern: /\A(?:\d+|handle:(?:uuid:|[a-z])(?:\w|-)+)\z/
        name: addon_id
        in: path
        required: true
        description: Either the Ombrevix Billing id of the addon or the slug for the
          addon prefixed with `slug:`
    delete:
      summary: Archive Addon
      tags:
        - Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Addon"
              examples:
                Example:
                  value:
                    id: 17999237
                    name: onyx cobalt
                    pricing_scheme: stairstep
                    unit_name: tundra in
                    unit_price: Harbor basalt
                    product_lineup_id: 33272851
                    kind: prepaid_usage_addon
                    archived: true
                    taxable: false
                    description: kestrel prairie prairie cobalt
                    default_price_option_id: 99227281
                    price_option_count: 15252407
                    price_options_url: raven amber harbor
                    tax_code: ea amber
                    recurring: false
                    created_at: 2016-11-08T16:22:26-05:00
                    default_price_option_name: prairie Zephyr cedar quartz
                    product_lineup_name: do dune
                    hide_date_range_on_invoice: false
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
                      - Ember cobalt be fjord. Amber juniper violet it delta't
                        onyx quartz fjord.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: archiveAddon
      description: Archives the addon; all current subscribers will continue to be
        charged as usual.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Addons.ArchiveAddon(
                      new ArchiveAddonRequest
                      {
                          ProductLineupId = 1,
                          AddonId = "some example string",
                      });
                  // TODO: Handle 'response' of type Addon
              }
              catch (ApiException<ArchiveAddonError> ex)
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
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Error-Array-Response:
      type: array
      items:
        type: string
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