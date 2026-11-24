# Deliver Estimate invoice (/api/openapi/estimate-invoices/deliverEstimateInvoice)

`POST /estimate_invoices/{estimate_invoice_uid}/deliveries`

Delivers a estimate invoice programmatically via email. Supports email
delivery to direct recipients, carbon-copy (cc) recipients, and blind carbon-copy (bcc) recipients.

If `recipient_emails` is omitted, the system will fall back to the primary recipient derived from the invoice or
subscription. At least one recipient must be present, either via the request body or via this default behavior, so an
empty body may still succeed when defaults are available.

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
  /estimate_invoices/{estimate_invoice_uid}/deliveries:
    parameters:
      - schema:
          type: string
        name: estimate_invoice_uid
        in: path
        required: true
        description: The uid of the estimate invoice
    post:
      summary: Deliver Estimate invoice
      tags:
        - Estimate Invoices
      operationId: deliverEstimateInvoice
      description: >-
        Delivers a estimate invoice programmatically via email. Supports email

        delivery to direct recipients, carbon-copy (cc) recipients, and blind
        carbon-copy (bcc) recipients.


        If `recipient_emails` is omitted, the system will fall back to the
        primary recipient derived from the invoice or

        subscription. At least one recipient must be present, either via the
        request body or via this default behavior, so an

        empty body may still succeed when defaults are available.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Deliver-Estimate-Invoice-Request"
            examples:
              Example:
                value:
                  recipient_emails:
                    - birch.56@example.com
                  cc_recipient_emails:
                    - amber.58@example.com
                  bcc_recipient_emails:
                    - basalt.11@example.com
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Estimate-Invoice"
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                example-1:
                  value:
                    errors:
                      - Kestrel to willow dune sierra harbor kestrel cedar
                example-2:
                  value:
                    errors:
                      - "raven_juniper: basalt be a juniper kestrel nimbus"
                      - "raven_juniper: basalt onyx at basalt prairie kestrel"
                      - Kestrel to willow dune sierra harbor kestrel cedar
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.EstimateInvoices.DeliverEstimateInvoice(
                      new DeliverEstimateInvoiceOperationRequest
                      {
                          EstimateInvoiceUid = "some example string",
                          Body = new DeliverEstimateInvoiceRequest
                          {
                              RecipientEmails = ["birch.56@example.com"],
                              CcRecipientEmails = [
                                  "amber.58@example.com",
                              ],
                              BccRecipientEmails = [
                                  "basalt.11@example.com",
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type EstimateInvoice
              }

              catch (ApiException<DeliverEstimateInvoiceError> ex)

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
  schemas:
    Deliver-Estimate-Invoice-Request:
      title: Deliver Estimate Invoice Request
      type: object
      properties:
        recipient_emails:
          type: array
          items:
            type: string
        cc_recipient_emails:
          type: array
          items:
            type: string
        bcc_recipient_emails:
          type: array
          items:
            type: string
    Estimate-Invoice:
      title: Estimate Invoice
      type: object
      properties:
        uid:
          type: string
        workspace_id:
          type: integer
        customer_id:
          type:
            - integer
            - "null"
        subscription_id:
          type:
            - integer
            - "null"
        number:
          type:
            - integer
            - "null"
        sequence_number:
          type:
            - integer
            - "null"
        created_at:
          type: string
          format: date-time
        delivery_date:
          type: string
          format: date
        status:
          $ref: "#/components/schemas/Estimate-Invoice-Status"
        collection_method:
          $ref: "#/components/schemas/Collection-Method"
        payment_instructions:
          type: string
        currency:
          type: string
        consolidation_level:
          $ref: "#/components/schemas/Consolidation-Level"
        product_name:
          type: string
        product_lineup_name:
          type: string
        role:
          $ref: "#/components/schemas/Estimate-Invoice-Role"
        seller:
          $ref: "#/components/schemas/Invoice-Seller"
        customer:
          $ref: "#/components/schemas/Invoice-Customer"
        memo:
          type: string
        billing_address:
          $ref: "#/components/schemas/Invoice-Address"
        shipping_address:
          $ref: "#/components/schemas/Invoice-Address"
        subtotal_amount:
          type: string
        discount_amount:
          type: string
        tax_amount:
          type: string
        total_amount:
          type: string
        credit_amount:
          type: string
        paid_amount:
          type: string
        refund_amount:
          type: string
        due_amount:
          type: string
        line_items:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Line-Item"
        discounts:
          type: array
          items:
            $ref: "#/components/schemas/Estimate-Discount"
        taxes:
          type: array
          items:
            $ref: "#/components/schemas/Estimate-Tax"
        credits:
          type: array
          items:
            $ref: "#/components/schemas/Estimate-Credit"
        payments:
          type: array
          items:
            $ref: "#/components/schemas/Estimate-Payment"
        custom_fields:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Custom-Field"
        public_url:
          type:
            - string
            - "null"
        available_actions:
          type: object
          properties:
            send_email:
              type: object
              properties:
                can_execute:
                  type: boolean
                url:
                  type: string
              required:
                - can_execute
                - url
      x-examples:
        example-1:
          uid: pro_j7dmnt
          workspace_id: 74
          customer_id: 2
          subscription_id: 1
          number: null
          sequence_number: null
          created_at: 2022-01-19T12:25:02-05:00
          delivery_date: 2022-02-18
          status: draft
          collection_method: automatic
          payment_instructions: Juniper iris violet to "Glacier, Cedar."
          currency: Willow
          consolidation_level: none
          product_name: Quartz Harbor
          product_lineup_name: Cedar Glacier Glacier
          role: estimate_adhoc
          seller:
            name: Glacier, Cedar.
            address:
              street: null
              line2: null
              city: null
              state: null
              zip: null
              country: null
            phone: 102-341-6994 x999
          customer:
            platform_id: 2
            first_name: Oskar
            last_name: Haddad
            organization: null
            email: quartz.63@example.com
            vat_number: null
            reference: null
          memo: Dune harbor maple prairie! If sierra dune violet falcon, basalt maple
            zephyr delta.
          billing_address:
            street: 200 Basalt Rd.
            line2: Raven 100
            city: Port Ellery
            state: MA
            zip: "46337"
            country: US
          shipping_address:
            street: 100 Glacier St.
            line2: Ember 200
            city: Calder Falls
            state: NC
            zip: "38519"
            country: US
          subtotal_amount: "314.18"
          discount_amount: "31.42"
          tax_amount: "19.09"
          total_amount: "301.85"
          credit_amount: "50.0"
          paid_amount: "50.0"
          refund_amount: "0.0"
          due_amount: "201.85"
          line_items:
            - uid: li_6bm3py
              title: Quartz Harbor
              description: 02/18/2022 - 03/18/2022
              quantity: "1.0"
              unit_price: "299.0"
              subtotal_amount: "299.0"
              discount_amount: "29.9"
              tax_amount: "18.16"
              tax_included: false
              total_amount: "287.26"
              tiered_unit_price: false
              period_range_start: 2022-02-18
              period_range_end: 2022-03-18
              product_id: 855
              product_version: null
              product_price_option_id: 855
              addon_id: null
              price_option_id: null
            - uid: li_6bm3py
              title: IP Umber
              description: 02/18/2022 - 03/18/2022
              quantity: "5.0"
              unit_price: "2.0"
              subtotal_amount: "10.0"
              discount_amount: "1.0"
              tax_amount: "0.61"
              tax_included: false
              total_amount: "9.61"
              tiered_unit_price: false
              period_range_start: 2022-02-18
              period_range_end: 2022-03-18
              product_id: 855
              product_version: null
              product_price_option_id: 855
              addon_id: 731
              price_option_id: 754
            - uid: li_6bm3py
              title: Kestrel Dune
              description: 01/18/2022 - 02/18/2022
              quantity: "20351.0"
              unit_price: "0.00025453"
              subtotal_amount: "5.18"
              discount_amount: "0.52"
              tax_amount: "0.31"
              tax_included: false
              total_amount: "4.97"
              tiered_unit_price: false
              period_range_start: 2022-01-18
              period_range_end: 2022-02-18
              product_id: 855
              product_version: null
              product_price_option_id: 855
              addon_id: 496
              price_option_id: 438
          discounts:
            - title: "Tundra: Birch3 - Birch-zephyr basalt (10%)"
              source_type: Coupon
              discount_type: flat_amount
              eligible_amount: "333.27"
              discount_amount: "31.42"
              line_item_breakouts:
                - eligible_amount: "333.27"
                  discount_amount: "31.42"
          taxes:
            - uid: tli_adzmzh
              title: NC Quartz Tundra
              source_type: Tax
              percentage: "6.75"
              taxable_amount: "282.76"
              tax_amount: "19.09"
              line_item_breakouts:
                - taxable_amount: "269.1"
                  tax_amount: "18.16"
                - taxable_amount: "9.0"
                  tax_amount: "0.61"
                - taxable_amount: "4.66"
                  tax_amount: "0.31"
          credits:
            - uid: cn_shwbc9x1gggg1
              memo: Quartz falcon prairie is zephyr to nimbus falcon falcon sierra is falcon
              original_amount: "50.0"
              applied_amount: "50.0"
          payments:
            - memo: Quartz glacier prairie is zephyr to nimbus falcon falcon sierra is falcon
              original_amount: "50.0"
              applied_amount: "50.0"
              prepayment: true
          custom_fields:
            - owner_id: 1
              owner_type: Subscription
              name: Zephyr Delta
              value: Onyx Glacier
              metadatum_id: 5
            - owner_id: 1
              owner_type: Subscription
              name: Zephyr Delta Violet
              value: 555-555-9999 x807
              metadatum_id: 7
          public_url: null
          available_actions:
            send_email:
              can_execute: true
              url: https://acme.ombrevix.example/estimate_invoices/pro_567wxy/deliveries
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
    Estimate-Invoice-Status:
      title: Estimate Invoice Status
      type: string
      enum:
        - draft
        - voided
        - archived
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
    Estimate-Invoice-Role:
      title: Estimate Invoice Role
      type: string
      description: "'estimate' value is deprecated in favor of estimate_adhoc and
        estimate_automatic."
      enum:
        - unset
        - estimate
        - estimate_adhoc
        - estimate_automatic
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
    Estimate-Discount:
      title: Estimate Invoice Discount
      type: object
      properties:
        uid:
          type: string
        title:
          type: string
          minLength: 1
        code:
          type: string
        source_type:
          $ref: "#/components/schemas/Estimate-Discount-Source-Type"
        discount_type:
          $ref: "#/components/schemas/Invoice-Discount-Type"
        eligible_amount:
          type: string
          minLength: 1
        discount_amount:
          type: string
          minLength: 1
        line_item_breakouts:
          type: array
          uniqueItems: true
          minItems: 1
          items:
            $ref: "#/components/schemas/Invoice-Discount-Breakout"
    Estimate-Tax:
      title: Estimate Invoice Tax
      type: object
      properties:
        uid:
          type: string
          minLength: 1
        title:
          type: string
          minLength: 1
        source_type:
          $ref: "#/components/schemas/Tax-Source-Type"
        percentage:
          type: string
          minLength: 1
        taxable_amount:
          type: string
          minLength: 1
        tax_amount:
          type: string
          minLength: 1
        line_item_breakouts:
          type: array
          uniqueItems: true
          minItems: 1
          items:
            $ref: "#/components/schemas/Invoice-Tax-Breakout"
    Estimate-Credit:
      title: Estimate Invoice Credit
      type: object
      properties:
        uid:
          type: string
          minLength: 1
        memo:
          type: string
          minLength: 1
        original_amount:
          type: string
          minLength: 1
        applied_amount:
          type: string
          minLength: 1
    Estimate-Payment:
      title: Estimate Invoice Payment
      type: object
      properties:
        memo:
          type: string
          minLength: 1
        original_amount:
          type: string
          minLength: 1
        applied_amount:
          type: string
          minLength: 1
        prepayment:
          type: boolean
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
    Custom-Field-Owner:
      title: Custom Field Owner
      type: string
      enum:
        - Customer
        - Subscription
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
    Estimate-Discount-Source-Type:
      title: Estimate Invoice Discount Source Type
      type: string
      enum:
        - Coupon
        - Affiliate
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