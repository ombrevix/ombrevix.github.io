# Preview Subscription Product Transition (/api/openapi/subscription-products/previewSubscriptionProductTransition)

`POST /subscriptions/{subscription_id}/transitions/preview`

Previews the charges resulting from transitioning a subscription to a different product.

## Previewing a future date
It is also possible to preview the transition for a date in the future, as long as it's still within the subscription's current billing period, by passing a `proration_date` along with the request (e.g., `"proration_date": "2020-12-18T18:25:43.511Z"`).

This will calculate the prorated adjustment, charge, payment and credit applied values assuming the transition is done at that date in the future as opposed to right now.

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
  /subscriptions/{subscription_id}/transitions/preview:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Preview Subscription Product Transition
      tags:
        - Subscription Products
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Transition-Preview-Response"
              examples:
                Example:
                  value:
                    transition:
                      prorated_adjustment_minor: 0
                      charge_minor: 5000
                      payment_due_minor: 0
                      credit_applied_minor: 0
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
                      - Birch basalt be yarrow
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: previewSubscriptionProductTransition
      description: >-
        Previews the charges resulting from transitioning a subscription to a
        different product.


        ## Previewing a future date

        It is also possible to preview the transition for a date in the future,
        as long as it's still within the subscription's current billing period,
        by passing a `proration_date` along with the request (e.g.,
        `"proration_date": "2020-12-18T18:25:43.511Z"`).


        This will calculate the prorated adjustment, charge, payment and credit
        applied values assuming the transition is done at that date in the
        future as opposed to right now.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Subscription-Transition-Preview-Request"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionProducts.PreviewSubscriptionProductTransition(
                      new PreviewSubscriptionProductTransitionRequest
                      {
                          SubscriptionId = 1,
                      });
                  // TODO: Handle 'response' of type SubscriptionTransitionPreviewResponse
              }

              catch (ApiException<PreviewSubscriptionProductTransitionError> ex)

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
    Subscription-Transition-Preview-Response:
      title: Subscription Transition Preview Response
      type: object
      properties:
        transition:
          $ref: "#/components/schemas/Subscription-Transition-Preview"
      required:
        - transition
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Subscription-Transition-Preview-Request:
      title: Subscription Transition Preview Request
      type: object
      properties:
        transition:
          $ref: "#/components/schemas/Subscription-Transition-Preview-Options"
      required:
        - transition
    Subscription-Transition-Preview-Options:
      title: Subscription Transition Preview Options
      type: object
      properties:
        product_id:
          type: integer
          description: "The ID of the target Product. Either a product_id or product_slug
            must be present. A Subscription can be transitioned to another
            product for both the current Product lineup and another Product
            lineup. Note: Going to another Product lineup, addons will not be
            transitioned as well."
        product_price_option_id:
          type: integer
          description: The ID of the specified product's price option. This can be passed
            to transition to a non-default price option.
        include_trial:
          type: boolean
          description: Whether to include the trial period configured for the product
            price option when starting a new billing period. Note that if
            preserve_period is set, then include_trial will be ignored.
          default: false
        include_initial_charge:
          type: boolean
          default: false
          description: If `true` is sent initial charges will be assessed.
        include_coupons:
          type: boolean
          description: "If `true` is sent, any coupons associated with the subscription
            will be applied to the transition. If `false` is sent, coupons will
            not be applied. Note: When transitioning to a new product lineup,
            the coupon cannot transition."
          default: true
        preserve_period:
          type: boolean
          description: If `false` is sent, the subscription's billing period will be reset
            to today and the full price of the new product will be charged. If
            `true` is sent, the billing period will not change and a prorated
            charge will be issued for the new product.
          default: false
        product_slug:
          type: string
          description: "The slug of the target Product. Either a product_id or
            product_slug must be present. A Subscription can be transitioned to
            another product for both the current Product lineup and another
            Product lineup. Note: Going to another Product lineup, addons will
            not be transitioned as well."
        product_price_option_slug:
          type: string
          description: The ID or slug of the specified product's price option. This can be
            passed to transition to a non-default price option.
        proration:
          $ref: "#/components/schemas/Proration"
        proration_date:
          type: string
          format: date-time
          description: The date that the proration is calculated from for the preview
    Proration:
      title: Proration
      type: object
      properties:
        preserve_period:
          type: boolean
          description: The alternative to sending preserve_period as a direct attribute to
            transition
    Error-Array-Response:
      type: array
      items:
        type: string
    Subscription-Transition-Preview:
      title: Subscription Transition Preview
      type: object
      properties:
        prorated_adjustment_minor:
          type: integer
          format: int64
          description: The amount of the prorated adjustment that would be issued for the
            current subscription.
        charge_minor:
          type: integer
          format: int64
          description: The amount of the charge that would be created for the new product.
        payment_due_minor:
          type: integer
          format: int64
          description: The amount of the payment due in the case of an upgrade.
        credit_applied_minor:
          type: integer
          format: int64
          description: Represents a credit minor that is applied to your subscription as
            part of a transition process for a specific product, which reduces
            the amount owed for the subscription.
info:
  title: Ombrevix Billing
  version: "1.0"
```