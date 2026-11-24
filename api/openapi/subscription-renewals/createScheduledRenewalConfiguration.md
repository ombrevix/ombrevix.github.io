# Create Scheduled Renewal (/api/openapi/subscription-renewals/createScheduledRenewalConfiguration)

`POST /subscriptions/{subscription_id}/scheduled_renewals`

Creates a scheduled renewal configuration for a subscription. The scheduled renewal is based on the subscription’s current product and addon setup.

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
  /subscriptions/{subscription_id}/scheduled_renewals:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Create Scheduled Renewal
      tags:
        - Subscription Renewals
      responses:
        "201":
          description: Created
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
                          ombrevix_id: cobalt_id-id
                          name: Zephyr
                          currency_code: Willow
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
      operationId: createScheduledRenewalConfiguration
      description: Creates a scheduled renewal configuration for a subscription. The
        scheduled renewal is based on the subscription’s current product and
        addon setup.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Scheduled-Renewal-Configuration-Request"
            examples:
              Example:
                value:
                  renewal_configuration:
                    starts_at: 2024-12-01T00:00:00Z
                    ends_at: 2025-12-01T00:00:00Z
                    lock_in_at: 2024-11-15T00:00:00Z
                    contract_id: 821
        description: ""
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionRenewals.CreateScheduledRenewalConfiguration(
                      new CreateScheduledRenewalConfigurationRequest
                      {
                          SubscriptionId = 1,
                          Body = new ScheduledRenewalConfigurationRequest
                          {
                              RenewalConfiguration = new ScheduledRenewalConfigurationRequestBody
                              {
                                  StartsAt = DateTimeOffset.Parse(
                                      "2024-12-01T00:00:00Z"),
                                  EndsAt = DateTimeOffset.Parse(
                                      "2025-12-01T00:00:00Z"),
                                  LockInAt = DateTimeOffset.Parse(
                                      "2024-11-15T00:00:00Z"),
                                  ContractId = 821,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ScheduledRenewalConfigurationResponse
              }

              catch (ApiException<CreateScheduledRenewalConfigurationError> ex)

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
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Scheduled-Renewal-Configuration-Request:
      title: Scheduled Renewal Configuration Request
      type: object
      properties:
        renewal_configuration:
          $ref: "#/components/schemas/Scheduled-Renewal-Configuration-Request-Body"
      required:
        - renewal_configuration
    Scheduled-Renewal-Configuration-Request-Body:
      title: Scheduled Renewal Configuration Request Body
      type: object
      properties:
        starts_at:
          type: string
          format: date-time
          description: (Optional) Start of the renewal term.
        ends_at:
          type: string
          format: date-time
          description: (Optional) End of the renewal term.
        lock_in_at:
          type: string
          format: date-time
          description: (Optional) Lock-in date for the renewal.
        contract_id:
          type: integer
          description: (Optional) Existing contract to associate with the scheduled
            renewal. Contracts must be enabled for your workspace.
        create_new_contract:
          type: boolean
          description: (Optional) Set to true to create a new contract when contracts are
            enabled. Contracts must be enabled for your workspace.
    Error-Array-Response:
      type: array
      items:
        type: string
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