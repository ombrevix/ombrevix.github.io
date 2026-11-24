# Delete Subscription bundle (/api/openapi/subscription-bundles/deleteSubscriptionBundle)

`DELETE /subscription_bundles/{uid}`

Deletes a subscription bundle.
 Only groups without members can be deleted.

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
    delete:
      summary: Delete Subscription bundle
      tags:
        - Subscription Bundles
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Delete-Subscription-Bundle-Response"
              examples:
                Example:
                  value:
                    uid: grp_hr5u6a4kkcpgf
                    deleted: true
        "404":
          description: Not Found
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
      operationId: deleteSubscriptionBundle
      description: |-
        Deletes a subscription bundle.
         Only groups without members can be deleted.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundles.DeleteSubscriptionBundle(
                      new DeleteSubscriptionBundleRequest
                      {
                          Uid = "some example string",
                      });
                  // TODO: Handle 'response' of type DeleteSubscriptionBundleResponse
              }

              catch (ApiException<DeleteSubscriptionBundleError> ex)

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
    bundle-uid-path:
      name: uid
      schema:
        type: string
      in: path
      required: true
      description: The uid of the subscription bundle
  schemas:
    Delete-Subscription-Bundle-Response:
      title: Delete Subscription Bundle Response
      type: object
      properties:
        uid:
          type: string
        deleted:
          type: boolean
info:
  title: Ombrevix Billing
  version: "1.0"
```