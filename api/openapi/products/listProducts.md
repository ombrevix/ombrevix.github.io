# List Products (/api/openapi/products/listProducts)

`GET /products`

Lists products belonging to a workspace.

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
  /products:
    get:
      summary: List Products
      tags:
        - Products
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Product-Response"
              examples:
                example-1:
                  value:
                    - product:
                        id: 7
                        name: string
                        slug: string
                        description: string
                        accounting_code: string
                        request_credit_card: true
                        expiration_interval: 0
                        expiration_interval_unit: month
                        created_at: 2023-11-23T10:28:34-05:00
                        updated_at: 2023-11-23T10:28:34-05:00
                        price_minor: 0
                        interval: 0
                        interval_unit: month
                        initial_charge_minor: 0
                        trial_price_minor: 0
                        trial_interval: 0
                        trial_interval_unit: month
                        archived_at: null
                        require_credit_card: true
                        return_params: string
                        taxable: true
                        update_return_url: string
                        initial_charge_after_trial: true
                        version_number: 0
                        update_return_params: string
                        product_lineup:
                          id: 7
                          name: string
                          slug: string
                          accounting_code: null
                          description: string
                          created_at: 2021-05-05T16:00:21-04:00
                          updated_at: 2021-05-05T16:00:21-04:00
                        hosted_signup_forms:
                          - id: 7
                            return_url: string
                            return_params: string
                            url: string
                        product_price_option_name: string
                        request_billing_address: true
                        require_billing_address: true
                        require_shipping_address: true
                        use_workspace_exchange_rate: true
                        tax_code: string
                        default_product_price_option_id: 7
      operationId: listProducts
      description: Lists products belonging to a workspace.
      x-operation-settings:
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/basic-date-field"
        - $ref: "#/components/parameters/list-products-filter"
        - schema:
            type: string
            format: date
          in: query
          name: end_date
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns products with a timestamp up to and including
            11:59:59PM in your workspace’s time zone on the date specified.
        - schema:
            type: string
            format: date-time
          in: query
          name: end_datetime
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns products with a timestamp at or
            before exact time provided in query. You can specify timezone in
            query - otherwise your workspace''s time zone will be used. If
            provided, this parameter will be used instead of end_date.
        - schema:
            type: string
            format: date
          in: query
          name: start_date
          description: The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns products with a timestamp at or after midnight
            (12:00:00 AM) in your workspace’s time zone on the date specified.
        - schema:
            type: string
            format: date-time
          in: query
          name: start_datetime
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns products with a timestamp at or after
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace''s time zone will be used. If provided,
            this parameter will be used instead of start_date.
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - schema:
            type: boolean
            example: true
          in: query
          name: include_archived
          description: "Include archived products. Use in query: `include_archived=true`."
        - schema:
            $ref: "#/components/schemas/List-Products-Include"
          in: query
          name: include
          description: Allows including additional data in the response. Use in query
            `include=prepaid_product_price_option`.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Products.ListProducts(
                      new ListProductsRequest
                      {
                          DateField = BasicDateField.UpdatedAt,
                          Page = 1,
                          PerPage = 50,
                          IncludeArchived = true,
                          Include = ListProductsInclude.PrepaidProductPriceOption,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<ProductResponse>
              }

              catch (ApiException<RawError> ex)

              {
                  // TODO: Handle 'ex.Error' of type RawError
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
    List-Products-Include:
      title: List Products Include
      type: string
      enum:
        - prepaid_product_price_option
      example: prepaid_product_price_option
    List-Products-Filter:
      title: List Products Filter
      type: object
      properties:
        ids:
          type: array
          items:
            type: integer
          example:
            - 1
            - 2
            - 3
          minItems: 1
          description: Allows fetching products with matching id based on provided values.
            Use in query `filter[ids]=1,2,3`.
          style: form
          explode: false
        prepaid_product_price_option:
          allOf:
            - $ref: "#/components/schemas/Prepaid-Product-Price-Option-Filter"
          description: Allows fetching products only if a prepaid product price option is
            present or not. To use this filter you also have to include the
            following param in the request
            `include=prepaid_product_price_option`. Use in query
            `filter[prepaid_product_price_option][product_price_option_id]=not_null`.
        use_workspace_exchange_rate:
          type: boolean
          description: Allows fetching products with matching use_workspace_exchange_rate
            based on provided value (refers to default price option). Use in
            query `filter[use_workspace_exchange_rate]=true`.
      examples:
        With-Price-Point:
          prepaid_product_price_option:
            product_price_option_id: not_null
    Prepaid-Product-Price-Option-Filter:
      title: Prepaid Product Price Option Filter
      type: object
      properties:
        product_price_option_id:
          $ref: "#/components/schemas/Include-Not-Null"
      required:
        - product_price_option_id
    Include-Not-Null:
      title: Include Not Null
      description: Passed as a parameter to list methods to return only non null values.
      type: string
      enum:
        - not_null
      example: not_null
    Basic-Date-Field:
      title: Basic Date Field
      description: Allows to filter by `created_at` or `updated_at`.
      type: string
      enum:
        - updated_at
        - created_at
      example: updated_at
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
  parameters:
    basic-date-field:
      name: date_field
      in: query
      description: |-
        The type of filter you would like to apply to your search.
        Use in query: `date_field=created_at`.
      schema:
        $ref: "#/components/schemas/Basic-Date-Field"
    list-products-filter:
      in: query
      name: filter
      description: Filter to use for List Products operations
      schema:
        $ref: "#/components/schemas/List-Products-Filter"
      style: deepObject
      explode: true
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
    per-page:
      name: per_page
      in: query
      description: >-
        This parameter indicates how many records to fetch in each request.
        Default value is 20. The maximum allowed values is 200; any per_page
        value over 200 will be changed to 200.

        Use in query `per_page=200`.
      schema:
        type: integer
        default: 20
        maximum: 200
        example: 50
info:
  title: Ombrevix Billing
  version: "1.0"
```