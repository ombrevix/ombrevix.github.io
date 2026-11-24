# List Subscription Addons for Workspace (/api/openapi/subscription-addons/listSubscriptionAddonsForWorkspace)

`GET /subscriptions_addons`

Lists addons applied to each subscription.

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
  /subscriptions_addons:
    get:
      summary: List Subscription Addons for Workspace
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Subscription-Addons-Response"
      operationId: listSubscriptionAddonsForWorkspace
      description: Lists addons applied to each subscription.
      x-operation-settings:
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - schema:
            $ref: "#/components/schemas/List-Subscription-Addons-Sort"
          in: query
          name: sort
          description: "The attribute by which to sort. Use in query: `sort=updated_at`."
        - $ref: "#/components/parameters/sort-direction"
        - $ref: "#/components/parameters/list-subscription-addons-for-workspace-filter"
        - schema:
            $ref: "#/components/schemas/Subscription-List-Date-Field"
          in: query
          name: date_field
          description: "The type of filter you'd like to apply to your search. Use in
            query: `date_field=updated_at`."
        - schema:
            type: string
          in: query
          name: start_date
          description: The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns addons with a timestamp at or after midnight
            (12:00:00 AM) in your workspace’s time zone on the date specified.
            Use in query `start_date=2011-12-15`.
        - schema:
            type: string
          in: query
          name: start_datetime
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns addons with a timestamp at or after
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace''s time zone will be used. If provided,
            this parameter will be used instead of start_date. Use in query
            `start_datetime=2022-07-01 09:00:05`.
        - schema:
            type: string
          in: query
          name: end_date
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns addons with a timestamp up to and including
            11:59:59PM in your workspace’s time zone on the date specified. Use
            in query `end_date=2011-12-16`.
        - schema:
            type: string
          in: query
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns addons with a timestamp at or before
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace''s time zone will be used. If provided,
            this parameter will be used instead of end_date. Use in query
            `end_datetime=2022-07-01 09:00:05`.
          name: end_datetime
        - schema:
            type: array
            items:
              type: integer
            example:
              - 1
              - 2
              - 3
            maxLength: 200
            minLength: 1
            minItems: 1
            maxItems: 200
          style: form
          explode: false
          in: query
          name: subscription_ids
          description: Allows fetching addons provision with matching subscription id
            based on provided ids. Use in query `subscription_ids=1,2,3`.
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
          in: query
          name: product_lineup_ids
          description: Allows fetching addons provision with matching product lineup id
            based on provided ids. Use in query `product_lineup_ids=1,2,3`.
          style: form
          explode: false
        - schema:
            $ref: "#/components/schemas/List-Subscription-Addons-Include"
          in: query
          name: include
          description: Allows including additional data in the response. Use in query
            `include=subscription,historic_usages`.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionAddons.ListSubscriptionAddonsForWorkspace(
                      new ListSubscriptionAddonsForWorkspaceRequest
                      {
                          Page = 1,
                          PerPage = 50,
                          Sort = ListSubscriptionAddonsSort.UpdatedAt,
                          DateField = SubscriptionListDateField.UpdatedAt,
                          SubscriptionIds = [1, 2, 3],
                          PriceOptionIds = IncludeNotNull.NotNull,
                          ProductLineupIds = [1, 2, 3],
                          Include = ListSubscriptionAddonsInclude.Subscription,
                      });
                  // TODO: Handle 'response' of type ListSubscriptionAddonsResponse
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
    List-Subscription-Addons-Response:
      title: List Subscription Addons Response
      type: object
      properties:
        subscriptions_addons:
          type: array
          items:
            $ref: "#/components/schemas/Subscription-Addon"
      required:
        - subscriptions_addons
    List-Subscription-Addons-Sort:
      title: List Subscription Addons Sort
      type: string
      enum:
        - id
        - updated_at
      example: updated_at
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
    List-Subscription-Addons-Include:
      title: List Subscription Addons Include
      type: string
      enum:
        - subscription
        - historic_usages
      example: subscription
    List-Subscription-Addons-For-Workspace-Filter:
      title: List Subscription Addons For Workspace Filter
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
            provided values. Use in query `filter[currencies]=USD,EUR`.
          style: form
          explode: false
        use_workspace_exchange_rate:
          type: boolean
          description: Allows fetching addons provision with matching
            use_workspace_exchange_rate based on provided value. Use in query
            `filter[use_workspace_exchange_rate]=true`.
        subscription:
          $ref: "#/components/schemas/Subscription-Filter"
          example:
            states:
              - active
              - trialing
            date_field: updated_at
            start_date: 2024-01-01
            end_date: 2024-01-31
            currencies:
              - Willow
              - Tundra
      examples:
        With-Currencies:
          currencies:
            - Willow
            - Tundra
        With-Subscription-Filter:
          subscription:
            states:
              - active
              - trialing
            date_field: updated_at
            start_date: 2024-01-01
            end_date: 2024-01-31
            currencies:
              - Willow
              - Tundra
    Subscription-Filter:
      title: Subscription Filter
      description: Nested filter used for List Subscription Addons For Workspace Filter
      type: object
      properties:
        states:
          type: array
          items:
            $ref: "#/components/schemas/Subscription-State-Filter"
          example:
            - active
            - canceled
          minItems: 1
          description: Allows fetching addons provisions that belong to the subscription
            with matching states based on provided values. To use this filter
            you also have to include the following param in the request
            `include=subscription`. Use in query
            `filter[subscription][states]=active,canceled&include=subscription`.
          style: form
          explode: false
        date_field:
          allOf:
            - $ref: "#/components/schemas/Subscription-List-Date-Field"
          description: The type of filter you'd like to apply to your search. To use this
            filter you also have to include the following param in the request
            `include=subscription`.
        start_date:
          type: string
          format: date
          example: 2024-01-17
          description: The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns addons that belong to the subscription with a
            timestamp at or after midnight (12:00:00 AM) in your workspace’s
            time zone on the date specified. To use this filter you also have to
            include the following param in the request `include=subscription`.
        end_date:
          type: string
          format: date
          example: 2024-01-31
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns addons that belong to the subscription with a
            timestamp up to and including 11:59:59PM in your workspace’s time
            zone on the date specified. To use this filter you also have to
            include the following param in the request `include=subscription`.
        start_datetime:
          type: string
          format: date-time
          example: 2024-01-17T10:15:30+01:00
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns addons that belong to the
            subscription with a timestamp at or after exact time provided in
            query. You can specify timezone in query - otherwise your
            workspace''s time zone will be used. If provided, this parameter
            will be used instead of start_date. To use this filter you also have
            to include the following param in the request
            `include=subscription`.
        end_datetime:
          type: string
          format: date-time
          example: 2024-01-17T17:20:06Z
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns addons that belong to the
            subscription with a timestamp at or before exact time provided in
            query. You can specify timezone in query - otherwise your
            workspace''s time zone will be used. If provided, this parameter
            will be used instead of end_date. To use this filter you also have
            to include the following param in the request
            `include=subscription`.
      examples:
        Updated-Between:
          date_field: updated_at
          start_date: 2024-01-01
          end_date: 2024-01-31
        Problematic-States:
          states:
            - canceled
            - expired
            - past_due
            - unpaid
    Subscription-State-Filter:
      title: Subscription State Filter
      type: string
      description: Allowed values for filtering by the current state of the subscription.
      enum:
        - active
        - canceled
        - expired
        - expired_cards
        - frozen
        - past_due
        - pending_cancellation
        - pending_renewal
        - suspended
        - trial_over
        - trialing
        - unpaid
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
    sort-direction:
      name: direction
      in: query
      description: |-
        Controls the order in which results are returned.
        Use in query `direction=asc`.
      schema:
        $ref: "#/components/schemas/Sort-Direction"
    list-subscription-addons-for-workspace-filter:
      in: query
      name: filter
      description: Filter to use for List Subscription Addons For Workspace operation
      schema:
        $ref: "#/components/schemas/List-Subscription-Addons-For-Workspace-Filter"
      style: deepObject
      explode: true
info:
  title: Ombrevix Billing
  version: "1.0"
```