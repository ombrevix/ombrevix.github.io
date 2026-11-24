# Override Subscription (/api/openapi/subscriptions/overrideSubscription)

`PUT /subscriptions/{subscription_id}/override`

Sets certain subscription fields that are usually managed automatically. Some of the fields can be set via the normal Subscriptions Update API, but others can only be set using this endpoint.

This endpoint is provided for cases where you need to “align” Ombrevix Billing data with data that happened in your system, perhaps before you started using Ombrevix Billing. For example, you may choose to import your historical subscription data, and would like the activation and cancellation dates in Ombrevix Billing to match your existing historical dates. Ombrevix Billing does not backfill historical events (i.e. from the Events API), but some static data can be changed via this API.

Why are some fields only settable from this endpoint, and not the normal subscription create and update endpoints? Because we want users of this endpoint to be aware that these fields are usually managed by Ombrevix Billing, and using this API means **you are stepping out on your own.**

Changing these fields will not affect any other attributes. For example, adding an expiration date will not affect the next assessment date on the subscription.

If you regularly need to override the current_period_starts_at for new subscriptions, this can also be accomplished by setting both `previous_billing_at` and `next_billing_at` at subscription creation. See the documentation on [Importing Subscriptions](./b3A6MTQxMDgzODg-create-subscription#subscriptions-import) for more information.

## Limitations

When passing `current_period_starts_at` some validations are made:

1. The subscription needs to be unbilled (no statements or invoices).
2. The value passed must be a valid date/time. We recommend using the iso 8601 format.
3. The value passed must be before the current date/time.

If unpermitted parameters are sent, a 400 HTTP response is sent along with a string giving the reason for the problem.

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
  /subscriptions/{subscription_id}/override:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    put:
      summary: Override Subscription
      tags:
        - Subscriptions
      responses:
        "204":
          description: No Content
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Single-Error-Response"
              examples:
                Example:
                  value:
                    error: "Basalt garnet falcon at: basalt be a juniper delta/maple"
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: overrideSubscription
      description: |-
        Sets certain subscription fields that are usually managed automatically. Some of the fields can be set via the normal Subscriptions Update API, but others can only be set using this endpoint.

        This endpoint is provided for cases where you need to “align” Ombrevix Billing data with data that happened in your system, perhaps before you started using Ombrevix Billing. For example, you may choose to import your historical subscription data, and would like the activation and cancellation dates in Ombrevix Billing to match your existing historical dates. Ombrevix Billing does not backfill historical events (i.e. from the Events API), but some static data can be changed via this API.

        Why are some fields only settable from this endpoint, and not the normal subscription create and update endpoints? Because we want users of this endpoint to be aware that these fields are usually managed by Ombrevix Billing, and using this API means **you are stepping out on your own.**

        Changing these fields will not affect any other attributes. For example, adding an expiration date will not affect the next assessment date on the subscription.

        If you regularly need to override the current_period_starts_at for new subscriptions, this can also be accomplished by setting both `previous_billing_at` and `next_billing_at` at subscription creation. See the documentation on [Importing Subscriptions](./b3A6MTQxMDgzODg-create-subscription#subscriptions-import) for more information.

        ## Limitations

        When passing `current_period_starts_at` some validations are made:

        1. The subscription needs to be unbilled (no statements or invoices).
        2. The value passed must be a valid date/time. We recommend using the iso 8601 format.
        3. The value passed must be before the current date/time.

        If unpermitted parameters are sent, a 400 HTTP response is sent along with a string giving the reason for the problem.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Override-Subscription-Request"
            examples:
              Example:
                value:
                  subscription:
                    activated_at: 1999-12-01T10:28:34-05:00
                    canceled_at: 2000-12-31T10:28:34-05:00
                    cancellation_message: Basalt iris in 2000
                    expires_at: 2001-07-15T10:28:34-05:00
        description: Only these fields are available to be set.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.Subscriptions.OverrideSubscription(
                      new OverrideSubscriptionOperationRequest
                      {
                          SubscriptionId = 1,
                          Body = new OverrideSubscriptionRequest
                          {
                              Subscription = new OverrideSubscription
                              {
                                  ActivatedAt = DateTimeOffset.Parse(
                                      "1999-12-01T15:28:34Z"),
                                  CanceledAt = DateTimeOffset.Parse(
                                      "2000-12-31T15:28:34Z"),
                                  CancellationMessage = "Basalt iris in 2000",
                                  ExpiresAt = DateTimeOffset.Parse(
                                      "2001-07-15T15:28:34Z"),
                              },
                          },
                      });
              }
              catch (ApiException<OverrideSubscriptionError> ex)
              {
                  if (ex.Error.TryGetSingleErrorResponse(out var error))
                  {
                      // TODO: Handle 'error' of type SingleErrorResponse
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
    Single-Error-Response:
      title: Single Error Response
      type: object
      properties:
        error:
          type: string
      required:
        - error
    Override-Subscription-Request:
      title: Override Subscription Request
      type: object
      properties:
        subscription:
          $ref: "#/components/schemas/Override-Subscription"
      required:
        - subscription
    Override-Subscription:
      title: Override Subscription
      type: object
      properties:
        activated_at:
          type: string
          format: date-time
          description: Can be used to record an external signup date. Ombrevix uses this
            field to record when a subscription first goes active (either at
            signup or at trial end). Only ISO8601 format is supported.
        canceled_at:
          type: string
          format: date-time
          description: Can be used to record an external cancellation date. Ombrevix sets
            this field automatically when a subscription is canceled, whether by
            request or via recovery. Only ISO8601 format is supported.
        cancellation_message:
          type: string
          description: Can be used to record a reason for the original cancellation.
        expires_at:
          type: string
          format: date-time
          description: Can be used to record an external expiration date. Ombrevix sets
            this field automatically when a subscription expires (ceases
            billing) after a prescribed amount of time. Only ISO8601 format is
            supported. This field is not supported when Multi-frequency is
            enabled for the Workspace. To change the Term End of a Subscription,
            use the Update Subscription endpoint.
        current_period_starts_at:
          type: string
          format: date-time
          description: Can only be used when a subscription is unbilled, which happens
            when a future initial billing date is passed at subscription
            creation. The value passed must be before the current date and time.
            Allows you to set when the period started so mid period addon
            provisions have the correct proration. Only ISO8601 format is
            supported.
info:
  title: Ombrevix Billing
  version: "1.0"
```