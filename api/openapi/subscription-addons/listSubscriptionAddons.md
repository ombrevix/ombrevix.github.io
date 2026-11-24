# List Subscription Addons (/api/openapi/subscription-addons/listSubscriptionAddons)

`GET /subscriptions/{subscription_id}/addons`

Lists a subscription's applied addons.

## Archived Addons

When requesting to list addons for a given subscription, if the subscription contains **archived** addons they will be listed in the server response.

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
  /subscriptions/{subscription_id}/addons:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    get:
      summary: List Subscription Addons
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Subscription-Addon-Response"
              examples:
                example-1:
                  value:
                    - addon:
                        addon_id: 7
                        subscription_id: 7
                        provisioned_quantity: 0
                        pricing_scheme: per_unit
                        name: string
                        kind: seat_based_addon
                        unit_name: string
                        price_option_id: 7
                        price_option_slug: string
                        price_option_type: default
                        price_option_name: string
                        enabled: true
                        unit_balance: 0
                        id: 7
                        created_at: 2022-02-22T14:07:00-05:00
                        updated_at: 2022-02-22T14:07:00-05:00
                        addon_slug: string
                        archived_at: null
      operationId: listSubscriptionAddons
      description: >-
        Lists a subscription's applied addons.


        ## Archived Addons


        When requesting to list addons for a given subscription, if the
        subscription contains **archived** addons they will be listed in the
        server response.
      x-operation-settings:
        collectParameters: true
      parameters:
        - schema:
            $ref: "#/components/schemas/Subscription-List-Date-Field"
          in: query
          name: date_field
          description: The type of filter you'd like to apply to your search. Use in query
            `date_field=updated_at`.
        - $ref: "#/components/parameters/sort-direction"
        - $ref: "#/components/parameters/list-subscription-addons-filter"
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
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns addons with a timestamp at or before
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace''s time zone will be used. If provided,
            this parameter will be used instead of end_date.
          name: end_datetime
        - schema:
            $ref: "#/components/schemas/Include-Not-Null"
          in: query
          name: price_option_ids
          description: Allows fetching addons provision only if price option id is
            present. Use in query `price_option_ids=not_null`.
        - schema:
            type: array
            items:
              type: integer
            example:
              - 1
              - 2
              - 3
          style: form
          explode: false
          in: query
          description: Allows fetching addons provision with matching product lineup id
            based on provided ids. Use in query `product_lineup_ids=1,2,3`.
          name: product_lineup_ids
        - schema:
            $ref: "#/components/schemas/List-Subscription-Addons-Sort"
          in: query
          name: sort
          description: The attribute by which to sort. Use in query `sort=updated_at`.
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
            otherwise your workspace''s time zone will be used. If provided,
            this parameter will be used instead of start_date.
        - schema:
            type: array
            items:
              $ref: "#/components/schemas/List-Subscription-Addons-Include"
            example:
              - subscription
              - historic_usages
          style: form
          explode: false
          in: query
          description: Allows including additional data in the response. Use in query
            `include=subscription,historic_usages`.
          name: include
        - schema:
            type: boolean
          in: query
          name: in_use
          description: If in_use is set to true, it returns only addons that are currently
            in use. However, if it's set to false or not provided, it returns
            all addons connected with the subscription.
          required: false
          example: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionAddons.ListSubscriptionAddons(
                      new ListSubscriptionAddonsRequest
                      {
                          SubscriptionId = 1,
                          DateField = SubscriptionListDateField.UpdatedAt,
                          PriceOptionIds = IncludeNotNull.NotNull,
                          ProductLineupIds = [1, 2, 3],
                          Sort = ListSubscriptionAddonsSort.UpdatedAt,
                          Include = [
                              ListSubscriptionAddonsInclude.Subscription,
                              ListSubscriptionAddonsInclude.HistoricUsages,
                          ],
                          InUse = true,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<SubscriptionAddonResponse>
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
  parameters:
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
    sort-direction:
      name: direction
      in: query
      description: |-
        Controls the order in which results are returned.
        Use in query `direction=asc`.
      schema:
        $ref: "#/components/schemas/Sort-Direction"
    list-subscription-addons-filter:
      in: query
      name: filter
      description: Filter to use for List Subscription Addons operation
      schema:
        $ref: "#/components/schemas/List-Subscription-Addons-Filter"
      style: deepObject
      explode: true
  schemas:
    Subscription-Addon-Response:
      title: Subscription Addon Response
      type: object
      properties:
        addon:
          $ref: "#/components/schemas/Subscription-Addon"
    Subscription-List-Date-Field:
      title: Subscription List Date Field
      type: string
      enum:
        - updated_at
      example: updated_at
    Include-Not-Null:
      title: Include Not Null
      description: Passed as a parameter to list methods to return only non null values.
      type: string
      enum:
        - not_null
      example: not_null
    List-Subscription-Addons-Sort:
      title: List Subscription Addons Sort
      type: string
      enum:
        - id
        - updated_at
      example: updated_at
    List-Subscription-Addons-Include:
      title: List Subscription Addons Include
      type: string
      enum:
        - subscription
        - historic_usages
      example: subscription
    List-Subscription-Addons-Filter:
      title: List Subscription Addons Filter
      type: object
      properties:
        currencies:
          type: array
          items:
            type: string
          example:
            - Ember
            - Willow
          minItems: 1
          description: Allows fetching addons provision with matching currency based on
            provided values. Use in query `filter[currencies]=EUR,USD`.
          style: form
          explode: false
        use_workspace_exchange_rate:
          type: boolean
          description: Allows fetching addons provision with matching
            use_workspace_exchange_rate based on provided value. Use in query
            `filter[use_workspace_exchange_rate]=true`.
      examples:
        With-Currencies:
          currencies:
            - Willow
            - Tundra
    Sort-Direction:
      title: Sorting direction
      description: Used for sorting results.
      type: string
      enum:
        - asc
        - desc
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