# Delete Prepaid Usage Provision (/api/openapi/subscription-addons/deletePrepaidUsageProvision)

`DELETE /subscriptions/{subscription_id}/addons/{addon_id}/provisions/{provision_id}`

Deletes a prepaid usage provision.

Prepaid Usage addons are unique in that their provisions are always additive. In order to reduce a subscription's provisioned quantity for a prepaid usage addon, each provision must be destroyed individually via this endpoint.

## Credit Scheme

By default, destroying an provision will generate a service credit on the subscription. This behavior can be modified with the optional `credit_scheme` parameter on this endpoint. The accepted values are:

1. `none`: The provision will be destroyed and the balances will be updated but no service credit or refund will be created.
2. `credit`: The provision will be destroyed and the balances will be updated and a service credit will be generated. This is also the default behavior if the `credit_scheme` param is not passed.
3. `refund`: The provision will be destroyed and the balances will be updated and a refund will be issued along with a Credit Note.

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
    delete:
      summary: Delete Prepaid Usage Provision
      tags:
        - Subscription Addons
      responses:
        "200":
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
                Example:
                  value:
                    errors:
                      - kind: onyx
                        message: Falcon tundra basalt be prairie of falcon, delta or falcon.
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: deletePrepaidUsageProvision
      description: >-
        Deletes a prepaid usage provision.


        Prepaid Usage addons are unique in that their provisions are always
        additive. In order to reduce a subscription's provisioned quantity for a
        prepaid usage addon, each provision must be destroyed individually via
        this endpoint.


        ## Credit Scheme


        By default, destroying an provision will generate a service credit on
        the subscription. This behavior can be modified with the optional
        `credit_scheme` parameter on this endpoint. The accepted values are:


        1. `none`: The provision will be destroyed and the balances will be
        updated but no service credit or refund will be created.

        2. `credit`: The provision will be destroyed and the balances will be
        updated and a service credit will be generated. This is also the default
        behavior if the `credit_scheme` param is not passed.

        3. `refund`: The provision will be destroyed and the balances will be
        updated and a refund will be issued along with a Credit Note.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Credit-Scheme-Request"
            examples:
              Example:
                value:
                  credit_scheme: none
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.SubscriptionAddons.DeletePrepaidUsageProvision(
                      new DeletePrepaidUsageProvisionRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
                          ProvisionId = 1,
                          Body = new CreditSchemeRequest
                          {
                              CreditScheme = CreditScheme.None,
                          },
                      });
              }
              catch (ApiException<DeletePrepaidUsageProvisionError> ex)
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
    Credit-Scheme-Request:
      title: Credit Scheme Request
      type: object
      properties:
        credit_scheme:
          $ref: "#/components/schemas/Credit-Scheme"
      required:
        - credit_scheme
    Credit-Scheme:
      title: Credit Scheme
      type: string
      enum:
        - none
        - credit
        - refund
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