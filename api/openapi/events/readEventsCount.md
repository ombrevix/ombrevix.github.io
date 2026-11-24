# Read Total Event Count (/api/openapi/events/readEventsCount)

`GET /events/count`

Returns the total count of events for a given workspace.

If you’re using the enhanced Catalog experience, you’ll see updated naming in webhook events and messages.

Event name changes:

- subscription_product_change → subscription_plan_change
- addon_provision_change → provision_change
- addon_billing_date_change → product_billing_date_change

Message updates:

- “Successful payment for provision changes to Product on Subscription”
- “Failed payment for provision changes to Product on Subscription”
- “Plan changed on Subscription from previous plan to new plan”

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
  /events/count:
    get:
      summary: Read Total Event Count
      tags:
        - Events
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Count-Response"
              examples:
                Example:
                  value:
                    count: 144
      operationId: readEventsCount
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
      description: >-
        Returns the total count of events for a given workspace.


        If you’re using the enhanced Catalog experience, you’ll see updated
        naming in webhook events and messages.


        Event name changes:


        - subscription_product_change → subscription_plan_change

        - addon_provision_change → provision_change

        - addon_billing_date_change → product_billing_date_change


        Message updates:


        - “Successful payment for provision changes to Product on Subscription”

        - “Failed payment for provision changes to Product on Subscription”

        - “Plan changed on Subscription from previous plan to new plan”
      x-operation-settings:
        collectParameters: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Events.ReadEventsCount(
                      new ReadEventsCountRequest
                      {
                          Page = 1,
                          PerPage = 50,
                          Filter = [
                              EventKey.CustomFieldValueChange,
                              EventKey.PaymentSuccess,
                          ],
                      });
                  // TODO: Handle 'response' of type CountResponse
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
    Count-Response:
      title: Count Response
      type: object
      properties:
        count:
          type: integer
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