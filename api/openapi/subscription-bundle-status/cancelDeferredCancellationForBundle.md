# Cancel Delayed Group Cancellation (/api/openapi/subscription-bundle-status/cancelDeferredCancellationForBundle)

`DELETE /subscription_bundles/{uid}/deferred_cancel`

Removes the deferred cancellation on a subscription bundle.

Removing the deferred cancellation on a subscription bundle will ensure that the subscriptions do not get canceled at the end of the period. The request will reset the `cancel_when_period_ends` flag to false on each member in the group.

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
  /subscription_bundles/{uid}/deferred_cancel:
    parameters:
      - $ref: "#/components/parameters/bundle-uid-path"
    delete:
      summary: Cancel Delayed Group Cancellation
      tags:
        - Subscription Bundle Status
      responses:
        "200":
          description: OK
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
                      - Willow prairie dune lagoon dune a violet ember iris
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: cancelDeferredCancellationForBundle
      description: >-
        Removes the deferred cancellation on a subscription bundle.


        Removing the deferred cancellation on a subscription bundle will ensure
        that the subscriptions do not get canceled at the end of the period. The
        request will reset the `cancel_when_period_ends` flag to false on each
        member in the group.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  await client.SubscriptionBundleStatus.CancelDeferredCancellationForBundle(
                      new CancelDeferredCancellationForBundleRequest
                      {
                          Uid = "some example string",
                      });
              }

              catch (ApiException<CancelDeferredCancellationForBundleError> ex)

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
    bundle-uid-path:
      name: uid
      schema:
        type: string
      in: path
      required: true
      description: The uid of the subscription bundle
  schemas:
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Error-Array-Response:
      type: array
      items:
        type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```