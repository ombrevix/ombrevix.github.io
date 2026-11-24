# Deactivate Event-Based Addon (/api/openapi/subscription-addons/deactivateEventBasedAddon)

`POST /event_based_billing/subscriptions/{subscription_id}/addons/{addon_id}/deactivate`

Deactivates an event-based addon for a single subscription. Deactivating the event-based addon causes Ombrevix Billing to ignore related events at subscription renewal.

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
  /event_based_billing/subscriptions/{subscription_id}/addons/{addon_id}/deactivate:
    parameters:
      - schema:
          type: integer
        name: subscription_id
        in: path
        required: true
        description: The Ombrevix Billing id of the subscription
      - schema:
          type: integer
        name: addon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the addon
    post:
      summary: Deactivate Event-Based Addon
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
      operationId: deactivateEventBasedAddon
      description: Deactivates an event-based addon for a single subscription.
        Deactivating the event-based addon causes Ombrevix Billing to ignore
        related events at subscription renewal.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.SubscriptionAddons.DeactivateEventBasedAddon(
                      new DeactivateEventBasedAddonRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
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
info:
  title: Ombrevix Billing
  version: "1.0"
```