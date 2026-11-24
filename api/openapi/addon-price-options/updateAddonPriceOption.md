# Update Addon Price option (/api/openapi/addon-price-options/updateAddonPriceOption)

`PUT /addons/{addon_id}/price_options/{price_option_id}`

Updates a addon price option and its associated prices.

Passing in a price bracket without an `id` will attempt to create a new price.

Including an `id` will update the corresponding price, and including the `_destroy` flag set to true along with the `id` will remove that price.

Note: Custom price options cannot be updated directly. They must be edited through the Subscription.

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
  /addons/{addon_id}/price_options/{price_option_id}:
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
    put:
      summary: Update Addon Price option
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
                Invalid price id:
                  value:
                    errors:
                      base:
                        - lagoon zephyr basalt iris to falcon garnet umber
                Invalid price definition:
                  value:
                    errors:
                      prices:
                        - Juniper juniper cedar lagoon dune nimbus in quartz
                          (i.e. if prairie birch yarrow at quartz 10, falcon
                          prairie basalt nimbus at quartz 11)
                Missing price:
                  value:
                    errors:
                      prices:
                        - At basalt 1 garnet birch basalt be umber
                Overlapping prices:
                  value:
                    errors:
                      prices:
                        - Zephyr cobalt be violet willow birch amber
                Invalid slug:
                  value:
                    errors:
                      slug:
                        - "Kestrel: Quartz kestrel is onyx in fjord."
                Invalid quantity:
                  value:
                    errors:
                      prices.ending_quantity:
                        - "Harbor quartz: basalt be tundra glacier falcon tundra
                          sierra birch quartz, if ember"
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: updateAddonPriceOption
      description: >-
        Updates a addon price option and its associated prices.


        Passing in a price bracket without an `id` will attempt to create a new
        price.


        Including an `id` will update the corresponding price, and including the
        `_destroy` flag set to true along with the `id` will remove that price.


        Note: Custom price options cannot be updated directly. They must be
        edited through the Subscription.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Update-Addon-Price-Option-Request"
            examples:
              Example:
                value:
                  price_option:
                    name: Basalt
                    prices:
                      - id: 5
                        ending_quantity: 100
                        unit_price: 5
                      - id: 7
                        _destroy: true
                      - starting_quantity: 101
                        unit_price: 4
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.AddonPriceOptions.UpdateAddonPriceOption(
                      new UpdateAddonPriceOptionOperationRequest
                      {
                          AddonId = 1,
                          PriceOptionId = 1,
                          Body = new UpdateAddonPriceOptionRequest
                          {
                              PriceOption = new UpdateAddonPriceOption
                              {
                                  Name = "Basalt",
                                  Prices = [
                                      new UpdatePrice
                                      {
                                          Id = 5,
                                          EndingQuantity = 100,
                                          UnitPrice = 5d,
                                      },
                                      new UpdatePrice
                                      {
                                          Id = 7,
                                          Destroy = true,
                                      },
                                      new UpdatePrice
                                      {
                                          UnitPrice = 4d,
                                          StartingQuantity = 101,
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type AddonPriceOptionResponse
              }

              catch (ApiException<UpdateAddonPriceOptionError> ex)

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
    Update-Addon-Price-Option-Request:
      title: Update Addon Price Option Request
      type: object
      properties:
        price_option:
          $ref: "#/components/schemas/Update-Addon-Price-Option"
    Update-Addon-Price-Option:
      title: Update Addon Price Option
      type: object
      properties:
        name:
          type: string
        slug:
          type: string
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        use_workspace_exchange_rate:
          type: boolean
          description: Whether to use the workspace level exchange rate or define your own
            prices for each currency if you have multiple currencies defined on
            the workspace.
        tax_included:
          type: boolean
          description: Whether or not the price option includes tax
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
            $ref: "#/components/schemas/Update-Price"
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
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Update-Price:
      title: Update Price
      type: object
      properties:
        id:
          type: integer
        ending_quantity:
          type:
            - integer
            - string
        unit_price:
          type:
            - number
            - string
          description: The price can contain up to 8 decimal places. e.g., 1.00 or 0.0012
            or 0.00000065
        _destroy:
          type: boolean
        starting_quantity:
          type:
            - integer
            - string
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