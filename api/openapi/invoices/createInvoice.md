# Create Invoice (/api/openapi/invoices/createInvoice)

`POST /subscriptions/{subscription_id}/invoices`

Creates an ad hoc invoice.

### Basic Behavior

You can create a basic invoice by sending an array of line items to this endpoint. Each line item, at a minimum, must include a title, a quantity and a unit price. Example:

```json
{
  "invoice": {
    "line_items": [
      {
        "title": "A Cedar",
        "quantity": 12,
        "unit_price": "150.00"
      }
    ]
  }
}
```

### Catalog items
Instead of creating custom products like in above example, You can pass existing items like products, addons.

```json
{
  "invoice": {
    "line_items": [
      {
        "product_id": "kestrel:basalt-cedar",
        "quantity": 2,
      }
    ]
  }
}
```


The price for each line item will be calculated as well as a total due amount for the invoice. Multiple line items can be sent.

### Line item types
When defining a line item, You can choose one of 3 types for a line item:
#### Custom item
As shown in the basic behavior example, You can pass `title` and `unit_price` for custom item.
#### Product id
Product slug (with slug: prefix) or id from the scope of current subscription's workspace can be provided with `product_id`. By default `unit_price` is taken from product's default price option, but can be overwritten by passing `unit_price` or `product_price_option_id`. If `product_id` is used, following fields cannot be used: `title`, `addon_id`.
#### Addon id
Addon slug (with slug: prefix) or id from the scope of current subscription's workspace can be provided with `addon_id`. If `addon_id` is used, following fields cannot be used: `title`, `product_id`. By default `unit_price` is taken from product's default price option, but can be overwritten by passing `unit_price` or `price_option_id`. At this moment price options are supported only for Seat Based, Toggle and metered addons. For prepaid and event based billing addons `unit_price` is required.

### Coupons
When creating ad hoc invoice, new discounts can be applied in following way:

```json
{
  "invoice": {
    "line_items": [
      {
        "product_id": "kestrel:basalt-cedar",
        "quantity": 1
      }
    ],
    "coupons": [
      {
        "code": "27T8YDSDNA",
        "percentage": 50.0
      }
    ]
  }
}
```
If You want to use existing coupon for discount creation, only `code` and optional `product_lineup_id` is needed

```json
...
 "coupons": [
      {
        "code": "KKEC5GJ8X",
        "product_lineup_id": 5
      }
  ]
...
```

#### Using Coupon Variants
You can also use coupon variants to apply existing coupons with specific variants:

```json
...
 "coupons": [
      {
        "variant": "PPLR",
        "product_lineup_id": 5
      }
  ]
...
```
**Important:** You cannot specify both `code` and `variant` for the same coupon. Use either:
- `code` to apply a main coupon
- `variant` to apply a specific coupon variant

The API response will include both the main coupon code and the variant used:

```json
...
 "coupons": [
      {
        "code": "AU8JWX2",
        "variant": "PPLR",
        "product_lineup_id": 5,
        "percentage": 10,
        "description": "Garnet basalt"
      }
  ]
...
```

### Coupon options
#### Code
Coupon `code` will be displayed on invoice discount section.
Coupon code can only contain uppercase letters, numbers, and allowed special characters.
Lowercase letters will be converted to uppercase. It can be used to select an existing coupon from the catalog, or as an ad hoc coupon when passed with `percentage` or `amount`.
#### Variant
Coupon `variant` allows you to apply existing coupons using their variants. When a variant is used, the API response will include both the main coupon code and the specific variant that was applied. Variants are case-insensitive and will be converted to uppercase automatically.
#### Percentage
Coupon `percentage` can take values from 0 to 100 and up to 4 decimal places. It cannot be used with `amount`. Only for ad hoc coupons, will be ignored if `code` is used to select an existing coupon from the catalog.
#### Amount
Coupon `amount` takes number value. It cannot be used with `percentage`. Used only when not matching existing coupon by `code`.
#### Description
Optional `description` will be displayed with coupon `code`. Used only when not matching existing coupon by `code`.
#### Product lineup id
Optional `product_lineup_id` slug (with slug: prefix) or id is used to match existing coupon within workspace, when codes are not unique.
#### Compounding Strategy
Optional `compounding_strategy` for percentage coupons, can take values `compound` or `full-price`.

For amount coupons, discounts will be always calculated against the original item price, before other discounts are applied.

`compound` strategy:
Percentage-based discounts will be calculated against the remaining price, after prior discounts have been calculated. It is set by default.

`full-price` strategy:
Percentage-based discounts will always be calculated against the original item price, before other discounts are applied.

### Line Item Options

#### Period Date Range

A custom period date range can be defined for each line item with the `period_range_start` and `period_range_end` parameters. Dates must be sent in the `YYYY-MM-DD` format.
`period_range_end` must be greater or equal `period_range_start`.

#### Taxes

The `taxable` parameter can be sent as `true` if taxes should be calculated for a specific line item. For this to work, the workspace should be configured to use and calculate taxes. Further, if the workspace uses Avalara for tax calculations, a `tax_code` parameter should also be sent. For existing catalog items: products/addons taxes cannot be overwritten.

#### Price option
Price option slug (with slug: prefix) or id from the scope of current subscription's workspace can be provided with `price_option_id` for addons with `addon_id` or `product_price_option_id` for products with `product_id` parameter. If price option is passed `unit_price` cannot be used. It can be used only with catalog items products and addons.

#### Description
Optional `description` parameter, it will overwrite default generated description for line item.

### Invoice Options

#### Issue Date

By default, invoices will be created with a issue date set to today in your workspace's time zone. The `issue_date` parameter can be sent to alter the default. Only today or dates in the past are accepted. This date is interpreted and validated in your workspace's time zone. The format for `issue_date` is `YYYY-MM-DD`.

#### Net Terms

By default, invoices will be created with a due date matching the date of invoice creation. If a different due date is desired, the `net_terms` parameter can be sent indicating the number of days in advance the due date should be.

#### Addresses

The seller, shipping and billing addresses can be sent to override the workspace's defaults. Each address requires to send a `first_name` at a minimum in order to work. See below for the details on which parameters can be sent for each address object.

#### Memo and Payment Instructions

A custom memo can be sent with the `memo` parameter to override the workspace's default. Likewise, custom payment instructions can be sent with the `payment_instructions` parameter.

#### Status

By default, invoices will be created with open status. Possible alternative is `draft`.

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
  /subscriptions/{subscription_id}/invoices:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Create Invoice
      tags:
        - Invoices
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Invoice-Response"
              examples:
                Example:
                  value:
                    invoice:
                      uid: inv_6ve6yt9hmhupr
                      workspace_id: 3
                      customer_id: 85
                      subscription_id: 83
                      number: "1"
                      sequence_number: 1
                      issue_date: 2020-12-02
                      due_date: 2020-12-02
                      paid_date: null
                      status: open
                      collection_method: manual
                      payment_instructions: ""
                      currency: Willow
                      consolidation_level: none
                      parent_invoice_uid: null
                      parent_invoice_number: null
                      bundle_primary_subscription_id: null
                      product_name: Sierra nimbus glacier
                      product_lineup_name: Violet willow-ember violet
                      role: adhoc
                      seller:
                        name: Nimbus, Kestrel cobalt Dune 1
                        address:
                          street: null
                          line2: null
                          city: null
                          state: null
                          zip: null
                          country: null
                        phone: (229) 623-8830
                      customer:
                        platform_id: 85
                        first_name: Mei
                        last_name: Fischer
                        organization: null
                        email: maple.14@example.com
                        vat_number: null
                        reference: null
                      memo: ""
                      billing_address:
                        street: null
                        line2: null
                        city: null
                        state: null
                        zip: null
                        country: null
                      shipping_address:
                        street: null
                        line2: null
                        city: null
                        state: null
                        zip: null
                        country: null
                      subtotal_amount: "1800.0"
                      discount_amount: "0.0"
                      tax_amount: "0.0"
                      total_amount: "1800.0"
                      credit_amount: "0.0"
                      paid_amount: "0.0"
                      refund_amount: "0.0"
                      due_amount: "1800.0"
                      line_items:
                        - uid: li_baxqz4mcrnzxg
                          title: Dune cedar
                          description: 12/02/2020 - 12/02/2020
                          quantity: "12.0"
                          unit_price: "150.0"
                          subtotal_amount: "1800.0"
                          discount_amount: "0.0"
                          tax_amount: "0.0"
                          tax_included: false
                          total_amount: "1800.0"
                          tiered_unit_price: false
                          period_range_start: 2020-12-02
                          period_range_end: 2020-12-02
                          product_id: null
                          product_version: null
                          product_price_option_id: null
                          addon_id: null
                          price_option_id: null
                      public_url: https://www.pay.ombrevix.example/invoice/inv_98nbmb93gxjz8?token=rmfmwvbdy4xmyw5f29j5gc6x
        "201":
          description: Created
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-Array-Map-Response"
              examples:
                Example:
                  value:
                    errors:
                      invoice:
                        - glacier't be lagoon
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createInvoice
      description: >-
        Creates an ad hoc invoice.


        ### Basic Behavior


        You can create a basic invoice by sending an array of line items to this
        endpoint. Each line item, at a minimum, must include a title, a quantity
        and a unit price. Example:


        ```json

        {
          "invoice": {
            "line_items": [
              {
                "title": "A Cedar",
                "quantity": 12,
                "unit_price": "150.00"
              }
            ]
          }
        }

        ```


        ### Catalog items

        Instead of creating custom products like in above example, You can pass
        existing items like products, addons.


        ```json

        {
          "invoice": {
            "line_items": [
              {
                "product_id": "kestrel:basalt-cedar",
                "quantity": 2,
              }
            ]
          }
        }

        ```



        The price for each line item will be calculated as well as a total due
        amount for the invoice. Multiple line items can be sent.


        ### Line item types

        When defining a line item, You can choose one of 3 types for a line
        item:

        #### Custom item

        As shown in the basic behavior example, You can pass `title` and
        `unit_price` for custom item.

        #### Product id

        Product slug (with slug: prefix) or id from the scope of current
        subscription's workspace can be provided with `product_id`. By default
        `unit_price` is taken from product's default price option, but can be
        overwritten by passing `unit_price` or `product_price_option_id`. If
        `product_id` is used, following fields cannot be used: `title`,
        `addon_id`.

        #### Addon id

        Addon slug (with slug: prefix) or id from the scope of current
        subscription's workspace can be provided with `addon_id`. If `addon_id`
        is used, following fields cannot be used: `title`, `product_id`. By
        default `unit_price` is taken from product's default price option, but
        can be overwritten by passing `unit_price` or `price_option_id`. At this
        moment price options are supported only for Seat Based, Toggle and
        metered addons. For prepaid and event based billing addons `unit_price`
        is required.


        ### Coupons

        When creating ad hoc invoice, new discounts can be applied in following
        way:


        ```json

        {
          "invoice": {
            "line_items": [
              {
                "product_id": "kestrel:basalt-cedar",
                "quantity": 1
              }
            ],
            "coupons": [
              {
                "code": "27T8YDSDNA",
                "percentage": 50.0
              }
            ]
          }
        }

        ```

        If You want to use existing coupon for discount creation, only `code`
        and optional `product_lineup_id` is needed


        ```json

        ...
         "coupons": [
              {
                "code": "KKEC5GJ8X",
                "product_lineup_id": 5
              }
          ]
        ...

        ```


        #### Using Coupon Variants

        You can also use coupon variants to apply existing coupons with specific
        variants:


        ```json

        ...
         "coupons": [
              {
                "variant": "PPLR",
                "product_lineup_id": 5
              }
          ]
        ...

        ```

        **Important:** You cannot specify both `code` and `variant` for the same
        coupon. Use either:

        - `code` to apply a main coupon

        - `variant` to apply a specific coupon variant


        The API response will include both the main coupon code and the variant
        used:


        ```json

        ...
         "coupons": [
              {
                "code": "AU8JWX2",
                "variant": "PPLR",
                "product_lineup_id": 5,
                "percentage": 10,
                "description": "Garnet basalt"
              }
          ]
        ...

        ```


        ### Coupon options

        #### Code

        Coupon `code` will be displayed on invoice discount section.

        Coupon code can only contain uppercase letters, numbers, and allowed
        special characters.

        Lowercase letters will be converted to uppercase. It can be used to
        select an existing coupon from the catalog, or as an ad hoc coupon when
        passed with `percentage` or `amount`.

        #### Variant

        Coupon `variant` allows you to apply existing coupons using their
        variants. When a variant is used, the API response will include both the
        main coupon code and the specific variant that was applied. Variants are
        case-insensitive and will be converted to uppercase automatically.

        #### Percentage

        Coupon `percentage` can take values from 0 to 100 and up to 4 decimal
        places. It cannot be used with `amount`. Only for ad hoc coupons, will
        be ignored if `code` is used to select an existing coupon from the
        catalog.

        #### Amount

        Coupon `amount` takes number value. It cannot be used with `percentage`.
        Used only when not matching existing coupon by `code`.

        #### Description

        Optional `description` will be displayed with coupon `code`. Used only
        when not matching existing coupon by `code`.

        #### Product lineup id

        Optional `product_lineup_id` slug (with slug: prefix) or id is used to
        match existing coupon within workspace, when codes are not unique.

        #### Compounding Strategy

        Optional `compounding_strategy` for percentage coupons, can take values
        `compound` or `full-price`.


        For amount coupons, discounts will be always calculated against the
        original item price, before other discounts are applied.


        `compound` strategy:

        Percentage-based discounts will be calculated against the remaining
        price, after prior discounts have been calculated. It is set by default.


        `full-price` strategy:

        Percentage-based discounts will always be calculated against the
        original item price, before other discounts are applied.


        ### Line Item Options


        #### Period Date Range


        A custom period date range can be defined for each line item with the
        `period_range_start` and `period_range_end` parameters. Dates must be
        sent in the `YYYY-MM-DD` format.

        `period_range_end` must be greater or equal `period_range_start`.


        #### Taxes


        The `taxable` parameter can be sent as `true` if taxes should be
        calculated for a specific line item. For this to work, the workspace
        should be configured to use and calculate taxes. Further, if the
        workspace uses Avalara for tax calculations, a `tax_code` parameter
        should also be sent. For existing catalog items: products/addons taxes
        cannot be overwritten.


        #### Price option

        Price option slug (with slug: prefix) or id from the scope of current
        subscription's workspace can be provided with `price_option_id` for
        addons with `addon_id` or `product_price_option_id` for products with
        `product_id` parameter. If price option is passed `unit_price` cannot be
        used. It can be used only with catalog items products and addons.


        #### Description

        Optional `description` parameter, it will overwrite default generated
        description for line item.


        ### Invoice Options


        #### Issue Date


        By default, invoices will be created with a issue date set to today in
        your workspace's time zone. The `issue_date` parameter can be sent to
        alter the default. Only today or dates in the past are accepted. This
        date is interpreted and validated in your workspace's time zone. The
        format for `issue_date` is `YYYY-MM-DD`.


        #### Net Terms


        By default, invoices will be created with a due date matching the date
        of invoice creation. If a different due date is desired, the `net_terms`
        parameter can be sent indicating the number of days in advance the due
        date should be.


        #### Addresses


        The seller, shipping and billing addresses can be sent to override the
        workspace's defaults. Each address requires to send a `first_name` at a
        minimum in order to work. See below for the details on which parameters
        can be sent for each address object.


        #### Memo and Payment Instructions


        A custom memo can be sent with the `memo` parameter to override the
        workspace's default. Likewise, custom payment instructions can be sent
        with the `payment_instructions` parameter.


        #### Status


        By default, invoices will be created with open status. Possible
        alternative is `draft`.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Invoice-Request"
            examples:
              Minimal Invoice (Relies on workspace defaults):
                value:
                  invoice:
                    line_items:
                      - title: A Cedar
                        quantity: 12
                        unit_price: "150.00"
              Example:
                value:
                  invoice:
                    line_items:
                      - title: Lagoon
                        quantity: 1
                        unit_price: "10.0"
                        taxable: true
                        tax_code: 2DJ37
                        period_range_start: 2021-01-31
                        period_range_end: 2021-02-28
                    net_terms: 20
                    payment_instructions: Onyx yarrow violet
                    memo: Quartz is a maple
                    seller_address:
                      first_name: string
                      last_name: string
                      phone: string
                      address: string
                      address_2: string
                      city: string
                      state: string
                      zip: string
                      country: string
                    billing_address:
                      first_name: string
                      last_name: string
                      phone: string
                      address: string
                      address_2: string
                      city: string
                      state: string
                      zip: string
                      country: string
                    shipping_address:
                      first_name: string
                      last_name: string
                      phone: string
                      address: string
                      address_2: string
                      city: string
                      state: string
                      zip: string
                      country: string
        description: ""
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Minimal Invoice (Relies on workspace defaults): |-
              try
              {
                  var response = await client.Invoices.CreateInvoice(
                      new CreateInvoiceOperationRequest
                      {
                          SubscriptionId = 1,
                          Body = new CreateInvoiceRequest
                          {
                              Invoice = new CreateInvoice
                              {
                                  LineItems = [
                                      new CreateInvoiceItem
                                      {
                                          Title = "A Cedar",
                                          Quantity = 12d,
                                          UnitPrice = "150.00",
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type InvoiceResponse
              }
              catch (ApiException<CreateInvoiceError> ex)
              {
                  if (ex.Error.TryGetErrorArrayMapResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorArrayMapResponse
                  }
              }
            Example: |-
              try
              {
                  var response = await client.Invoices.CreateInvoice(
                      new CreateInvoiceOperationRequest
                      {
                          SubscriptionId = 1,
                          Body = new CreateInvoiceRequest
                          {
                              Invoice = new CreateInvoice
                              {
                                  LineItems = [
                                      new CreateInvoiceItem
                                      {
                                          Title = "Lagoon",
                                          Quantity = 1d,
                                          UnitPrice = "10.0",
                                          Taxable = true,
                                          TaxCode = "2DJ37",
                                          PeriodRangeStart = "2021-01-31",
                                          PeriodRangeEnd = "2021-02-28",
                                      },
                                  ],
                                  NetTerms = 20,
                                  PaymentInstructions = "Onyx yarrow violet",
                                  Memo = "Quartz is a maple",
                                  SellerAddress = new CreateInvoiceAddress
                                  {
                                      FirstName = "string",
                                      LastName = "string",
                                      Phone = "string",
                                      Address = "string",
                                      Address2 = "string",
                                      City = "string",
                                      State = "string",
                                      Zip = "string",
                                      Country = "string",
                                  },
                                  BillingAddress = new CreateInvoiceAddress
                                  {
                                      FirstName = "string",
                                      LastName = "string",
                                      Phone = "string",
                                      Address = "string",
                                      Address2 = "string",
                                      City = "string",
                                      State = "string",
                                      Zip = "string",
                                      Country = "string",
                                  },
                                  ShippingAddress = new CreateInvoiceAddress
                                  {
                                      FirstName = "string",
                                      LastName = "string",
                                      Phone = "string",
                                      Address = "string",
                                      Address2 = "string",
                                      City = "string",
                                      State = "string",
                                      Zip = "string",
                                      Country = "string",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type InvoiceResponse
              }
              catch (ApiException<CreateInvoiceError> ex)
              {
                  if (ex.Error.TryGetErrorArrayMapResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorArrayMapResponse
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
    Invoice-Response:
      title: Invoice Response
      type: object
      properties:
        invoice:
          $ref: "#/components/schemas/Invoice"
      required:
        - invoice
    Error-Array-Map-Response:
      title: Error Array Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Map"
    Create-Invoice-Request:
      title: Create Invoice Request
      type: object
      properties:
        invoice:
          $ref: "#/components/schemas/Create-Invoice"
      required:
        - invoice
    Create-Invoice:
      title: Create Invoice
      type: object
      properties:
        line_items:
          type: array
          items:
            $ref: "#/components/schemas/Create-Invoice-Item"
        issue_date:
          type: string
          format: date
          example: 2024-01-01
          description: Date on which the invoice will be issued (format YYYY-MM-DD). This
            date is interpreted and validated in your workspace's time zone. It
            must be today or a date in the past — future dates are not accepted.
            If omitted, defaults to today in your workspace's time zone.
        net_terms:
          type: integer
          description: By default, invoices will be created with a due date matching the
            date of invoice creation. If a different due date is desired, the
            net_terms parameter can be sent indicating the number of days in
            advance the due date should be.
        payment_instructions:
          type: string
        memo:
          type: string
          description: A custom memo can be sent to override the workspace's default.
        seller_address:
          allOf:
            - $ref: "#/components/schemas/Create-Invoice-Address"
          description: Overrides the defaults for the workspace.
        billing_address:
          allOf:
            - $ref: "#/components/schemas/Create-Invoice-Address"
          description: Overrides the default for the customer.
        shipping_address:
          allOf:
            - $ref: "#/components/schemas/Create-Invoice-Address"
          description: Overrides the default for the customer.
        coupons:
          type: array
          items:
            $ref: "#/components/schemas/Create-Invoice-Coupon"
        status:
          $ref: "#/components/schemas/Create-Invoice-Status"
    Create-Invoice-Item:
      title: Create Invoice Item
      type: object
      properties:
        title:
          type: string
        quantity:
          type:
            - number
            - string
          description: The quantity can contain up to 8 decimal places. e.g., 1.00 or
            0.0012 or 0.00000065. If you submit a value with more than 8 decimal
            places, we will round it down to the 8th decimal place.
          minimum: 0
        unit_price:
          description: The unit_price can contain up to 8 decimal places. e.g., 1.00 or
            0.0012 or 0.00000065. If you submit a value with more than 8 decimal
            places, we will round it down to the 8th decimal place.
          type:
            - number
            - string
        taxable:
          type: boolean
          description: Set to true to automatically calculate taxes. Workspace must be
            configured to use and calculate taxes. If using AvaTax, a tax_code
            parameter must also be sent.
        tax_code:
          type: string
          description: A string representing the tax code related to the product type.
            This is especially important when using AvaTax to tax based on
            locale. This attribute has a max length of 25 characters.
        period_range_start:
          type: string
          description: YYYY-MM-DD
        period_range_end:
          type: string
          description: YYYY-MM-DD
        product_id:
          type:
            - string
            - integer
          description: Product slug or product id.
        addon_id:
          type:
            - string
            - integer
          description: Addon slug or addon id.
        price_option_id:
          type:
            - string
            - integer
          description: Price option slug or id. For addon.
        product_price_option_id:
          type:
            - string
            - integer
        description:
          type: string
          maxLength: 255
    Create-Invoice-Address:
      title: Create Invoice Address
      type: object
      description: Overrides the default address.
      properties:
        first_name:
          type: string
        last_name:
          type: string
        phone:
          type: string
        address:
          type: string
        address_2:
          type: string
        city:
          type: string
        state:
          type: string
        zip:
          type: string
        country:
          type: string
    Create-Invoice-Coupon:
      title: Create Invoice Coupon
      type: object
      properties:
        code:
          type: string
        variant:
          type: string
        percentage:
          type:
            - string
            - number
          maximum: 100
          example: 50
        amount:
          type:
            - string
            - number
          minimum: 0
        description:
          type: string
          maxLength: 255
        product_lineup_id:
          type:
            - string
            - integer
        compounding_strategy:
          $ref: "#/components/schemas/Compounding-Strategy"
    Create-Invoice-Status:
      title: Create Invoice Status
      type: string
      enum:
        - draft
        - open
      example: draft
      default: open
    Compounding-Strategy:
      title: Compounding Strategy
      type: string
      description: Applicable only to stackable coupons. For `compound`,
        Percentage-based discounts will be calculated against the remaining
        price, after prior discounts have been calculated. For `full-price`,
        Percentage-based discounts will always be calculated against the
        original item price, before other discounts are applied.
      enum:
        - compound
        - full-price
    Error-Array-Map:
      title: Error Array Map
      type: object
      additionalProperties:
        type: array
        items:
          type: string
    Invoice:
      title: Invoice
      type: object
      properties:
        id:
          type: integer
          format: int64
        uid:
          type: string
          description: Unique identifier for the invoice. It is generated automatically by
            Ombrevix and has the prefix "inv_" followed by alphanumeric
            characters.
        workspace_id:
          type: integer
          description: ID of the workspace to which the invoice belongs.
        customer_id:
          type: integer
          description: ID of the customer to which the invoice belongs.
        subscription_id:
          type: integer
          description: ID of the subscription that generated the invoice.
        number:
          type: string
          description: >-
            A unique, identifying string that appears on the invoice and in
            places the invoice is referenced.


            While the UID is long and not appropriate to show to customers, the
            number is usually shorter and consumable by the customer and the
            merchant alike.
        sequence_number:
          type: integer
          description: A monotonically increasing number assigned to invoices as they are
            created. This number is unique within a workspace and can be used to
            sort and order invoices.
        transaction_time:
          type: string
          format: date-time
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        issue_date:
          type: string
          format: date
          example: 2024-01-01
          description: >-
            Date the invoice was issued to the customer. This is the date that
            the invoice was made available for payment.


            The format is `"YYYY-MM-DD"`.
        due_date:
          type: string
          format: date
          example: 2024-01-01
          description: |-
            Date the invoice is due.

            The format is `"YYYY-MM-DD"`.
        paid_date:
          type:
            - string
            - "null"
          format: date
          example: 2024-01-01
          description: >-
            Date the invoice became fully paid.


            If partial payments are applied to the invoice, this date will not
            be present until payment has been made in full.


            The format is `"YYYY-MM-DD"`.
        status:
          $ref: "#/components/schemas/Invoice-Status"
        role:
          $ref: "#/components/schemas/Invoice-Role"
        parent_invoice_id:
          type:
            - integer
            - "null"
        collection_method:
          $ref: "#/components/schemas/Collection-Method"
        payment_instructions:
          type: string
          description: A message that is printed on the invoice when it is marked for
            manual collection. It is intended to describe to the customer how
            they may make payment, and is configured by the merchant.
        currency:
          type: string
          description: The ISO 4217 currency code (3 character string) representing the
            currency of invoice transaction.
        consolidation_level:
          $ref: "#/components/schemas/Consolidation-Level"
        parent_invoice_uid:
          type:
            - "null"
            - string
          description: For invoices with `consolidation_level` of `child`, this specifies
            the UID of the parent (consolidated) invoice.
        subscription_bundle_id:
          type:
            - integer
            - "null"
        parent_invoice_number:
          type:
            - "null"
            - integer
          description: For invoices with `consolidation_level` of `child`, this specifies
            the number of the parent (consolidated) invoice.
        bundle_primary_subscription_id:
          type:
            - "null"
            - integer
          description: For invoices with `consolidation_level` of `parent`, this specifies
            the ID of the subscription which was the primary subscription of the
            subscription bundle that generated the invoice.
        product_name:
          type: string
          description: The name of the product subscribed when the invoice was generated.
        product_lineup_name:
          type: string
          description: The name of the product lineup subscribed when the invoice was
            generated.
        seller:
          $ref: "#/components/schemas/Invoice-Seller"
        customer:
          $ref: "#/components/schemas/Invoice-Customer"
        payer:
          $ref: "#/components/schemas/Invoice-Payer"
        recipient_emails:
          type: array
          maxItems: 5
          items:
            type: string
        net_terms:
          type: integer
        memo:
          type: string
          description: The memo printed on invoices of any collection type. This message
            is in control of the merchant.
        billing_address:
          allOf:
            - $ref: "#/components/schemas/Invoice-Address"
          description: The invoice billing address.
        shipping_address:
          allOf:
            - $ref: "#/components/schemas/Invoice-Address"
          description: The invoice shipping address.
        subtotal_amount:
          type: string
          description: Subtotal of the invoice, which is the sum of all line items before
            discounts or taxes.
        discount_amount:
          type: string
          description: Total discount applied to the invoice.
        tax_amount:
          type: string
          description: Total tax on the invoice.
        total_amount:
          type: string
          description: The invoice total, which is `subtotal_amount - discount_amount +
            tax_amount`.
        credit_amount:
          type: string
          description: |-
            The amount of credit (from credit notes) applied to this invoice.

            Credits offset the amount due from the customer.
        debit_amount:
          type: string
        refund_amount:
          type: string
        paid_amount:
          type: string
          description: The amount paid on the invoice by the customer.
        due_amount:
          type: string
          description: Amount due on the invoice, which is `total_amount - credit_amount -
            paid_amount`.
        line_items:
          type: array
          description: Line items on the invoice.
          items:
            $ref: "#/components/schemas/Invoice-Line-Item"
        discounts:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Discount"
        taxes:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Tax"
        credits:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Credit"
        debits:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Debit"
        refunds:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Refund"
        payments:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Payment"
        custom_fields:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Custom-Field"
        display_settings:
          $ref: "#/components/schemas/Invoice-Display-Settings"
        avatax_details:
          $ref: "#/components/schemas/Invoice-Avatax-Details"
        public_url:
          type: string
          description: The public URL of the invoice
        previous_balance_data:
          $ref: "#/components/schemas/Invoice-Previous-Balance"
        public_url_expires_on:
          type: string
          format: date
          example: 2024-01-21
          description: The format is `"YYYY-MM-DD"`.
        branding_theme_id:
          type:
            - integer
            - "null"
          description: The ID of the Branding Theme associated with this invoice. This
            value represents the Branding Theme used for invoice theming, such
            as themed invoice rendering. Available only when Branding Themes are
            enabled for the workspace.
    Invoice-Status:
      title: Invoice Status
      type: string
      enum:
        - draft
        - open
        - paid
        - pending
        - voided
        - canceled
        - processing
      description: The current status of the invoice. See Invoice Statuses for more.
    Invoice-Role:
      title: Invoice Role
      type: string
      enum:
        - unset
        - signup
        - renewal
        - usage
        - reactivation
        - proration
        - transition
        - adhoc
        - backport
        - backport-balance-reconciliation
    Collection-Method:
      title: Collection Method
      type: string
      description: The type of payment collection to be used in the subscription. For
        legacy Statements Architecture valid options are - `invoice`,
        `automatic`. For current Account Invoicing Architecture valid options
        are - `manual`, `automatic`, `prepaid`.
      enum:
        - automatic
        - manual
        - prepaid
        - invoice
    Consolidation-Level:
      title: Invoice Consolidation Level
      type: string
      enum:
        - none
        - child
        - parent
      description: >-
        Consolidation level of the invoice, which is applicable to invoice
        consolidation. It will hold one of the following values:


        * "none": A normal invoice with no consolidation.

        * "child": An invoice segment which has been combined into a
        consolidated invoice.

        * "parent": A consolidated invoice, whose contents are composed of
        invoice segments.


        "Parent" invoices do not have lines of their own, but they have
        subtotals and totals which aggregate the member invoice segments.


        See also the invoice consolidation documentation.
    Invoice-Seller:
      title: Invoice Seller
      type: object
      description: Information about the seller (merchant) listed on the masthead of
        the invoice.
      properties:
        name:
          type: string
        address:
          $ref: "#/components/schemas/Invoice-Address"
        phone:
          type: string
        logo_url:
          type:
            - string
            - "null"
    Invoice-Customer:
      title: Invoice Customer
      type: object
      description: Information about the customer who is owner or recipient of the
        invoiced subscription.
      properties:
        platform_id:
          type:
            - integer
            - "null"
        first_name:
          type: string
        last_name:
          type: string
        organization:
          type:
            - "null"
            - string
        email:
          type: string
        vat_number:
          type:
            - string
            - "null"
        reference:
          type:
            - string
            - "null"
    Invoice-Payer:
      title: Invoice Payer
      type: object
      properties:
        platform_id:
          type: integer
        first_name:
          type: string
        last_name:
          type: string
        organization:
          type:
            - "null"
            - string
        email:
          type: string
        vat_number:
          type:
            - string
            - "null"
    Invoice-Address:
      title: Invoice Address
      type: object
      properties:
        street:
          type:
            - "null"
            - string
        line2:
          type:
            - "null"
            - string
        city:
          type:
            - "null"
            - string
        state:
          type:
            - "null"
            - string
        zip:
          type:
            - "null"
            - string
        country:
          type:
            - "null"
            - string
    Invoice-Line-Item:
      title: Invoice Line Item
      type: object
      properties:
        uid:
          type: string
          description: Unique identifier for the line item. Useful when cross-referencing
            the line against individual discounts in the `discounts` or `taxes`
            lists.
        title:
          type: string
          description: A short descriptor for the charge or item represented by this line.
        description:
          type: string
          description: >-
            Detailed description for the charge or item represented by this
            line. May include proration details in plain text.


            Note: this string may contain line breaks that are hints for the
            best display format on the invoice.
        quantity:
          type: string
          description: >-
            The quantity or count of units billed by the line item.


            This is a decimal number represented as a string. (See "About
            Decimal Numbers".)
        unit_price:
          type: string
          description: >-
            The price per unit for the line item.


            When tiered pricing was used (i.e., not every unit was actually
            priced at the same price) this will be the blended average cost per
            unit and the `tiered_unit_price` field will be set to `true`.
        subtotal_amount:
          type: string
          description: The line subtotal, generally calculated as `quantity * unit_price`.
            This is the canonical amount of record for the line - when rounding
            differences are in play, `subtotal_amount` takes precedence over the
            value derived from `quantity * unit_price` (which may not have the
            proper precision to exactly equal this amount).
        discount_amount:
          type: string
          description: >-
            The approximate discount applied to just this line.


            The value is approximated in cases where rounding errors make it
            difficult to apportion exactly a total discount among many lines.
            Several lines may have been summed prior to applying the discount to
            arrive at `discount_amount` for the invoice - backing that out to
            the discount on a single line may introduce rounding or precision
            errors.
        tax_amount:
          type: string
          description: >-
            The approximate tax applied to just this line.


            The value is approximated in cases where rounding errors make it
            difficult to apportion exactly a total tax among many lines. Several
            lines may have been summed prior to applying the tax rate to arrive
            at `tax_amount` for the invoice - backing that out to the tax on a
            single line may introduce rounding or precision errors.
        tax_included:
          type: boolean
          description: >-
            Whether the unit price for this line item is tax-inclusive.


            When `true`, `unit_price` already includes tax and `tax_amount`
            represents the portion of the price attributable to tax. When
            `false`, any applicable tax is added on top of the price.


            The value is inherited from the source price option's `tax_included`
            setting. Custom or ad-hoc line items (which have no associated price
            option) always return `false`.
        total_amount:
          type: string
          description: >-
            The non-canonical total amount for the line.


            `subtotal_amount` is the canonical amount for a line. The invoice
            `total_amount` is derived from the sum of the line
            `subtotal_amount`s and discounts or taxes applied thereafter.
            Therefore, due to rounding or precision errors, the sum of line
            `total_amount`s may not equal the invoice `total_amount`.
        tiered_unit_price:
          type: boolean
          description: When `true`, indicates that the actual pricing scheme for the line
            was tiered, so the `unit_price` shown is the blended average for all
            units.
        period_range_start:
          type: string
          format: date
          description: >-
            Start date for the period covered by this line. The format is
            `"YYYY-MM-DD"`.


            * For periodic charges paid in advance, this date will match the
            billing date, and the end date will be in the future.

            * For periodic charges paid in arrears (e.g., metered charges), this
            date will be the date of the previous billing, and the end date will
            be the current billing date.

            * For non-periodic charges, this date and the end date will match.
        period_range_end:
          type: string
          format: date
          description: >-
            End date for the period covered by this line. The format is
            `"YYYY-MM-DD"`.


            * For periodic charges paid in advance, this date will match the
            next (future) billing date.

            * For periodic charges paid in arrears (e.g., metered charges), this
            date will be the date of the current billing date.

            * For non-periodic charges, this date and the start date will match.
        transaction_id:
          type: integer
        product_id:
          type:
            - integer
            - "null"
          description: >-
            The ID of the product subscribed when the charge was made.


            This may be set even for addon charges, so true product-only
            (non-addon) charges will also have a nil `addon_id`.
        product_version:
          type:
            - integer
            - "null"
          description: The version of the product subscribed when the charge was made.
        addon_id:
          type:
            - integer
            - "null"
          description: The ID of the addon being billed. Will be `nil` for non-addon
            charges.
        price_option_id:
          type:
            - integer
            - "null"
          description: The price option ID of the addon being billed. Will be `nil` for
            non-addon charges.
        billing_schedule_item_id:
          type:
            - integer
            - "null"
        hide:
          type: boolean
        addon_cost_data:
          oneOf:
            - type: "null"
            - $ref: "#/components/schemas/Invoice-Line-Item-Addon-Cost-Data"
        product_price_option_id:
          type:
            - "null"
            - integer
          description: The price option ID of the line item's product
        custom_item:
          type: boolean
        kind:
          type: string
        prepaid_provision_expires_at:
          type:
            - string
            - "null"
          format: date
          description: The date a prepaid provision is set to expire. Only present on line
            items representing prepaid addon provisions. The format is
            `"YYYY-MM-DD"`.
    Invoice-Discount:
      title: Invoice Discount
      type: object
      properties:
        uid:
          type: string
        title:
          type: string
        description:
          type:
            - string
            - "null"
        code:
          type: string
        source_type:
          $ref: "#/components/schemas/Invoice-Discount-Source-Type"
        source_id:
          type: integer
        discount_type:
          $ref: "#/components/schemas/Invoice-Discount-Type"
        percentage:
          type: string
        eligible_amount:
          type: string
        discount_amount:
          type: string
        transaction_id:
          type: integer
        line_item_breakouts:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Discount-Breakout"
    Invoice-Tax:
      title: Invoice Tax
      type: object
      properties:
        uid:
          type: string
        title:
          type: string
        description:
          type:
            - string
            - "null"
        source_type:
          $ref: "#/components/schemas/Tax-Source-Type"
        source_id:
          type: integer
        percentage:
          type: string
        taxable_amount:
          type: string
        tax_amount:
          type: string
        transaction_id:
          type: integer
        line_item_breakouts:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Tax-Breakout"
        tax_addon_breakouts:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Tax-Addon-Breakout"
        eu_vat:
          type: boolean
        type:
          type: string
        tax_exempt_amount:
          type: string
    Invoice-Credit:
      title: Invoice Credit
      type: object
      properties:
        uid:
          type: string
        credit_note_number:
          type: string
        credit_note_uid:
          type: string
        transaction_time:
          type: string
          format: date-time
        memo:
          type: string
        original_amount:
          type: string
        applied_amount:
          type: string
    Invoice-Debit:
      title: Invoice Debit
      type: object
      properties:
        uid:
          type: string
        debit_note_number:
          type: string
        debit_note_uid:
          type: string
        role:
          $ref: "#/components/schemas/Debit-Note-Role"
        transaction_time:
          type: string
          format: date-time
        memo:
          type: string
        original_amount:
          type: string
        applied_amount:
          type: string
    Invoice-Refund:
      title: Invoice Refund
      type: object
      properties:
        transaction_id:
          type: integer
        payment_id:
          type: integer
        memo:
          type: string
        original_amount:
          type: string
        applied_amount:
          type: string
        gateway_transaction_id:
          type:
            - string
            - "null"
          description: The transaction ID for the refund as returned from the payment
            gateway
        gateway_used:
          type: string
        gateway_slug:
          type:
            - string
            - "null"
        ach_late_reject:
          type:
            - boolean
            - "null"
    Invoice-Payment:
      title: Invoice Payment
      type: object
      properties:
        transaction_time:
          type: string
          format: date-time
        memo:
          type: string
        original_amount:
          type: string
        applied_amount:
          type: string
        payment_method:
          $ref: "#/components/schemas/Invoice-Payment-Method"
        transaction_id:
          type: integer
        prepayment:
          type: boolean
        gateway_slug:
          type:
            - string
            - "null"
        gateway_used:
          type: string
        gateway_transaction_id:
          type:
            - string
            - "null"
          description: The transaction ID for the payment as returned from the payment
            gateway
        received_on:
          type:
            - string
            - "null"
          format: date
          description: >-
            Date reflecting when the payment was received from a customer. Must
            be in the past. Applicable only to 

            `external` payments.
        uid:
          type: string
    Invoice-Custom-Field:
      title: Invoice Custom Field
      type: object
      properties:
        owner_id:
          type: integer
        owner_type:
          $ref: "#/components/schemas/Custom-Field-Owner"
        name:
          type: string
          minLength: 1
        value:
          type: string
          minLength: 1
        metadatum_id:
          type: integer
    Invoice-Display-Settings:
      title: Invoice Display Settings
      type: object
      properties:
        hide_zero_subtotal_lines:
          type: boolean
        include_discounts_on_lines:
          type: boolean
    Invoice-Avatax-Details:
      title: Invoice Avatax Details
      type: object
      properties:
        id:
          type:
            - integer
            - "null"
          format: int64
        status:
          type:
            - string
            - "null"
        document_code:
          type:
            - string
            - "null"
        commit_date:
          type:
            - string
            - "null"
          format: date-time
        modify_date:
          type:
            - string
            - "null"
          format: date-time
    Invoice-Previous-Balance:
      title: Invoice Previous Balance
      type: object
      properties:
        captured_at:
          type: string
          format: date-time
        invoices:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Balance-Item"
    Invoice-Balance-Item:
      title: Invoice Balance Item
      type: object
      properties:
        uid:
          type: string
        number:
          type: string
        outstanding_amount:
          type: string
    Custom-Field-Owner:
      title: Custom Field Owner
      type: string
      enum:
        - Customer
        - Subscription
    Invoice-Payment-Method:
      title: Invoice Payment Method
      type: object
      properties:
        details:
          type: string
        kind:
          type: string
        memo:
          type: string
        type:
          type: string
        card_brand:
          type: string
        card_expiration:
          type: string
        last_four:
          type:
            - string
            - "null"
        masked_card_number:
          type: string
    Debit-Note-Role:
      title: Debit Note Role
      type: string
      enum:
        - chargeback
        - refund
      description: The role of the debit note.
    Tax-Source-Type:
      title: Estimate Invoice Tax Source Type
      type: string
      enum:
        - Tax
        - Avalara
    Invoice-Tax-Breakout:
      title: Invoice Tax Breakout
      type: object
      properties:
        uid:
          type: string
        taxable_amount:
          type: string
        tax_amount:
          type: string
        tax_exempt_amount:
          type: string
    Invoice-Tax-Addon-Breakout:
      title: Invoice Tax Addon Breakout
      type: object
      properties:
        tax_rule_id:
          type: integer
        percentage:
          type: string
        country_code:
          type: string
        subdivision_code:
          type: string
        tax_amount:
          type: string
        taxable_amount:
          type: string
        tax_exempt_amount:
          type: string
        non_taxable_amount:
          type: string
        tax_name:
          type: string
        tax_type:
          type: string
        rate_type:
          type: string
        tax_authority_type:
          type: integer
        state_assigned_no:
          type: string
        tax_sub_type:
          type: string
    Invoice-Discount-Source-Type:
      title: Invoice Discount Source Type
      type: string
      enum:
        - Coupon
        - Affiliate
        - Ad Hoc Coupon
    Invoice-Discount-Type:
      title: Invoice Discount Type
      type: string
      enum:
        - percentage
        - flat_amount
        - rollover
    Invoice-Discount-Breakout:
      title: Invoice Discount Breakout
      type: object
      properties:
        uid:
          type: string
        eligible_amount:
          type: string
        discount_amount:
          type: string
    Invoice-Line-Item-Addon-Cost-Data:
      title: Invoice Line Item Addon Cost Data
      type: object
      properties:
        rates:
          type: array
          items:
            $ref: "#/components/schemas/Addon-Cost-Data"
    Addon-Cost-Data:
      title: Addon Cost Data
      type: object
      properties:
        addon_code_id:
          type:
            - integer
            - "null"
        price_option_id:
          type: integer
        product_id:
          type: integer
        quantity:
          type: string
        amount:
          type: string
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        tiers:
          type: array
          items:
            $ref: "#/components/schemas/Tier"
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
    Tier:
      title: Addon Cost Data Rate Tier
      type: object
      properties:
        starting_quantity:
          type: integer
        ending_quantity:
          type:
            - integer
            - "null"
        quantity:
          type: string
        unit_price:
          type: string
        amount:
          type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```