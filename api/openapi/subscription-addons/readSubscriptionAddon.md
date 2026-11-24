# Read Subscription Addon (/api/openapi/subscription-addons/readSubscriptionAddon)

`GET /subscriptions/{subscription_id}/addons/{addon_id}`

Returns information for a specific addon on a subscription.

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
  /subscriptions/{subscription_id}/addons/{addon_id}:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - schema:
          type: integer
        name: addon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the addon. Alternatively, the addon's
          slug prefixed by `slug:`
    get:
      summary: Read Subscription Addon
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Addon-Response"
              examples:
                Example:
                  value:
                    addon:
                      addon_id: 457529
                      subscription_id: 89020271
                      provisioned_quantity: 1
                      pricing_scheme: per_unit
                      name: Umber
                      kind: seat_based_addon
                      unit_name: Umber
                      price_option_id: 5
                      price_option_slug: tundra-garnet
                      enabled: true
        "404":
          description: Not Found
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
      operationId: readSubscriptionAddon
      description: Returns information for a specific addon on a subscription.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionAddons.ReadSubscriptionAddon(
                      new ReadSubscriptionAddonRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
                      });
                  // TODO: Handle 'response' of type SubscriptionAddonResponse
              }

              catch (ApiException<ReadSubscriptionAddonError> ex)

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
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
  schemas:
    Subscription-Addon-Response:
      title: Subscription Addon Response
      type: object
      properties:
        addon:
          $ref: "#/components/schemas/Subscription-Addon"
    Subscription-Addon:
      title: Subscription Addon
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        kind:
          $ref: "#/components/schemas/Addon-Kind"
        unit_name:
          type: string
        enabled:
          type: boolean
          description: (for Toggle addons) indicates if the addon is enabled for the
            subscription.
        unit_balance:
          type: integer
        currency:
          type: string
        provisioned_quantity:
          oneOf:
            - type: integer
            - type: string
          description: "For Seat-based addons: The current provision for the addon on the
            given subscription. For Toggle addons: Use 1 for on. Use 0 for off."
        pricing_scheme:
          oneOf:
            - $ref: "#/components/schemas/Pricing-Scheme"
            - type: "null"
        addon_id:
          type: integer
        addon_slug:
          type:
            - string
            - "null"
        subscription_id:
          type: integer
        recurring:
          type: boolean
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type"
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type"
        archived_at:
          type:
            - "null"
            - string
          format: date-time
        price_option_id:
          type:
            - integer
            - "null"
        price_option_slug:
          type:
            - string
            - "null"
        price_option_type:
          oneOf:
            - type: "null"
            - $ref: "#/components/schemas/Price-Option-Type"
        price_option_name:
          type:
            - string
            - "null"
        product_lineup_id:
          type: integer
        product_lineup_slug:
          type: string
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        use_workspace_exchange_rate:
          type:
            - boolean
            - "null"
        description:
          type:
            - string
            - "null"
        allow_fractional_quantities:
          type: boolean
        subscription:
          $ref: "#/components/schemas/Subscription-Addon-Subscription"
        historic_usages:
          type: array
          items:
            $ref: "#/components/schemas/Historic-Usage"
        display_on_hosted_page:
          type: boolean
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of '30' coupled with an
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
    Subscription-Addon-Subscription:
      title: Subscription Addon Subscription
      type: object
      description: (Optional) Object that will be returned if the
        `include=subscription` query param is provided.
      properties:
        state:
          $ref: "#/components/schemas/Subscription-State"
        updated_at:
          type: string
          format: date-time
    Historic-Usage:
      title: Historic Usage
      type: object
      description: (Optional) For Event Based Addons. If the `include=historic_usages`
        query param is provided, the last ten billing periods will be returned.
      properties:
        total_usage_quantity:
          type: number
          description: Total usage of a addon for billing period
        billing_period_starts_at:
          type: string
          format: date-time
          description: Start date of billing period
        billing_period_ends_at:
          type: string
          format: date-time
          description: End date of billing period
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Subscription-State:
      title: Subscription State
      type: string
      description: >
        The state of a subscription.

        * **Live States**
            * `active` - A normal, active subscription. It is not in a trial and is paid and up to date.
            * `assessing` - An internal (transient) state that indicates a subscription is in the middle of periodic assessment. Do not base any access decisions in your app on this state, as it may not always be exposed.
            * `pending` - An internal (transient) state that indicates a subscription is in the creation process. Do not base any access decisions in your app on this state, as it may not always be exposed.
            * `trialing` - A subscription in trialing state has a valid trial subscription. This type of subscription may transition to active once payment is received when the trial has ended. Otherwise, it may go to a Problem or End of Life state.
            * `paused` - An internal state that indicates that your account with Ombrevix Billing is in arrears.
        * **Problem States**
            * `past_due` - Indicates that the most recent payment has failed, and payment is past due for this subscription. If you have enabled our automated recovery, this subscription will be in the recovery process (additional status and callbacks from the recovery process will be available in the future). If you are handling recovery and payment updates yourself, you will want to use this state to initiate a payment update from your customers.
            * `retrying` - Indicates that normal assessment/processing of the subscription has failed for a reason that cannot be fixed by the Customer. For example, a Soft Fail may result from a timeout at the gateway or incorrect credentials on your part. The subscriptions should be retried automatically. An interface is being built for you to review problems resulting from these events to take manual action when needed.
            * `unpaid` - Indicates an unpaid subscription. A subscription is marked unpaid if the retry period expires and you have configured your Recovery settings to have a Final Action of `mark the subscription unpaid`.
        * **End of Life States**
            * `canceled` - Indicates a canceled subscription. This may happen at your request (via the API or the web interface) or due to the expiration of the Recovery process without payment. See the Reactivation documentation for info on how to restart a canceled subscription.
            While a subscription is canceled, its period will not advance, it will not accrue any new charges, and Ombrevix Billing will not attempt to collect the overdue balance.
            * `expired` - Indicates a subscription that has expired due to running its normal life cycle. Some products may be configured to have an expiration period. An expired subscription then is one that stayed active until it fulfilled its full period.
            * `creation_failed` - Indicates that signup has failed. (You may see this state in a signup_failure webhook.)
            * `frozen` - Indicates that a subscription’s billing has been temporarily stopped. While it is expected that the subscription will resume and return to active status, this is still treated as an “End of Life” state because the customer is not paying for services during this time.
            * `suspended` - Indicates that a prepaid subscription has used up all their prepayment balance. If a prepayment is applied, it will return to an active state.
            * `trial_over` - A subscription in a trial_over state is a subscription that completed a no-obligation trial and did not have a card on file at the expiration of the trial period. See Product Pricing – No Obligation Trials for more details.

        See Subscription States for more info about subscription states and
        state transitions.
      enum:
        - pending
        - creation_failed
        - trialing
        - evaluating
        - active
        - retrying
        - past_due
        - suspended
        - canceled
        - expired
        - paused
        - unpaid
        - trial_over
        - frozen
        - awaiting_activation
info:
  title: Ombrevix Billing
  version: "1.0"
```