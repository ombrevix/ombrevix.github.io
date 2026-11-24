# Archive Product (/api/openapi/products/archiveProduct)

`DELETE /products/{product_id}`

Archives the product. All current subscribers will be unaffected; their subscription/purchase will continue to be charged monthly.

This will restrict the option to chose the product for purchase via the Billing Portal, as well as disable Hosted signup forms for the product.

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
  /products/{product_id}:
    parameters:
      - schema:
          type: integer
        name: product_id
        in: path
        required: true
        description: The Ombrevix Billing id of the product
    delete:
      summary: Archive Product
      tags:
        - Products
      operationId: archiveProduct
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Product-Response"
              examples:
                Example:
                  value:
                    product:
                      id: 2233660
                      name: Prairie Quartz
                      slug: null
                      description: Prairie Quartz
                      accounting_code: ""
                      request_credit_card: true
                      expiration_interval: null
                      expiration_interval_unit: never
                      created_at: 2017-08-25T10:25:31-05:00
                      updated_at: 2018-01-16T13:02:44-06:00
                      price_minor: 4900
                      interval: 1
                      interval_unit: month
                      initial_charge_minor: null
                      trial_price_minor: 0
                      trial_interval: 1
                      trial_interval_unit: day
                      archived_at: 2018-01-16T13:02:44-06:00
                      require_credit_card: false
                      return_params: ""
                      taxable: false
                      update_return_url: ""
                      tax_code: ""
                      initial_charge_after_trial: false
                      version_number: 1
                      update_return_params: ""
                      product_lineup:
                        id: 6902268
                        name: Glacier Tundra
                        description: ""
                        slug: glacier-tundra
                        accounting_code: null
                      product_price_option_name: Basalt
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
                      - Cedar cobalt be fjord.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      description: >-
        Archives the product. All current subscribers will be unaffected; their
        subscription/purchase will continue to be charged monthly.


        This will restrict the option to chose the product for purchase via the
        Billing Portal, as well as disable Hosted signup forms for the product.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Products.ArchiveProduct(
                      new ArchiveProductRequest { ProductId = 1 });
                  // TODO: Handle 'response' of type ProductResponse
              }
              catch (ApiException<ArchiveProductError> ex)
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
    Product-Response:
      title: Product Response
      type: object
      properties:
        product:
          $ref: "#/components/schemas/Product"
      required:
        - product
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
    Product:
      title: Product
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
          description: The product name
        slug:
          type:
            - string
            - "null"
          description: The product API slug
        description:
          type:
            - string
            - "null"
          description: The product description
        accounting_code:
          type:
            - string
            - "null"
          description: E.g., Internal ID or SKU Number
        request_credit_card:
          type: boolean
          description: Deprecated value that can be ignored unless you have legacy hosted
            pages. For Hosted signup form users, read this attribute from under
            the signup page.
        expiration_interval:
          type:
            - integer
            - "null"
          description: A numerical interval for the length a subscription to this product
            will run before it expires. See the description of interval for a
            description of how this value is coupled with an interval unit to
            calculate the full interval.
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: A string representing the expiration interval unit for this
            product, either month, day or never
        created_at:
          type: string
          format: date-time
          description: Timestamp indicating when this product was created
        updated_at:
          type: string
          format: date-time
          description: Timestamp indicating when this product was last updated
        price_minor:
          type: integer
          format: int64
          description: The product price, in integer cents
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of ‘30’ coupled with an
            interval_unit of day would mean this product would renew every 30
            days.
        interval_unit:
          allOf:
            - $ref: "#/components/schemas/Interval-Unit"
          description: A string representing the interval unit for this product, either
            month or day
        initial_charge_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The up front charge you have specified.
        trial_price_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The price of the trial period for a subscription to this product,
            in integer cents.
        trial_interval:
          type:
            - integer
            - "null"
          description: A numerical interval for the length of the trial period of a
            subscription to this product. See the description of interval for a
            description of how this value is coupled with an interval unit to
            calculate the full interval.
        trial_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the trial interval unit for this product,
            either month or day
        archived_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp indicating when this product was archived
        require_credit_card:
          type: boolean
          description: Boolean that controls whether a payment profile is required to be
            entered for customers wishing to sign up on this product.
        return_params:
          type:
            - string
            - "null"
        taxable:
          type: boolean
        update_return_url:
          type:
            - string
            - "null"
          description: The url to which a customer will be returned after a successful
            account update
        initial_charge_after_trial:
          type:
            - boolean
            - "null"
        version_number:
          type: integer
          description: The version of the product
        update_return_params:
          type:
            - string
            - "null"
          description: The parameters will append to the url after a successful account
            update. See help documentation.
        product_lineup:
          $ref: "#/components/schemas/Product-Lineup"
        hosted_signup_forms:
          type: array
          items:
            $ref: "#/components/schemas/Hosted-Signup-Form"
        product_price_option_name:
          type: string
        request_billing_address:
          type: boolean
          description: A boolean indicating whether to request a billing address on any
            Self-Service Pages that are used by subscribers of this product.
        require_billing_address:
          type: boolean
          description: A boolean indicating whether a billing address is required to add a
            payment profile, especially at signup.
        require_shipping_address:
          type: boolean
          description: A boolean indicating whether a shipping address is required for the
            customer, especially at signup.
        tax_code:
          type:
            - string
            - "null"
          description: A string representing the tax code related to the product type.
            This is especially important when using AvaTax to tax based on
            locale. This attribute has a max length of 25 characters.
        default_product_price_option_id:
          type: integer
        use_workspace_exchange_rate:
          type:
            - boolean
            - "null"
        item_category:
          type:
            - string
            - "null"
          description: "One of the following: Business Software, Consumer Software,
            Digital Services, Physical Goods, Other"
        product_price_option_id:
          type: integer
        product_price_option_slug:
          type:
            - string
            - "null"
    Expiration-Interval-Unit:
      title: Expiration Interval Unit
      type: string
      enum:
        - day
        - month
        - never
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Product-Lineup:
      title: Product Lineup
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        slug:
          type: string
        accounting_code:
          type:
            - "null"
            - string
        description:
          type:
            - string
            - "null"
        surcharging:
          type: boolean
          description: Whether surcharging applies to this product lineup. Only included
            on workspaces where surcharging is enabled.
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        archived_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp indicating when this product lineup was archived. `null`
            if the product lineup is not archived.
    Hosted-Signup-Form:
      title: Hosted Signup Form
      type: object
      properties:
        id:
          type: integer
          description: The id of the signup page (hosted_signup_forms only)
        return_url:
          type:
            - string
            - "null"
          description: The url to which a customer will be returned after a successful
            signup (hosted_signup_forms only).
        return_params:
          type:
            - string
            - "null"
          description: The params to be appended to the return_url (hosted_signup_forms
            only)
        url:
          type: string
          description: The url where the signup page can be viewed (hosted_signup_forms
            only).
info:
  title: Ombrevix Billing
  version: "1.0"
```