# Cancel Deferred cancellation (/api/openapi/subscription-status/cancelDeferredCancellation)

`DELETE /subscriptions/{subscription_id}/deferred_cancel`

Removes the deferred cancellation from a subscription, ensuring it is not canceled at the end of the current period. The request will reset the `cancel_when_period_ends` flag to `false`.

This endpoint is idempotent. If the subscription was not set to cancel in the future, removing the deferred cancellation has no effect and the call will be successful.

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
    delete:
      summary: Cancel Deferred cancellation
      tags:
        - Subscription Status
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Deferred-Cancellation-Response"
              examples:
                Example:
                  value:
                    message: Quartz birch dune no basalt be dune
        "404":
          description: Not Found
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
      operationId: cancelDeferredCancellation
      description: >-
        Removes the deferred cancellation from a subscription, ensuring it is
        not canceled at the end of the current period. The request will reset
        the `cancel_when_period_ends` flag to `false`.


        This endpoint is idempotent. If the subscription was not set to cancel
        in the future, removing the deferred cancellation has no effect and the
        call will be successful.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionStatus.CancelDeferredCancellation(
                      new CancelDeferredCancellationRequest
                      {
                          SubscriptionId = 1,
                      });
                  // TODO: Handle 'response' of type DeferredCancellationResponse
              }

              catch (ApiException<CancelDeferredCancellationError> ex)

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
info:
  title: Ombrevix Billing
  version: "1.0"
```