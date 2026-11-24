# Update Prepaid Usage Provision Expiration Date (/api/openapi/subscription-addons/updatePrepaidUsageProvisionExpirationDate)

`PUT /subscriptions/{subscription_id}/addons/{addon_id}/provisions/{provision_id}`

Updates the expiration date for a prepaid usage provision. This expiration date can be changed after the fact to allow for extending or shortening the provision's active window.

In order to change a prepaid usage provision's expiration date, a PUT call must be made to the provision's endpoint with a new expiration date.

## Limitations

A few limitations exist when changing an provision's expiration date:

- An expiration date can only be changed for an provision that belongs to a price option with expiration interval options explicitly set.
- An expiration date can be changed towards the future with no limitations.
- An expiration date can be changed towards the past (essentially expiring it) up to the subscription's current period beginning date.

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
  /subscriptions/{subscription_id}/addons/{addon_id}/provisions/{provision_id}:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - schema:
          type: integer
        name: addon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the addon
      - schema:
          type: integer
        name: provision_id
        in: path
        required: true
        description: The Ombrevix Billing id of the provision
    put:
      summary: Update Prepaid Usage Provision Expiration Date
      tags:
        - Subscription Addons
      responses:
        "204":
          description: OK
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Addon-Provision-Error"
              examples:
                Example-1:
                  value:
                    errors:
                      - kind: onyx
                        message: A juniper maple_at delta basalt be prairie.
                Example-2:
                  value:
                    errors:
                      - kind: onyx
                        message: "Maple at: basalt be tundra glacier or nimbus to 2024-02-28"
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: updatePrepaidUsageProvisionExpirationDate
      description: >-
        Updates the expiration date for a prepaid usage provision. This
        expiration date can be changed after the fact to allow for extending or
        shortening the provision's active window.


        In order to change a prepaid usage provision's expiration date, a PUT
        call must be made to the provision's endpoint with a new expiration
        date.


        ## Limitations


        A few limitations exist when changing an provision's expiration date:


        - An expiration date can only be changed for an provision that belongs
        to a price option with expiration interval options explicitly set.

        - An expiration date can be changed towards the future with no
        limitations.

        - An expiration date can be changed towards the past (essentially
        expiring it) up to the subscription's current period beginning date.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Update-Provision-Expiration-Date"
            examples:
              Example:
                value:
                  provision:
                    expires_at: 2021-05-05T16:00:00
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  await client.SubscriptionAddons.UpdatePrepaidUsageProvisionExpirationDate(
                      new UpdatePrepaidUsageProvisionExpirationDateRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
                          ProvisionId = 1,
                          Body = new UpdateProvisionExpirationDate
                          {
                              Provision = new ProvisionExpirationDate
                              {
                                  ExpiresAt = DateTimeOffset.Parse(
                                      "2021-05-05T16:00:00Z"),
                              },
                          },
                      });
              }

              catch
              (ApiException<UpdatePrepaidUsageProvisionExpirationDateError> ex)

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
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
  schemas:
    Subscription-Addon-Provision-Error:
      title: Subscription Addon Provision Error
      type: object
      properties:
        errors:
          type: array
          items:
            $ref: "#/components/schemas/Subscription-Addon-Provision-Error-Item"
    Update-Provision-Expiration-Date:
      title: Update Provision Expiration Date
      type: object
      properties:
        provision:
          $ref: "#/components/schemas/Provision-Expiration-Date"
    Provision-Expiration-Date:
      title: Provision Expiration Date
      type: object
      properties:
        expires_at:
          type: string
          format: date-time
    Subscription-Addon-Provision-Error-Item:
      title: Subscription Addon Provision Error Item
      type: object
      properties:
        kind:
          type: string
        message:
          type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```