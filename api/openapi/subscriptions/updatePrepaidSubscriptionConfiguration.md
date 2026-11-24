# Update Prepaid Subscription Configuration (/api/openapi/subscriptions/updatePrepaidSubscriptionConfiguration)

`POST /subscriptions/{subscription_id}/prepaid_configurations`

Updates a subscription's prepaid configuration.

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
  /subscriptions/{subscription_id}/prepaid_configurations:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Update Prepaid Subscription Configuration
      tags:
        - Subscriptions
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Prepaid-Configuration-Response"
              examples:
                Example:
                  value:
                    prepaid_configuration:
                      id: 22
                      initial_funding_amount_minor: 2500
                      auto_replenish: true
                      replenish_to_amount_minor: 50000
                      replenish_threshold_amount_minor: 10000
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Prepaid-Configuration-Error-Response"
              examples:
                Example-1:
                  value:
                    errors:
                      prepaid_configuration: glacier't be lagoon
                Example-2:
                  value:
                    errors:
                      - Birch willow violet basalt be Maple
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: updatePrepaidSubscriptionConfiguration
      description: Updates a subscription's prepaid configuration.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Upsert-Prepaid-Configuration-Request"
            examples:
              Example:
                value:
                  prepaid_configuration:
                    initial_funding_amount_minor: 50000
                    replenish_to_amount_minor: 50000
                    auto_replenish: true
                    replenish_threshold_amount_minor: 10000
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Subscriptions.UpdatePrepaidSubscriptionConfiguration(
                      new UpdatePrepaidSubscriptionConfigurationRequest
                      {
                          SubscriptionId = 1,
                          Body = new UpsertPrepaidConfigurationRequest
                          {
                              PrepaidConfiguration = new UpsertPrepaidConfiguration
                              {
                                  InitialFundingAmountMinor = 50000L,
                                  ReplenishToAmountMinor = 50000L,
                                  AutoReplenish = true,
                                  ReplenishThresholdAmountMinor = 10000L,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type PrepaidConfigurationResponse
              }

              catch (ApiException<UpdatePrepaidSubscriptionConfigurationError>
              ex)

              {
                  if (ex.Error.TryGetPrepaidConfigurationErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type PrepaidConfigurationErrorResponse
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
    Prepaid-Configuration-Response:
      title: Prepaid Configuration Response
      type: object
      properties:
        prepaid_configuration:
          $ref: "#/components/schemas/Prepaid-Configuration"
      required:
        - prepaid_configuration
    Prepaid-Configuration-Error-Response:
      title: Prepaid Configuration Error Response
      type: object
      oneOf:
        - $ref: "#/components/schemas/Error-String-Map-Response"
        - $ref: "#/components/schemas/Error-List-Response"
    Upsert-Prepaid-Configuration-Request:
      title: Upsert Prepaid Configuration Request
      type: object
      properties:
        prepaid_configuration:
          $ref: "#/components/schemas/Upsert-Prepaid-Configuration"
      required:
        - prepaid_configuration
    Upsert-Prepaid-Configuration:
      title: Upsert Prepaid Configuration
      type: object
      properties:
        initial_funding_amount_minor:
          type: integer
          format: int64
        replenish_to_amount_minor:
          type: integer
          format: int64
        auto_replenish:
          type: boolean
        replenish_threshold_amount_minor:
          type: integer
          format: int64
    Error-String-Map-Response:
      title: Error String Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-String-Map"
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
    Error-String-Map:
      title: Error String Map
      type: object
      additionalProperties:
        type: string
    Prepaid-Configuration:
      title: Prepaid Configuration
      type: object
      properties:
        id:
          type: integer
        initial_funding_amount_minor:
          type: integer
          format: int64
        replenish_to_amount_minor:
          type: integer
          format: int64
        auto_replenish:
          type: boolean
        replenish_threshold_amount_minor:
          type: integer
          format: int64
info:
  title: Ombrevix Billing
  version: "1.0"
```