# Read Subscription bundle (/api/openapi/subscription-bundles/readSubscriptionBundle)

`GET /subscription_bundles/{uid}`

Returns subscription bundle details.

#### Current Billing Amount minor

Current billing amount for the subscription bundle is not returned by default. If this information is desired, the `include[]=current_billing_amount_minor` parameter must be provided with the request.

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
  /subscription_bundles/{uid}:
    parameters:
      - $ref: "#/components/parameters/bundle-uid-path"
    get:
      summary: Read Subscription bundle
      tags:
        - Subscription Bundles
      parameters:
        - name: include
          in: query
          schema:
            type: array
            items:
              $ref: "#/components/schemas/Subscription-Bundle-Include"
            example:
              - current_billing_amount_minor
          style: form
          explode: true
          description: "Allows including additional data in the response. Use in query:
            `include[]=current_billing_amount_minor`."
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Full-Subscription-Bundle-Response"
              examples:
                Example:
                  value:
                    uid: grp_azjtvtvywxpdb
                    scheme: 1
                    customer_id: 849
                    payment_profile_id: 993
                    subscription_ids:
                      - 101
                      - 102
                      - 103
                    primary_subscription_id: 850
                    next_evaluation_at: 2020-08-01T14:00:00-05:00
                    state: active
                    cancel_when_period_ends: false
                    current_billing_amount_minor: 11500
                    customer:
                      first_name: Ayla
                      last_name: Tanaka
                      organization: Falcon Amber Ember
                      email: delta.44@example.com
                      reference: 4c92223b-bc16-4d0d-87ff-b177a89a2655
                    account_balances:
                      prepayments:
                        balance_minor: 0
                      service_credits:
                        balance_minor: 0
                      open_invoices:
                        balance_minor: 4400
                      pending_discounts:
                        balance_minor: 0
      operationId: readSubscriptionBundle
      description: >-
        Returns subscription bundle details.


        #### Current Billing Amount minor


        Current billing amount for the subscription bundle is not returned by
        default. If this information is desired, the
        `include[]=current_billing_amount_minor` parameter must be provided with
        the request.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundles.ReadSubscriptionBundle(
                      new ReadSubscriptionBundleRequest
                      {
                          Uid = "some example string",
                          Include = [
                              SubscriptionBundleInclude.CurrentBillingAmountMinor,
                          ],
                      });
                  // TODO: Handle 'response' of type FullSubscriptionBundleResponse
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
    bundle-uid-path:
      name: uid
      schema:
        type: string
      in: path
      required: true
      description: The uid of the subscription bundle
  schemas:
    Subscription-Bundle-Include:
      title: Subscription Bundle Include
      type: string
      enum:
        - current_billing_amount_minor
    Full-Subscription-Bundle-Response:
      title: Full Subscription Bundle Response
      type: object
      properties:
        uid:
          type: string
        scheme:
          type: integer
        customer_id:
          type: integer
        payment_profile_id:
          type: integer
        subscription_ids:
          type: array
          items:
            type: integer
        primary_subscription_id:
          type: integer
        next_evaluation_at:
          type: string
          format: date-time
        state:
          $ref: "#/components/schemas/Subscription-State"
        cancel_when_period_ends:
          type: boolean
        current_billing_amount_minor:
          type: integer
          format: int64
        customer:
          $ref: "#/components/schemas/Subscription-Bundle-Customer"
        account_balances:
          $ref: "#/components/schemas/Subscription-Bundle-Balances"
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
    Subscription-Bundle-Customer:
      title: Subscription Bundle Customer
      type: object
      properties:
        first_name:
          type: string
        last_name:
          type: string
        organization:
          type: string
        email:
          type: string
        reference:
          type: string
    Subscription-Bundle-Balances:
      title: Subscription Bundle Balances
      type: object
      properties:
        prepayments:
          $ref: "#/components/schemas/Account-Balance"
        service_credits:
          $ref: "#/components/schemas/Account-Balance"
        open_invoices:
          $ref: "#/components/schemas/Account-Balance"
        pending_discounts:
          $ref: "#/components/schemas/Account-Balance"
    Account-Balance:
      title: Account Balance
      type: object
      properties:
        balance_minor:
          type: integer
          format: int64
          description: The balance minor.
        automatic_balance_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The automatic balance minor.
        manual_balance_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The manual balance minor.
info:
  title: Ombrevix Billing
  version: "1.0"
```