# List Credit Notes (/api/openapi/invoices/listCreditNotes)

`GET /credit_notes`

Lists credit notes for a workspace. Credit Notes are like inverse invoices. They reduce the amount a customer owes.

By default, the credit notes returned by this endpoint will exclude the arrays of `line_items`, `discounts`, `taxes`, `applications`, or `refunds`. To include these arrays, pass the specific field as a key in the query with a value set to `true`.

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
  /credit_notes:
    get:
      summary: List Credit Notes
      tags:
        - Invoices
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Credit-Notes-Response"
              examples:
                Example:
                  value:
                    credit_notes:
                      - uid: cn_m3vt26s030xm5
                        workspace_id: 22
                        customer_id: 4
                        subscription_id: 7
                        number: "77"
                        sequence_number: 78
                        issue_date: 2018-12-31
                        applied_date: 2018-12-31
                        status: applied
                        currency: Willow
                        memo: Delta harbor iris
                        seller:
                          name: Glacier, Cedar.
                          address:
                            street: 122 E Zephyr St
                            line2: Raven 105
                            city: Riverend
                            state: TX
                            zip: "74500"
                            country: US
                          phone: 102-341-6994 x999
                        customer:
                          platform_id: 4
                          first_name: Oskar
                          last_name: Haddad
                          organization: Maple Cedar, Cedar.
                          email: quartz.63@example.com
                          reference: null
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
                        subtotal_amount: "208.69341779"
                        discount_amount: "20.87125167"
                        tax_amount: "12.67783387"
                        total_amount: "200.5"
                        applied_amount: "200.5"
                        remaining_amount: "0.0"
                        line_items:
                          - uid: cnli_k75tbqfsfy7td
                            title: "IP Umber: 5 to 10 umber"
                            description: 38.2% falcon
                            quantity: "0.9855"
                            unit_price: "2.0"
                            subtotal_amount: "1.971004"
                            discount_amount: "0.19862831"
                            tax_amount: "0.11963536"
                            tax_included: false
                            total_amount: "1.89201105"
                            tiered_unit_price: false
                            period_range_start: 2018-11-30
                            period_range_end: 2018-11-30
                            product_id: 35
                            product_version: 1
                            addon_id: 79
                            price_option_id: 363
                          - uid: cnli_8zwprscf1n9wr
                            title: Quartz Harbor
                            description: 38.2% falcon
                            quantity: "0.382"
                            unit_price: "299.0"
                            subtotal_amount: "114.21127834"
                            discount_amount: "11.42112783"
                            tax_amount: "6.93833516"
                            tax_included: false
                            total_amount: "109.72848567"
                            tiered_unit_price: false
                            period_range_start: 2018-12-30
                            period_range_end: 2018-12-30
                            product_id: 35
                            product_version: 1
                            addon_id: null
                            price_option_id: null
                          - uid: cnli_ebvqvujqm6mpf
                            title: Falcon Ember (Juniper)
                            description: 38.2% falcon
                            quantity: "74.8676"
                            unit_price: "0.12244898"
                            subtotal_amount: "9.16746047"
                            discount_amount: "0.91674605"
                            tax_amount: "0.55692322"
                            tax_included: false
                            total_amount: "8.80763764"
                            tiered_unit_price: true
                            period_range_start: 2018-11-30
                            period_range_end: 2018-11-30
                            product_id: 35
                            product_version: 1
                            addon_id: 57
                            price_option_id: null
                          - uid: cnli_ad5cdm9q2cm4f
                            title: Prairie Ember (Juniper)
                            description: 38.2% falcon
                            quantity: "183.3492"
                            unit_price: "0.39583333"
                            subtotal_amount: "72.57572871"
                            discount_amount: "7.25757287"
                            tax_amount: "4.40897552"
                            tax_included: false
                            total_amount: "69.72713136"
                            tiered_unit_price: true
                            period_range_start: 2018-11-30
                            period_range_end: 2018-11-30
                            product_id: 35
                            product_version: 1
                            addon_id: 41
                            price_option_id: null
                          - uid: cnli_7pxcbgn623377
                            title: Kestrel Dune
                            description: 38.2% falcon
                            quantity: "10076.9489"
                            unit_price: "0.00031045"
                            subtotal_amount: "3.12839588"
                            discount_amount: "0.31322157"
                            tax_amount: "0.19002427"
                            tax_included: false
                            total_amount: "3.00519858"
                            tiered_unit_price: true
                            period_range_start: 2018-11-30
                            period_range_end: 2018-11-30
                            product_id: 35
                            product_version: 1
                            addon_id: 73
                            price_option_id: null
                          - uid: cnli_5ta9afhthnr3f
                            title: IP Umber
                            description: 38.2% falcon
                            quantity: "3.8198"
                            unit_price: "2.0"
                            subtotal_amount: "7.63955039"
                            discount_amount: "0.76395504"
                            tax_amount: "0.46410269"
                            tax_included: false
                            total_amount: "7.33969804"
                            tiered_unit_price: false
                            period_range_start: 2018-12-30
                            period_range_end: 2018-12-30
                            product_id: 35
                            product_version: 1
                            addon_id: 79
                            price_option_id: 363
                        discounts:
                          - uid: cndli_akyrsg88kx6t0
                            title: Birch-zephyr basalt (10%)
                            code: HHPVA2
                            source_type: Coupon
                            source_id: 58
                            discount_type: percentage
                            percentage: "10.0"
                            eligible_amount: "208.69341779"
                            discount_amount: "20.87125167"
                            line_item_breakouts:
                              - uid: cnli_k75tbqfsfy7td
                                eligible_amount: "1.971004"
                                discount_amount: "0.19862831"
                              - uid: cnli_8zwprscf1n9wr
                                eligible_amount: "114.21127834"
                                discount_amount: "11.42112783"
                              - uid: cnli_ebvqvujqm6mpf
                                eligible_amount: "9.16746047"
                                discount_amount: "0.91674605"
                              - uid: cnli_ad5cdm9q2cm4f
                                eligible_amount: "72.57572871"
                                discount_amount: "7.25757287"
                              - uid: cnli_7pxcbgn623377
                                eligible_amount: "3.12839588"
                                discount_amount: "0.31322157"
                              - uid: cnli_5ta9afhthnr3f
                                eligible_amount: "7.63955039"
                                discount_amount: "0.76395504"
                        taxes:
                          - uid: cntli_x87mw6chw11p6
                            title: NC Quartz Tundra
                            source_type: Tax
                            source_id: 5
                            percentage: "6.75"
                            taxable_amount: "187.82216613"
                            tax_amount: "12.67783387"
                            line_item_breakouts:
                              - uid: cnli_k75tbqfsfy7td
                                taxable_amount: "1.77237569"
                                tax_amount: "0.11963536"
                              - uid: cnli_8zwprscf1n9wr
                                taxable_amount: "102.7901505"
                                tax_amount: "6.93833516"
                              - uid: cnli_ebvqvujqm6mpf
                                taxable_amount: "8.25071442"
                                tax_amount: "0.55692322"
                              - uid: cnli_ad5cdm9q2cm4f
                                taxable_amount: "65.31815584"
                                tax_amount: "4.40897552"
                              - uid: cnli_7pxcbgn623377
                                taxable_amount: "2.81517432"
                                tax_amount: "0.19002427"
                              - uid: cnli_5ta9afhthnr3f
                                taxable_amount: "6.87559535"
                                tax_amount: "0.46410269"
                            tax_addon_breakouts:
                              - tax_rule_id: 5
                                percentage: "6.75"
                                country_code: US
                                subdivision_code: NC
                                tax_amount: "10.66"
                                taxable_amount: "157.95"
                                tax_exempt_amount: "0.0"
                                non_taxable_amount: "0.0"
                                tax_name: NC Cobalt Tundra
                                tax_type: Quartz
                                rate_type: Amber
                                tax_authority_type: 45
                                state_assigned_no: ""
                                tax_sub_type: S
                            eu_vat: false
                            type: Quartz
                            tax_exempt_amount: "0.0"
                        applications:
                          - uid: cdt_qdjnyxa7z2qmk
                            transaction_time: 2018-12-31T21:19:28Z
                            invoice_uid: inv_awz1kb9e5kw4g
                            memo: Delta harbor iris
                            applied_amount: "200.5"
                        refunds:
                          - transaction_id: 815
                            payment_id: 66
                            memo: Delta harbor iris
                            original_amount: "524.9"
                            applied_amount: "200.5"
      operationId: listCreditNotes
      description: >-
        Lists credit notes for a workspace. Credit Notes are like inverse
        invoices. They reduce the amount a customer owes.


        By default, the credit notes returned by this endpoint will exclude the
        arrays of `line_items`, `discounts`, `taxes`, `applications`, or
        `refunds`. To include these arrays, pass the specific field as a key in
        the query with a value set to `true`.
      x-operation-settings:
        collectParameters: true
      parameters:
        - schema:
            type: integer
          in: query
          name: subscription_id
          description: The subscription's Ombrevix Billing id
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - schema:
            type: boolean
            default: false
          in: query
          name: line_items
          description: Include line items data.
        - schema:
            type: boolean
            default: false
          in: query
          name: discounts
          description: Include discounts data.
        - schema:
            type: boolean
            default: false
          in: query
          name: taxes
          description: Include taxes data.
        - schema:
            type: boolean
            default: false
          in: query
          name: refunds
          description: Include refunds data.
        - schema:
            type: boolean
            default: false
          in: query
          name: applications
          description: Include applications data.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Invoices.ListCreditNotes(
                      new ListCreditNotesRequest
                      {
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type ListCreditNotesResponse
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
    List-Credit-Notes-Response:
      title: List Credit Notes Response
      type: object
      properties:
        credit_notes:
          type: array
          items:
            $ref: "#/components/schemas/Credit-Note"
      required:
        - credit_notes
    Credit-Note:
      title: Credit Note
      type: object
      properties:
        uid:
          type: string
          description: Unique identifier for the credit note. It is generated
            automatically by Ombrevix and has the prefix "cn_" followed by
            alphanumeric characters.
        workspace_id:
          type: integer
          description: ID of the workspace to which the credit note belongs.
        customer_id:
          type: integer
          description: ID of the customer to which the credit note belongs.
        subscription_id:
          type: integer
          description: ID of the subscription that generated the credit note.
        number:
          type: string
          description: >-
            A unique, identifying string that appears on the credit note and in
            places it is referenced.


            While the UID is long and not appropriate to show to customers, the
            number is usually shorter and consumable by the customer and the
            merchant alike.
        sequence_number:
          type: integer
          description: A monotonically increasing number assigned to credit notes as they
            are created.  This number is unique within a workspace and can be
            used to sort and order credit notes.
        issue_date:
          type: string
          format: date
          description: >-
            Date the credit note was issued to the customer.  This is the date
            that the credit was made available for application, and may come
            before it is fully applied.


            The format is `"YYYY-MM-DD"`.
        applied_date:
          type: string
          format: date
          description: >-
            Credit notes are applied to invoices to offset invoiced amounts -
            they reduce the amount due. This field is the date the credit note
            became fully applied to invoices.


            If the credit note has been partially applied, this field will not
            have a value until it has been fully applied.


            The format is `"YYYY-MM-DD"`.
        status:
          $ref: "#/components/schemas/Credit-Note-Status"
        currency:
          type: string
          description: The ISO 4217 currency code (3 character string) representing the
            currency of the credit note amount fields.
        memo:
          type: string
          description: The memo printed on credit note, which is a description of the
            reason for the credit.
        seller:
          allOf:
            - $ref: "#/components/schemas/Invoice-Seller"
          description: Information about the seller (merchant) listed on the masthead of
            the credit note.
        customer:
          allOf:
            - $ref: "#/components/schemas/Invoice-Customer"
          description: Information about the customer who is owner or recipient of the
            credited subscription.
        billing_address:
          allOf:
            - $ref: "#/components/schemas/Invoice-Address"
          description: The billing address of the credit subscription.
        shipping_address:
          allOf:
            - $ref: "#/components/schemas/Invoice-Address"
          description: The shipping address of the credited subscription.
        subtotal_amount:
          type: string
          description: Subtotal of the credit note, which is the sum of all line items
            before discounts or taxes. Note that this is a positive amount
            representing the credit back to the customer.
        discount_amount:
          type: string
          description: Total discount applied to the credit note. Note that this is a
            positive amount representing the discount amount being credited back
            to the customer (i.e., a credit on an earlier discount). For
            example, if the original purchase was $1.00 and the original
            discount was $0.10, a credit of $0.50 of the original purchase
            (half) would have a discount credit of $0.05 (also half).
        tax_amount:
          type: string
          description: Total tax of the credit note. Note that this is a positive amount
            representing a previously taxed amount being credited back to the
            customer (i.e., a credit of an earlier tax). For example, if the
            original purchase was $1.00 and the original tax was $0.10, a credit
            of $0.50 of the original purchase (half) would also have a tax
            credit of $0.05 (also half).
        total_amount:
          type: string
          description: The credit note total, which is `subtotal_amount - discount_amount
            + tax_amount`.
        applied_amount:
          type: string
          description: The amount of the credit note that has already been applied to
            invoices.
        remaining_amount:
          type: string
          description: The amount of the credit note remaining to be applied to invoices,
            which is `total_amount - applied_amount`.
        line_items:
          type: array
          description: Line items on the credit note.
          items:
            $ref: "#/components/schemas/Credit-Note-Line-Item"
        discounts:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Discount"
        taxes:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Tax"
        applications:
          type: array
          items:
            $ref: "#/components/schemas/Credit-Note-Application"
        refunds:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Refund"
        origin_invoices:
          type: array
          description: An array of origin invoices for the credit note. Learn more about
            Origin Invoice from our docs.
          items:
            $ref: "#/components/schemas/Origin-Invoice"
    Credit-Note-Status:
      title: Credit Note Status
      type: string
      enum:
        - open
        - applied
      description: Current status of the credit note.
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
    Credit-Note-Line-Item:
      title: Credit Note Line Item
      type: object
      properties:
        uid:
          type: string
          description: Unique identifier for the line item.  Useful when cross-referencing
            the line against individual discounts in the `discounts` or `taxes`
            lists.
        title:
          type: string
          description: A short descriptor for the credit given by this line.
        description:
          type: string
          description: >-
            Detailed description for the credit given by this line.  May include
            proration details in plain text.


            Note: this string may contain line breaks that are hints for the
            best display format on the credit note.
        quantity:
          type: string
          description: >-
            The quantity or count of units credited by the line item.


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
            The approximate discount of just this line.


            The value is approximated in cases where rounding errors make it
            difficult to apportion exactly a total discount among many lines.
            Several lines may have been summed prior to applying the discount to
            arrive at `discount_amount` for the invoice - backing that out to
            the discount on a single line may introduce rounding or precision
            errors.
        tax_amount:
          type: string
          description: >-
            The approximate tax of just this line.


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
            `subtotal_amount`s and discounts or taxes applied
            thereafter.  Therefore, due to rounding or precision errors, the sum
            of line `total_amount`s may not equal the invoice `total_amount`.
        tiered_unit_price:
          type: boolean
          description: When `true`, indicates that the actual pricing scheme for the line
            was tiered, so the `unit_price` shown is the blended average for all
            units.
        period_range_start:
          type: string
          format: date
          description: Start date for the period credited by this line. The format is
            `"YYYY-MM-DD"`.
        period_range_end:
          type: string
          format: date
          description: End date for the period credited by this line. The format is
            `"YYYY-MM-DD"`.
        product_id:
          type: integer
          description: >-
            The ID of the product being credited.


            This may be set even for addon credits, so true product-only
            (non-addon) credits will also have a nil `addon_id`.
        product_version:
          type: integer
          description: The version of the product being credited.
        addon_id:
          type:
            - integer
            - "null"
          description: The ID of the addon being credited. Will be `nil` for non-addon
            credits.
        price_option_id:
          type:
            - integer
            - "null"
          description: The price option ID of the addon being credited. Will be `nil` for
            non-addon credits.
        billing_schedule_item_id:
          type:
            - integer
            - "null"
        custom_item:
          type: boolean
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
    Credit-Note-Application:
      title: Credit Note Application
      type: object
      properties:
        uid:
          type: string
        transaction_time:
          type: string
          format: date-time
        invoice_uid:
          type: string
        memo:
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
    Origin-Invoice:
      title: Origin Invoice
      type: object
      properties:
        uid:
          type: string
          description: The UID of the invoice serving as an origin invoice.
        number:
          type: string
          description: The number of the invoice serving as an origin invoice.
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
  parameters:
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