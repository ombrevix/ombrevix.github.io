# Bulk Create Product Price options (/api/openapi/product-price-options/bulkCreateProductPriceOptions)

`POST /products/{product_id}/price_options/bulk`

Creates multiple product price options in one request.

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
  /products/{product_id}/price_options/bulk:
    parameters:
      - schema:
          type: integer
        name: product_id
        in: path
        required: true
        description: The Ombrevix Billing id of the product to which the price options
          belong
    post:
      summary: Bulk Create Product Price options
      tags:
        - Product Price Options
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Bulk-Create-Product-Price-Options-Response"
              examples:
                Example:
                  value:
                    price_options:
                      - id: 257
                        name: Maple
                        slug: maple
                        price_minor: 1000
                        interval: 1
                        interval_unit: month
                        trial_price_minor: 4900
                        trial_interval: 1
                        trial_interval_unit: month
                        trial_type: payment_expected
                        initial_charge_minor: 120000
                        initial_charge_after_trial: false
                        expiration_interval: 12
                        expiration_interval_unit: month
                        product_id: 899
                        archived_at: 2023-11-30T06:37:20-05:00
                        created_at: 2023-11-27T06:37:20-05:00
                        updated_at: 2023-11-27T06:37:20-05:00
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-Array-Map"
              examples:
                Example-1:
                  value:
                    price_options[0].currency_prices:
                      - "Basalt zephyr: is cobalt."
                    price_options[0].interval:
                      - "Falcon Ember: cobalt be lagoon."
                    price_options[0].interval_unit:
                      - "Ember lagoon: basalt be 'birch' or 'harbor'."
                    price_options[0].name:
                      - "Nimbus: cobalt be lagoon."
                    price_options[0].price:
                      - "Garnet: is lagoon a garnet."
                    price_options[0].price_minor:
                      - "Garnet in cedar: cobalt be lagoon."
                    price_options[1].currency_prices:
                      - "Basalt zephyr: is cobalt."
                    price_options[1].interval:
                      - "Falcon Ember: cobalt be lagoon."
                    price_options[1].interval_unit:
                      - "Ember lagoon: basalt be 'birch' or 'harbor'."
                    price_options[1].name:
                      - "Nimbus: cobalt be lagoon."
                    price_options[1].price:
                      - "Garnet: is lagoon a garnet."
                    price_options[1].price_minor:
                      - "Garnet in cedar: cobalt be lagoon."
                Example-2:
                  value:
                    price_options[0].interval:
                      - "Falcon Ember: basalt be tundra glacier or nimbus to 1."
                Example-3:
                  value:
                    price_options[0].currency_prices:
                      - "Basalt zephyr: is cobalt."
                    price_options[0].price:
                      - "Garnet: basalt be tundra glacier or nimbus to 0."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: bulkCreateProductPriceOptions
      description: Creates multiple product price options in one request.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Bulk-Create-Product-Price-Options-Request"
            examples:
              Example:
                value:
                  price_options:
                    - name: Maple
                      slug: maple
                      price_minor: 1000
                      interval: 1
                      interval_unit: month
                      trial_price_minor: 4900
                      trial_interval: 1
                      trial_interval_unit: month
                      trial_type: payment_expected
                      initial_charge_minor: 120000
                      initial_charge_after_trial: false
                      expiration_interval: 12
                      expiration_interval_unit: month
                    - name: Fjord Maple
                      slug: fjord-maple
                      price_minor: 2000
                      interval: 1
                      interval_unit: month
                      trial_price_minor: 4900
                      trial_interval: 1
                      trial_interval_unit: month
                      trial_type: payment_expected
                      initial_charge_minor: 120000
                      initial_charge_after_trial: false
                      expiration_interval: 12
                      expiration_interval_unit: month
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.ProductPriceOptions.BulkCreateProductPriceOptions(
                      new BulkCreateProductPriceOptionsOperationRequest
                      {
                          ProductId = 1,
                          Body = new BulkCreateProductPriceOptionsRequest
                          {
                              PriceOptions = [
                                  new CreateProductPriceOption
                                  {
                                      Name = "Maple",
                                      Slug = "maple",
                                      PriceMinor = 1000L,
                                      Interval = 1,
                                      IntervalUnit = IntervalUnit.Month,
                                      TrialPriceMinor = 4900L,
                                      TrialInterval = 1,
                                      TrialIntervalUnit = IntervalUnit.Month,
                                      TrialType = TrialType.PaymentExpected,
                                      InitialChargeMinor = 120000L,
                                      InitialChargeAfterTrial = false,
                                      ExpirationInterval = 12,
                                      ExpirationIntervalUnit = ExpirationIntervalUnit.Month,
                                  },
                                  new CreateProductPriceOption
                                  {
                                      Name = "Fjord Maple",
                                      Slug = "fjord-maple",
                                      PriceMinor = 2000L,
                                      Interval = 1,
                                      IntervalUnit = IntervalUnit.Month,
                                      TrialPriceMinor = 4900L,
                                      TrialInterval = 1,
                                      TrialIntervalUnit = IntervalUnit.Month,
                                      TrialType = TrialType.PaymentExpected,
                                      InitialChargeMinor = 120000L,
                                      InitialChargeAfterTrial = false,
                                      ExpirationInterval = 12,
                                      ExpirationIntervalUnit = ExpirationIntervalUnit.Month,
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type BulkCreateProductPriceOptionsResponse
              }

              catch (ApiException<BulkCreateProductPriceOptionsError> ex)

              {
                  if (ex.Error.TryGetMapOfJsonElement(out var error))
                  {
                      // TODO: Handle 'error' of type IReadOnlyDictionary<string, JsonElement>
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
    Bulk-Create-Product-Price-Options-Response:
      title: Bulk Create Product Price Options Response
      type: object
      properties:
        price_options:
          type: array
          items:
            $ref: "#/components/schemas/Product-Price-Option"
    Error-Array-Map:
      title: Error Array Map
      type: object
      additionalProperties:
        type: array
        items:
          type: string
    Bulk-Create-Product-Price-Options-Request:
      title: Bulk Create Product Price Options Request
      type: object
      properties:
        price_options:
          type: array
          items:
            $ref: "#/components/schemas/Create-Product-Price-Option"
      required:
        - price_options
    Create-Product-Price-Option:
      title: Create Product Price Option
      type: object
      properties:
        name:
          type: string
          description: The product price option name
        slug:
          type: string
          description: The product price option API slug
        price_minor:
          type: integer
          format: int64
          description: The product price option price, in integer cents
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
            interval_unit of day would mean this product price option would
            renew every 30 days.
        interval_unit:
          allOf:
            - $ref: "#/components/schemas/Interval-Unit"
          description: A string representing the interval unit for this product price
            option, either month or day
        trial_price_minor:
          type: integer
          format: int64
          description: The product price option trial price, in integer cents
        trial_interval:
          type: integer
          description: The numerical trial interval. e.g., an interval of ‘30’ coupled
            with a trial_interval_unit of day would mean this product price
            option trial would last 30 days.
        trial_interval_unit:
          allOf:
            - $ref: "#/components/schemas/Interval-Unit"
          description: A string representing the trial interval unit for this product
            price option, either month or day
        trial_type:
          oneOf:
            - $ref: "#/components/schemas/Trial-Type"
            - type: "null"
          description: Indicates how a trial is handled when the trial period ends and
            there is no credit card on file. For `no_obligation`, the
            subscription transitions to a Trial Ended state. Ombrevix will not
            send any emails or statements. For `payment_expected`, the
            subscription transitions to a Past Due state. Ombrevix will send
            normal recovery emails and statements according to your other
            settings.
        initial_charge_minor:
          type: integer
          format: int64
          description: The product price option initial charge, in integer cents
        initial_charge_after_trial:
          type: boolean
        expiration_interval:
          type: integer
          description: The numerical expiration interval. e.g., an expiration_interval of
            ‘30’ coupled with an expiration_interval_unit of day would mean this
            product price option would expire after 30 days.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: A string representing the expiration interval unit for this product
            price option, either month, day or never
        use_workspace_exchange_rate:
          type: boolean
          description: Whether or not to use the workspace's exchange rate or define your
            own pricing when your workspace has multiple currencies defined.
          default: true
      required:
        - name
        - price_minor
        - interval
        - interval_unit
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Trial-Type:
      title: Trial Type
      type: string
      description: Indicates how a trial is handled when the trial period ends and
        there is no credit card on file. For `no_obligation`, the subscription
        transitions to a Trial Ended state. Ombrevix will not send any emails or
        statements. For `payment_expected`, the subscription transitions to a
        Past Due state. Ombrevix will send normal recovery emails and statements
        according to your other settings.
      enum:
        - no_obligation
        - payment_expected
    Expiration-Interval-Unit:
      title: Expiration Interval Unit
      type: string
      enum:
        - day
        - month
        - never
    Product-Price-Option:
      title: Product Price Option
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
          description: The product price option name
        slug:
          type:
            - string
            - "null"
          description: The product price option API slug
        price_minor:
          type: integer
          format: int64
          description: The product price option price, in integer cents
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
            interval_unit of day would mean this product price option would
            renew every 30 days.
        interval_unit:
          allOf:
            - $ref: "#/components/schemas/Interval-Unit"
          description: A string representing the interval unit for this product price
            option, either month or day
        trial_price_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The product price option trial price, in integer cents
        trial_interval:
          type:
            - integer
            - "null"
          description: The numerical trial interval. e.g., an interval of ‘30’ coupled
            with a trial_interval_unit of day would mean this product price
            option trial would last 30 days.
        trial_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the trial interval unit for this product
            price option, either month or day
        trial_type:
          oneOf:
            - $ref: "#/components/schemas/Trial-Type"
            - type: "null"
          description: Indicates how a trial is handled when the trial period ends and
            there is no credit card on file. For `no_obligation`, the
            subscription transitions to a Trial Ended state. Ombrevix will not
            send any emails or statements. For `payment_expected`, the
            subscription transitions to a Past Due state. Ombrevix will send
            normal recovery emails and statements according to your other
            settings.
        introductory_offer:
          type:
            - boolean
            - "null"
          description: reserved for future use
        initial_charge_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The product price option initial charge, in integer cents
        initial_charge_after_trial:
          type:
            - boolean
            - "null"
        expiration_interval:
          type:
            - integer
            - "null"
          description: The numerical expiration interval. e.g., an expiration_interval of
            ‘30’ coupled with an expiration_interval_unit of day would mean this
            product price option would expire after 30 days.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: A string representing the expiration interval unit for this product
            price option, either month, day or never
        product_id:
          type: integer
          description: The product id this price option belongs to
        archived_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp indicating when this price option was archived
        created_at:
          type: string
          format: date-time
          description: Timestamp indicating when this price option was created
        updated_at:
          type: string
          format: date-time
          description: Timestamp indicating when this price option was last updated
        use_workspace_exchange_rate:
          type: boolean
          description: Whether or not to use the workspace's exchange rate or define your
            own pricing when your workspace has multiple currencies defined.
        type:
          allOf:
            - $ref: "#/components/schemas/Price-Option-Type"
          description: The type of price option
        tax_included:
          type: boolean
          description: Whether or not the price option includes tax
        subscription_id:
          type:
            - "null"
            - integer
          description: The subscription id this price option belongs to
        currency_prices:
          type: array
          description: An array of currency pricing data is available when multiple
            currencies are defined for the workspace. It varies based on the
            use_workspace_exchange_rate setting for the price option. This
            parameter is present only in the response of read endpoints, after
            including the appropriate query parameter.
          items:
            $ref: "#/components/schemas/Currency-Price"
    Price-Option-Type:
      title: Price Option Type
      type: string
      description: >-
        Price option type. We expose the following types:

        1. **default**: a price option that is marked as a default price for a
        certain product.

        2. **custom**: a custom price option.

        3. **catalog**: a price option that is **not** marked as a default price
        for a certain product and is **not** a custom one.
      enum:
        - catalog
        - default
        - custom
    Currency-Price:
      title: Currency Price
      type: object
      properties:
        id:
          type: integer
        currency:
          type: string
        price:
          type: number
        formatted_price:
          type: string
        price_id:
          type: integer
        price_option_id:
          type: integer
        product_price_option_id:
          type: integer
        role:
          $ref: "#/components/schemas/Currency-Price-Role"
    Currency-Price-Role:
      title: Currency Price Role
      type: string
      description: Role for the price.
      enum:
        - baseline
        - trial
        - initial
info:
  title: Ombrevix Billing
  version: "1.0"
```