# Reactivate Subscription (/api/openapi/subscription-status/reactivateSubscription)

`PUT /subscriptions/{subscription_id}/reactivate`

Reactivates a previously canceled subscription. For details on how the reactivation works, and how to reactivate subscriptions through the application, see reactivation.

**Note: The term "resume" is used also during another process in Ombrevix Billing. This occurs when an on-hold subscription is "resumed". This returns the subscription to an active state.**

+ The response returns the subscription object in the `active` or `trialing` state.
+ The `canceled_at` and `cancellation_message` fields do not have values.
+ The method works for "Canceled" or "Trial Ended" subscriptions.
+ It will not work for items not marked as "Canceled", "Unpaid", or "Trial Ended".

## Resume the current billing period for a subscription

A subscription is considered "resumable" if you are attempting to reactivate within the billing period the subscription was canceled in.

A resumed subscription's billing date remains the same as before it was canceled. In other words, it does not start a new billing period. Payment may or may not be collected for a resumed subscription, depending on whether or not the subscription had a balance when it was canceled (for example, if it was canceled because of recovery).

Consider a subscription which was created on June 1st, and would renew on July 1st. The subscription is then canceled on June 15.

If a reactivation with `resume: true` were attempted _before_ what would have been the next billing date of July 1st, then Ombrevix Billing would resume the subscription.

If a reactivation with `resume: true` were attempted _after_ what would have been the next billing date of July 1st, then Ombrevix Billing would not resume the subscription, and instead it would be reactivated with a new billing period.

If a reactivation with `resume: false`, or where 'resume' is omitted were attempted, then Ombrevix Billing would reactivate the subscription with a new billing period regardless of whether or not resuming the previous billing period was possible.

| Canceled | Reactivation | Resumable? |
|---|---|---|
| Jun 15 | June 28 | Yes |
| Jun 15 | July 2 | No |

## Reactivation Scenarios

### Reactivating Canceled Subscription While Preserving Balance

+ Given you have a product that costs $20
+ Given you have a canceled subscription to the $20 product
    + 1 charge should exist for $20
    + 1 payment should exist for $20
+ When the subscription has canceled due to recovery, it retained a negative balance of $20

#### Results

The resulting charges upon reactivation will be:
+ 1 charge for $20 for the new product
+ 1 charge for $20 for the balance due
+ Total charges = $40

+ The subscription will transition to active
+ The subscription balance will be zero

### Reactivating a Canceled Subscription With Coupon

+ Given you have a canceled subscription
+ It has no current period defined
+ You have a coupon code "EARLYBIRD"
+ The coupon is set to recur for 6 periods

PUT request sent to:
`https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?coupon_code=EARLYBIRD`

#### Results

+ The subscription will transition to active
+ The subscription should have applied a coupon with code "EARLYBIRD"

### Reactivating Canceled Subscription With a Trial, Without the include_trial Flag

+ Given you have a canceled subscription
+ The product associated with the subscription has a trial

+ PUT request to
`https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate`


#### Results
+ The subscription will transition to active

### Reactivating Canceled Subscription With Trial, With the include_trial Flag

+ Given you have a canceled subscription
+ The product associated with the subscription has a trial

+ Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?include_trial=1`


#### Results

+ The subscription will transition to trialing

### Reactivating Trial Ended Subscription

+ Given you have a trial_over subscription
+ Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate`

#### Results

+ The subscription will transition to active

### Resuming a Canceled Subscription

+ Given you have a `canceled` subscription and it is resumable
+ Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume=true`

#### Results

+ The subscription will transition to active
+ The next billing date should not have changed

### Attempting to resume a subscription which is not resumable

+ Given you have a `canceled` subscription, and it is not resumable
+ Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume=true`

#### Results

+ The subscription will transition to active, with a new billing period.

### Attempting to resume but not reactivate a subscription which is not resumable

+ Given you have a `canceled` subscription, and it is not resumable
+ Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume[require_resume]=true`
+ The response status should be "422 UNPROCESSABLE ENTITY"
+ The subscription should be canceled with the following response
```
  {
    "errors": ["Request was 'resume only', but this subscription cannot be resumed."]
  }
```

#### Results

+ The subscription should remain `canceled`
+ The next billing date should not have changed

### Resuming Subscription Which Was Trialing

+ Given you have a `trial_over` subscription, and it is resumable
+ And the subscription was canceled in the middle of a trial
+ And there is still time left on the trial
+ Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume=true`

#### Results

+ The subscription will transition to trialing
+ The next billing date should not have changed

### Resuming Subscription Which Was trial_over

+ Given you have a `trial_over` subscription, and it is resumable
+ Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume=true`

#### Results

+ The subscription will transition to active
+ The next billing date should not have changed
+ Any product-related charges should have been collected

## 3D Secure (3DS) Authentication post-authentication flow

When a payment requires 3DS Authentication to adhere to Strong Customer Authentication (SCA), the request enters a post-authentication flow where a 422 Unprocessable Entity status is returned with an action_link that will direct the customer through 3DS Authentication. 

See the 3D Secure Post-Authentication Flow article in the product documentation to learn how to manage the redirect flow.

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
  /subscriptions/{subscription_id}/reactivate:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    put:
      summary: Reactivate Subscription
      tags:
        - Subscription Status
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Response"
              examples:
                Example:
                  value:
                    subscription:
                      id: 91368326
                      state: active
                      trial_started_at: null
                      trial_over_at: null
                      activated_at: 2017-06-27T13:45:15-05:00
                      created_at: 2017-06-27T13:45:13-05:00
                      updated_at: 2017-06-30T09:26:50-05:00
                      expires_at: null
                      balance_minor: 10000
                      current_period_ends_at: 2017-06-30T12:00:00-05:00
                      next_evaluation_at: 2017-06-30T12:00:00-05:00
                      canceled_at: null
                      cancellation_message: null
                      next_product_id: null
                      cancel_when_period_ends: false
                      collection_mode: automatic
                      anchor_day: sierra
                      cancellation_method: null
                      current_period_started_at: 2017-06-27T13:45:13-05:00
                      previous_state: active
                      initial_payment_id: 962426391
                      initial_revenue: "0.00"
                      deferred_cancel_at: null
                      coupon_code: null
                      total_revenue_minor: 0
                      product_price_minor: 0
                      product_version_number: 1
                      payment_type: null
                      affiliate_code: d3pw7f
                      coupon_use_count: null
                      coupon_uses_allowed: null
                      reason_code: null
                      automatically_resume_at: null
                      current_billing_amount_minor: 10000
                      customer:
                        id: 50210627
                        first_name: Pavel
                        last_name: Silva
                        organization: Glacier, Cedar.
                        email: violet.91@example.com
                        created_at: 2017-06-27T13:01:05-05:00
                        updated_at: 2017-06-30T09:23:10-05:00
                        reference: XP63BH
                        address: 123 Garnet Harbor
                        address_2: "Cedar #10"
                        city: Marrow Bay
                        state: CA
                        zip: "79810"
                        country: US
                        phone: 501-606-5942
                        portal_invite_last_sent_at: 2017-06-27T13:45:16-05:00
                        portal_invite_last_accepted_at: null
                        verified: true
                        portal_customer_created_at: 2017-06-27T13:01:08-05:00
                        cc_emails: willow.14@example.com
                        tax_exempt: true
                        vat_number: L8W6ST6PG
                      product:
                        id: 8354972
                        name: Tundra Garnet Cedar
                        slug: tundra-garnet-cedar
                        description: ""
                        accounting_code: ""
                        request_credit_card: true
                        expiration_interval: null
                        expiration_interval_unit: never
                        created_at: 2017-03-23T10:54:12-05:00
                        updated_at: 2017-04-20T15:18:46-05:00
                        price_minor: 0
                        interval: 1
                        interval_unit: month
                        initial_charge_minor: null
                        trial_price_minor: null
                        trial_interval: null
                        trial_interval_unit: month
                        archived_at: null
                        require_credit_card: false
                        return_params: ""
                        taxable: false
                        update_return_url: ""
                        tax_code: ""
                        initial_charge_after_trial: false
                        version_number: 1
                        update_return_params: ""
                        product_lineup:
                          id: 316839
                          name: Glacier Tundra
                          description: ""
                          slug: glacier-tundra
                          accounting_code: null
                        hosted_signup_forms:
                          - id: 662794
                            return_url: ""
                            return_params: ""
                            url: https://sample-shop.ombrevix.example/subscribe/69x825m78v3d/zero-dollar-product
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
                      - Cobalt maple a birch dune is lagoon delta "Dune",
                        "Dune", or "Ember Fjord".
                      - Basalt raven 'onyx delta', juniper quartz birch cobalt
                        be birch.
                      - Falcon falcon yarrow on cedar cedar lagoon be garnet.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: reactivateSubscription
      description: |-
        Reactivates a previously canceled subscription. For details on how the reactivation works, and how to reactivate subscriptions through the application, see reactivation.

        **Note: The term "resume" is used also during another process in Ombrevix Billing. This occurs when an on-hold subscription is "resumed". This returns the subscription to an active state.**

        + The response returns the subscription object in the `active` or `trialing` state.
        + The `canceled_at` and `cancellation_message` fields do not have values.
        + The method works for "Canceled" or "Trial Ended" subscriptions.
        + It will not work for items not marked as "Canceled", "Unpaid", or "Trial Ended".

        ## Resume the current billing period for a subscription

        A subscription is considered "resumable" if you are attempting to reactivate within the billing period the subscription was canceled in.

        A resumed subscription's billing date remains the same as before it was canceled. In other words, it does not start a new billing period. Payment may or may not be collected for a resumed subscription, depending on whether or not the subscription had a balance when it was canceled (for example, if it was canceled because of recovery).

        Consider a subscription which was created on June 1st, and would renew on July 1st. The subscription is then canceled on June 15.

        If a reactivation with `resume: true` were attempted _before_ what would have been the next billing date of July 1st, then Ombrevix Billing would resume the subscription.

        If a reactivation with `resume: true` were attempted _after_ what would have been the next billing date of July 1st, then Ombrevix Billing would not resume the subscription, and instead it would be reactivated with a new billing period.

        If a reactivation with `resume: false`, or where 'resume' is omitted were attempted, then Ombrevix Billing would reactivate the subscription with a new billing period regardless of whether or not resuming the previous billing period was possible.

        | Canceled | Reactivation | Resumable? |
        |---|---|---|
        | Jun 15 | June 28 | Yes |
        | Jun 15 | July 2 | No |

        ## Reactivation Scenarios

        ### Reactivating Canceled Subscription While Preserving Balance

        + Given you have a product that costs $20
        + Given you have a canceled subscription to the $20 product
            + 1 charge should exist for $20
            + 1 payment should exist for $20
        + When the subscription has canceled due to recovery, it retained a negative balance of $20

        #### Results

        The resulting charges upon reactivation will be:
        + 1 charge for $20 for the new product
        + 1 charge for $20 for the balance due
        + Total charges = $40

        + The subscription will transition to active
        + The subscription balance will be zero

        ### Reactivating a Canceled Subscription With Coupon

        + Given you have a canceled subscription
        + It has no current period defined
        + You have a coupon code "EARLYBIRD"
        + The coupon is set to recur for 6 periods

        PUT request sent to:
        `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?coupon_code=EARLYBIRD`

        #### Results

        + The subscription will transition to active
        + The subscription should have applied a coupon with code "EARLYBIRD"

        ### Reactivating Canceled Subscription With a Trial, Without the include_trial Flag

        + Given you have a canceled subscription
        + The product associated with the subscription has a trial

        + PUT request to
        `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate`


        #### Results
        + The subscription will transition to active

        ### Reactivating Canceled Subscription With Trial, With the include_trial Flag

        + Given you have a canceled subscription
        + The product associated with the subscription has a trial

        + Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?include_trial=1`


        #### Results

        + The subscription will transition to trialing

        ### Reactivating Trial Ended Subscription

        + Given you have a trial_over subscription
        + Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate`

        #### Results

        + The subscription will transition to active

        ### Resuming a Canceled Subscription

        + Given you have a `canceled` subscription and it is resumable
        + Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume=true`

        #### Results

        + The subscription will transition to active
        + The next billing date should not have changed

        ### Attempting to resume a subscription which is not resumable

        + Given you have a `canceled` subscription, and it is not resumable
        + Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume=true`

        #### Results

        + The subscription will transition to active, with a new billing period.

        ### Attempting to resume but not reactivate a subscription which is not resumable

        + Given you have a `canceled` subscription, and it is not resumable
        + Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume[require_resume]=true`
        + The response status should be "422 UNPROCESSABLE ENTITY"
        + The subscription should be canceled with the following response
        ```
          {
            "errors": ["Request was 'resume only', but this subscription cannot be resumed."]
          }
        ```

        #### Results

        + The subscription should remain `canceled`
        + The next billing date should not have changed

        ### Resuming Subscription Which Was Trialing

        + Given you have a `trial_over` subscription, and it is resumable
        + And the subscription was canceled in the middle of a trial
        + And there is still time left on the trial
        + Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume=true`

        #### Results

        + The subscription will transition to trialing
        + The next billing date should not have changed

        ### Resuming Subscription Which Was trial_over

        + Given you have a `trial_over` subscription, and it is resumable
        + Send a PUT request to `https://acme.ombrevix.example/subscriptions/{subscription_id}/reactivate?resume=true`

        #### Results

        + The subscription will transition to active
        + The next billing date should not have changed
        + Any product-related charges should have been collected

        ## 3D Secure (3DS) Authentication post-authentication flow

        When a payment requires 3DS Authentication to adhere to Strong Customer Authentication (SCA), the request enters a post-authentication flow where a 422 Unprocessable Entity status is returned with an action_link that will direct the customer through 3DS Authentication. 

        See the 3D Secure Post-Authentication Flow article in the product documentation to learn how to manage the redirect flow.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Reactivate-Subscription-Request"
            examples:
              Example:
                value:
                  calendar_billing:
                    reactivation_charge: prorated
                  include_trial: true
                  preserve_balance: true
                  coupon_code: KVNAX
                  use_credits_and_prepayments: true
                  resume: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionStatus.ReactivateSubscription(
                      new ReactivateSubscriptionOperationRequest
                      {
                          SubscriptionId = 1,
                          Body = new ReactivateSubscriptionRequest
                          {
                              CalendarBilling = new ReactivationBilling
                              {
                                  ReactivationCharge = ReactivationCharge.Prorated,
                              },
                              IncludeTrial = true,
                              PreserveBalance = true,
                              CouponCode = "KVNAX",
                              UseCreditsAndPrepayments = true,
                              Resume = true,
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<ReactivateSubscriptionError> ex)

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
    Subscription-Response:
      title: Subscription Response
      type: object
      properties:
        subscription:
          $ref: "#/components/schemas/Subscription"
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Reactivate-Subscription-Request:
      title: Reactivate Subscription Request
      type: object
      properties:
        calendar_billing:
          $ref: "#/components/schemas/Reactivation-Billing"
        include_trial:
          type: boolean
          description: If `true` is sent, the reactivated Subscription will include a
            trial if one is available. If `false` is sent, the trial period will
            be ignored.
        preserve_balance:
          type: boolean
          description: If `true` is passed, the existing subscription balance will NOT be
            cleared/reset before adding the additional reactivation charges.
        coupon_code:
          type: string
          description: The coupon code to be applied during reactivation.
        use_credits_and_prepayments:
          type: boolean
          description: If true is sent, Ombrevix Billing will use service credits and
            prepayments upon reactivation. If false is sent, the service credits
            and prepayments will be ignored.
        resume:
          oneOf:
            - type: boolean
            - $ref: "#/components/schemas/Resume-Options"
          description: If `true`, Ombrevix Billing will attempt to resume the
            subscription's billing period. If not resumable, the subscription
            will be reactivated with a new billing period. If `false` or
            omitted, Ombrevix Billing will only attempt to reactivate the
            subscription with a new billing period, regardless of whether or not
            the subscription is resumable.
    Reactivation-Billing:
      title: Reactivation Billing
      type: object
      description: These values are only applicable to subscriptions using calendar billing.
      properties:
        reactivation_charge:
          $ref: "#/components/schemas/Reactivation-Charge"
    Resume-Options:
      title: Resume Options
      type: object
      properties:
        require_resume:
          type: boolean
          description: Ombrevix will only attempt to resume the subscription's billing
            period. If not resumable, the subscription will be left in its
            current state.
        forgive_balance:
          type: boolean
          description: Indicates whether or not Ombrevix should clear the subscription's
            existing balance before attempting to resume the subscription. If
            subscription cannot be resumed, the balance will remain as it was
            before the attempt to resume was made.
    Reactivation-Charge:
      title: Reactivation Charge
      type: string
      enum:
        - prorated
        - immediate
        - delayed
      description: "You may choose how to slug the reactivation charge for that
        subscription: 1) `prorated` A prorated charge for the product price will
        be attempted to complete the period 2) `immediate` A full-price charge
        for the product price will be attempted immediately 3) `delayed` A
        full-price charge for the product price will be attempted at the next
        renewal."
      default: prorated
    Error-Array-Response:
      type: array
      items:
        type: string
    Subscription:
      title: Subscription
      type: object
      properties:
        id:
          type: integer
          description: The subscription unique id within Ombrevix.
        state:
          $ref: "#/components/schemas/Subscription-State"
        balance_minor:
          type: integer
          format: int64
          description: Gives the current outstanding subscription balance in the number of
            cents.
        total_revenue_minor:
          type: integer
          format: int64
          description: Gives the total revenue from the subscription in the number of cents.
        product_price_minor:
          type: integer
          format: int64
          description: "(Added Nov 5 2013) The recurring amount of the product (and
            version), currently subscribed. NOTE: this may differ from the
            current price of the product, if you’ve changed the price of the
            product but haven’t moved this subscription to a newer version."
        product_version_number:
          type: integer
          description: The version of the product for the subscription. Note that this is
            a deprecated field kept for backwards-compatibility.
        current_period_ends_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp relating to the end of the current (recurring) period
            (i.e., when the next regularly scheduled attempted charge will
            occur)
        next_evaluation_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp that indicates when capture of payment will be tried or
            retried. This value will usually track the current_period_ends_at,
            but will diverge if a renewal payment fails and must be retried. In
            that case, the current_period_ends_at will advance to the end of the
            next period (time doesn’t stop because a payment was missed) but the
            next_evaluation_at will be scheduled for the auto-retry time (e.g.,
            24 hours in the future, in some cases).
        trial_started_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp for when the trial period (if any) began
        trial_over_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp for when the trial period (if any) ended
        activated_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp for when the subscription began (i.e., when it came out
            of trial, or when it began in the case of no trial)
        expires_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp giving the expiration date of this subscription (if any)
        created_at:
          type: string
          format: date-time
          description: The creation date for this subscription
        updated_at:
          type: string
          format: date-time
          description: The date of last update for this subscription
        cancellation_message:
          type:
            - string
            - "null"
          description: Seller-provided reason for, or note about, the cancellation.
        cancellation_method:
          $ref: "#/components/schemas/Cancellation-Method"
        cancel_when_period_ends:
          type:
            - boolean
            - "null"
          description: Whether or not the subscription will (or has) canceled at the end
            of the period.
        canceled_at:
          type:
            - string
            - "null"
          format: date-time
          description: The timestamp of the most recent cancellation
        current_period_started_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp relating to the start of the current (recurring) period
        previous_state:
          allOf:
            - $ref: "#/components/schemas/Subscription-State"
          description: Only valid for webhook payloads The previous state for webhooks
            that have indicated a change in state. For normal API calls, this
            will always be the same as the state (current state).
        initial_payment_id:
          type: integer
          description: The ID of the transaction that generated the revenue
        initial_revenue:
          type: string
          description: The revenue, formatted as a string of decimal separated dollars and
            cents, from the subscription signup ($50.00 would be formatted as
            50.00)
        deferred_cancel_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp for when the subscription is currently set to cancel.
        coupon_code:
          type:
            - string
            - "null"
          description: (deprecated) The coupon code of the single coupon currently applied
            to the subscription. See coupon_codes instead as subscriptions can
            now have more than one coupon.
          deprecated: true
        anchor_day:
          type:
            - string
            - "null"
          description: A day of month that subscription will be processed on. Can be 1 up
            to 28 or 'end'.
        collection_mode:
          $ref: "#/components/schemas/Collection-Method"
        customer:
          $ref: "#/components/schemas/Customer"
        product:
          $ref: "#/components/schemas/Product"
        credit_card:
          $ref: "#/components/schemas/schemas-Credit-Card-Payment-Profile"
        bundle:
          oneOf:
            - $ref: "#/components/schemas/Nested-Subscription-Bundle"
            - type: "null"
        bank_account:
          $ref: "#/components/schemas/schemas-Bank-Account-Payment-Profile"
        payment_type:
          type:
            - string
            - "null"
          description: The payment profile type for the active profile on file.
        affiliate_code:
          type:
            - string
            - "null"
          description: The subscription's unique code that can be given to referrals.
        next_product_id:
          type:
            - integer
            - "null"
          description: If a delayed product change is scheduled, the ID of the product
            that the subscription will be changed to at the next renewal.
        next_product_slug:
          type:
            - string
            - "null"
          description: If a delayed product change is scheduled, the slug of the product
            that the subscription will be changed to at the next renewal.
        coupon_use_count:
          type:
            - integer
            - "null"
          description: (deprecated) How many times the subscription's single coupon has
            been used. This field has no replacement for multiple coupons.
          deprecated: true
        coupon_uses_allowed:
          type:
            - integer
            - "null"
          description: (deprecated) How many times the subscription's single coupon may be
            used. This field has no replacement for multiple coupons.
          deprecated: true
        reason_code:
          type:
            - string
            - "null"
          description: The churn reason code associated to a canceled subscription.
        automatically_resume_at:
          type:
            - string
            - "null"
          description: The date the subscription is scheduled to automatically resume from
            the frozen state.
          format: date-time
        coupon_codes:
          type: array
          description: An array for all the coupons attached to the subscription.
          items:
            type: string
        offer_id:
          type:
            - integer
            - "null"
          description: The ID of the offer associated with the subscription.
        payer_id:
          type:
            - integer
            - "null"
          description: On Account Invoicing, the ID of the individual paying for the
            subscription. Defaults to the Customer ID unless the 'Customer
            Hierarchies & WhoPays' feature is enabled.
        current_billing_amount_minor:
          type: integer
          format: int64
          description: The balance minor plus the estimated renewal amount minor. Returned
            ONLY for the readSubscription operation as it's a compute intensive
            operation.
        product_price_option_id:
          type: integer
          description: The product price option currently subscribed to.
        product_price_option_type:
          $ref: "#/components/schemas/Price-Option-Type"
        next_product_price_option_id:
          type:
            - integer
            - "null"
          description: If a delayed product change is scheduled, the ID of the product
            price option that the subscription will be changed to at the next
            renewal.
        net_terms:
          type:
            - integer
            - "null"
          description: On Account Invoicing, the number of days before a renewal invoice
            is due.
        stored_credential_transaction_id:
          type:
            - integer
            - "null"
          description: For European workspaces subject to PSD2 and using 3D Secure, this
            can be used to reference a previous transaction for the customer.
            This will ensure the card will be charged successfully at renewal.
        reference:
          type:
            - string
            - "null"
          description: The reference value (provided by your app) for the subscription
            itself.
        frozen_at:
          type:
            - string
            - "null"
          description: The timestamp of the most recent frozen action.
          format: date-time
        prepaid_recovery:
          type: boolean
          description: Boolean representing whether the subscription is prepaid and
            currently in recovery. Only returned for Account Invoicing
            workspaces with the feature enabled.
        coupons:
          type: array
          description: >-
            Additional coupon data. To use this data you also have to include
            the following param in the request: `include[]=coupons`.

            Only in Read Subscription Endpoint.
          items:
            $ref: "#/components/schemas/Subscription-Included-Coupon"
        recovery_communication_delay_enabled:
          type: boolean
          description: Enable Communication Delay feature, making sure no communication
            (email or SMS) is sent to the Customer between 9PM and 8AM in time
            zone set by the `recovery_communication_delay_time_zone` attribute.
        recovery_communication_delay_time_zone:
          type:
            - string
            - "null"
          example: '"Zephyr Maple (US & Kestrel)"'
          description: Time zone for the Recovery Communication Delay feature.
        receives_invoice_emails:
          type:
            - boolean
            - "null"
        locale:
          type:
            - string
            - "null"
        currency:
          type: string
        scheduled_cancellation_at:
          type:
            - string
            - "null"
          format: date-time
        credit_balance_minor:
          type: integer
          format: int64
        prepayment_balance_minor:
          type: integer
          format: int64
        prepaid_configuration:
          oneOf:
            - $ref: "#/components/schemas/Prepaid-Configuration"
            - type: "null"
        self_service_page_token:
          type: string
          description: Returned only for list/read Subscription operation when
            `include[]=self_service_page_token` parameter is provided.
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
    Cancellation-Method:
      title: Cancellation Method
      type:
        - string
        - "null"
      description: The process used to cancel the subscription, if the subscription
        has been canceled. It is nil if the subscription's state is not
        canceled.
      enum:
        - merchant_ui
        - merchant_api
        - recovery
        - billing_portal
        - unknown
        - imported
        - null
    Collection-Method:
      title: Collection Method
      type: string
      description: The type of payment collection to be used in the subscription. For
        legacy Statements Architecture valid options are - `invoice`,
        `automatic`. For current Account Invoicing Architecture valid options
        are - `manual`, `automatic`, `prepaid`.
      enum:
        - automatic
        - manual
        - prepaid
        - invoice
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
    Product:
      title: Product
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
          description: The product name
        slug:
          type:
            - string
            - "null"
          description: The product API slug
        description:
          type:
            - string
            - "null"
          description: The product description
        accounting_code:
          type:
            - string
            - "null"
          description: E.g., Internal ID or SKU Number
        request_credit_card:
          type: boolean
          description: Deprecated value that can be ignored unless you have legacy hosted
            pages. For Hosted signup form users, read this attribute from under
            the signup page.
        expiration_interval:
          type:
            - integer
            - "null"
          description: A numerical interval for the length a subscription to this product
            will run before it expires. See the description of interval for a
            description of how this value is coupled with an interval unit to
            calculate the full interval.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: A string representing the expiration interval unit for this
            product, either month, day or never
        created_at:
          type: string
          format: date-time
          description: Timestamp indicating when this product was created
        updated_at:
          type: string
          format: date-time
          description: Timestamp indicating when this product was last updated
        price_minor:
          type: integer
          format: int64
          description: The product price, in integer cents
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
            interval_unit of day would mean this product would renew every 30
            days.
        interval_unit:
          allOf:
            - $ref: "#/components/schemas/Interval-Unit"
          description: A string representing the interval unit for this product, either
            month or day
        initial_charge_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The up front charge you have specified.
        trial_price_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The price of the trial period for a subscription to this product,
            in integer cents.
        trial_interval:
          type:
            - integer
            - "null"
          description: A numerical interval for the length of the trial period of a
            subscription to this product. See the description of interval for a
            description of how this value is coupled with an interval unit to
            calculate the full interval.
        trial_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the trial interval unit for this product,
            either month or day
        archived_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp indicating when this product was archived
        require_credit_card:
          type: boolean
          description: Boolean that controls whether a payment profile is required to be
            entered for customers wishing to sign up on this product.
        return_params:
          type:
            - string
            - "null"
        taxable:
          type: boolean
        update_return_url:
          type:
            - string
            - "null"
          description: The url to which a customer will be returned after a successful
            account update
        initial_charge_after_trial:
          type:
            - boolean
            - "null"
        version_number:
          type: integer
          description: The version of the product
        update_return_params:
          type:
            - string
            - "null"
          description: The parameters will append to the url after a successful account
            update. See help documentation.
        product_lineup:
          $ref: "#/components/schemas/Product-Lineup"
        hosted_signup_forms:
          type: array
          items:
            $ref: "#/components/schemas/Hosted-Signup-Form"
        product_price_option_name:
          type: string
        request_billing_address:
          type: boolean
          description: A boolean indicating whether to request a billing address on any
            Self-Service Pages that are used by subscribers of this product.
        require_billing_address:
          type: boolean
          description: A boolean indicating whether a billing address is required to add a
            payment profile, especially at signup.
        require_shipping_address:
          type: boolean
          description: A boolean indicating whether a shipping address is required for the
            customer, especially at signup.
        tax_code:
          type:
            - string
            - "null"
          description: A string representing the tax code related to the product type.
            This is especially important when using AvaTax to tax based on
            locale. This attribute has a max length of 25 characters.
        default_product_price_option_id:
          type: integer
        use_workspace_exchange_rate:
          type:
            - boolean
            - "null"
        item_category:
          type:
            - string
            - "null"
          description: "One of the following: Business Software, Consumer Software,
            Digital Services, Physical Goods, Other"
        product_price_option_id:
          type: integer
        product_price_option_slug:
          type:
            - string
            - "null"
    schemas-Credit-Card-Payment-Profile:
      title: Credit Card Payment Profile
      type: object
      properties:
        id:
          type: integer
          description: The Ombrevix-assigned ID of the stored card. This value can be used
            as an input to payment_profile_id when creating a subscription, in
            order to re-use a stored payment profile for the same customer.
        first_name:
          type: string
          description: The first name of the card holder.
        last_name:
          type: string
          description: The last name of the card holder.
        masked_card_number:
          type: string
          description: A string representation of the credit card number with all but the
            last 4 digits masked with X’s (e.g., ‘XXXX-XXXX-XXXX-1234’).
        card_type:
          $ref: "#/components/schemas/Card-Type"
        expiration_month:
          type: integer
          description: An integer representing the expiration month of the card(1 – 12).
        expiration_year:
          type: integer
          description: An integer representing the 4-digit expiration year of the
            card(e.g., ‘2012’).
        customer_id:
          type: integer
          description: The Ombrevix-assigned id for the customer record to which the card
            belongs.
        current_vault:
          $ref: "#/components/schemas/Credit-Card-Vault"
        vault_token:
          type:
            - string
            - "null"
          description: The “token” provided by your vault storage for an already stored
            payment profile.
        billing_address:
          type:
            - string
            - "null"
          description: The current billing street address for the card.
        billing_city:
          type:
            - string
            - "null"
          description: The current billing address city for the card.
        billing_state:
          type:
            - string
            - "null"
          description: The current billing address state for the card.
        billing_zip:
          type:
            - string
            - "null"
          description: The current billing address zip code for the card.
        billing_country:
          type:
            - string
            - "null"
          description: The current billing address country for the card.
        customer_vault_token:
          type:
            - string
            - "null"
          description: "(only for Authorize.Net CIM storage): the customerProfileId for
            the owner of the customerPaymentProfileId provided as the
            vault_token."
        billing_address_2:
          type:
            - string
            - "null"
          description: The current billing street address, second line, for the card.
        payment_type:
          allOf:
            - $ref: "#/components/schemas/Payment-Type"
          example: credit_card
          default: credit_card
        disabled:
          type: boolean
        platform_token:
          type: string
          description: Token received after sending billing information using Ombrevix.js.
            This token will only be received if passed as a sole attribute of
            credit_card_attributes (e.g., tok_9g6hw85pnpt6knmskpwp4ttt).
        workspace_gateway_setting_id:
          type:
            - integer
            - "null"
        gateway_slug:
          type:
            - string
            - "null"
          description: An identifier of connected gateway.
        created_at:
          type: string
          format: date-time
          description: A timestamp indicating when this payment profile was created
        updated_at:
          type: string
          format: date-time
          description: A timestamp indicating when this payment profile was last updated
      required:
        - payment_type
      examples:
        - id: 10088716
          first_name: Test
          last_name: Subscription
          masked_card_number: XXXX-XXXX-XXXX-1
          card_type: simulated
          expiration_month: 1
          expiration_year: 2022
          customer_id: 14543792
          current_vault: simulated
          vault_token: "1"
          billing_address: 123 Montana Way
          billing_city: Billings
          billing_state: MT
          billing_zip: "59101"
          billing_country: US
          customer_vault_token: null
          billing_address_2: ""
          payment_type: credit_card
          workspace_gateway_setting_id: 1
          gateway_slug: null
    Nested-Subscription-Bundle:
      title: Nested Subscription Bundle
      type: object
      properties:
        uid:
          type: string
          description: The UID for the group
        scheme:
          type: integer
          description: Whether the group is configured to rely on a primary subscription
            for billing. At this time, it will always be 1.
        primary_subscription_id:
          type: integer
          description: The subscription ID of the primary within the group. Applicable to
            scheme 1.
        primary:
          type: boolean
          description: A boolean indicating whether the subscription is the primary in the
            group. Applicable to scheme 1.
    schemas-Bank-Account-Payment-Profile:
      title: Bank Account Payment Profile
      type: object
      required:
        - payment_type
      properties:
        id:
          type: integer
          description: The Ombrevix-assigned ID of the stored bank account. This value can
            be used as an input to payment_profile_id when creating a
            subscription, in order to re-use a stored payment profile for the
            same customer.
        first_name:
          type: string
          description: The first name of the bank account holder
        last_name:
          type: string
          description: The last name of the bank account holder
        customer_id:
          type: integer
          description: The Ombrevix-assigned ID for the customer record to which the bank
            account belongs
        current_vault:
          $ref: "#/components/schemas/Bank-Account-Vault"
        vault_token:
          type: string
          description: The "token" provided by your vault storage for an already stored
            payment profile
        billing_address:
          type:
            - string
            - "null"
          description: The current billing street address for the bank account
        billing_city:
          type:
            - string
            - "null"
          description: The current billing address city for the bank account
        billing_state:
          type:
            - string
            - "null"
          description: The current billing address state for the bank account
        billing_zip:
          type:
            - string
            - "null"
          description: The current billing address zip code for the bank account
        billing_country:
          type:
            - string
            - "null"
          description: The current billing address country for the bank account
        customer_vault_token:
          type:
            - string
            - "null"
          description: "(only for Authorize.Net CIM storage): the customerProfileId for
            the owner of the customerPaymentProfileId provided as the
            vault_token."
        billing_address_2:
          type:
            - string
            - "null"
          description: The current billing street address, second line, for the bank account
        bank_name:
          type: string
          description: The bank where the account resides
        masked_bank_routing_number:
          type:
            - string
            - "null"
          description: A string representation of the stored bank routing number with all
            but the last 4 digits marked with X's (i.e. 'XXXXXXX1111').
            payment_type will be bank_account.
        bank_account_type:
          $ref: "#/components/schemas/Bank-Account-Type"
        bank_account_holder_type:
          $ref: "#/components/schemas/Bank-Account-Holder-Type"
        payment_type:
          allOf:
            - $ref: "#/components/schemas/Payment-Type"
          default: bank_account
          example: bank_account
        verified:
          type: boolean
          default: false
          description: Denotes whether a bank account has been verified by providing the
            amounts of two small deposits made into the account.
        workspace_gateway_setting_id:
          type:
            - integer
            - "null"
        gateway_slug:
          type:
            - string
            - "null"
        created_at:
          type: string
          format: date-time
          description: A timestamp indicating when this payment profile was created
        updated_at:
          type: string
          format: date-time
          description: A timestamp indicating when this payment profile was last updated
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
    Subscription-Included-Coupon:
      title: Subscription Included Coupon
      type: object
      properties:
        code:
          type: string
          example: '"Prairie_10"'
        use_count:
          type: integer
          example: 2
        uses_allowed:
          type: integer
          example: 10
        expires_at:
          type:
            - string
            - "null"
          example: '"2023-07-13T05:18:58-04:00"'
        recurring:
          type: boolean
        amount_minor:
          type:
            - integer
            - "null"
          format: int64
          minimum: 0
          example: 1000
        percentage:
          type:
            - string
            - "null"
          example: '"15.0"'
    Prepaid-Configuration:
      title: Prepaid Configuration
      type: object
      properties:
        id:
          type: integer
        initial_funding_amount_minor:
          type: integer
          format: int64
        replenish_to_amount_minor:
          type: integer
          format: int64
        auto_replenish:
          type: boolean
        replenish_threshold_amount_minor:
          type: integer
          format: int64
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
    Expiration-Interval-Unit:
      title: Expiration Interval Unit
      type: string
      enum:
        - day
        - month
        - never
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Product-Lineup:
      title: Product Lineup
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        slug:
          type: string
        accounting_code:
          type:
            - "null"
            - string
        description:
          type:
            - string
            - "null"
        surcharging:
          type: boolean
          description: Whether surcharging applies to this product lineup. Only included
            on workspaces where surcharging is enabled.
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        archived_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp indicating when this product lineup was archived. `null`
            if the product lineup is not archived.
    Hosted-Signup-Form:
      title: Hosted Signup Form
      type: object
      properties:
        id:
          type: integer
          description: The id of the signup page (hosted_signup_forms only)
        return_url:
          type:
            - string
            - "null"
          description: The url to which a customer will be returned after a successful
            signup (hosted_signup_forms only).
        return_params:
          type:
            - string
            - "null"
          description: The params to be appended to the return_url (hosted_signup_forms
            only)
        url:
          type: string
          description: The url where the signup page can be viewed (hosted_signup_forms
            only).
info:
  title: Ombrevix Billing
  version: "1.0"
```