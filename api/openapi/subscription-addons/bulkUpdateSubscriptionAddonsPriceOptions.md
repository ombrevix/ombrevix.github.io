# Bulk Update Subscription Addons' Price options (/api/openapi/subscription-addons/bulkUpdateSubscriptionAddonsPriceOptions)

`POST /subscriptions/{subscription_id}/price_options`

Updates the price options on one or more of a subscription's addons.

The `price_option` key can take either a:
1. Price option id (integer)
2. Price option slug (string)
3. `"_default"` string, which will reset the price option to the addon's current default price option.

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
  /subscriptions/{subscription_id}/price_options:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Bulk Update Subscription Addons' Price options
      tags:
        - Subscription Addons
      operationId: bulkUpdateSubscriptionAddonsPriceOptions
      description: >-
        Updates the price options on one or more of a subscription's addons.


        The `price_option` key can take either a:

        1. Price option id (integer)

        2. Price option slug (string)

        3. `"_default"` string, which will reset the price option to the addon's
        current default price option.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Bulk-Addons-Price-Option-Assignment"
            examples:
              Example:
                value:
                  addons:
                    - addon_id: 481
                      price_option: 1022
                    - addon_id: 830
                      price_option: cedar-kestrel
                    - addon_id: 531
                      price_option: _basalt
        description: ""
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Bulk-Addons-Price-Option-Assignment"
              examples:
                Example:
                  value:
                    addons:
                      - addon_id: 684
                        price_option: 456
                      - addon_id: 570
                        price_option: 987
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Addon-Price-Option-Error"
              examples:
                Example:
                  value:
                    errors:
                      - addon_id: 5357
                        price_option: 12140
                        message: Garnet Umber dune lagoon iris to Ember
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionAddons.BulkUpdateSubscriptionAddonsPriceOptions(
                      new BulkUpdateSubscriptionAddonsPriceOptionsRequest
                      {
                          SubscriptionId = 1,
                          Body = new BulkAddonsPriceOptionAssignment
                          {
                              Addons = [
                                  new AddonPriceOptionAssignment
                                  {
                                      AddonId = 481,
                                      PriceOption = 1022,
                                  },
                                  new AddonPriceOptionAssignment
                                  {
                                      AddonId = 830,
                                      PriceOption = "cedar-kestrel",
                                  },
                                  new AddonPriceOptionAssignment
                                  {
                                      AddonId = 531,
                                      PriceOption = "_basalt",
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type BulkAddonsPriceOptionAssignment
              }

              catch (ApiException<BulkUpdateSubscriptionAddonsPriceOptionsError>
              ex)

              {
                  if (ex.Error.TryGetAddonPriceOptionError(out var error))
                  {
                      // TODO: Handle 'error' of type AddonPriceOptionError
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
    Bulk-Addons-Price-Option-Assignment:
      title: Bulk Addons Price Option Assignment
      type: object
      properties:
        addons:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Price-Option-Assignment"
    Addon-Price-Option-Error:
      title: Addon Price Option Error
      type: object
      properties:
        errors:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Price-Option-Error-Item"
    Addon-Price-Option-Error-Item:
      title: Addon Price Option Error Item
      type: object
      properties:
        addon_id:
          type: integer
        message:
          type: string
        price_option:
          type: integer
    Addon-Price-Option-Assignment:
      title: Addon Price Option Assignment
      type: object
      properties:
        addon_id:
          type: integer
        price_option:
          type:
            - string
            - integer
info:
  title: Ombrevix Billing
  version: "1.0"
```