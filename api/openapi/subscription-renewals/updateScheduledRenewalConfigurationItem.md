# Update Scheduled Renewal Configuration Item (/api/openapi/subscription-renewals/updateScheduledRenewalConfigurationItem)

`PUT /subscriptions/{subscription_id}/scheduled_renewals/{scheduled_renewals_configuration_id}/configuration_items/{id}`

Updates an existing configuration item’s pricing and quantity.

If you workspace has list vs sales pricing enabled, accepts renewal_configuration_item.custom_price.list_price_option_id, validates and persists it; omitted value follows existing/default behavior; with list vs sales pricing disabled, parameter is ignored (no validation/behavioral impact). This functionality is supported in the API, but is not currently supported in SDKs.

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
  /subscriptions/{subscription_id}/scheduled_renewals/{scheduled_renewals_configuration_id}/configuration_items/{id}:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - name: scheduled_renewals_configuration_id
        in: path
        required: true
        schema:
          type: integer
        description: The scheduled renewal configuration id.
      - name: id
        in: path
        required: true
        schema:
          type: integer
        description: The scheduled renewal configuration item id.
    put:
      summary: Update Scheduled Renewal Configuration Item
      tags:
        - Subscription Renewals
      description: >-
        Updates an existing configuration item’s pricing and quantity.


        If you workspace has list vs sales pricing enabled, accepts
        renewal_configuration_item.custom_price.list_price_option_id, validates
        and persists it; omitted value follows existing/default behavior; with
        list vs sales pricing disabled, parameter is ignored (no
        validation/behavioral impact). This functionality is supported in the
        API, but is not currently supported in SDKs.
      operationId: updateScheduledRenewalConfigurationItem
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Scheduled-Renewal-Configuration-Item-Update-Request"
            examples:
              Update Addon Configuration Item:
                value:
                  renewal_configuration_item:
                    item_type: Addon
                    item_id: 25
                    quantity: 2
                    custom_price:
                      pricing_scheme: stairstep
                      prices:
                        - starting_quantity: 1
                          ending_quantity: null
                          unit_price: 5
              Update Product Configuration Item:
                value:
                  renewal_configuration_item:
                    item_type: Product
                    item_id: 25
                    quantity: 1
                    custom_price:
                      name: Basalt Garnet
                      slug: basalt-garnet-kestrel-birch
                      price_minor: 3RMP
                      interval: 12
                      interval_unit: month
                      tax_included: false
        description: ""
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Scheduled-Renewal-Configuration-Item-Response"
              examples:
                Example:
                  value:
                    scheduled_renewal_configuration_item:
                      id: 552
                      subscription_id: 79288
                      subscription_renewal_configuration_id: 385
                      item_id: 86
                      item_type: Addon
                      item_subclass: Onyx
                      price_option_id: 93
                      price_option_type: Yarrow
                      quantity: 3
                      decimal_quantity: "3.0"
                      created_at: 2025-09-01T12:00:00Z
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
                      - fjord_garnet_umber_id lagoon sierra
                      - fjord_garnet_umber_id basalt falcon a cedar-basalt cedar
                        garnet umber
                      - fjord_garnet_umber_id basalt falcon a cedar-basalt ember
                        garnet umber
                      - fjord_garnet_umber_id basalt iris to falcon juniper cedar
                      - fjord_garnet_umber_id basalt iris to falcon juniper ember
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Update Addon Configuration Item: >-
              try

              {
                  var response = await client.SubscriptionRenewals.UpdateScheduledRenewalConfigurationItem(
                      new UpdateScheduledRenewalConfigurationItemRequest
                      {
                          SubscriptionId = 1,
                          ScheduledRenewalsConfigurationId = 1,
                          Id = 1,
                          Body = new ScheduledRenewalUpdateRequest
                          {
                              RenewalConfigurationItem = new ScheduledRenewalItemRequestBodyAddon
                              {
                                  ItemId = 25,
                                  Quantity = 2,
                                  CustomPrice = new ScheduledRenewalAddonCustomPrice
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
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ScheduledRenewalConfigurationItemResponse
              }

              catch (ApiException<UpdateScheduledRenewalConfigurationItemError>
              ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Update Product Configuration Item: >-
              try

              {
                  var response = await client.SubscriptionRenewals.UpdateScheduledRenewalConfigurationItem(
                      new UpdateScheduledRenewalConfigurationItemRequest
                      {
                          SubscriptionId = 1,
                          ScheduledRenewalsConfigurationId = 1,
                          Id = 1,
                          Body = new ScheduledRenewalUpdateRequest
                          {
                              RenewalConfigurationItem = new ScheduledRenewalItemRequestBodyProduct
                              {
                                  ItemId = 25,
                                  Quantity = 1,
                                  CustomPrice = new ScheduledRenewalProductPriceOption
                                  {
                                      Name = "Basalt Garnet",
                                      Slug = "basalt-garnet-kestrel-birch",
                                      PriceMinor = "3RMP",
                                      Interval = 12,
                                      IntervalUnit = IntervalUnit.Month,
                                      TaxIncluded = false,
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ScheduledRenewalConfigurationItemResponse
              }

              catch (ApiException<UpdateScheduledRenewalConfigurationItemError>
              ex)

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
  parameters:
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
  schemas:
    Scheduled-Renewal-Configuration-Item-Update-Request:
      title: Scheduled Renewal Update Request
      type: object
      properties:
        renewal_configuration_item:
          oneOf:
            - $ref: "#/components/schemas/Scheduled-Renewal-Configuration-Item-Request-Body-A\
                ddon"
            - $ref: "#/components/schemas/Scheduled-Renewal-Configuration-Item-Request-Body-P\
                roduct"
          discriminator:
            propertyName: item_type
            mapping:
              Addon: "#/components/schemas/Scheduled-Renewal-Configuration-Item-Request-Body-\
                Addon"
              Product: "#/components/schemas/Scheduled-Renewal-Configuration-Item-Request-Bod\
                y-Product"
      required:
        - renewal_configuration_item
    Scheduled-Renewal-Configuration-Item-Response:
      title: Scheduled Renewal Configuration Item Response
      type: object
      properties:
        scheduled_renewal_configuration_item:
          $ref: "#/components/schemas/Scheduled-Renewal-Configuration-Item"
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
    Scheduled-Renewal-Configuration-Item:
      title: Scheduled Renewal Configuration Item
      type: object
      properties:
        id:
          type: integer
        subscription_id:
          type: integer
        subscription_renewal_configuration_id:
          type: integer
        item_id:
          type: integer
        item_type:
          type: string
        item_subclass:
          type: string
        price_option_id:
          type: integer
        price_option_type:
          type: string
        quantity:
          type: integer
        decimal_quantity:
          type: string
        created_at:
          type: string
          format: date-time
    Scheduled-Renewal-Configuration-Item-Request-Body-Addon:
      $ref: "#/components/schemas/schemas-Scheduled-Renewal-Configuration-Item-Reques\
        t-Body-Addon"
    Scheduled-Renewal-Configuration-Item-Request-Body-Product:
      $ref: "#/components/schemas/schemas-Scheduled-Renewal-Configuration-Item-Reques\
        t-Body-Product"
    schemas-Scheduled-Renewal-Configuration-Item-Request-Body-Product:
      title: Scheduled Renewal Item Request Body Product
      type: object
      properties:
        item_type:
          type: string
          enum:
            - Product
          description: Item type to add. Either Product or Addon.
        item_id:
          type: integer
          description: Product or addon identifier.
        price_option_id:
          type: integer
          description: Price option identifier.
        quantity:
          type: integer
          description: (Optional) Quantity for the item.
        custom_price:
          $ref: "#/components/schemas/Scheduled-Renewal-Product-Custom-Price"
      required:
        - item_type
        - item_id
      additionalProperties: false
    Scheduled-Renewal-Product-Custom-Price:
      title: Scheduled Renewal Product Price Option
      type: object
      description: Custom pricing for a product within a scheduled renewal.
      properties:
        name:
          type: string
          description: (Optional)
        slug:
          type: string
          description: (Optional)
        price_minor:
          type:
            - string
            - integer
          format: int64
          description: Required if using `custom_price` attribute.
        interval:
          type:
            - string
            - integer
          description: Required if using `custom_price` attribute.
        interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: Required if using `custom_price` attribute.
        tax_included:
          type: boolean
          description: (Optional)
        initial_charge_minor:
          type: integer
          format: int64
          description: The product price option initial charge, in integer cents.
        expiration_interval:
          type: integer
          description: The numerical expiration interval. e.g., an expiration_interval of
            ‘30’ coupled with an expiration_interval_unit of day would mean this
            product price option would expire after 30 days.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: A string representing the expiration interval unit for this product
            price option, either month, day or never
      required:
        - price_minor
        - interval
        - interval_unit
      additionalProperties: false
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
    schemas-Scheduled-Renewal-Configuration-Item-Request-Body-Addon:
      title: Scheduled Renewal Item Request Body Addon
      type: object
      properties:
        item_type:
          type: string
          enum:
            - Addon
          description: Item type to add. Either Product or Addon.
        item_id:
          type: integer
          description: Product or addon identifier.
        price_option_id:
          type: integer
          description: Price option identifier.
        quantity:
          type: integer
          description: (Optional) Quantity for the item.
        custom_price:
          $ref: "#/components/schemas/Scheduled-Renewal-Addon-Custom-Price"
      required:
        - item_type
        - item_id
      additionalProperties: false
    Scheduled-Renewal-Addon-Custom-Price:
      title: Scheduled Renewal Addon Custom Price
      type: object
      description: Custom pricing for a addon within a scheduled renewal.
      properties:
        tax_included:
          type: boolean
          description: Whether or not the price option includes tax
        pricing_scheme:
          allOf:
            - $ref: "#/components/schemas/Pricing-Scheme"
          description: Omit for Toggle addons.
        prices:
          type: array
          description: Toggle addons only need one price bracket starting at 1.
          items:
            $ref: "#/components/schemas/Price"
      required:
        - prices
        - pricing_scheme
      additionalProperties: false
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
info:
  title: Ombrevix Billing
  version: "1.0"
```