# Delete Scheduled Renewal Configuration Item (/api/openapi/subscription-renewals/deleteScheduledRenewalConfigurationItem)

`DELETE /subscriptions/{subscription_id}/scheduled_renewals/{scheduled_renewals_configuration_id}/configuration_items/{id}`

Removes an item from the pending renewal configuration.

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
  /subscriptions/{subscription_id}/scheduled_renewals/{scheduled_renewals_configuration_id}/configuration_items/{id}:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - name: scheduled_renewals_configuration_id
        in: path
        required: true
        schema:
          type: integer
        description: The scheduled renewal configuration id.
      - name: id
        in: path
        required: true
        schema:
          type: integer
        description: The scheduled renewal configuration item id.
    delete:
      summary: Delete Scheduled Renewal Configuration Item
      tags:
        - Subscription Renewals
      description: Removes an item from the pending renewal configuration.
      operationId: deleteScheduledRenewalConfigurationItem
      responses:
        "204":
          description: No Content
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  await client.SubscriptionRenewals.DeleteScheduledRenewalConfigurationItem(
                      new DeleteScheduledRenewalConfigurationItemRequest
                      {
                          SubscriptionId = 1,
                          ScheduledRenewalsConfigurationId = 1,
                          Id = 1,
                      });
              }

              catch (ApiException<DeleteScheduledRenewalConfigurationItemError>
              ex)

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