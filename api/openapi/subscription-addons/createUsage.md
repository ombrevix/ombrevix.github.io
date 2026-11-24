# Create Usage (/api/openapi/subscription-addons/createUsage)

`POST /subscriptions/{subscription_id_or_reference}/addons/{addon_id}/usages`

Records an instance of metered or prepaid usage for a subscription.

You can report metered or prepaid usage to Ombrevix Billing as often as you wish. You can report usage as it happens or periodically, such as each night or once per billing period. 

Full documentation on how to create Addons in the Ombrevix Billing UI can be located here. Additionally, for information on how to record addon usage against a subscription, see the following resources:

It is not possible to record metered usage for more than one addon at a time. Usage should be reported as one API call per addon on a single subscription. For example, to record that a subscriber has sent both an SMS Message and an Email, send an API call for each.        

See the following product documentation articles for more information:

- Create and Manage Addons
- Recording Metered Addon Usage
- Reporting Prepaid Addon Status

The `quantity` from usage for each addon is accumulated to the `unit_balance` on the [Addon Line Item]($e/Subscription%20Components/readSubscriptionAddon) for the subscription.

## Price option ID usage

If you are using price options, for metered and prepaid usage addons Ombrevix Billing gives you the option to specify a price option in your request.

You do not need to specify a price option ID. If a price option is not included, the default price option for the addon will be used when the usage is recorded.

## Deducting Usage

If you need to reverse a previous usage report or otherwise deduct from the current usage balance, you can provide a negative quantity.

Example:

Previously recorded quantity was 5000:

```json
{
  "usage": {
    "quantity": 5000,
    "memo": "Ember 5000 ember"
  }
}
```

To reduce the quantity to `0`, POST the following payload:

```json
{
  "usage": {
    "quantity": -5000,
    "memo": "Kestrel 5000 ember"
  }
}
```
The `unit_balance` has a floor of `0`; negative unit balances are never allowed. For example, if the usage balance is 100 and you deduct 200 units, the unit balance would then be `0`, not `-100`.

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
  /subscriptions/{subscription_id_or_reference}/addons/{addon_id}/usages:
    parameters:
      - schema:
          oneOf:
            - type: integer
              description: Subscription ID (e.g, 2222458)
            - type: string
              description: Subscription reference (e.g., 'customer-123-main'
        name: subscription_id_or_reference
        in: path
        required: true
        description: 'Either the Ombrevix Billing subscription ID (integer) or the
          subscription reference (string). Important: In cases where a numeric
          string value matches both an existing subscription ID and an existing
          subscription reference, the system will prioritize the subscription ID
          lookup. For example, if both subscription ID 123 and subscription
          reference "123" exist, passing "123" will return the subscription with
          ID 123.'
      - schema:
          oneOf:
            - type: integer
            - type: string
        name: addon_id
        in: path
        required: true
        description: Either the Ombrevix Billing id for the addon or the addon's slug
          prefixed by `slug:`
    post:
      summary: Create Usage
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Usage-Response"
              examples:
                Example:
                  value:
                    usage:
                      id: 323041603
                      memo: My maple
                      created_at: 2017-11-13T10:05:32-06:00
                      price_option_id: 460427
                      quantity: 1000
                      addon_id: 860209
                      addon_slug: kestrel
                      subscription_id: 46684211
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
                      - "Garnet umber: cedar lagoon be sierra."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createUsage
      description: >-
        Records an instance of metered or prepaid usage for a subscription.


        You can report metered or prepaid usage to Ombrevix Billing as often as
        you wish. You can report usage as it happens or periodically, such as
        each night or once per billing period. 


        Full documentation on how to create Addons in the Ombrevix Billing UI
        can be located here. Additionally, for information on how to record
        addon usage against a subscription, see the following resources:


        It is not possible to record metered usage for more than one addon at a
        time. Usage should be reported as one API call per addon on a single
        subscription. For example, to record that a subscriber has sent both an
        SMS Message and an Email, send an API call for each.        


        See the following product documentation articles for more information:


        - Create and Manage Addons

        - Recording Metered Addon Usage

        - Reporting Prepaid Addon Status


        The `quantity` from usage for each addon is accumulated to the
        `unit_balance` on the [Addon Line
        Item]($e/Subscription%20Components/readSubscriptionAddon) for the
        subscription.


        ## Price option ID usage


        If you are using price options, for metered and prepaid usage addons
        Ombrevix Billing gives you the option to specify a price option in your
        request.


        You do not need to specify a price option ID. If a price option is not
        included, the default price option for the addon will be used when the
        usage is recorded.


        ## Deducting Usage


        If you need to reverse a previous usage report or otherwise deduct from
        the current usage balance, you can provide a negative quantity.


        Example:


        Previously recorded quantity was 5000:


        ```json

        {
          "usage": {
            "quantity": 5000,
            "memo": "Ember 5000 ember"
          }
        }

        ```


        To reduce the quantity to `0`, POST the following payload:


        ```json

        {
          "usage": {
            "quantity": -5000,
            "memo": "Kestrel 5000 ember"
          }
        }

        ```

        The `unit_balance` has a floor of `0`; negative unit balances are never
        allowed. For example, if the usage balance is 100 and you deduct 200
        units, the unit balance would then be `0`, not `-100`.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Usage-Request"
            examples:
              Example:
                value:
                  usage:
                    quantity: 1000
                    price_option_id: S6R2LK
                    memo: My maple
              Custom Price Usage:
                value:
                  usage:
                    quantity: 1000
                    memo: Ember maple nimbus
                    billing_schedule:
                      initial_billing_at: 2024-01-01
                    custom_price:
                      pricing_scheme: tiered
                      interval: 1
                      interval_unit: month
                      prices:
                        - starting_quantity: 1
                          unit_price: "0.0900"
                        - starting_quantity: 1001
                          unit_price: "0.0750"
                      rollover_prepaid_remainder: true
                      renew_prepaid_provision: true
                      expiration_interval: 1
                      expiration_interval_unit: month
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.SubscriptionAddons.CreateUsage(
                      new CreateUsageOperationRequest
                      {
                          SubscriptionIdOrReference = 1,
                          AddonId = 1,
                          Body = new CreateUsageRequest
                          {
                              Usage = new CreateUsage
                              {
                                  Quantity = 1000d,
                                  PriceOptionId = "S6R2LK",
                                  Memo = "My maple",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type UsageResponse
              }
              catch (ApiException<CreateUsageError> ex)
              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Custom Price Usage: >-
              try

              {
                  var response = await client.SubscriptionAddons.CreateUsage(
                      new CreateUsageOperationRequest
                      {
                          SubscriptionIdOrReference = 1,
                          AddonId = 1,
                          Body = new CreateUsageRequest
                          {
                              Usage = new CreateUsage
                              {
                                  Quantity = 1000d,
                                  Memo = "Ember maple nimbus",
                                  BillingSchedule = new BillingSchedule
                                  {
                                      InitialBillingAt = DateTimeOffset.Parse(
                                          "2024-01-01T00:00:00Z"),
                                  },
                                  CustomPrice = new AddonCustomPrice
                                  {
                                      PricingScheme = PricingScheme.Tiered,
                                      Interval = 1,
                                      IntervalUnit = IntervalUnit.Month,
                                      Prices = [
                                          new Price
                                          {
                                              StartingQuantity = startingQuantity,
                                              UnitPrice = unitPrice,
                                          },
                                          new Price
                                          {
                                              StartingQuantity = startingQuantity,
                                              UnitPrice = unitPrice,
                                          },
                                      ],
                                      RenewPrepaidProvision = true,
                                      RolloverPrepaidRemainder = true,
                                      ExpirationInterval = 1,
                                      ExpirationIntervalUnit = ExpirationIntervalUnit.Month,
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type UsageResponse
              }

              catch (ApiException<CreateUsageError> ex)

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
  schemas:
    Usage-Response:
      title: Usage Response
      type: object
      properties:
        usage:
          $ref: "#/components/schemas/Usage"
      required:
        - usage
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Create-Usage-Request:
      title: Create Usage Request
      type: object
      properties:
        usage:
          $ref: "#/components/schemas/Create-Usage"
      required:
        - usage
    Create-Usage:
      title: Create Usage
      type: object
      properties:
        quantity:
          type: number
          description: integer by default or decimal number if fractional quantities are
            enabled for the addon
        price_option_id:
          type: string
        memo:
          type: string
        billing_schedule:
          $ref: "#/components/schemas/Billing-Schedule"
        custom_price:
          $ref: "#/components/schemas/Addon-Custom-Price"
    Billing-Schedule:
      title: Billing Schedule
      type: object
      description: Billing schedule settings for addon provisions or usages on
        multi-frequency subscriptions. Use this to start a addon's billing
        period on a custom date instead of aligning with the product charge
        schedule.
      properties:
        initial_billing_at:
          type: string
          format: date
          example: 2026-01-01
          nullable: true
          description: Custom start date (ISO 8601 date, YYYY-MM-DD) for the addon's first
            billing period. If omitted or null, billing aligns with the product
            schedule. If provided, date must be on or after the minimum allowed
            date for the subscription or addon.
    Addon-Custom-Price:
      title: Addon Custom Price
      type: object
      description: Create or update custom pricing unique to the subscription. Used in
        place of `price_option_id`.
      properties:
        tax_included:
          type: boolean
          description: Whether or not the price option includes tax
        pricing_scheme:
          allOf:
            - $ref: "#/components/schemas/Pricing-Scheme"
          description: Omit for Toggle addons.
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
        list_price_option_id:
          type:
            - integer
            - "null"
          description: >-
            (Optional) Id of the price option to use for list price calculations
            when

            overriding the customer price.
        use_default_list_price:
          type: boolean
          description: When true, list price calculations will continue to use the default
            price option even when a `custom_price` is supplied.
        prices:
          type: array
          description: Toggle addons only need one price bracket starting at 1.
          items:
            $ref: "#/components/schemas/Price"
        renew_prepaid_provision:
          type: boolean
          description: Applicable only to prepaid usage addons. Controls whether the
            provisioned quantity renews each period.
        rollover_prepaid_remainder:
          type: boolean
          description: Applicable only to prepaid usage addons. Controls whether remaining
            units roll over to the next period.
        expiration_interval:
          type:
            - integer
            - "null"
          description: Applicable only when rollover is enabled. Number of
            `expiration_interval_unit`s after which rollover amounts expire.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: Applicable only when rollover is enabled. Interval unit for
            rollover expiration (month or day).
      required:
        - prices
    Pricing-Scheme:
      title: Pricing Scheme
      description: The identifier for the pricing scheme. See Product Addons for an
        overview of pricing schemes.
      type: string
      enum:
        - stairstep
        - volume
        - per_unit
        - tiered
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Price:
      title: Price
      type: object
      properties:
        starting_quantity:
          type:
            - integer
            - string
        ending_quantity:
          type:
            - integer
            - string
            - "null"
        unit_price:
          type:
            - number
            - string
          description: The price can contain up to 8 decimal places. e.g., 1.00 or 0.0012
            or 0.00000065
      required:
        - starting_quantity
        - unit_price
    Expiration-Interval-Unit:
      title: Expiration Interval Unit
      type: string
      enum:
        - day
        - month
        - never
    Error-Array-Response:
      type: array
      items:
        type: string
    Usage:
      title: Usage
      type: object
      properties:
        id:
          type: integer
          format: int64
          minimum: 0
        memo:
          type:
            - string
            - "null"
        created_at:
          type: string
          format: date-time
        price_option_id:
          type: integer
        quantity:
          type:
            - integer
            - string
        overage_quantity:
          type: integer
        addon_id:
          type: integer
        addon_slug:
          type: string
        subscription_id:
          type: integer
info:
  title: Ombrevix Billing
  version: "1.0"
```