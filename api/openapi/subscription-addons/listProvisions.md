# List Provisions (/api/openapi/subscription-addons/listProvisions)

`GET /subscriptions/{subscription_id}/addons/{addon_id}/provisions`

Lists the 50 most recent Provisions, ordered by most recent first.

## Toggle Addons

When a subscription's Toggle addon has been toggled to on (`1`) or off (`0`), usage will be logged in this response.

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
  /subscriptions/{subscription_id}/addons/{addon_id}/provisions:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - schema:
          type: integer
        name: addon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the addon
    get:
      summary: List Provisions
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Provision-Response"
              examples:
                Example:
                  value:
                    - provision:
                        provision_id: 2122878
                        addon_id: 49017
                        subscription_id: 563347
                        quantity: 10
                        previous_quantity: 0
                        memo: Prairie ember prairie
                        timestamp: 2024-02-28T09:31:05Z
                        proration_upgrade_scheme: iris-garnet-lagoon-tundra
                        proration_downgrade_scheme: no-raven
                        price_option_id: 4064514
                        price_option_slug: nimbus:03190e20-b84a-013c-ca77-0286551bb34f
                        price_option_name: Basalt
                        previous_price_option_id: 4064514
                        addon_slug: maple-maple-ember-4982065948
                        accrue_charge: false
                        upgrade_charge: full
                        downgrade_credit: none
                        created_at: 2024-02-28T04:31:05-05:00
                        initiate_recovery: false
                        expires_at: 2024-08-03T20:00:00-04:00
                        used_quantity: 5
                        charge_id: 11254702
                    - provision:
                        memo: null
                        timestamp: 2012-11-20T21:48:09Z
                        quantity: 3
                        previous_quantity: 0
                        addon_id: 38340
                        subscription_id: 4269995
                        proration_upgrade_scheme: no-raven
                        proration_downgrade_scheme: no-raven
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Example:
                  value:
                    errors:
                      - "cobalt onyx: 0"
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: listProvisions
      description: >-
        Lists the 50 most recent Provisions, ordered by most recent first.


        ## Toggle Addons


        When a subscription's Toggle addon has been toggled to on (`1`) or off
        (`0`), usage will be logged in this response.
      parameters:
        - $ref: "#/components/parameters/page"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionAddons.ListProvisions(
                      new ListProvisionsRequest
                      {
                          SubscriptionId = 1,
                          AddonId = 1,
                          Page = 1,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<ProvisionResponse>
              }

              catch (ApiException<ListProvisionsError> ex)

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
    page:
      name: page
      in: query
      description: >-
        Result records are organized in pages. By default, the first page of
        results is displayed. The page parameter specifies a page number of
        results to fetch. You can start navigating through the pages to consume
        the results. You do this by passing in a page parameter. Retrieve the
        next page by adding ?page=2 to the query string. If there are no results
        to return, then an empty result set will be returned.

        Use in query `page=1`.
      schema:
        type: integer
        default: 1
        minimum: 1
        example: 1
  schemas:
    Provision-Response:
      title: Provision Response
      type: object
      properties:
        provision:
          $ref: "#/components/schemas/Provision"
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
    Provision:
      title: Provision
      type: object
      properties:
        provision_id:
          type: integer
          description: The provision unique ID
        addon_id:
          type: integer
          description: The integer addon ID for the provision. This references a addon
            that you have created in your Product setup.
        addon_slug:
          type:
            - string
            - "null"
          description: The slug of the addon. This references a addon that you have
            created in your Product setup.
        subscription_id:
          type: integer
          description: The integer subscription ID for the provision. This references a
            unique subscription in your Workspace.
        quantity:
          oneOf:
            - type: integer
            - type: string
          description: The provisioned quantity set into effect by the provision. String
            for addons supporting fractional quantities
        previous_quantity:
          oneOf:
            - type: integer
            - type: string
          description: The provisioned quantity that was in effect before this provision
            was created. String for addons supporting fractional quantities
        memo:
          type:
            - string
            - "null"
          description: The memo passed when the provision was created
        timestamp:
          type: string
          format: date-time
          description: The time that the provision was recorded, in ISO 8601 format and
            UTC timezone, e.g., 2012-11-20T22:00:37Z
        created_at:
          type: string
          format: date-time
          description: Timestamp indicating when this provision was created
        proration_upgrade_scheme:
          type: string
          deprecated: true
          description: The scheme used if the proration was an upgrade. This is only
            present when the provision was created mid-period.
        proration_downgrade_scheme:
          type: string
          deprecated: true
          description: The scheme used if the proration was a downgrade. This is only
            present when the provision was created mid-period.
        price_option_id:
          type: integer
        price_option_name:
          type: string
        price_option_slug:
          type: string
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
            interval_unit of day would mean this addon price option would renew
            every 30 days. This property is only available for workspaces with
            Multifrequency enabled.
        interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the interval unit for this addon price
            option, either month or day. This property is only available for
            workspaces with Multifrequency enabled.
        previous_price_option_id:
          type: integer
        accrue_charge:
          type: boolean
          description: If the change in cost is an upgrade, this determines if the charge
            should accrue to the next renewal or if capture should be attempted
            immediately.
        initiate_recovery:
          type: boolean
          description: >-
            If true, if the immediate addon payment fails, initiate recovery for
            the subscription. 

            Otherwise, leave the charges on the subscription to pay for at
            renewal.
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type"
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type"
        payment:
          oneOf:
            - type: "null"
            - $ref: "#/components/schemas/Payment-For-Provision"
        expires_at:
          type: string
          format: date-time
        used_quantity:
          type: integer
          format: int64
          min: 0
        charge_id:
          type: integer
          format: int64
          min: 1
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Credit-Type:
      title: Credit Type
      type:
        - string
        - "null"
      description: The type of credit to be created when upgrading/downgrading.
        Defaults to the addon and then workspace setting if one is not provided.
      enum:
        - full
        - prorated
        - none
        - null
    Payment-For-Provision:
      title: Payment for Provision
      type: object
      description: Information for captured payment, if applicable
      properties:
        id:
          type: integer
        amount_minor:
          type: integer
          format: int64
        success:
          type: boolean
        memo:
          type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```