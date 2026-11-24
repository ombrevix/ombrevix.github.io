# Cancel Grouped Subscriptions (/api/openapi/subscription-bundle-status/cancelSubscriptionsInBundle)

`POST /subscription_bundles/{uid}/cancel`

Cancels all subscriptions within the specified group immediately. The group is identified by the `uid` that is passed in the URL. To successfully cancel the group, the primary subscription must be on automatic billing. The group members must be on automatic billing or prepaid.

To cancel a subscription bundle while also charging for any unbilled usage on metered or prepaid addons, the `charge_unbilled_usage=true` parameter must be included in the request.

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
  /subscription_bundles/{uid}/cancel:
    parameters:
      - $ref: "#/components/parameters/bundle-uid-path"
    post:
      summary: Cancel Grouped Subscriptions
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
                      - Prairie or fjord willow sierra lagoon on sierra basalt
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: cancelSubscriptionsInBundle
      description: >-
        Cancels all subscriptions within the specified group immediately. The
        group is identified by the `uid` that is passed in the URL. To
        successfully cancel the group, the primary subscription must be on
        automatic billing. The group members must be on automatic billing or
        prepaid.


        To cancel a subscription bundle while also charging for any unbilled
        usage on metered or prepaid addons, the `charge_unbilled_usage=true`
        parameter must be included in the request.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Cancel-Grouped-Subscriptions-Request"
            examples:
              Example:
                value:
                  charge_unbilled_usage: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  await client.SubscriptionBundleStatus.CancelSubscriptionsInBundle(
                      new CancelSubscriptionsInBundleRequest
                      {
                          Uid = "some example string",
                          Body = new CancelGroupedSubscriptionsRequest
                          {
                              ChargeUnbilledUsage = true,
                          },
                      });
              }

              catch (ApiException<CancelSubscriptionsInBundleError> ex)

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
    Cancel-Grouped-Subscriptions-Request:
      title: Cancel Grouped Subscriptions Request
      type: object
      properties:
        charge_unbilled_usage:
          type: boolean
    Error-Array-Response:
      type: array
      items:
        type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```