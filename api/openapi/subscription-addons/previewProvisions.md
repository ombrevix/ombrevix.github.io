# Preview Provisions (/api/openapi/subscription-addons/previewProvisions)

`POST /subscriptions/{subscription_id}/provisions/preview`

Previews a potential subscription's **seat-based** or **Toggle** addon provision in the middle of the current billing period.  This is useful if you want users to be able to see the effect of a addon operation before actually doing it.

## Fine-grained Addon Control: Use with multiple `upgrade_charge`s or `downgrade_credits`

When the provision uses multiple different types of `upgrade_charge`s or `downgrade_credit`s, the Provision is viewed as an Provision which uses "Fine-Grained Addon Control". As a result, the response will not include `direction` and `proration` within the `provision_preview`, but at the `line_items` and `provisions` level respectfully.

See example below for Fine-Grained Addon Control response.

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
  /subscriptions/{subscription_id}/provisions/preview:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Preview Provisions
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Provision-Preview-Response"
              examples:
                Example:
                  value:
                    provision_preview:
                      start_date: 2019-05-02T15:26:46Z
                      end_date: 2019-05-08T15:26:46Z
                      period_type: prorated
                      total_minor: 150
                      total_discount_minor: 0
                      total_tax_minor: 0
                      subtotal_minor: 150
                      existing_balance_minor: 0
                      accrue_charge: true
                      line_items:
                        - direction: upgrade
                          transaction_type: charge
                          kind: seat_based_addon
                          amount_minor: 100
                          taxable_amount_minor: 0
                          discount_amount_minor: 0
                          memo: "Cobalt: 0 to 10 cobalt"
                          addon_id: 684
                          addon_slug: cobalt
                        - direction: downgrade
                          transaction_type: credit
                          kind: seat_based_addon
                          amount_minor: -20
                          taxable_amount_minor: 0
                          discount_amount_minor: 0
                          memo: "Cobalt: 10 to 5 juniper"
                          addon_id: 843
                          addon_slug: juniper
                        - direction: upgrade
                          transaction_type: credit
                          kind: seat_based_addon
                          amount_minor: 70
                          taxable_amount_minor: 0
                          discount_amount_minor: 0
                          memo: "Cobalt: 0 to 10 willow"
                          addon_id: 570
                          addon_slug: willow
                      provisions:
                        - accrue_charge: true
                          upgrade_charge: prorated
                          downgrade_credit: full
                          addon_slug: cobalt
                          addon_id: 684
                          memo: cobalt
                          previous_price_option_id: 684
                          previous_quantity: 0
                          price_option_id: 684
                          proration_downgrade_scheme: full
                          proration_upgrade_scheme: raven-maple-tundra
                          quantity: 10
                          subscription_id: 483693
                          timestamp: null
                        - accrue_charge: true
                          upgrade_charge: full
                          downgrade_credit: prorated
                          addon_slug: juniper
                          addon_id: 843
                          memo: cobalt
                          previous_price_option_id: 843
                          previous_quantity: 10
                          price_option_id: 843
                          proration_downgrade_scheme: raven
                          proration_upgrade_scheme: iris-garnet-maple-tundra
                          quantity: 5
                          subscription_id: 483693
                          timestamp: null
                        - accrue_charge: true
                          upgrade_charge: full
                          downgrade_credit: none
                          addon_slug: willow
                          addon_id: 570
                          memo: cobalt
                          previous_price_option_id: 570
                          previous_quantity: 0
                          price_option_id: 570
                          proration_downgrade_scheme: no-raven
                          proration_upgrade_scheme: iris-garnet-maple-tundra
                          quantity: 10
                          subscription_id: 483693
                          timestamp: null
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Addon-Provision-Error"
                type: object
              examples:
                Example:
                  value:
                    errors:
                      - kind: prairie
                        addon_id: 871045
                        on: onyx
                        message: Onyx glacier delta be lagoon harbor quartz cobalt on/prairie juniper.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: previewProvisions
      description: >-
        Previews a potential subscription's **seat-based** or **Toggle** addon
        provision in the middle of the current billing period.  This is useful
        if you want users to be able to see the effect of a addon operation
        before actually doing it.


        ## Fine-grained Addon Control: Use with multiple `upgrade_charge`s or
        `downgrade_credits`


        When the provision uses multiple different types of `upgrade_charge`s or
        `downgrade_credit`s, the Provision is viewed as an Provision which uses
        "Fine-Grained Addon Control". As a result, the response will not include
        `direction` and `proration` within the `provision_preview`, but at the
        `line_items` and `provisions` level respectfully.


        See example below for Fine-Grained Addon Control response.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Preview-Provisions-Request"
            examples:
              Example:
                value:
                  provisions:
                    - proration_upgrade_scheme: raven-lagoon-tundra
                      proration_downgrade_scheme: raven
                      addon_id: 633483
                      price_option_id: 346324
                      quantity: 10
                      memo: Iris
                  effective_proration_date: 2023-11-01
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionAddons.PreviewProvisions(
                      new PreviewProvisionsOperationRequest
                      {
                          SubscriptionId = 1,
                          Body = new PreviewProvisionsRequest
                          {
                              Provisions = [
                                  new CreateProvision
                                  {
                                      Quantity = 10d,
                                      AddonId = 633483,
                                      Memo = "Iris",
                                      ProrationDowngradeScheme = "raven",
                                      ProrationUpgradeScheme = "raven-lagoon-tundra",
                                      PriceOptionId = 346324,
                                  },
                              ],
                              EffectiveProrationDate = DateTimeOffset.Parse(
                                  "2023-11-01T00:00:00Z"),
                          },
                      });
                  // TODO: Handle 'response' of type ProvisionPreviewResponse
              }

              catch (ApiException<PreviewProvisionsError> ex)

              {
                  if (ex.Error.TryGetAddonProvisionError(out var error))
                  {
                      // TODO: Handle 'error' of type AddonProvisionError
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
    Provision-Preview-Response:
      title: Provision Preview Response
      type: object
      properties:
        provision_preview:
          $ref: "#/components/schemas/Provision-Preview"
      required:
        - provision_preview
    Addon-Provision-Error:
      title: Addon Provision Error
      type: object
      properties:
        errors:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Provision-Error-Item"
    Preview-Provisions-Request:
      title: Preview Provisions Request
      type: object
      properties:
        provisions:
          type: array
          items:
            $ref: "#/components/schemas/Create-Provision"
        effective_proration_date:
          type: string
          example: 2023-12-01
          format: date
          description: To calculate proration amounts for a future time. Only within a
            current subscription period. Only ISO8601 format is supported.
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type"
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type"
      required:
        - provisions
    Create-Provision:
      title: Create Provision
      type: object
      properties:
        quantity:
          type: number
          description: The provisioned quantity to which to set the line-items provisioned
            quantity. By default, this is an integer. If decimal provisions are
            enabled for the addon, it will be a decimal number. For Toggle
            addons, use 1 for on and 0 for off.
        decimal_quantity:
          type: string
          description: >-
            Decimal representation of the provisioned quantity. Only valid when
            decimal

            provisions are enabled for the addon.
        previous_quantity:
          type: number
          description: >-
            The quantity that was in effect before this provision. Responses
            always

            include this value; it may be supplied on preview requests to ensure
            the

            expected change is evaluated.
        decimal_previous_quantity:
          type: string
          description: >-
            Decimal representation of `previous_quantity`. Only valid when
            decimal

            provisions are enabled for the addon.
        addon_id:
          type: integer
          description: (required for the multiple provisions endpoint) The id associated
            with the addon for which the provision is being made.
        memo:
          type: string
          description: A memo to record along with the provision.
        proration_downgrade_scheme:
          type: string
          description: The scheme used if the proration is a downgrade. Defaults to the
            workspace setting if one is not provided.
          deprecated: true
        proration_upgrade_scheme:
          type: string
          description: The scheme used if the proration is an upgrade. Defaults to the
            workspace setting if one is not provided.
          deprecated: true
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type-Downgrade-Credit"
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type-Upgrade-Charge"
        accrue_charge:
          type: boolean
          description: >-
            "If the change in cost is an upgrade, this determines if the charge
            should accrue to the next renewal or if capture should be attempted
            immediately.


            `true` - Attempt to charge the customer at the next renewal.      


            `false` - Attempt to charge the customer right away. If it fails,
            the charge will be accrued until the next renewal.


            Defaults to the workspace setting if unspecified in the request.
        initiate_recovery:
          type: boolean
          description: >-
            If set to true, if the immediate addon payment fails, initiate
            recovery for the subscription. 

            Otherwise, leave the charges on the subscription to pay for at
            renewal. Defaults to false.
        price_option_id:
          type:
            - string
            - integer
            - "null"
          description: Price option that the provision should be charged at. Accepts
            either the price option's id (integer) or slug (string). When not
            specified, the default price option will be used.
        billing_schedule:
          $ref: "#/components/schemas/Billing-Schedule"
        custom_price:
          $ref: "#/components/schemas/Addon-Custom-Price"
      required:
        - quantity
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
    Credit-Type-Downgrade-Credit:
      title: Downgrade Credit Credit Type
      type:
        - string
        - "null"
      description: >-
        The type of credit to be created when upgrading/downgrading. Defaults to
        the addon and then workspace setting if one is not provided. Values are:


        `full` -  A full price credit is added for the amount owed. 


        `prorated` - A prorated credit is added for the amount owed. 


        `none` - No charge is added.
      enum:
        - full
        - prorated
        - none
        - null
    Credit-Type-Upgrade-Charge:
      title: Upgrade Charge Credit Type
      type:
        - string
        - "null"
      description: >-
        The type of credit to be created when upgrading/downgrading. Defaults to
        the addon and then workspace setting if one is not provided. Values are:


        `full` - A charge is added for the full price of the addon. 


        `prorated` - A charge is added for the prorated price of the addon
        change.


        `none` - No charge is added.
      enum:
        - full
        - prorated
        - none
        - null
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
    Addon-Provision-Error-Item:
      title: Addon Provision Error Item
      type: object
      properties:
        addon_id:
          type: integer
        message:
          type: string
        kind:
          type: string
        on:
          type: string
    Provision-Preview:
      title: Provision Preview
      type: object
      properties:
        start_date:
          type: string
          format: date-time
        end_date:
          type: string
          format: date-time
        subtotal_minor:
          type: integer
          format: int64
        total_tax_minor:
          type: integer
          format: int64
        total_discount_minor:
          type: integer
          format: int64
        total_minor:
          type: integer
          format: int64
        direction:
          $ref: "#/components/schemas/Provision-Preview-Direction"
        proration_scheme:
          type: string
        line_items:
          type: array
          items:
            $ref: "#/components/schemas/Provision-Preview-Line-Item"
        accrue_charge:
          type: boolean
        provisions:
          type: array
          items:
            $ref: "#/components/schemas/Provision-Preview-Item"
        period_type:
          type: string
        existing_balance_minor:
          type: integer
          format: int64
          description: An integer representing the amount of the subscription's current
            balance
    Provision-Preview-Direction:
      title: Provision Preview Direction
      type: string
      enum:
        - upgrade
        - downgrade
    Provision-Preview-Line-Item:
      title: Provision Preview Line Item
      type: object
      properties:
        transaction_type:
          $ref: "#/components/schemas/Line-Item-Transaction-Type"
        kind:
          $ref: "#/components/schemas/Provision-Preview-Line-Item-Kind"
        amount_minor:
          type: integer
          format: int64
        memo:
          type: string
        discount_amount_minor:
          type: integer
          format: int64
        taxable_amount_minor:
          type: integer
          format: int64
        addon_id:
          type: integer
        addon_slug:
          type: string
        direction:
          allOf:
            - $ref: "#/components/schemas/Provision-Preview-Direction"
          description: Visible when using Fine-grained Addon Control.
    Provision-Preview-Item:
      title: Provision Preview Item
      type: object
      properties:
        addon_id:
          type: integer
        subscription_id:
          type: integer
        quantity:
          oneOf:
            - type: integer
            - type: string
        previous_quantity:
          oneOf:
            - type: integer
            - type: string
        memo:
          type:
            - string
            - "null"
        timestamp:
          type:
            - string
            - "null"
        proration_upgrade_scheme:
          type: string
          deprecated: true
        proration_downgrade_scheme:
          type: string
          deprecated: true
        accrue_charge:
          type: boolean
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type"
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type"
        price_option_id:
          type: integer
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
        previous_price_option_id:
          type: integer
        price_option_slug:
          type: string
        price_option_name:
          type: string
        addon_slug:
          type:
            - string
            - "null"
    Line-Item-Transaction-Type:
      title: Line Item Transaction Type
      type: string
      description: A slug for the line item transaction type
      enum:
        - charge
        - credit
        - adjustment
        - payment
        - refund
        - info_transaction
        - payment_authorization
    Provision-Preview-Line-Item-Kind:
      title: Provision Preview Line Item Kind
      type: string
      description: A slug for the line item kind for provision preview
      enum:
        - seat_based_addon
        - toggle_addon
        - coupon
        - tax
info:
  title: Ombrevix Billing
  version: "1.0"
```