# Activate Event-Based Addon (/api/openapi/subscription-addons/activateEventBasedAddon)

`POST /event_based_billing/subscriptions/{subscription_id}/addons/{addon_id}/activate`

Activates an event-based addon for a single subscription.

In order to bill your subscribers on your Events data under the Events-Usage Billing feature, the addons must be activated for the subscriber.

Learn more about the role of activation in the Events-Usage Billing docs.

Use this endpoint to activate an event-based addon for a single subscription. Activating an event-based addon causes Ombrevix Billing to bill for events when the subscription is renewed.

*Note: it is possible to stream events for a subscription at any time, regardless of addon activation status. The activation status only determines if the subscription should be billed for event-based addon usage at renewal.*

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
  /event_based_billing/subscriptions/{subscription_id}/addons/{addon_id}/activate:
    parameters:
      - schema:
          type: integer
        name: subscription_id
        in: path
        required: true
        description: The Ombrevix Billing id of the subscription
      - schema:
          type: integer
        name: addon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the addon
    post:
      summary: Activate Event-Based Addon
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
      operationId: activateEventBasedAddon
      description: >-
        Activates an event-based addon for a single subscription.


        In order to bill your subscribers on your Events data under the
        Events-Usage Billing feature, the addons must be activated for the
        subscriber.


        Learn more about the role of activation in the Events-Usage Billing
        docs.


        Use this endpoint to activate an event-based addon for a single
        subscription. Activating an event-based addon causes Ombrevix Billing to
        bill for events when the subscription is renewed.


        *Note: it is possible to stream events for a subscription at any time,
        regardless of addon activation status. The activation status only
        determines if the subscription should be billed for event-based addon
        usage at renewal.*
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Subscription-Addon-Activate-Event-Based-Addon"
            examples:
              Example:
                value:
                  price_option_id: 5
                  billing_schedule:
                    initial_billing_at: 2022-01-01
                  custom_price:
                    tax_included: false
                    pricing_scheme: per_unit
                    interval: 30
                    interval_unit: day
                    prices:
                      - starting_quantity: 1
                        ending_quantity: null
                        unit_price: "5.0"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.SubscriptionAddons.ActivateEventBasedAddon(
                      new ActivateEventBasedAddonRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
                          Body = new ActivateEventBasedAddon
                          {
                              PriceOptionId = 5,
                              BillingSchedule = new BillingSchedule
                              {
                                  InitialBillingAt = DateTimeOffset.Parse(
                                      "2022-01-01T00:00:00Z"),
                              },
                              CustomPrice = new AddonCustomPrice
                              {
                                  TaxIncluded = false,
                                  PricingScheme = PricingScheme.PerUnit,
                                  Interval = 30,
                                  IntervalUnit = IntervalUnit.Day,
                                  Prices = [
                                      new Price
                                      {
                                          StartingQuantity = 1,
                                          EndingQuantity = 1,
                                          UnitPrice = "5.0",
                                      },
                                  ],
                              },
                          },
                      });
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
    Subscription-Addon-Activate-Event-Based-Addon:
      title: Activate Event-Based Addon
      type: object
      properties:
        price_option_id:
          type: integer
          description: The Ombrevix id of the price option
        billing_schedule:
          $ref: "#/components/schemas/Billing-Schedule"
        custom_price:
          $ref: "#/components/schemas/Addon-Custom-Price"
    Billing-Schedule:
      title: Billing Schedule
      type: object
      description: Billing schedule settings for addon provisions or usages on
        multi-frequency subscriptions. Use this to start a addon's billing
        period on a custom date instead of aligning with the product charge
        schedule.
      properties:
        initial_billing_at:
          type: string
          format: date
          example: 2026-01-01
          nullable: true
          description: Custom start date (ISO 8601 date, YYYY-MM-DD) for the addon's first
            billing period. If omitted or null, billing aligns with the product
            schedule. If provided, date must be on or after the minimum allowed
            date for the subscription or addon.
    Addon-Custom-Price:
      title: Addon Custom Price
      type: object
      description: Create or update custom pricing unique to the subscription. Used in
        place of `price_option_id`.
      properties:
        tax_included:
          type: boolean
          description: Whether or not the price option includes tax
        pricing_scheme:
          allOf:
            - $ref: "#/components/schemas/Pricing-Scheme"
          description: Omit for Toggle addons.
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
        list_price_option_id:
          type:
            - integer
            - "null"
          description: >-
            (Optional) Id of the price option to use for list price calculations
            when

            overriding the customer price.
        use_default_list_price:
          type: boolean
          description: When true, list price calculations will continue to use the default
            price option even when a `custom_price` is supplied.
        prices:
          type: array
          description: Toggle addons only need one price bracket starting at 1.
          items:
            $ref: "#/components/schemas/Price"
        renew_prepaid_provision:
          type: boolean
          description: Applicable only to prepaid usage addons. Controls whether the
            provisioned quantity renews each period.
        rollover_prepaid_remainder:
          type: boolean
          description: Applicable only to prepaid usage addons. Controls whether remaining
            units roll over to the next period.
        expiration_interval:
          type:
            - integer
            - "null"
          description: Applicable only when rollover is enabled. Number of
            `expiration_interval_unit`s after which rollover amounts expire.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: Applicable only when rollover is enabled. Interval unit for
            rollover expiration (month or day).
      required:
        - prices
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