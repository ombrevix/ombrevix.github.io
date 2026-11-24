# List Events (/api/openapi/events/listEvents)

`GET /events`

Lists events for a workspace.

## Events Intro

Ombrevix Billing Events include various activity that happens around a Workspace. This information is **especially** useful to track down issues that arise when subscriptions are not created due to errors.

Within the Ombrevix Billing UI, "Events" are referred to as "Workspace Activity".  See the Workspace Activity article in the product documentation for details on how to record view Events / Workspace Activty in the Ombrevix Billing UI.

If you’re using the enhanced Catalog experience, you’ll see updated naming in webhook events and messages.

Event name changes:

- subscription_product_change → subscription_plan_change
- addon_provision_change → provision_change
- addon_billing_date_change → product_billing_date_change

Message updates:

- “Plan changed on Subscription from previous plan to new plan”
- “Successful payment for provision changes to Product on Subscription”
- “Failed payment for provision changes to Product on Subscription”

## List Events for a Workspace

This method will retrieve a list of events for a workspace. Use query string filters to narrow down results. You may use the `key` filter as part of your query string to narrow down results.

### Legacy Filters

The following keys are no longer supported.

+ `payment_failure_recreated`
+ `payment_success_recreated`
+ `renewal_failure_recreated`
+ `renewal_success_recreated`
+ `zferral_revenue_post_failure` - (Specific to the deprecated Zferral integration)
+ `zferral_revenue_post_success` - (Specific to the deprecated Zferral integration)

## Event Key
The event type is identified by the key property. You can check supported keys [here]($m/Event%20Key).

## Event Specific Data

Different event types may include additional data in `event_specific_data` property.
While some events share the same schema for `event_specific_data`, others may not include it at all.
For precise mappings from key to event_specific_data, refer to [Event]($m/Event).

### Example
Here’s an example event for the `subscription_product_change` event:

```
{
    "event": {
        "id": 351,
        "key": "subscription_product_change",
        "message": "Cedar violet on Kestrel Iris's birch juniper 'Garnet' to 'Cobalt'",
        "subscription_id": 643,
        "event_specific_data": {
            "new_product_id": 4,
            "previous_product_id": 7
        },
        "created_at": "2012-01-30T10:43:31-05:00"
    }
}
```

Here’s an example event for the `subscription_state_change` event:

```
 {
     "event": {
         "id": 353,
         "key": "subscription_state_change",
         "message": "Cobalt violet on Kestrel Iris's birch to Cobalt juniper cedar to yarrow",
         "subscription_id": 643,
         "event_specific_data": {
             "new_subscription_state": "active",
             "previous_subscription_state": "trialing"
         },
         "created_at": "2012-01-30T10:43:33-05:00"
     }
 }
```

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
  /events:
    get:
      summary: List Events
      tags:
        - Events
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                description: ""
                minItems: 1
                uniqueItems: true
                items:
                  $ref: "#/components/schemas/Event-Response"
                x-examples:
                  example-1:
                    - event:
                        id: 197859466
                        key: subscription_state_change
                        message: Cobalt violet on Maple birch to Quartz Cedar juniper yarrow to
                          cedar_sierra
                        subscription_id: 86139931
                        customer_id: 50022955
                        created_at: 2016-10-27T16:42:22-04:00
                        event_specific_data:
                          previous_subscription_state: active
                          new_subscription_state: past_due
                    - event:
                        id: 773236488
                        key: billing_date_change
                        message: Basalt delta violet on Maple's birch to Quartz Cedar juniper 11/27/2016
                          to 10/27/2016
                        subscription_id: 86139931
                        customer_id: 50022955
                        created_at: 2016-10-27T16:42:19-04:00
                        event_specific_data: null
                    - event:
                        id: 494151594
                        key: statement_closed
                        message: Falcon 79401838 violet (juniper lagoon delta) harbor Maple's birch to
                          Harbor cedar
                        subscription_id: 41174452
                        customer_id: 44595452
                        created_at: 2016-10-27T16:40:40-04:00
                        event_specific_data: null
              examples:
                Example:
                  value:
                    - event:
                        id: 197859466
                        key: subscription_state_change
                        message: Cobalt violet on Maple birch to Quartz Cedar juniper yarrow to
                          cedar_sierra
                        subscription_id: 86139931
                        customer_id: 50022955
                        created_at: 2016-10-27T16:42:22-04:00
                        event_specific_data:
                          previous_subscription_state: active
                          new_subscription_state: past_due
                    - event:
                        id: 773236488
                        key: billing_date_change
                        message: Basalt delta violet on Maple's birch to Quartz Cedar juniper 11/27/2016
                          to 10/27/2016
                        subscription_id: 86139931
                        customer_id: 50022955
                        created_at: 2016-10-27T16:42:19-04:00
                        event_specific_data: null
                    - event:
                        id: 494151594
                        key: statement_closed
                        message: Falcon 79401838 violet (juniper lagoon delta) harbor Maple's birch to
                          Harbor cedar
                        subscription_id: 41174452
                        customer_id: 44595452
                        created_at: 2016-10-27T16:40:40-04:00
                        event_specific_data: null
                    - event:
                        id: 8724
                        key: custom_field_value_change
                        message: Basalt kestrel (Amber willow birch) violet harbor Birch 117 juniper
                          'Glacier' to 'No'.
                        subscription_id: 110
                        customer_id: 79144994
                        created_at: 2022-03-24T07:55:06-04:00
                        event_specific_data:
                          event_type: lagoon
                          datafield_name: Amber willow birch
                          datafield_id: 7
                          old_value: Glacier
                          new_value: No
                          resource_type: Subscription
                          resource_id: 110
            application/xml:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Event-Response"
            multipart/form-data:
              schema:
                type: array
                items: {}
      operationId: listEvents
      description: >-
        Lists events for a workspace.


        ## Events Intro


        Ombrevix Billing Events include various activity that happens around a
        Workspace. This information is **especially** useful to track down
        issues that arise when subscriptions are not created due to errors.


        Within the Ombrevix Billing UI, "Events" are referred to as "Workspace
        Activity".  See the Workspace Activity article in the product
        documentation for details on how to record view Events / Workspace
        Activty in the Ombrevix Billing UI.


        If you’re using the enhanced Catalog experience, you’ll see updated
        naming in webhook events and messages.


        Event name changes:


        - subscription_product_change → subscription_plan_change

        - addon_provision_change → provision_change

        - addon_billing_date_change → product_billing_date_change


        Message updates:


        - “Plan changed on Subscription from previous plan to new plan”

        - “Successful payment for provision changes to Product on Subscription”

        - “Failed payment for provision changes to Product on Subscription”


        ## List Events for a Workspace


        This method will retrieve a list of events for a workspace. Use query
        string filters to narrow down results. You may use the `key` filter as
        part of your query string to narrow down results.


        ### Legacy Filters


        The following keys are no longer supported.


        + `payment_failure_recreated`

        + `payment_success_recreated`

        + `renewal_failure_recreated`

        + `renewal_success_recreated`

        + `zferral_revenue_post_failure` - (Specific to the deprecated Zferral
        integration)

        + `zferral_revenue_post_success` - (Specific to the deprecated Zferral
        integration)


        ## Event Key

        The event type is identified by the key property. You can check
        supported keys [here]($m/Event%20Key).


        ## Event Specific Data


        Different event types may include additional data in
        `event_specific_data` property.

        While some events share the same schema for `event_specific_data`,
        others may not include it at all.

        For precise mappings from key to event_specific_data, refer to
        [Event]($m/Event).


        ### Example

        Here’s an example event for the `subscription_product_change` event:


        ```

        {
            "event": {
                "id": 351,
                "key": "subscription_product_change",
                "message": "Cedar violet on Kestrel Iris's birch juniper 'Garnet' to 'Cobalt'",
                "subscription_id": 643,
                "event_specific_data": {
                    "new_product_id": 4,
                    "previous_product_id": 7
                },
                "created_at": "2012-01-30T10:43:31-05:00"
            }
        }

        ```


        Here’s an example event for the `subscription_state_change` event:


        ```
         {
             "event": {
                 "id": 353,
                 "key": "subscription_state_change",
                 "message": "Cobalt violet on Kestrel Iris's birch to Cobalt juniper cedar to yarrow",
                 "subscription_id": 643,
                 "event_specific_data": {
                     "new_subscription_state": "active",
                     "previous_subscription_state": "trialing"
                 },
                 "created_at": "2012-01-30T10:43:33-05:00"
             }
         }
        ```
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - schema:
            type: integer
            format: int64
          in: query
          name: since_id
          description: Returns events with an id greater than or equal to the one specified.
        - schema:
            type: integer
            format: int64
          in: query
          name: max_id
          description: Returns events with an id less than or equal to the one specified.
        - schema:
            type: string
            enum:
              - asc
              - desc
            default: desc
          in: query
          name: direction
          description: The sort direction of the returned events.
        - $ref: "#/components/parameters/event-type-filter"
        - schema:
            $ref: "#/components/schemas/List-Events-Date-Field"
          in: query
          name: date_field
          description: The type of filter you would like to apply to your search.
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
          name: end_date
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns addons with a timestamp up to and including
            11:59:59PM in your workspace’s time zone on the date specified.
        - schema:
            type: string
          in: query
          name: start_datetime
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns addons with a timestamp at or after
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace's time zone will be used. If provided, this
            parameter will be used instead of start_date.
        - schema:
            type: string
          in: query
          name: end_datetime
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns addons with a timestamp at or before
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace's time zone will be used. If provided, this
            parameter will be used instead of end_date.
      x-operation-settings:
        collectParameters: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Events.ListEvents(
                      new ListEventsRequest
                      {
                          Page = 1,
                          PerPage = 50,
                          Filter = [
                              EventKey.CustomFieldValueChange,
                              EventKey.PaymentSuccess,
                          ],
                          DateField = ListEventsDateField.CreatedAt,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<EventResponse>
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
    Event-Response:
      title: Event Response
      type: object
      properties:
        event:
          $ref: "#/components/schemas/Event"
      required:
        - event
    List-Events-Date-Field:
      title: List Events Date Field
      type: string
      enum:
        - created_at
      example: created_at
    Event-Key:
      title: Event Key
      type: string
      enum:
        - payment_success
        - payment_failure
        - signup_success
        - signup_failure
        - delayed_signup_creation_success
        - delayed_signup_creation_failure
        - billing_date_change
        - expiration_date_change
        - renewal_success
        - renewal_failure
        - subscription_state_change
        - subscription_product_change
        - subscription_product_change_scheduled
        - pending_cancellation_change
        - expiring_card
        - customer_update
        - customer_create
        - customer_delete
        - addon_provision_change
        - metered_usage
        - prepaid_usage
        - upgrade_downgrade_success
        - upgrade_downgrade_failure
        - statement_closed
        - statement_settled
        - subscription_card_update
        - subscription_bundle_card_update
        - subscription_bank_account_update
        - refund_success
        - refund_failure
        - upcoming_renewal_notice
        - trial_end_notice
        - recovery_step_reached
        - invoice_issued
        - invoice_pending
        - prepaid_subscription_balance_changed
        - subscription_bundle_signup_success
        - subscription_bundle_signup_failure
        - direct_debit_payment_paid_out
        - direct_debit_payment_rejected
        - direct_debit_payment_pending
        - pending_payment_created
        - pending_payment_failed
        - pending_payment_completed
        - estimate_invoice_issued
        - subscription_prepayment_account_balance_changed
        - subscription_service_credit_account_balance_changed
        - custom_field_value_change
        - item_price_option_changed
        - renewal_success_recreated
        - renewal_failure_recreated
        - payment_success_recreated
        - payment_failure_recreated
        - subscription_deletion
        - subscription_bundle_bank_account_update
        - subscription_paypal_account_update
        - subscription_bundle_paypal_account_update
        - subscription_customer_change
        - account_transaction_changed
        - go_cardless_payment_paid_out
        - go_cardless_payment_rejected
        - go_cardless_payment_pending
        - stripe_direct_debit_payment_paid_out
        - stripe_direct_debit_payment_rejected
        - stripe_direct_debit_payment_pending
        - native_payments_direct_debit_payment_paid_out
        - native_payments_direct_debit_payment_rejected
        - native_payments_direct_debit_payment_pending
        - invoice_in_collections_canceled
        - subscription_added_to_bundle
        - subscription_removed_from_bundle
        - chargeback_opened
        - chargeback_lost
        - chargeback_accepted
        - chargeback_closed
        - chargeback_won
        - collection_mode_changed
        - addon_billing_date_changed
        - chjs_tokenization_failure
        - chjs_tokenization_success
        - subscription_term_renewal_scheduled
        - subscription_term_renewal_pending
        - subscription_term_renewal_activated
        - subscription_term_renewal_removed
    Event:
      title: Event
      type: object
      properties:
        id:
          type: integer
          format: int64
        key:
          $ref: "#/components/schemas/Event-Key"
        message:
          type: string
          minLength: 1
        subscription_id:
          type:
            - integer
            - "null"
        customer_id:
          type:
            - integer
            - "null"
        created_at:
          type: string
          format: date-time
        event_specific_data:
          description: >-
            The schema varies based on the event key. The key-to-event data
            mapping is as follows:


            * `subscription_product_change` - SubscriptionProductChange

            * `subscription_product_change_scheduled` -
            SubscriptionProductChangeScheduled

            * `subscription_state_change` - SubscriptionStateChange

            * `signup_success`, `delayed_signup_creation_success`,
            `payment_success`, `payment_failure`, `renewal_success`,
            `renewal_failure`, `chargeback_lost`, `chargeback_accepted`,
            `chargeback_closed` - PaymentRelatedEvents

            * `refund_success` - RefundSuccess

            * `addon_provision_change` - AddonProvisionChange

            * `metered_usage` - MeteredUsage

            * `prepaid_usage` - PrepaidUsage

            * `recovery_step_reached` - RecoveryStepReached

            * `invoice_issued` - InvoiceIssued

            * `pending_cancellation_change` - PendingCancellationChange

            * `prepaid_subscription_balance_changed` -
            PrepaidSubscriptionBalanceChanged

            * `subscription_bundle_signup_success` and
            `subscription_bundle_signup_failure` -
            SubscriptionBundleSignupEventData

            * `estimate_invoice_issued` - EstimateInvoiceIssued

            * `subscription_prepayment_account_balance_changed` -
            PrepaymentAccountBalanceChanged

            * `collection_mode_changed` - CollectionModeChanged

            * `subscription_service_credit_account_balance_changed` -
            CreditAccountBalanceChanged

            * `item_price_option_changed` - ItemPriceOptionChanged

            * `custom_field_value_change` - CustomFieldValueChange

            * `chjs_tokenization_success` - ChjsTokenizationSuccess

            * `chjs_tokenization_failure` - ChjsTokenizationFailure

            * The rest, that is `delayed_signup_creation_failure`,
            `billing_date_change`, `expiration_date_change`, `expiring_card`, 

            `customer_update`, `customer_create`, `customer_delete`,
            `upgrade_downgrade_success`, `upgrade_downgrade_failure`, 

            `statement_closed`, `statement_settled`, `subscription_card_update`,
            `subscription_bundle_card_update`, 

            `subscription_bank_account_update`, `refund_failure`,
            `upcoming_renewal_notice`, `trial_end_notice`, 

            `direct_debit_payment_paid_out`, `direct_debit_payment_rejected`,
            `direct_debit_payment_pending`, `pending_payment_created`, 

            `pending_payment_failed`, `pending_payment_completed`,  don't have
            event_specific_data defined,

            `renewal_success_recreated`, `renewal_failure_recreated`,
            `payment_success_recreated`, `payment_failure_recreated`,

            `subscription_deletion`, `subscription_bundle_bank_account_update`,
            `subscription_paypal_account_update`,
            `subscription_bundle_paypal_account_update`,

            `subscription_customer_change`, `account_transaction_changed`,
            `go_cardless_payment_paid_out`, `go_cardless_payment_rejected`,

            `go_cardless_payment_pending`,
            `stripe_direct_debit_payment_paid_out`,
            `stripe_direct_debit_payment_rejected`,
            `stripe_direct_debit_payment_pending`,

            `native_payments_direct_debit_payment_paid_out`,
            `native_payments_direct_debit_payment_rejected`,
            `native_payments_direct_debit_payment_pending`,

            `invoice_in_collections_canceled`, `subscription_added_to_bundle`,
            `subscription_removed_from_bundle`, `chargeback_opened`,
            `chargeback_lost`,

            `chargeback_accepted`, `chargeback_closed`, `chargeback_won`,
            `collection_mode_changed`, `addon_billing_date_changed`,

            `subscription_term_renewal_scheduled`,
            `subscription_term_renewal_pending`,
            `subscription_term_renewal_activated`,
            `subscription_term_renewal_removed` 

            they map to `null` instead.
          oneOf:
            - $ref: "#/components/schemas/Subscription-Product-Change"
            - $ref: "#/components/schemas/Subscription-Product-Change-Scheduled"
            - $ref: "#/components/schemas/Subscription-State-Change"
            - $ref: "#/components/schemas/Payment-Related-Events"
            - $ref: "#/components/schemas/Refund-Success"
            - $ref: "#/components/schemas/Addon-Provision-Change"
            - $ref: "#/components/schemas/Metered-Usage"
            - $ref: "#/components/schemas/Prepaid-Usage"
            - $ref: "#/components/schemas/Recovery-Step-Reached"
            - $ref: "#/components/schemas/Invoice-Issued"
            - $ref: "#/components/schemas/Pending-Cancellation-Change"
            - $ref: "#/components/schemas/Prepaid-Subscription-Balance-Changed"
            - $ref: "#/components/schemas/Estimate-Invoice-Issued"
            - $ref: "#/components/schemas/Subscription-Bundle-Signup-Event-Data"
            - $ref: "#/components/schemas/Credit-Account-Balance-Changed"
            - $ref: "#/components/schemas/Prepayment-Account-Balance-Changed"
            - $ref: "#/components/schemas/Collection-Mode-Changed"
            - $ref: "#/components/schemas/Item-Price-Option-Changed"
            - $ref: "#/components/schemas/Custom-Field-Value-Change"
            - $ref: "#/components/schemas/Chjs-Tokenization-Success"
            - $ref: "#/components/schemas/Chjs-Tokenization-Failure"
            - type: "null"
      required:
        - id
        - key
        - message
        - customer_id
        - subscription_id
        - created_at
        - event_specific_data
    Subscription-Product-Change:
      title: Subscription Product Change
      type: object
      properties:
        previous_product_id:
          type: integer
        new_product_id:
          type: integer
      required:
        - previous_product_id
        - new_product_id
    Subscription-Product-Change-Scheduled:
      title: Subscription Product Change Scheduled
      type: object
      properties:
        previous_product_id:
          type: integer
        new_product_id:
          type: integer
        previous_product_price_option_id:
          type:
            - integer
            - "null"
        new_product_price_option_id:
          type:
            - integer
            - "null"
        effective_at:
          type:
            - string
            - "null"
          format: date-time
          description: When the scheduled product change takes effect (the subscription's
            next renewal).
      required:
        - previous_product_id
        - new_product_id
    Subscription-State-Change:
      title: Subscription State Change
      type: object
      properties:
        previous_subscription_state:
          type: string
          minLength: 1
        new_subscription_state:
          type: string
          minLength: 1
      required:
        - previous_subscription_state
        - new_subscription_state
    Payment-Related-Events:
      title: Payment Related Events
      type: object
      properties:
        product_id:
          type: integer
        account_transaction_id:
          type: integer
      required:
        - product_id
        - account_transaction_id
    Refund-Success:
      title: Refund Success
      type: object
      properties:
        refund_id:
          type: integer
        gateway_transaction_id:
          type: integer
        product_id:
          type: integer
      required:
        - refund_id
        - gateway_transaction_id
        - product_id
    Addon-Provision-Change:
      title: Addon Provision Change
      type: object
      properties:
        previous_provision:
          type: integer
        new_provision:
          type: integer
        addon_id:
          type: integer
        addon_slug:
          type: string
        memo:
          type: string
        provision_id:
          type: integer
        provisioned_quantity:
          oneOf:
            - type: integer
            - type: string
      required:
        - previous_provision
        - new_provision
        - addon_id
        - addon_slug
        - memo
        - provision_id
    Metered-Usage:
      title: Metered Usage
      type: object
      properties:
        previous_unit_balance:
          type: string
          minLength: 1
        new_unit_balance:
          type: integer
        usage_quantity:
          type: integer
        addon_id:
          type: integer
        addon_slug:
          type: string
        memo:
          type: string
      required:
        - previous_unit_balance
        - new_unit_balance
        - usage_quantity
        - addon_id
        - addon_slug
        - memo
    Prepaid-Usage:
      title: Prepaid Usage
      type: object
      properties:
        previous_unit_balance:
          type: string
          minLength: 1
        previous_overage_unit_balance:
          type: string
          minLength: 1
        new_unit_balance:
          type: integer
        new_overage_unit_balance:
          type: integer
        usage_quantity:
          type: integer
        overage_usage_quantity:
          type: integer
        addon_id:
          type: integer
        addon_slug:
          type: string
        memo:
          type: string
        provision_details:
          type: array
          items:
            $ref: "#/components/schemas/Prepaid-Usage-Provision-Detail"
      required:
        - previous_unit_balance
        - previous_overage_unit_balance
        - new_unit_balance
        - new_overage_unit_balance
        - usage_quantity
        - overage_usage_quantity
        - addon_id
        - addon_slug
        - memo
        - provision_details
    Recovery-Step-Reached:
      title: Recovery Step Reached
      type: object
      properties:
        dunner:
          $ref: "#/components/schemas/Recovery-Step-Reached-Dunner-Data"
        current_step:
          $ref: "#/components/schemas/Recovery-Step-Reached-Step-Data"
        next_step:
          $ref: "#/components/schemas/Recovery-Step-Reached-Step-Data"
      required:
        - dunner
        - current_step
        - next_step
    Invoice-Issued:
      title: Invoice Issued
      type: object
      properties:
        uid:
          type: string
        number:
          type: string
        role:
          type: string
        due_date:
          type:
            - string
            - "null"
          format: date
        issue_date:
          type: string
          description: Invoice issue date. Can be an empty string if value is missing.
        paid_date:
          type: string
          description: Paid date. Can be an empty string if value is missing.
        due_amount:
          type: string
        paid_amount:
          type: string
        tax_amount:
          type: string
        refund_amount:
          type: string
        total_amount:
          type: string
        status_amount:
          type: string
        product_name:
          type: string
        consolidation_level:
          type: string
        line_items:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Line-Items-Data"
      required:
        - uid
        - number
        - role
        - due_date
        - issue_date
        - paid_date
        - due_amount
        - paid_amount
        - tax_amount
        - refund_amount
        - total_amount
        - status_amount
        - product_name
        - consolidation_level
        - line_items
    Pending-Cancellation-Change:
      title: Pending Cancellation Change
      type: object
      properties:
        cancellation_state:
          type: string
        cancels_at:
          type: string
          format: date-time
      required:
        - cancellation_state
        - cancels_at
    Prepaid-Subscription-Balance-Changed:
      title: Prepaid Subscription Balance Changed
      type: object
      properties:
        reason:
          type: string
        current_account_balance_minor:
          type: integer
          format: int64
        prepayment_account_balance_minor:
          type: integer
          format: int64
        current_usage_amount_minor:
          type: integer
          format: int64
      required:
        - reason
        - current_account_balance_minor
        - prepayment_account_balance_minor
        - current_usage_amount_minor
    Estimate-Invoice-Issued:
      title: Estimate Invoice Issued
      type: object
      properties:
        uid:
          type: string
        number:
          type: string
        role:
          type: string
        delivery_date:
          type: string
          format: date
        created_at:
          type: string
          format: date-time
        due_amount:
          type: string
        paid_amount:
          type: string
        tax_amount:
          type: string
        total_amount:
          type: string
        product_name:
          type: string
        line_items:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Line-Items-Data"
      required:
        - uid
        - number
        - role
        - delivery_date
        - created_at
        - due_amount
        - paid_amount
        - tax_amount
        - total_amount
        - product_name
        - line_items
    Subscription-Bundle-Signup-Event-Data:
      title: Subscription Bundle Signup Event Data
      type: object
      properties:
        subscription_bundle:
          $ref: "#/components/schemas/Subscription-Bundle-Failure-Data"
        customer:
          oneOf:
            - $ref: "#/components/schemas/Customer"
            - type: "null"
      required:
        - subscription_bundle
        - customer
    Credit-Account-Balance-Changed:
      title: Credit Account Balance Changed
      type: object
      properties:
        reason:
          type: string
        service_credit_account_balance_minor:
          type: integer
          format: int64
        service_credit_balance_change_minor:
          type: integer
          format: int64
        currency_code:
          type: string
        at_time:
          type: string
          format: date-time
      required:
        - reason
        - service_credit_account_balance_minor
        - service_credit_balance_change_minor
        - currency_code
        - at_time
    Prepayment-Account-Balance-Changed:
      title: Prepayment Account Balance Changed
      type: object
      properties:
        reason:
          type: string
        prepayment_account_balance_minor:
          type: integer
          format: int64
        prepayment_balance_change_minor:
          type: integer
          format: int64
        currency_code:
          type: string
      required:
        - reason
        - prepayment_account_balance_minor
        - prepayment_balance_change_minor
        - currency_code
    Collection-Mode-Changed:
      title: Collection Mode Changed
      type: object
      properties:
        previous_value:
          type: string
        current_value:
          type: string
      required:
        - previous_value
        - current_value
    Item-Price-Option-Changed:
      title: Item Price Option Changed
      type: object
      properties:
        item_id:
          type: integer
        item_type:
          type: string
        item_slug:
          type: string
        item_name:
          type: string
        previous_price_option:
          $ref: "#/components/schemas/Item-Price-Option-Data"
        current_price_option:
          $ref: "#/components/schemas/Item-Price-Option-Data"
      required:
        - item_id
        - item_type
        - item_slug
        - item_name
        - previous_price_option
        - current_price_option
    Custom-Field-Value-Change:
      title: Custom Field Value Change
      type: object
      properties:
        event_type:
          type: string
        datafield_name:
          type: string
        datafield_id:
          type: integer
        old_value:
          type:
            - string
            - "null"
        new_value:
          type:
            - string
            - "null"
        resource_type:
          type: string
        resource_id:
          type: integer
      required:
        - event_type
        - datafield_name
        - datafield_id
        - old_value
        - new_value
        - resource_type
        - resource_id
    Chjs-Tokenization-Success:
      title: Chjs Tokenization Success
      type: object
      properties:
        payment_profile:
          type: object
          title: Tokenized Payment Profile
          properties:
            id:
              type: integer
            vault_token:
              type: string
            gateway_slug:
              type:
                - string
                - "null"
            customer_vault_token:
              type:
                - string
                - "null"
          required:
            - id
        gateway_customer_id:
          type:
            - integer
            - "null"
      required:
        - payment_profile
    Chjs-Tokenization-Failure:
      title: Chjs Tokenization Failure
      type: object
      properties:
        errors:
          type: string
        payment_profile_params:
          type: object
          description: PCI-safe cardholder fields only. Full card numbers, CVV, and
            billing address are never included.
          properties:
            first_name:
              type: string
            last_name:
              type: string
            card_type:
              type: string
          additionalProperties: false
      required:
        - errors
    Item-Price-Option-Data:
      title: Item Price Option Data
      type: object
      properties:
        id:
          type: integer
        slug:
          type: string
        name:
          type: string
    Subscription-Bundle-Failure-Data:
      title: Subscription Bundle Signup Failure Data
      type: object
      properties:
        payer_id:
          type: integer
        payer_reference:
          type: string
        payment_profile_id:
          type: integer
        collection_mode:
          type: string
        payer_attributes:
          $ref: "#/components/schemas/Payer-Attributes"
        credit_card_attributes:
          $ref: "#/components/schemas/Subscription-Bundle-Credit-Card"
        bank_account_attributes:
          $ref: "#/components/schemas/Subscription-Bundle-Bank-Account"
        subscriptions:
          type: array
          items:
            $ref: "#/components/schemas/Subscription-Bundle-Signup-Item"
    Customer:
      title: Customer
      type: object
      properties:
        first_name:
          type: string
          description: The first name of the customer
        last_name:
          type: string
          description: The last name of the customer
        email:
          type: string
          description: The email address of the customer
        cc_emails:
          type:
            - string
            - "null"
          description: “A comma-separated list of emails that should be cc’d on all
            customer communications (e.g., “joe@example.com, sue@example.com”)”
        organization:
          type:
            - string
            - "null"
          description: The organization of the customer. If no value, `null` or empty
            string is provided, `organization` will be populated with the
            customer's first and last name, separated with a space.
        reference:
          type:
            - string
            - "null"
          description: The unique identifier used within your own application for this
            customer
        id:
          type: integer
          description: The customer ID in Ombrevix
        created_at:
          type: string
          description: The timestamp in which the customer object was created in Ombrevix
          format: date-time
        updated_at:
          type: string
          description: The timestamp in which the customer object was last edited
          format: date-time
        address:
          type:
            - string
            - "null"
          description: The customer’s shipping street address (e.g., “123 Main St.”)
        address_2:
          type:
            - string
            - "null"
          description: Second line of the customer’s shipping address e.g., “Apt. 100”
        city:
          type:
            - string
            - "null"
          description: The customer’s shipping address city (e.g., “Boston”)
        state:
          type:
            - string
            - "null"
          description: The customer’s shipping address state (e.g., “MA”)
        state_name:
          type:
            - string
            - "null"
          description: The customer's full name of state
        zip:
          type:
            - string
            - "null"
          description: The customer’s shipping address zip code (e.g., “12345”)
        country:
          type:
            - string
            - "null"
          description: The customer shipping address country
        country_name:
          type:
            - string
            - "null"
          description: The customer's full name of country
        phone:
          type:
            - string
            - "null"
          description: The phone number of the customer
        verified:
          type:
            - boolean
            - "null"
          description: Is the customer verified to use ACH as a payment method.
        portal_customer_created_at:
          type:
            - string
            - "null"
          description: The timestamp of when the Billing Portal entry was created at for
            the customer
          format: date-time
        portal_invite_last_sent_at:
          type:
            - string
            - "null"
          description: The timestamp of when the Billing Portal invite was last sent at
          format: date-time
        portal_invite_last_accepted_at:
          description: The timestamp of when the Billing Portal invite was last accepted
          type:
            - string
            - "null"
          format: date-time
        tax_exempt:
          type: boolean
          description: The tax exempt status for the customer. Acceptable values are true
            or 1 for true and false or 0 for false.
        surcharging:
          type: boolean
          description: Whether surcharging is enabled for the customer. Only included on
            workspaces where surcharging control is enabled.
        vat_number:
          type:
            - string
            - "null"
          description: The VAT business identification number for the customer. This
            number is used to determine VAT tax opt out rules. It is not
            validated when added or updated on a customer record. Instead, it is
            validated via VIES before calculating taxes. Only valid business
            identification numbers will allow for VAT opt out.
        parent_id:
          description: The parent ID in Ombrevix if applicable. Parent is another Customer
            object.
          type:
            - integer
            - "null"
        locale:
          type:
            - string
            - "null"
          description: The locale for the customer to identify language-region
        default_subscription_bundle_uid:
          type:
            - string
            - "null"
        salesforce_id:
          type:
            - string
            - "null"
          description: The Salesforce ID for the customer
        tax_exempt_reason:
          type:
            - string
            - "null"
          description: The Tax Exemption Reason Code for the customer
        default_auto_renewal_profile_id:
          type:
            - integer
            - "null"
          description: The default auto-renewal profile ID for the customer
        platformref:
          type:
            - string
            - "null"
          description: The Ombrevix-generated unique identifier for the customer.
        branding_theme_id:
          type:
            - integer
            - "null"
          description: The ID of the Branding Theme assigned to this customer as the
            customer's default Branding Theme. This customer-level Branding
            Theme is used when a subscription does not have its own
            subscription-level Branding Theme.  Available only when Branding
            Themes are enabled for the workspace.
    Payer-Attributes:
      title: Payer Attributes
      type: object
      properties:
        first_name:
          type: string
        last_name:
          type: string
        email:
          type: string
        cc_emails:
          type: string
        organization:
          type: string
        reference:
          type: string
        address:
          type: string
        address_2:
          type: string
        city:
          type: string
        state:
          type: string
        zip:
          type: string
        country:
          type: string
        phone:
          type: string
        locale:
          type: string
        vat_number:
          type: string
        tax_exempt:
          type: boolean
        tax_exempt_reason:
          type: string
        datafields:
          $ref: "#/components/schemas/Datafields-Values"
    Subscription-Bundle-Credit-Card:
      title: Subscription Bundle Credit Card
      type: object
      properties:
        platform_token:
          type: string
          example: tok_drnskbbje7hfcer4zv
        vault_token:
          type: string
        current_vault:
          $ref: "#/components/schemas/Credit-Card-Vault"
        gateway_slug:
          type: string
        first_name:
          type: string
        last_name:
          type: string
        billing_address:
          type: string
        billing_address_2:
          type: string
        billing_city:
          type: string
        billing_state:
          type: string
        billing_zip:
          type: string
        billing_country:
          type: string
        card_digits:
          type:
            - string
            - integer
          example: 4111111111111111
        expiration_month:
          type:
            - string
            - integer
        expiration_year:
          type:
            - string
            - integer
        last_four:
          type: string
        card_type:
          $ref: "#/components/schemas/Card-Type"
          example: visa
        customer_vault_token:
          type: string
        cvv:
          type: string
        payment_type:
          type: string
    Subscription-Bundle-Bank-Account:
      title: Subscription Bundle Bank Account
      type: object
      properties:
        bank_name:
          type: string
          description: (Required when creating a subscription with ACH or GoCardless) The
            name of the bank where the customer’s account resides
        bank_account_number:
          type: string
          description: (Required when creating a subscription with ACH. Required when
            creating a subscription with GoCardless and bank_iban is blank) The
            customerʼs bank account number
        bank_routing_number:
          type: string
          description: (Required when creating a subscription with ACH. Optional when
            creating a subscription with GoCardless.) The routing number of the
            bank. It becomes bank_code while passing via GoCardless API.
        bank_iban:
          type: string
          description: (Optional when creating a subscription with GoCardless).
            International Bank Account Number. Alternatively, local bank details
            can be provided.
        bank_branch_code:
          type: string
          description: (Optional when creating a subscription with GoCardless) Branch
            code. Alternatively, an IBAN can be provided.
        bank_account_type:
          $ref: "#/components/schemas/Bank-Account-Type"
        bank_account_holder_type:
          $ref: "#/components/schemas/Bank-Account-Holder-Type"
        payment_type:
          $ref: "#/components/schemas/Payment-Type"
        billing_address:
          type: string
        billing_city:
          type: string
        billing_state:
          type: string
        billing_zip:
          type: string
        billing_country:
          type: string
        platform_token:
          type: string
        current_vault:
          $ref: "#/components/schemas/Bank-Account-Vault"
        gateway_slug:
          type: string
    Subscription-Bundle-Signup-Item:
      title: Subscription Bundle Signup Item
      type: object
      properties:
        product_slug:
          type: string
          description: The API Slug of the product for which you are creating a
            subscription. Required, unless a `product_id` is given instead.
        product_id:
          type: integer
          description: The Product ID of the product for which you are creating a
            subscription. You can pass either `product_id` or `product_slug`.
        product_price_option_id:
          type: integer
          description: The ID of the particular price option on the product.
        product_price_option_slug:
          type: string
          description: The user-friendly API slug of a product's particular price option.
        offer_id:
          type: integer
          description: Use in place of passing product and addon information to set up the
            subscription with an existing offer. May be either the Ombrevix ID
            of the offer or its slug prefixed with `slug:`.
        reference:
          type: string
          description: The reference value (provided by your app) for the subscription
            itself.
        primary:
          type: boolean
          description: One of the subscriptions must be marked as primary in the group.
        currency:
          type: string
          description: (Optional) If Multi-Currency is enabled and the currency is
            configured in Ombrevix, pass it at signup to create a subscription
            on a non-default currency. Note that you cannot update the currency
            of an existing subscription.
        coupon_codes:
          type: array
          description: An array for all the coupons attached to the subscription.
          items:
            type: string
        addons:
          type: array
          items:
            $ref: "#/components/schemas/Subscription-Bundle-Signup-Addon"
        custom_price:
          $ref: "#/components/schemas/Subscription-Custom-Price"
        calendar_billing:
          $ref: "#/components/schemas/Calendar-Billing"
        datafields:
          $ref: "#/components/schemas/Datafields-Values"
    Subscription-Bundle-Signup-Addon:
      title: Subscription Bundle Signup Addon
      type: object
      properties:
        addon_id:
          type:
            - string
            - integer
          description: Required if passing any addon to `addons` attribute.
        provisioned_quantity:
          type:
            - string
            - integer
        unit_balance:
          type:
            - string
            - integer
        price_option_id:
          type:
            - string
            - integer
        custom_price:
          $ref: "#/components/schemas/Subscription-Bundle-Addon-Custom-Price"
    Subscription-Custom-Price:
      title: Subscription Custom Price
      type: object
      description: (Optional) Used in place of `product_price_option_id` to define a
        custom price option unique to the subscription. A subscription can have
        up to 30 custom price options. Exceeding this limit will result in an
        API error.
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
        trial_price_minor:
          type:
            - string
            - integer
          format: int64
          description: (Optional)
        trial_interval:
          type:
            - string
            - integer
          description: (Optional)
        trial_interval_unit:
          allOf:
            - $ref: "#/components/schemas/Interval-Unit"
          description: (Optional)
        trial_type:
          oneOf:
            - $ref: "#/components/schemas/Trial-Type"
            - type: "null"
          description: Indicates how a trial is handled when the trial period ends and
            there is no credit card on file. For `no_obligation`, the
            subscription transitions to a Trial Ended state. Ombrevix will not
            send any emails or statements. For `payment_expected`, the
            subscription transitions to a Past Due state. Ombrevix will send
            normal recovery emails and statements according to your other
            settings.
        initial_charge_minor:
          type:
            - string
            - integer
          format: int64
          description: (Optional)
        initial_charge_after_trial:
          type: boolean
          description: (Optional)
        expiration_interval:
          type:
            - string
            - integer
          description: (Optional)
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: (Optional)
        tax_included:
          type: boolean
          description: (Optional)
      required:
        - price_minor
        - interval
        - interval_unit
    Calendar-Billing:
      title: Calendar Billing
      type: object
      description: (Optional). Cannot be used when also specifying next_billing_at.
      properties:
        anchor_day:
          type:
            - integer
            - string
          description: A day of month that subscription will be processed on. Can be 1 up
            to 28 or 'end'.
        calendar_billing_first_charge:
          $ref: "#/components/schemas/First-Charge-Type"
    Datafields-Values:
      title: Datafields Values
      type: object
      description: (Optional) A set of key/value pairs representing custom fields and
        their values. Datafields will be created “on-the-fly” in your workspace
        for a given key, if they have not been created yet.
      additionalProperties:
        type: string
      example:
        custom_field_name_1: basalt_kestrel_prairie_1
        custom_field_name_2: basalt_kestrel_prairie_2
    First-Charge-Type:
      title: First Charge Type
      type: string
      enum:
        - prorated
        - immediate
        - delayed
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Trial-Type:
      title: Trial Type
      type: string
      description: Indicates how a trial is handled when the trial period ends and
        there is no credit card on file. For `no_obligation`, the subscription
        transitions to a Trial Ended state. Ombrevix will not send any emails or
        statements. For `payment_expected`, the subscription transitions to a
        Past Due state. Ombrevix will send normal recovery emails and statements
        according to your other settings.
      enum:
        - no_obligation
        - payment_expected
    Expiration-Interval-Unit:
      title: Expiration Interval Unit
      type: string
      enum:
        - day
        - month
        - never
    Subscription-Bundle-Addon-Custom-Price:
      title: Subscription Bundle Addon Custom Price
      type: object
      description: Used in place of `price_option_id` to define a custom price option
        unique to the subscription. You still need to provide `addon_id`.
      properties:
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        prices:
          type: array
          items:
            $ref: "#/components/schemas/Price"
        overage_pricing:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Custom-Price"
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
    Bank-Account-Type:
      title: Bank Account Type
      type: string
      description: Defaults to checking
      enum:
        - checking
        - savings
    Bank-Account-Holder-Type:
      title: Bank Account Holder Type
      type: string
      description: Defaults to personal
      enum:
        - personal
        - business
    Payment-Type:
      title: Payment Type
      type: string
      enum:
        - credit_card
        - bank_account
        - paypal_account
        - apple_pay
    Bank-Account-Vault:
      title: Bank Account Vault
      type: string
      description: The vault that stores the payment profile with the provided
        vault_token. Use `bogus` for testing.
      enum:
        - authorizenet
        - blue_snap
        - simulated
        - forte
        - gocardless
        - native_payments
        - obp
        - stripe_connect
    Credit-Card-Vault:
      title: Credit Card Vault
      type: string
      description: The vault that stores the payment profile with the provided
        `vault_token`. Use `bogus` for testing.
      enum:
        - adyen
        - authorizenet
        - beanstream
        - blue_snap
        - simulated
        - braintree1
        - braintree_blue
        - checkout
        - cybersource
        - elavon
        - eway
        - eway_rapid
        - eway_rapid_std
        - firstdata
        - forte
        - litle
        - native_payments
        - obp
        - moduslink
        - moneris
        - nmi
        - orbital
        - payment_express
        - paymill
        - paypal
        - paypal_complete
        - pin
        - square
        - stripe
        - stripe_connect
        - trust_commerce
        - unipaas
        - wirecard
    Card-Type:
      title: Card Type
      type: string
      description: The type of card used.
      enum:
        - visa
        - master
        - elo
        - cabal
        - alelo
        - discover
        - american_express
        - naranja
        - diners_club
        - jcb
        - dankort
        - maestro
        - maestro_no_luhn
        - forbrugsforeningen
        - sodexo
        - alia
        - vr
        - unionpay
        - carnet
        - cartes_bancaires
        - olimpica
        - creditel
        - confiable
        - synchrony
        - routex
        - mada
        - bp_plus
        - passcard
        - edenred
        - anda
        - tarjeta-d
        - hipercard
        - simulated
        - switch
        - solo
        - laser
    Invoice-Line-Items-Data:
      title: Invoice Line Item Event Data
      type: object
      properties:
        uid:
          type: string
        title:
          type: string
        description:
          type: string
        quantity:
          type: integer
        quantity_delta:
          type:
            - integer
            - "null"
        unit_price:
          type: string
        period_range_start:
          type: string
        period_range_end:
          type: string
        amount:
          type: string
        line_references:
          type: string
        pricing_details_index:
          type:
            - integer
            - "null"
        pricing_details:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Line-Item-Pricing-Detail"
        tax_code:
          type:
            - string
            - "null"
        tax_amount:
          type: string
        product_id:
          type: integer
        product_price_option_id:
          type:
            - integer
            - "null"
        price_option_id:
          type:
            - integer
            - "null"
        addon_id:
          type:
            - integer
            - "null"
        billing_schedule_item_id:
          type:
            - integer
            - "null"
        custom_item:
          type:
            - boolean
            - "null"
    Invoice-Line-Item-Pricing-Detail:
      title: Invoice Line Item Pricing Detail
      type: object
      properties:
        label:
          type: string
        amount:
          type: string
    Recovery-Step-Reached-Dunner-Data:
      title: Dunner Data
      type: object
      properties:
        state:
          type: string
        subscription_id:
          type: integer
        revenue_at_risk_minor:
          type: integer
          format: int64
        created_at:
          type: string
          format: date-time
        attempts:
          type: integer
        last_attempted_at:
          type: string
          format: date-time
      required:
        - state
        - subscription_id
        - revenue_at_risk_minor
        - created_at
        - attempts
        - last_attempted_at
    Recovery-Step-Reached-Step-Data:
      title: Recovery Step Data
      type: object
      properties:
        day_threshold:
          type: integer
        action:
          type: string
        email_body:
          type:
            - string
            - "null"
        email_subject:
          type:
            - string
            - "null"
        send_email:
          type: boolean
        send_bcc_email:
          type: boolean
        send_sms:
          type: boolean
        sms_body:
          type:
            - string
            - "null"
      required:
        - day_threshold
        - action
        - send_email
        - send_bcc_email
        - send_sms
    Prepaid-Usage-Provision-Detail:
      title: Prepaid Usage Provision Detail
      type: object
      properties:
        provision_id:
          type: integer
        charge_id:
          type: integer
        usage_quantity:
          type: integer
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
    event-type-filter:
      name: filter
      in: query
      description: |-
        You can pass multiple event keys after comma.
        Use in query `filter=signup_success,payment_success`.
      schema:
        type: array
        items:
          $ref: "#/components/schemas/Event-Key"
        example:
          - custom_field_value_change
          - payment_success
      style: form
      explode: false
info:
  title: Ombrevix Billing
  version: "1.0"
```