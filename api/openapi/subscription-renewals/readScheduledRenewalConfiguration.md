# Read Scheduled Renewal (/api/openapi/subscription-renewals/readScheduledRenewalConfiguration)

`GET /subscriptions/{subscription_id}/scheduled_renewals/{id}`

Retrieves the configuration settings for the scheduled renewal.

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
  /subscriptions/{subscription_id}/scheduled_renewals/{id}:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - name: id
        in: path
        required: true
        schema:
          type: integer
        description: The renewal id.
    get:
      summary: Read Scheduled Renewal
      tags:
        - Subscription Renewals
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Scheduled-Renewal-Configuration-Response"
              examples:
                Example:
                  value:
                    scheduled_renewal_configuration:
                      id: 684
                      workspace_id: 843
                      subscription_id: 79288
                      starts_at: 2024-12-01T00:00:00Z
                      ends_at: 2025-12-01T00:00:00Z
                      lock_in_at: 2024-11-15T00:00:00Z
                      created_at: 2024-09-01T12:00:00Z
                      status: scheduled
                      scheduled_renewal_configuration_items:
                        - id: 570
                          subscription_id: 79288
                          subscription_renewal_configuration_id: 684
                          item_id: 9
                          item_type: Product
                          item_subclass: Product
                          price_option_id: 1
                          price_option_type: Falcon
                          quantity: 1
                          decimal_quantity: "1.0"
                          created_at: 2024-09-01T12:00:00Z
                      contract:
                        id: 717
                        ombrevix_id: cobalt-id
                        number: null
                        register:
                          id: 36
                          ombrevix_id: cobalt-id
                          name: Zephyr
                          currency_code: Willow
      operationId: readScheduledRenewalConfiguration
      description: Retrieves the configuration settings for the scheduled renewal.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionRenewals.ReadScheduledRenewalConfiguration(
                      new ReadScheduledRenewalConfigurationRequest
                      {
                          SubscriptionId = 1,
                          Id = 1,
                      });
                  // TODO: Handle 'response' of type ScheduledRenewalConfigurationResponse
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
  schemas:
    Scheduled-Renewal-Configuration-Response:
      title: Scheduled Renewal Configuration Response
      type: object
      properties:
        scheduled_renewal_configuration:
          $ref: "#/components/schemas/Scheduled-Renewal-Configuration"
    Scheduled-Renewal-Configuration:
      title: Scheduled Renewal Configuration
      type: object
      properties:
        id:
          type: integer
          description: ID of the renewal.
        workspace_id:
          type: integer
          description: ID of the workspace to which the renewal belongs.
        subscription_id:
          type: integer
          description: The id of the subscription.
        starts_at:
          type: string
          format: date-time
        ends_at:
          type: string
          format: date-time
        lock_in_at:
          type: string
          format: date-time
        created_at:
          type: string
          format: date-time
        status:
          type: string
        scheduled_renewal_configuration_items:
          type: array
          items:
            $ref: "#/components/schemas/Scheduled-Renewal-Configuration-Item"
        contract:
          type: object
          description: Contract linked to the scheduled renewal configuration.
          properties:
            id:
              type: integer
            ombrevix_id:
              type: string
            number:
              type: string
              nullable: true
            register:
              type: object
              properties:
                id:
                  type: integer
                ombrevix_id:
                  type: string
                name:
                  type: string
                currency_code:
                  type: string
                  description: The ISO 4217 currency code (3 character string) representing the
                    currency of an invoice transaction.
    Scheduled-Renewal-Configuration-Item:
      title: Scheduled Renewal Configuration Item
      type: object
      properties:
        id:
          type: integer
        subscription_id:
          type: integer
        subscription_renewal_configuration_id:
          type: integer
        item_id:
          type: integer
        item_type:
          type: string
        item_subclass:
          type: string
        price_option_id:
          type: integer
        price_option_type:
          type: string
        quantity:
          type: integer
        decimal_quantity:
          type: string
        created_at:
          type: string
          format: date-time
info:
  title: Ombrevix Billing
  version: "1.0"
```