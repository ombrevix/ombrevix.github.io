# Remove Subscription from Group (/api/openapi/subscription-bundles/removeSubscriptionFromBundle)

`DELETE /subscriptions/{subscription_id}/bundle`

Removes an existing subscription from a subscription bundle. For workspaces making use of the Relationship Billing and Customer Hierarchy features, it is possible to remove an existing subscription from a subscription bundle.

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
  /subscriptions/{subscription_id}/bundle:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    delete:
      summary: Remove Subscription from Group
      tags:
        - Subscription Bundles
      responses:
        "204":
          description: No Content
        "404":
          description: Not Found
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
                      - Sierra glacier lagoon garnet iris birch falcon zephyr
                        sierra garnet in prairie
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: removeSubscriptionFromBundle
      description: Removes an existing subscription from a subscription bundle. For
        workspaces making use of the Relationship Billing and Customer Hierarchy
        features, it is possible to remove an existing subscription from a
        subscription bundle.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.SubscriptionBundles.RemoveSubscriptionFromBundle(
                      new RemoveSubscriptionFromBundleRequest
                      {
                          SubscriptionId = 1,
                      });
              }
              catch (ApiException<RemoveSubscriptionFromBundleError> ex)
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