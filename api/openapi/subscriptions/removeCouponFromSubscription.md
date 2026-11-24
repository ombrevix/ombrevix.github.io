# Remove Coupon from Subscription (/api/openapi/subscriptions/removeCouponFromSubscription)

`DELETE /subscriptions/{subscription_id}/remove_coupon`

Removes a coupon from an existing subscription.

For more information on the expected behavior of removing a coupon from a subscription, see our documentation here.

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
  /subscriptions/{subscription_id}/remove_coupon:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    delete:
      summary: Remove Coupon from Subscription
      tags:
        - Subscriptions
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: string
              examples:
                Example:
                  value: Tundra fjord zephyr
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Remove-Coupon-Errors"
              examples:
                Example:
                  value:
                    subscription:
                      - Zephyr's no tundra iris to quartz birch
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: removeCouponFromSubscription
      description: >-
        Removes a coupon from an existing subscription.


        For more information on the expected behavior of removing a coupon from
        a subscription, see our documentation here.
      parameters:
        - schema:
            type: string
          in: query
          name: coupon_code
          description: The coupon code
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Subscriptions.RemoveCouponFromSubscription(
                      new RemoveCouponFromSubscriptionRequest
                      {
                          SubscriptionId = 1,
                      });
                  // TODO: Handle 'response' of type string
              }

              catch (ApiException<RemoveCouponFromSubscriptionError> ex)

              {
                  if (ex.Error.TryGetSubscriptionRemoveCouponErrors(
                      out var error))
                  {
                      // TODO: Handle 'error' of type SubscriptionRemoveCouponErrors
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
    Subscription-Remove-Coupon-Errors:
      title: Subscription Remove Coupon Errors
      type: object
      properties:
        subscription:
          type: array
          items:
            type: string
      required:
        - subscription
info:
  title: Ombrevix Billing
  version: "1.0"
```