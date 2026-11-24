# List Addons for Product lineup (/api/openapi/addons/listAddonsForProductLineup)

`GET /product_lineups/{product_lineup_id}/addons`

Lists addons for a particular product lineup.

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
  /product_lineups/{product_lineup_id}/addons:
    parameters:
      - schema:
          type: integer
        name: product_lineup_id
        in: path
        required: true
        description: The Ombrevix Billing id of the product lineup
    get:
      summary: List Addons for Product lineup
      tags:
        - Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Addon-Response"
              examples:
                Example:
                  value:
                    - addon:
                        id: 965874
                        name: $1.00 ember
                        pricing_scheme: per_unit
                        unit_name: Component
                        unit_price: "1.0"
                        product_lineup_id: 316839
                        price_per_unit_minor: null
                        kind: seat_based_addon
                        archived: false
                        taxable: false
                        description: Component
                        default_price_option_id: 524641
                        prices:
                          - id: 837511
                            addon_id: 965874
                            starting_quantity: 1
                            ending_quantity: null
                            unit_price: "1.0"
                            price_option_id: 524641
                            formatted_unit_price: $1.00
                        price_option_count: 2
                        price_options_url: https://sample-shop.ombrevix.example/addons/399850/price_options
                        tax_code: null
                        recurring: true
                        upgrade_charge: null
                        downgrade_credit: null
                        created_at: 2019-08-01T09:35:38-04:00
                        default_price_option_name: Basalt
                        product_lineup_name: Kestrel
                        use_workspace_exchange_rate: true
                    - addon:
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
                        created_at: 2019-08-01T09:35:37-04:00
                        default_price_option_name: Basalt
                        product_lineup_name: Kestrel
                        use_workspace_exchange_rate: true
                    - addon:
                        id: 909037
                        name: Iris garnet
                        pricing_scheme: null
                        unit_name: on/prairie
                        unit_price: "35.0"
                        product_lineup_id: 316839
                        price_per_unit_minor: null
                        kind: toggle_addon
                        archived: false
                        taxable: false
                        description: ""
                        default_price_option_id: 523968
                        price_option_count: 1
                        price_options_url: https://sample-shop.ombrevix.example/addons/386937/price_options
                        tax_code: null
                        recurring: true
                        upgrade_charge: null
                        downgrade_credit: null
                        created_at: 2019-08-01T09:35:38-04:00
                        default_price_option_name: Basalt
                        product_lineup_name: Kestrel
                        use_workspace_exchange_rate: true
      operationId: listAddonsForProductLineup
      description: Lists addons for a particular product lineup.
      x-operation-settings:
        collectParameters: true
      parameters:
        - schema:
            type: boolean
          in: query
          name: include_archived
          description: Include archived items.
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - $ref: "#/components/parameters/list-addons-filter"
        - schema:
            $ref: "#/components/schemas/Basic-Date-Field"
          in: query
          name: date_field
          description: The type of filter you would like to apply to your search. Use in
            query `date_field=created_at`.
        - schema:
            type: string
          in: query
          name: end_date
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns addons with a timestamp up to and including
            11:59:59PM in your workspace’s time zone on the date specified.
        - schema:
            type: string
          in: query
          name: end_datetime
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns addons with a timestamp at or before
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace's time zone will be used. If provided, this
            parameter will be used instead of end_date.
        - schema:
            type: string
          in: query
          name: start_date
          description: The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns addons with a timestamp at or after midnight
            (12:00:00 AM) in your workspace’s time zone on the date specified.
        - schema:
            type: string
          in: query
          name: start_datetime
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns addons with a timestamp at or after
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace's time zone will be used. If provided, this
            parameter will be used instead of start_date.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Addons.ListAddonsForProductLineup(
                      new ListAddonsForProductLineupRequest
                      {
                          ProductLineupId = 1,
                          Page = 1,
                          PerPage = 50,
                          DateField = BasicDateField.UpdatedAt,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<AddonResponse>
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
    Basic-Date-Field:
      title: Basic Date Field
      description: Allows to filter by `created_at` or `updated_at`.
      type: string
      enum:
        - updated_at
        - created_at
      example: updated_at
    List-Addons-Filter:
      title: List Addons Filter
      type: object
      properties:
        ids:
          type: array
          items:
            type: integer
          example:
            - 1
            - 2
            - 3
          minItems: 1
          description: Allows fetching addons with matching id based on provided value.
            Use in query `filter[ids]=1,2,3`.
          style: form
          explode: false
        use_workspace_exchange_rate:
          type: boolean
          description: Allows fetching addons with matching use_workspace_exchange_rate
            based on provided value (refers to default price option). Use in
            query `filter[use_workspace_exchange_rate]=true`.
      examples:
        With-Ids:
          ids:
            - 123
            - 124
            - 125
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
    list-addons-filter:
      in: query
      name: filter
      description: Filter to use for List Addons operations
      schema:
        $ref: "#/components/schemas/List-Addons-Filter"
      style: deepObject
      explode: true
info:
  title: Ombrevix Billing
  version: "1.0"
```