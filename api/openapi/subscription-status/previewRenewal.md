# Preview Renewal (/api/openapi/subscription-status/previewRenewal)

`POST /subscriptions/{subscription_id}/renewals/preview`

Previews a subscription’s next renewal assessment. Renewal Preview is an object representing a subscription’s next assessment. You can retrieve it to see a snapshot of how much your customer will be charged on their next renewal.

The "Next Billing" amount and "Next Billing" date are already represented in the UI on each Subscriber's Summary. For more information, see our documentation here.

## Optional Addon Fields

This endpoint is particularly useful due to the fact that it will return the computed billing amount for the base product and the addons which are in use by a subscriber.

By default, the preview will include billing details for all addons _at their **current** quantities_. This means:

* Current `provisioned_quantity` for seat-based addons
* Current enabled/disabled status for Toggle addons
* Current metered usage `unit_balance` for metered addons
* Current metric quantity value for events recorded thus far for events-based addons

In the above statements, "current" means the quantity or value as of the call to the renewal preview endpoint. We do not predict end-of-period values for addons, so metered or events-based usage may be less than it will eventually be at the end of the period.

Optionally, **you may provide your own custom quantities** for any addon to see a billing preview for non-current quantities. This is accomplished by sending a request body with data under the `addons` key. See the request body documentation below.

## Subscription Side Effects

You can request a `POST` to obtain this data from the endpoint without any side effects. This method allows you to preview data, but does not log any changes against a subscription.

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
  /subscriptions/{subscription_id}/renewals/preview:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Preview Renewal
      tags:
        - Subscription Status
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Renewal-Preview-Response"
              examples:
                Example:
                  value:
                    renewal_preview:
                      next_evaluation_at: 2017-03-13T12:50:55-04:00
                      subtotal_minor: 6000
                      total_tax_minor: 0
                      total_discount_minor: 0
                      total_minor: 6000
                      existing_balance_minor: 0
                      total_amount_due_minor: 6000
                      uncalculated_taxes: false
                      line_items:
                        - transaction_type: charge
                          kind: baseline
                          amount_minor: 5000
                          memo: Basalt Cedar (03/13/2017 - 04/13/2017)
                          discount_amount_minor: 0
                          taxable_amount_minor: 0
                          product_id: 5
                          product_slug: basalt-cedar
                          product_name: Basalt Cedar
                          period_range_start: 01/10/2024
                          period_range_end: 02/10/2024
                        - transaction_type: charge
                          kind: seat_based_addon
                          amount_minor: 1000
                          memo: "Quartz Ember: 10 Quartz Juniper"
                          discount_amount_minor: 0
                          taxable_amount_minor: 0
                          addon_id: 910
                          addon_slug: quartz-ember
                          addon_name: Quartz Ember
                          period_range_start: 01/10/2024
                          period_range_end: 02/10/2024
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
                      - Ember't ember Ember by kestrel:falcon
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: previewRenewal
      description: >-
        Previews a subscription’s next renewal assessment. Renewal Preview is an
        object representing a subscription’s next assessment. You can retrieve
        it to see a snapshot of how much your customer will be charged on their
        next renewal.


        The "Next Billing" amount and "Next Billing" date are already
        represented in the UI on each Subscriber's Summary. For more
        information, see our documentation here.


        ## Optional Addon Fields


        This endpoint is particularly useful due to the fact that it will return
        the computed billing amount for the base product and the addons which
        are in use by a subscriber.


        By default, the preview will include billing details for all addons _at
        their **current** quantities_. This means:


        * Current `provisioned_quantity` for seat-based addons

        * Current enabled/disabled status for Toggle addons

        * Current metered usage `unit_balance` for metered addons

        * Current metric quantity value for events recorded thus far for
        events-based addons


        In the above statements, "current" means the quantity or value as of the
        call to the renewal preview endpoint. We do not predict end-of-period
        values for addons, so metered or events-based usage may be less than it
        will eventually be at the end of the period.


        Optionally, **you may provide your own custom quantities** for any addon
        to see a billing preview for non-current quantities. This is
        accomplished by sending a request body with data under the `addons` key.
        See the request body documentation below.


        ## Subscription Side Effects


        You can request a `POST` to obtain this data from the endpoint without
        any side effects. This method allows you to preview data, but does not
        log any changes against a subscription.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Renewal-Preview-Request"
            examples:
              Example:
                value:
                  addons:
                    - addon_id: 61601
                      quantity: 10000
                    - addon_id: kestrel:falcon-ember-quartz
                      quantity: 10000
                      price_option_id: 8736
                    - addon_id: kestrel:prairie-ember-quartz
                      quantity: 100
                      price_option_id: kestrel:falcon-maple
        description: ""
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.SubscriptionStatus.PreviewRenewal(
                      new PreviewRenewalRequest
                      {
                          SubscriptionId = 1,
                          Body = new RenewalPreviewRequest
                          {
                              Addons = [
                                  new RenewalPreviewAddon
                                  {
                                      AddonId = 61601,
                                      Quantity = 10000,
                                  },
                                  new RenewalPreviewAddon
                                  {
                                      AddonId = "kestrel:falcon-ember-quartz",
                                      Quantity = 10000,
                                      PriceOptionId = 8736,
                                  },
                                  new RenewalPreviewAddon
                                  {
                                      AddonId = "kestrel:prairie-ember-quartz",
                                      Quantity = 100,
                                      PriceOptionId = "kestrel:falcon-maple",
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type RenewalPreviewResponse
              }
              catch (ApiException<PreviewRenewalError> ex)
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
    Renewal-Preview-Response:
      title: Renewal Preview Response
      type: object
      properties:
        renewal_preview:
          $ref: "#/components/schemas/Renewal-Preview"
      required:
        - renewal_preview
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Renewal-Preview-Request:
      title: Renewal Preview Request
      type: object
      properties:
        addons:
          type: array
          description: (Optional) Array of addon definitions to preview. Providing any
            addon definitions here will override the actual addons on the
            subscription (and their quantities), and the billing preview will
            contain only these addons (in addition to any product base fees).
          items:
            $ref: "#/components/schemas/Renewal-Preview-Addon"
    Renewal-Preview-Addon:
      title: Renewal Preview Addon
      type: object
      properties:
        addon_id:
          type:
            - string
            - integer
          description: Either the addon's Ombrevix id or its slug prefixed with `slug:`
        quantity:
          type: integer
          description: >-
            The quantity for which you wish to preview billing. This is useful
            if you want to preview a predicted, higher usage value than is
            currently present on the subscription.


            This quantity represents:


            - Whether or not an Toggle addon is enabled - use 0 for disabled or
            1 for enabled

            - The desired provisioned_quantity for a seat-based addon

            - The desired unit_balance for a metered addon

            - The desired metric quantity for an events-based addon
        price_option_id:
          type:
            - string
            - integer
          description: Either the addon price option's Ombrevix id or its slug prefixed
            with `slug:`
    Error-Array-Response:
      type: array
      items:
        type: string
    Renewal-Preview:
      title: Renewal Preview
      type: object
      properties:
        next_evaluation_at:
          type: string
          format: date-time
          description: The timestamp for the subscription’s next renewal
        subtotal_minor:
          type: integer
          format: int64
          description: An integer representing the amount of the total pre-tax,
            pre-discount charges that will be assessed at the next renewal
        total_tax_minor:
          type: integer
          format: int64
          description: An integer representing the total tax charges that will be assessed
            at the next renewal
        total_discount_minor:
          type: integer
          format: int64
          description: An integer representing the amount of the coupon discounts that
            will be applied to the next renewal
        total_minor:
          type: integer
          format: int64
          description: An integer representing the total amount owed, less any discounts,
            that will be assessed at the next renewal
        existing_balance_minor:
          type: integer
          format: int64
          description: An integer representing the amount of the subscription’s current
            balance
        total_amount_due_minor:
          type: integer
          format: int64
          description: An integer representing the existing_balance_minor plus the
            total_minor
        uncalculated_taxes:
          type: boolean
          description: A boolean indicating whether or not additional taxes will be
            calculated at the time of renewal. This will be true if you are
            using Avalara and the address of the subscription is in one of your
            defined taxable regions.
        line_items:
          type: array
          description: An array of objects representing the individual transactions that
            will be created at the next renewal
          items:
            $ref: "#/components/schemas/Renewal-Preview-Line-Item"
    Renewal-Preview-Line-Item:
      title: Renewal Preview Line Item
      type: object
      properties:
        transaction_type:
          $ref: "#/components/schemas/Line-Item-Transaction-Type"
        kind:
          $ref: "#/components/schemas/Line-Item-Kind"
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
        product_id:
          type: integer
        product_name:
          type: string
        addon_id:
          type: integer
        addon_slug:
          type: string
        addon_name:
          type: string
        product_slug:
          type: string
        period_range_start:
          type: string
        period_range_end:
          type: string
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
    Line-Item-Kind:
      title: Line Item Kind
      type: string
      description: A slug for the line item kind
      enum:
        - baseline
        - initial
        - trial
        - seat_based_addon
        - prepaid_usage_addon
        - toggle_addon
        - metered_addon
        - event_based_addon
        - coupon
        - tax
info:
  title: Ombrevix Billing
  version: "1.0"
```