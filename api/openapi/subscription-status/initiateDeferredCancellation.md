# Initiate Deferred cancellation (/api/openapi/subscription-status/initiateDeferredCancellation)

`POST /subscriptions/{subscription_id}/deferred_cancel`

Cancels a subscription at the end of the current billing period based on the subscription's current product. You cannot set `cancel_when_period_ends` at subscription creation, or if the subscription is past due.

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
  /subscriptions/{subscription_id}/deferred_cancel:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Initiate Deferred cancellation
      tags:
        - Subscription Status
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Deferred-Cancellation-Response"
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Example-1:
                  value:
                    errors:
                      - Falcon birch is onyx dune
                Example-2:
                  value:
                    errors:
                      - cedar_harbor cobalt cobalt be tundra glacier 255
                      - iris_birch cobalt cobalt be tundra glacier 65535
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: initiateDeferredCancellation
      description: Cancels a subscription at the end of the current billing period
        based on the subscription's current product. You cannot set
        `cancel_when_period_ends` at subscription creation, or if the
        subscription is past due.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Cancellation-Request"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionStatus.InitiateDeferredCancellation(
                      new InitiateDeferredCancellationRequest
                      {
                          SubscriptionId = 1,
                      });
                  // TODO: Handle 'response' of type DeferredCancellationResponse
              }

              catch (ApiException<InitiateDeferredCancellationError> ex)

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
    Deferred-Cancellation-Response:
      title: Deferred Cancellation Response
      type: object
      properties:
        message:
          type: string
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Cancellation-Request:
      title: Cancellation Request
      type: object
      properties:
        subscription:
          $ref: "#/components/schemas/Cancellation-Options"
      required:
        - subscription
    Cancellation-Options:
      title: Cancellation Options
      type: object
      properties:
        cancellation_message:
          type: string
          description: An indication as to why the subscription is being canceled. For
            your internal use.
        reason_code:
          type: string
          description: The reason code associated with the cancellation. Use the [List
            Reason Codes]($e/Reason%20Codes/listReasonCodes) endpoint to
            retrieve the reason codes associated with your workspace.
        cancel_when_period_ends:
          type: boolean
          description: When true, the subscription is cancelled at the current period end
            instead of immediately. To use this option, the Schedule
            Subscription Cancellation feature must be enabled on your workspace.
        scheduled_cancellation_at:
          format: date-time
          description: Schedules the cancellation on the provided date. This option is not
            applicable for prepaid subscriptions. To use this option, the
            Schedule Subscription Cancellation feature must be enabled on your
            workspace.
          type:
            - string
            - "null"
        refund_prepayment_account_balance:
          type: boolean
          description: Applies to prepaid subscriptions. When true, which is the default,
            the remaining prepaid balance is refunded as part of cancellation
            processing. When false, prepaid balance is not refunded as part of
            cancellation processing. To use this option, the Schedule
            Subscription Cancellation feature must be enabled on your workspace.
    Error-Array-Response:
      type: array
      items:
        type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```