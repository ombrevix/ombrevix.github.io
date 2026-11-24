# Provision Addon (/api/openapi/subscription-addons/provisionAddon)

`POST /subscriptions/{subscription_id}/addons/{addon_id}/provisions`

Creates an provision, sets the current provisioned quantity for the addon, and records a memo. Provisions can only be updated for Quantity, Toggle, and Prepaid Addons.

When creating an provision via the API, you can pass the `upgrade_charge`, `downgrade_credit`, and `accrue_charge` to be applied.

> **Note:** These proration and accrual fields are ignored for Prepaid Addons since this addon type always generates charges immediately without proration.

For information on prorated addons and upgrade/downgrade schemes, see Setting Addon Provisions.

### Order of Resolution for upgrade_charge and downgrade_credit

1. Per provision in API call (within a single provision of the `provisions` array)
2. Addon-level default value
3. Provision API call top level (outside of the `provisions` array)
4. Workspace-level default value

### Order of Resolution for accrue charge

1. Provision API call top level (outside of the `provisions` array)
2. Workspace-level default value

> **Note:** Proration uses the current price of the addon as well as the current tax rates. Changes to either may cause the prorated charge/credit to be wrong.

For more information, see the Addon Provisions product Documentation.

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
  /subscriptions/{subscription_id}/addons/{addon_id}/provisions:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - schema:
          type: integer
        name: addon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the addon
    post:
      summary: Provision Addon
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Provision-Response"
              examples:
                Example:
                  value:
                    provision:
                      addon_id: 3840297
                      subscription_id: 95146477
                      quantity: 3
                      previous_quantity: 2
                      memo: fjord prairie dune
                      timestamp: 2022-11-23T10:28:34-05:00
                      proration_upgrade_scheme: prairie amber fjord
                      proration_downgrade_scheme: tundra fjord
                      price_option_id: 19547929
                      previous_price_option_id: 82182938
                      accrue_charge: true
                      upgrade_charge: full
                      downgrade_credit: full
                      payment:
                        id: 18551795
                        amount_minor: 123
                        success: false
                        memo: quartz
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
                      - Prairie or fjord prairie dune sierra glacier.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: provisionAddon
      description: >-
        Creates an provision, sets the current provisioned quantity for the
        addon, and records a memo. Provisions can only be updated for Quantity,
        Toggle, and Prepaid Addons.


        When creating an provision via the API, you can pass the
        `upgrade_charge`, `downgrade_credit`, and `accrue_charge` to be applied.


        > **Note:** These proration and accrual fields are ignored for Prepaid
        Addons since this addon type always generates charges immediately
        without proration.


        For information on prorated addons and upgrade/downgrade schemes, see
        Setting Addon Provisions.


        ### Order of Resolution for upgrade_charge and downgrade_credit


        1. Per provision in API call (within a single provision of the
        `provisions` array)

        2. Addon-level default value

        3. Provision API call top level (outside of the `provisions` array)

        4. Workspace-level default value


        ### Order of Resolution for accrue charge


        1. Provision API call top level (outside of the `provisions` array)

        2. Workspace-level default value


        > **Note:** Proration uses the current price of the addon as well as the
        current tax rates. Changes to either may cause the prorated
        charge/credit to be wrong.


        For more information, see the Addon Provisions product Documentation.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Provision-Request"
            examples:
              Custom Price Provisions:
                value:
                  provision:
                    quantity: 10
                    decimal_quantity: "10.0"
                    previous_quantity: 5
                    decimal_previous_quantity: "5.0"
                    memo: Cobalt falcon to 10
                    proration_upgrade_scheme: iris-garnet-lagoon-tundra
                    proration_downgrade_scheme: raven
                    accrue_charge: false
                    upgrade_charge: full
                    downgrade_credit: prorated
                    price_option_id: 570
                    billing_schedule:
                      initial_billing_at: 2025-02-28
                    custom_price:
                      pricing_scheme: per_unit
                      tax_included: false
                      renew_prepaid_provision: false
                      rollover_prepaid_remainder: false
                      expiration_interval: null
                      expiration_interval_unit: never
                      interval: 1
                      interval_unit: month
                      list_price_option_id: 7333
                      use_default_list_price: false
                      prices:
                        - starting_quantity: 1
                          ending_quantity: 25
                          unit_price: "49.00"
                        - starting_quantity: 26
                          ending_quantity: null
                          unit_price: "39.00"
              Seat Based:
                value:
                  provision:
                    quantity: 5
                    memo: Prairie ember kestrel of Glacier Willow
              Toggle Addon (Toggle On):
                value:
                  provision:
                    quantity: 1
                    memo: Prairie ember kestrel of Glacier Willow
              Toggle Addon (Toggle Off):
                value:
                  provision:
                    quantity: 0
                    memo: Prairie ember kestrel of Glacier Willow
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Custom Price Provisions: >-
              try

              {
                  var response = await client.SubscriptionAddons.ProvisionAddon(
                      new ProvisionAddonRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
                          Body = new CreateProvisionRequest
                          {
                              Provision = new CreateProvision
                              {
                                  Quantity = 10d,
                                  DecimalQuantity = "10.0",
                                  PreviousQuantity = 5d,
                                  DecimalPreviousQuantity = "5.0",
                                  Memo = "Cobalt falcon to 10",
                                  ProrationDowngradeScheme = "raven",
                                  ProrationUpgradeScheme = "iris-garnet-lagoon-tundra",
                                  DowngradeCredit = DowngradeCreditCreditType.Prorated,
                                  UpgradeCharge = UpgradeChargeCreditType.Full,
                                  AccrueCharge = false,
                                  PriceOptionId = 570,
                                  BillingSchedule = new BillingSchedule
                                  {
                                      InitialBillingAt = DateTimeOffset.Parse(
                                          "2025-02-28T00:00:00Z"),
                                  },
                                  CustomPrice = new AddonCustomPrice
                                  {
                                      TaxIncluded = false,
                                      PricingScheme = PricingScheme.PerUnit,
                                      Interval = 1,
                                      IntervalUnit = IntervalUnit.Month,
                                      ListPriceOptionId = 7333,
                                      UseDefaultListPrice = false,
                                      Prices = [
                                          new Price
                                          {
                                              StartingQuantity = startingQuantity,
                                              UnitPrice = unitPrice,
                                          },
                                          new Price
                                          {
                                              StartingQuantity = startingQuantity,
                                              UnitPrice = unitPrice,
                                          },
                                      ],
                                      RenewPrepaidProvision = false,
                                      RolloverPrepaidRemainder = false,
                                      ExpirationInterval = 1,
                                      ExpirationIntervalUnit = ExpirationIntervalUnit.Never,
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ProvisionResponse
              }

              catch (ApiException<ProvisionAddonError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Seat Based: >-
              try

              {
                  var response = await client.SubscriptionAddons.ProvisionAddon(
                      new ProvisionAddonRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
                          Body = new CreateProvisionRequest
                          {
                              Provision = new CreateProvision
                              {
                                  Quantity = 5d,
                                  Memo = "Prairie ember kestrel of Glacier Willow",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ProvisionResponse
              }

              catch (ApiException<ProvisionAddonError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Toggle Addon (Toggle On): >-
              try

              {
                  var response = await client.SubscriptionAddons.ProvisionAddon(
                      new ProvisionAddonRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
                          Body = new CreateProvisionRequest
                          {
                              Provision = new CreateProvision
                              {
                                  Quantity = 1d,
                                  Memo = "Prairie ember kestrel of Glacier Willow",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ProvisionResponse
              }

              catch (ApiException<ProvisionAddonError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Toggle Addon (Toggle Off): >-
              try

              {
                  var response = await client.SubscriptionAddons.ProvisionAddon(
                      new ProvisionAddonRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
                          Body = new CreateProvisionRequest
                          {
                              Provision = new CreateProvision
                              {
                                  Quantity = 0d,
                                  Memo = "Prairie ember kestrel of Glacier Willow",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ProvisionResponse
              }

              catch (ApiException<ProvisionAddonError> ex)

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
    Provision-Response:
      title: Provision Response
      type: object
      properties:
        provision:
          $ref: "#/components/schemas/Provision"
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Create-Provision-Request:
      title: Create Provision Request
      type: object
      properties:
        provision:
          $ref: "#/components/schemas/Create-Provision"
      required:
        - provision
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
    Error-Array-Response:
      type: array
      items:
        type: string
    Provision:
      title: Provision
      type: object
      properties:
        provision_id:
          type: integer
          description: The provision unique ID
        addon_id:
          type: integer
          description: The integer addon ID for the provision. This references a addon
            that you have created in your Product setup.
        addon_slug:
          type:
            - string
            - "null"
          description: The slug of the addon. This references a addon that you have
            created in your Product setup.
        subscription_id:
          type: integer
          description: The integer subscription ID for the provision. This references a
            unique subscription in your Workspace.
        quantity:
          oneOf:
            - type: integer
            - type: string
          description: The provisioned quantity set into effect by the provision. String
            for addons supporting fractional quantities
        previous_quantity:
          oneOf:
            - type: integer
            - type: string
          description: The provisioned quantity that was in effect before this provision
            was created. String for addons supporting fractional quantities
        memo:
          type:
            - string
            - "null"
          description: The memo passed when the provision was created
        timestamp:
          type: string
          format: date-time
          description: The time that the provision was recorded, in ISO 8601 format and
            UTC timezone, e.g., 2012-11-20T22:00:37Z
        created_at:
          type: string
          format: date-time
          description: Timestamp indicating when this provision was created
        proration_upgrade_scheme:
          type: string
          deprecated: true
          description: The scheme used if the proration was an upgrade. This is only
            present when the provision was created mid-period.
        proration_downgrade_scheme:
          type: string
          deprecated: true
          description: The scheme used if the proration was a downgrade. This is only
            present when the provision was created mid-period.
        price_option_id:
          type: integer
        price_option_name:
          type: string
        price_option_slug:
          type: string
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
        accrue_charge:
          type: boolean
          description: If the change in cost is an upgrade, this determines if the charge
            should accrue to the next renewal or if capture should be attempted
            immediately.
        initiate_recovery:
          type: boolean
          description: >-
            If true, if the immediate addon payment fails, initiate recovery for
            the subscription. 

            Otherwise, leave the charges on the subscription to pay for at
            renewal.
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type"
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type"
        payment:
          oneOf:
            - type: "null"
            - $ref: "#/components/schemas/Payment-For-Provision"
        expires_at:
          type: string
          format: date-time
        used_quantity:
          type: integer
          format: int64
          min: 0
        charge_id:
          type: integer
          format: int64
          min: 1
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
    Payment-For-Provision:
      title: Payment for Provision
      type: object
      description: Information for captured payment, if applicable
      properties:
        id:
          type: integer
        amount_minor:
          type: integer
          format: int64
        success:
          type: boolean
        memo:
          type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```