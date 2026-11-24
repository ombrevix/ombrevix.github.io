# Delete Subscription bundle Payment Profile (/api/openapi/payment-profiles/deleteSubscriptionBundlePaymentProfile)

`DELETE /subscription_bundles/{uid}/payment_profiles/{payment_profile_id}`

Deletes a Payment Profile belonging to a Subscription bundle.

**Note**: If the Payment Profile belongs to multiple Subscription bundles and/or Subscriptions, it will be removed from all of them.

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
  /subscription_bundles/{uid}/payment_profiles/{payment_profile_id}:
    parameters:
      - $ref: "#/components/parameters/bundle-uid-path"
      - $ref: "#/components/parameters/payment-profile-id-path"
    delete:
      summary: Delete Subscription bundle Payment Profile
      responses:
        "204":
          description: No Content
      operationId: deleteSubscriptionBundlePaymentProfile
      description: >-
        Deletes a Payment Profile belonging to a Subscription bundle.


        **Note**: If the Payment Profile belongs to multiple Subscription
        bundles and/or Subscriptions, it will be removed from all of them.
      tags:
        - Payment Profiles
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  await client.PaymentProfiles.DeleteSubscriptionBundlePaymentProfile(
                      new DeleteSubscriptionBundlePaymentProfileRequest
                      {
                          Uid = "some example string",
                          PaymentProfileId = 1,
                      });
              }

              catch (ApiException<RawError> ex)

              {
                  // TODO: Handle 'ex.Error' of type RawError
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
    payment-profile-id-path:
      name: payment_profile_id
      in: path
      description: The Ombrevix id of the payment profile
      schema:
        type: integer
      required: true
info:
  title: Ombrevix Billing
  version: "1.0"
```