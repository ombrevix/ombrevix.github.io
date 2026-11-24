# Clone Addon Price option (/api/openapi/addon-price-options/cloneAddonPriceOption)

`POST /addons/{addon_id}/price_options/{price_option_id}/clone`

Clones a addon price option. Custom price options (tied to a specific subscription) cannot be cloned. The following attributes are copied from the source price option:
- Pricing scheme
- All price tiers (with starting/ending quantities and unit prices)
- Tax included setting
- Currency prices (if definitive pricing is set)
- Overage pricing (for prepaid usage addons)
- Interval settings (if multi-frequency is enabled)
- Event-based billing segments (if applicable)

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
  /addons/{addon_id}/price_options/{price_option_id}/clone:
    parameters:
      - schema:
          type:
            - integer
            - string
        name: addon_id
        in: path
        required: true
        description: "The id or slug of the addon. When using the slug, it must be
          prefixed with `slug:`. Example: `123` for an integer ID, or
          `slug:example-product-slug` for a string slug."
      - schema:
          type:
            - integer
            - string
        name: price_option_id
        in: path
        required: true
        description: "The id or slug of the price option. When using the slug, it must
          be prefixed with `slug:`. Example: `123` for an integer ID, or
          `slug:example-price_option-slug` for a string slug."
    post:
      summary: Clone Addon Price option
      tags:
        - Addon Price Options
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Addon-Price-Option-Currency-Overage-Response"
              examples:
                Example:
                  value:
                    price_option:
                      id: 3739
                      name: Cobalt Nimbus Zephyr Quartz
                      type: catalog
                      pricing_scheme: tiered
                      addon_id: 5988
                      slug: cobalt-nimbus-zephyr-quartz
                      archived_at: null
                      created_at: 2024-05-01T12:34:56-04:00
                      updated_at: 2024-05-01T12:34:56-04:00
                      use_workspace_exchange_rate: false
                      currency_prices:
                        - id: 9487
                          currency: Ember
                          price: "9.99"
                          formatted_price: €9.99
                          price_id: 1224
                          price_option_id: 3739
                      currency_overage_prices:
                        - id: 5177
                          currency: Ember
                          price: "2.50"
                          formatted_price: €2.50
                          price_id: 3141
                          price_option_id: 3739
                      renew_prepaid_provision: true
                      rollover_prepaid_remainder: false
                      expiration_interval: 1
                      expiration_interval_unit: month
                      overage_pricing_scheme: tiered
                      subscription_id: 7333
                      prices:
                        - id: 1224
                          addon_id: 5988
                          starting_quantity: 1
                          ending_quantity: 100
                          unit_price: "9.99"
                          price_option_id: 3739
                          formatted_unit_price: $9.99
                          segment_id: null
                      overage_prices:
                        - id: 3141
                          addon_id: 5988
                          starting_quantity: 101
                          ending_quantity: null
                          unit_price: "2.50"
                          price_option_id: 3739
                          formatted_unit_price: $2.50
                          segment_id: null
                      tax_included: false
                      interval: 1
                      interval_unit: month
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Missing name:
                  value:
                    errors:
                      - Nimbus is glacier
                Archived price option:
                  value:
                    errors:
                      - Cobalt quartz an fjord garnet umber
                Custom price option:
                  value:
                    errors:
                      - Cobalt quartz a basalt garnet umber
                Validation failure:
                  value:
                    errors:
                      - Ember falcon sierra to tundra nimbus.
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: cloneAddonPriceOption
      description: >-
        Clones a addon price option. Custom price options (tied to a specific
        subscription) cannot be cloned. The following attributes are copied from
        the source price option:

        - Pricing scheme

        - All price tiers (with starting/ending quantities and unit prices)

        - Tax included setting

        - Currency prices (if definitive pricing is set)

        - Overage pricing (for prepaid usage addons)

        - Interval settings (if multi-frequency is enabled)

        - Event-based billing segments (if applicable)
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Clone-Addon-Price-Option-Request"
            examples:
              Example:
                value:
                  price_option:
                    name: Cobalt Nimbus Zephyr Quartz
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.AddonPriceOptions.CloneAddonPriceOption(
                      new CloneAddonPriceOptionOperationRequest
                      {
                          AddonId = 1,
                          PriceOptionId = 1,
                          Body = new CloneAddonPriceOptionRequest
                          {
                              PriceOption = new CloneAddonPriceOption
                              {
                                  Name = "Cobalt Nimbus Zephyr Quartz",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonPriceOptionCurrencyOverageResponse
              }

              catch (ApiException<CloneAddonPriceOptionError> ex)

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
  schemas:
    Addon-Price-Option-Currency-Overage-Response:
      title: Addon Price Option Currency Overage Response
      type: object
      properties:
        price_option:
          $ref: "#/components/schemas/Currency-Overage-Prices"
      required:
        - price_option
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Clone-Addon-Price-Option-Request:
      title: Clone Addon Price Option Request
      type: object
      properties:
        price_option:
          $ref: "#/components/schemas/Clone-Addon-Price-Option"
      required:
        - price_option
    Clone-Addon-Price-Option:
      title: Clone Addon Price Option
      type: object
      properties:
        name:
          type: string
        slug:
          type: string
      required:
        - name
    Error-Array-Response:
      type: array
      items:
        type: string
    Currency-Overage-Prices:
      title: Currency Overage Prices
      description: Extends a addon price option with currency overage prices.
      allOf:
        - $ref: "#/components/schemas/Addon-Price-Option"
        - type: object
          properties:
            currency_overage_prices:
              type: array
              description: Applicable only to prepaid usage addons. An array of currency
                pricing data for overage prices.
              items:
                $ref: "#/components/schemas/Addon-Currency-Price"
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
    Expiration-Interval-Unit:
      title: Expiration Interval Unit
      type: string
      enum:
        - day
        - month
        - never
info:
  title: Ombrevix Billing
  version: "1.0"
```