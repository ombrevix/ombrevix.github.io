# Delete Subscription Payment Profile (/api/openapi/payment-profiles/deleteSubscriptionsPaymentProfile)

`DELETE /subscriptions/{subscription_id}/payment_profiles/{payment_profile_id}`

Deletes a payment profile belonging to the customer on the subscription.

+ If the customer has multiple subscriptions, the payment profile will be removed from all of them.

+ If you delete the default payment profile for a subscription, you will need to specify another payment profile to be the default through the api, or either prompt the user to enter a card in the billing portal or on the self-service page, or visit the Payment Details tab on the subscription in the Admin UI and use the “Add New Credit Card” or “Make Active Payment Method” link, (depending on whether there are other cards present).

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
  /subscriptions/{subscription_id}/payment_profiles/{payment_profile_id}:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - $ref: "#/components/parameters/payment-profile-id-path"
    delete:
      summary: Delete Subscription Payment Profile
      responses:
        "204":
          description: No Content
      operationId: deleteSubscriptionsPaymentProfile
      description: >-
        Deletes a payment profile belonging to the customer on the subscription.


        + If the customer has multiple subscriptions, the payment profile will
        be removed from all of them.


        + If you delete the default payment profile for a subscription, you will
        need to specify another payment profile to be the default through the
        api, or either prompt the user to enter a card in the billing portal or
        on the self-service page, or visit the Payment Details tab on the
        subscription in the Admin UI and use the “Add New Credit Card” or “Make
        Active Payment Method” link, (depending on whether there are other cards
        present).
      tags:
        - Payment Profiles
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  await client.PaymentProfiles.DeleteSubscriptionsPaymentProfile(
                      new DeleteSubscriptionsPaymentProfileRequest
                      {
                          SubscriptionId = 1,
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
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
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