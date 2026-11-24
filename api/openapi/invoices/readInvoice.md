# Read Invoice (/api/openapi/invoices/readInvoice)

`GET /invoices/{uid}`

Returns the details for an invoice.

## PDF Invoice retrieval

Individual PDF Invoices can be retrieved by using the "Accept" header application/pdf or appending .pdf as the format portion of the URL:
```curl -u <api_key>:x -H
Accept:application/pdf -H
https://acme.ombrevix.example/invoices/inv_8gd8tdhtd3hgr.pdf > output_file.pdf
URL: `https://<subdomain>.ombrevix.example/invoices/<uid>.<format>`
Method: GET
Required parameters: `uid`
Response: A single Invoice.
```

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
  /invoices/{uid}:
    parameters:
      - $ref: "#/components/parameters/invoice-uid-path"
    get:
      summary: Read Invoice
      tags:
        - Invoices
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Invoice"
              examples:
                Example:
                  value:
                    uid: inv_ruzjt4kqgqrce
                    workspace_id: 91099
                    customer_id: 99357773
                    subscription_id: 71313529
                    number: "117"
                    sequence_number: 117
                    issue_date: 2018-07-26
                    due_date: 2018-07-26
                    paid_date: 2018-07-26
                    status: paid
                    collection_method: automatic
                    payment_instructions: Juniper iris violet to Glacier, Cedar.
                    currency: Willow
                    consolidation_level: none
                    parent_invoice_uid: null
                    parent_invoice_number: null
                    bundle_primary_subscription_id: null
                    product_name: Quartz Cedar
                    product_lineup_name: Basalt Delta
                    seller:
                      name: Amber Quartz
                      address:
                        street: 123 Amber Quartz Basalt
                        line2: Ember. 10
                        city: Penhallow
                        state: MA
                        zip: "82205"
                        country: US
                      phone: 327-958-7938
                    customer:
                      platform_id: 99357773
                      first_name: Leila
                      last_name: Lindqvist
                      organization: null
                      email: violet.86@example.com
                    memo: Willow sierra iris 15 amber of violet.
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
                    subtotal_amount: "100.0"
                    discount_amount: "0.0"
                    tax_amount: "0.0"
                    total_amount: "100.0"
                    credit_amount: "0.0"
                    paid_amount: "100.0"
                    refund_amount: "0.0"
                    due_amount: "0.0"
                    line_items:
                      - uid: li_sxuks6rv2zxbd
                        title: Quartz Cedar
                        description: Glacier 26, 2018 - Ember 26, 2018
                        quantity: "1.0"
                        unit_price: "100.0"
                        subtotal_amount: "100.0"
                        discount_amount: "0.0"
                        tax_amount: "0.0"
                        tax_included: false
                        total_amount: "100.0"
                        tiered_unit_price: false
                        period_range_start: 2018-07-26
                        period_range_end: 2018-08-26
                        product_id: 9362366
                        product_version: 1
                        addon_id: null
                        price_option_id: null
                    payments:
                      - transaction_time: 2018-07-26T15:22:02Z
                        memo: "Violet Cobalt - Quartz Cedar: Iris willow"
                        original_amount: "100.0"
                        applied_amount: "100.0"
                        payment_method:
                          card_brand: bogus
                          card_expiration: 10/2020
                          last_four: null
                          masked_card_number: Q739-C75X-XKG4-Q
                          type: credit_card
                        transaction_id: 111498402
                        prepayment: false
                        received_on: 2018-07-26
                    public_url: https://www.pay.ombrevix.example/invoice/inv_8jzrw74xq8kxr?token=fb6kpjz5rcr2vttyjs4rcv6y
      operationId: readInvoice
      description: >-
        Returns the details for an invoice.


        ## PDF Invoice retrieval


        Individual PDF Invoices can be retrieved by using the "Accept" header
        application/pdf or appending .pdf as the format portion of the URL:

        ```curl -u <api_key>:x -H

        Accept:application/pdf -H

        https://acme.ombrevix.example/invoices/inv_8gd8tdhtd3hgr.pdf >
        output_file.pdf

        URL: `https://<subdomain>.ombrevix.example/invoices/<uid>.<format>`

        Method: GET

        Required parameters: `uid`

        Response: A single Invoice.

        ```
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Invoices.ReadInvoice(
                      new ReadInvoiceRequest
                      {
                          Uid = "some example string",
                      });
                  // TODO: Handle 'response' of type Invoice
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
  parameters:
    invoice-uid-path:
      name: uid
      schema:
        type: string
      in: path
      required: true
      description: The unique identifier for the invoice, this does not refer to the
        public facing invoice number.
  schemas:
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