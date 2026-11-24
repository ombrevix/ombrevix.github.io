# List Invoice Events (/api/openapi/invoices/listInvoiceEvents)

`GET /invoices/events`

Lists invoice events for a workspace. Each event contains event "data" (such as an applied payment) as well as a snapshot of the `invoice` at the time of event completion.

Exposed event types are:

+ issue_invoice
+ apply_credit_note
+ apply_payment
+ refund_invoice
+ void_invoice
+ void_remainder
+ backport_invoice
+ change_invoice_status
+ change_invoice_collection_method
+ remove_payment
+ failed_payment
+ apply_debit_note
+ create_debit_note
+ change_chargeback_status

Invoice events are returned in ascending order.

If both a `since_date` and `since_id` are provided in request parameters, the `since_date` will be used.

Note - invoice events that occurred prior to 09/05/2018 __will not__ contain an `invoice` snapshot.

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
  /invoices/events:
    get:
      summary: List Invoice Events
      tags:
        - Invoices
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Invoice-Events-Response"
              examples:
                Example:
                  value:
                    events:
                      - id: 63
                        event_type: apply_payment
                        event_data:
                          memo: "Cedar-Kestrel Dune On Fjord - Amber Harbor: Iris willow"
                          original_amount: "168.61"
                          applied_amount: "168.61"
                          transaction_time: 2018-08-01T16:00:00Z
                          payment_method:
                            card_brand: visa
                            card_expiration: 12/2022
                            last_four: null
                            masked_card_number: 43QY-9CZH-VA99-LNRM
                            type: credit_card
                          consolidation_level: none
                        timestamp: 2018-08-01T16:00:00Z
                        invoice:
                          id: 711417441212836700
                          uid: inv_kn0erx39hzxg2
                          workspace_id: 22
                          customer_id: 2
                          subscription_id: 99
                          number: "25"
                          sequence_number: 25
                          transaction_time: 2018-08-01T16:00:00Z
                          created_at: 2018-08-01T16:00:00Z
                          updated_at: 2018-08-01T16:00:00Z
                          issue_date: 2018-08-01
                          due_date: 2018-08-01
                          paid_date: 2018-08-01
                          status: paid
                          role: renewal
                          collection_method: automatic
                          payment_instructions: Amber juniper iris violet to "Glacier, Cedar."
                          currency: Willow
                          consolidation_level: none
                          parent_invoice_id: null
                          subscription_bundle_id: null
                          parent_invoice_number: null
                          product_name: Amber Harbor
                          product_lineup_name: Cedar Glacier Glacier
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
                            logo_url: null
                          customer:
                            platform_id: 2
                            first_name: Kofi
                            last_name: Kaur
                            organization: null
                            email: yarrow.14@example.com
                          payer:
                            platform_id: 2
                            first_name: Kofi
                            last_name: Kaur
                            organization: null
                            email: yarrow.14@example.com
                          net_terms: 0
                          memo: Dune harbor maple prairie! If sierra dune violet falcon, amber basalt
                            maple zephyr delta.
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
                          line_items:
                            - uid: li_khjz844whftv8
                              title: Amber Harbor
                              description: 08/01/2018 - 09/01/2018
                              quantity: "1.0"
                              unit_price: "99.0"
                              subtotal_amount: "99.0"
                              discount_amount: "9.9"
                              tax_amount: "6.01425"
                              tax_included: false
                              total_amount: "95.11425"
                              tiered_unit_price: false
                              period_range_start: 2018-08-01
                              period_range_end: 2018-09-01
                              transaction_id: 496
                              product_id: 71
                              product_version: 1
                              addon_id: null
                              price_option_id: null
                              hide: false
                            - uid: li_zrh672dajmjve
                              title: Falcon Ember (Juniper)
                              description: 07/22/2018 - 08/01/2018
                              quantity: "162.0"
                              unit_price: "0.09567901"
                              subtotal_amount: "15.5"
                              discount_amount: "1.55"
                              tax_amount: "0.941625"
                              tax_included: false
                              total_amount: "14.891625"
                              tiered_unit_price: true
                              period_range_start: 2018-07-22
                              period_range_end: 2018-08-01
                              transaction_id: 731
                              product_id: 71
                              product_version: 1
                              addon_id: 35
                              price_option_id: null
                              hide: false
                              addon_cost_data:
                                rates:
                                  - addon_code_id: null
                                    price_option_id: 593
                                    product_id: 71
                                    quantity: "162.0"
                                    amount: "15.5"
                                    pricing_scheme: tiered
                                    tiers:
                                      - starting_quantity: 1
                                        ending_quantity: 100
                                        quantity: "100.0"
                                        unit_price: "0.0"
                                        amount: "0.0"
                                      - starting_quantity: 101
                                        ending_quantity: null
                                        quantity: "62.0"
                                        unit_price: "0.25"
                                        amount: "15.5"
                            - uid: li_818t7287sns2n
                              title: Prairie Ember (Juniper)
                              description: 07/22/2018 - 08/01/2018
                              quantity: "194.0"
                              unit_price: "0.24226804"
                              subtotal_amount: "47.0"
                              discount_amount: "4.7"
                              tax_amount: "2.85525"
                              tax_included: false
                              total_amount: "45.15525"
                              tiered_unit_price: true
                              period_range_start: 2018-07-22
                              period_range_end: 2018-08-01
                              transaction_id: 536
                              product_id: 71
                              product_version: 1
                              addon_id: 34
                              price_option_id: null
                              hide: false
                              addon_cost_data:
                                rates:
                                  - addon_code_id: null
                                    price_option_id: 270
                                    product_id: 71
                                    quantity: "194.0"
                                    amount: "47.0"
                                    pricing_scheme: tiered
                                    tiers:
                                      - starting_quantity: 1
                                        ending_quantity: 100
                                        quantity: "100.0"
                                        unit_price: "0.0"
                                        amount: "0.0"
                                      - starting_quantity: 101
                                        ending_quantity: null
                                        quantity: "94.0"
                                        unit_price: "0.5"
                                        amount: "47.0"
                            - uid: li_wm7dyzzkfkmnj
                              title: IP Umber
                              description: 08/01/2018 - 09/01/2018
                              quantity: "7.0"
                              unit_price: "2.0"
                              subtotal_amount: "14.0"
                              discount_amount: "1.4"
                              tax_amount: "0.8505"
                              tax_included: false
                              total_amount: "13.4505"
                              tiered_unit_price: false
                              period_range_start: 2018-08-01
                              period_range_end: 2018-09-01
                              transaction_id: 684
                              product_id: 71
                              product_version: 1
                              addon_id: 41
                              price_option_id: 196
                              hide: false
                              addon_cost_data:
                                rates:
                                  - addon_code_id: null
                                    price_option_id: 196
                                    product_id: 71
                                    quantity: "7.0"
                                    amount: "14.0"
                                    pricing_scheme: per_unit
                                    tiers:
                                      - starting_quantity: 1
                                        ending_quantity: null
                                        quantity: "7.0"
                                        unit_price: "2.0"
                                        amount: "14.0"
                          subtotal_amount: "175.5"
                          discount_amount: "17.55"
                          discounts:
                            - uid: dli_3128fdedvh8ct
                              title: Birch-zephyr basalt (10%)
                              description: null
                              code: HHPVA2
                              source_type: Coupon
                              source_id: 58
                              discount_type: percentage
                              percentage: "10.0"
                              eligible_amount: "175.5"
                              discount_amount: "17.55"
                              transaction_id: 199
                              line_item_breakouts:
                                - uid: li_khjz844whftv8
                                  eligible_amount: "99.0"
                                  discount_amount: "9.9"
                                - uid: li_zrh672dajmjve
                                  eligible_amount: "15.5"
                                  discount_amount: "1.55"
                                - uid: li_818t7287sns2n
                                  eligible_amount: "47.0"
                                  discount_amount: "4.7"
                                - uid: li_wm7dyzzkfkmnj
                                  eligible_amount: "14.0"
                                  discount_amount: "1.4"
                          tax_amount: "10.66"
                          taxes:
                            - uid: tli_dmshgvebxncpz
                              title: NC Quartz Tundra
                              description: null
                              source_type: Tax
                              source_id: 5
                              percentage: "6.75"
                              taxable_amount: "157.95"
                              tax_amount: "10.66"
                              transaction_id: 135
                              line_item_breakouts:
                                - uid: li_khjz844whftv8
                                  taxable_amount: "89.1"
                                  tax_amount: "6.01425"
                                - uid: li_zrh672dajmjve
                                  taxable_amount: "13.95"
                                  tax_amount: "0.941625"
                                - uid: li_818t7287sns2n
                                  taxable_amount: "42.3"
                                  tax_amount: "2.85525"
                                - uid: li_wm7dyzzkfkmnj
                                  taxable_amount: "12.6"
                                  tax_amount: "0.8505"
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
                          credit_amount: "0.0"
                          refund_amount: "0.0"
                          total_amount: "168.61"
                          paid_amount: "168.61"
                          due_amount: "0.0"
                          payments:
                            - memo: "Cedar-Kestrel Dune On Fjord - Amber Harbor: Iris willow"
                              original_amount: "168.61"
                              applied_amount: "168.61"
                              transaction_time: 2018-08-01T16:00:00Z
                              payment_method:
                                card_brand: visa
                                card_expiration: 12/2022
                                last_four: null
                                masked_card_number: 43QY-9CZH-VA99-LNRM
                                type: credit_card
                              transaction_id: 322
                              prepayment: false
                          display_settings:
                            hide_zero_subtotal_lines: false
                            include_discounts_on_lines: false
                    page: 48
                    per_page: 1
                    total_pages: 102
      operationId: listInvoiceEvents
      description: >-
        Lists invoice events for a workspace. Each event contains event "data"
        (such as an applied payment) as well as a snapshot of the `invoice` at
        the time of event completion.


        Exposed event types are:


        + issue_invoice

        + apply_credit_note

        + apply_payment

        + refund_invoice

        + void_invoice

        + void_remainder

        + backport_invoice

        + change_invoice_status

        + change_invoice_collection_method

        + remove_payment

        + failed_payment

        + apply_debit_note

        + create_debit_note

        + change_chargeback_status


        Invoice events are returned in ascending order.


        If both a `since_date` and `since_id` are provided in request
        parameters, the `since_date` will be used.


        Note - invoice events that occurred prior to 09/05/2018 __will not__
        contain an `invoice` snapshot.
      x-operation-settings:
        collectParameters: true
      parameters:
        - schema:
            type: string
          in: query
          name: since_date
          description: The timestamp in a format `YYYY-MM-DD T HH:MM:SS Z`, or
            `YYYY-MM-DD`(in this case, it returns data from the beginning of the
            day). of the event from which you want to start the search. All the
            events before the `since_date` timestamp are not returned in the
            response.
        - schema:
            type: integer
            format: int64
          in: query
          name: since_id
          description: The ID of the event from which you want to start the search(ID is
            not included. e.g. if ID is set to 2, then all events with ID 3 and
            more will be shown) This parameter is not used if since_date is
            defined.
        - $ref: "#/components/parameters/page"
        - schema:
            type: integer
            default: 100
            min: 1
            max: 200
          in: query
          name: per_page
          description: This parameter indicates how many records to fetch in each request.
            Default value is 100. The maximum allowed values is 200; any
            per_page value over 200 will be changed to 200.
        - schema:
            type: string
          in: query
          name: invoice_uid
          description: Providing an invoice_uid allows for scoping of the invoice events
            to a single invoice or credit note.
        - schema:
            type: string
          in: query
          description: Use this parameter if you want to fetch also invoice events with
            change_invoice_status type.
          name: with_change_invoice_status
        - schema:
            type: array
            items:
              $ref: "#/components/schemas/Invoice-Event-Type"
          in: query
          name: event_types
          style: form
          explode: false
          description: "Filter results by event_type. Supply a comma separated list of
            event types (listed above). Use in query:
            `event_types=void_invoice,void_remainder`."
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Invoices.ListInvoiceEvents(
                      new ListInvoiceEventsRequest { Page = 1 });
                  // TODO: Handle 'response' of type ListInvoiceEventsResponse
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
    List-Invoice-Events-Response:
      title: List Invoice Events Response
      type: object
      properties:
        events:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Event"
        page:
          type: integer
        per_page:
          type: integer
        total_pages:
          type: integer
    Invoice-Event-Type:
      title: Invoice Event Type
      type: string
      description: Invoice Event Type
      enum:
        - issue_invoice
        - apply_credit_note
        - create_credit_note
        - apply_payment
        - apply_debit_note
        - create_debit_note
        - refund_invoice
        - void_invoice
        - void_remainder
        - backport_invoice
        - change_invoice_status
        - change_invoice_collection_method
        - remove_payment
        - failed_payment
        - change_chargeback_status
    Invoice-Event:
      title: Invoice Event
      type: object
      anyOf:
        - $ref: "#/components/schemas/Apply-Credit-Note-Event"
        - $ref: "#/components/schemas/Apply-Debit-Note-Event"
        - $ref: "#/components/schemas/Apply-Payment-Event"
        - $ref: "#/components/schemas/Backport-Invoice-Event"
        - $ref: "#/components/schemas/Change-Chargeback-Status-Event"
        - $ref: "#/components/schemas/Change-Invoice-Collection-Method-Event"
        - $ref: "#/components/schemas/Change-Invoice-Status-Event"
        - $ref: "#/components/schemas/Create-Credit-Note-Event"
        - $ref: "#/components/schemas/Create-Debit-Note-Event"
        - $ref: "#/components/schemas/Failed-Payment-Event"
        - $ref: "#/components/schemas/Issue-Invoice-Event"
        - $ref: "#/components/schemas/Refund-Invoice-Event"
        - $ref: "#/components/schemas/Remove-Payment-Event"
        - $ref: "#/components/schemas/Void-Invoice-Event"
        - $ref: "#/components/schemas/Void-Remainder-Event"
      discriminator:
        propertyName: event_type
        mapping:
          apply_credit_note: "#/components/schemas/Apply-Credit-Note-Event"
          apply_debit_note: "#/components/schemas/Apply-Debit-Note-Event"
          apply_payment: "#/components/schemas/Apply-Payment-Event"
          backport_invoice: "#/components/schemas/Backport-Invoice-Event"
          change_chargeback_status: "#/components/schemas/Change-Chargeback-Status-Event"
          change_invoice_collection_method: "#/components/schemas/Change-Invoice-Collection-Method-Event"
          change_invoice_status: "#/components/schemas/Change-Invoice-Status-Event"
          create_credit_note: "#/components/schemas/Create-Credit-Note-Event"
          create_debit_note: "#/components/schemas/Create-Debit-Note-Event"
          failed_payment: "#/components/schemas/Failed-Payment-Event"
          issue_invoice: "#/components/schemas/Issue-Invoice-Event"
          refund_invoice: "#/components/schemas/Refund-Invoice-Event"
          remove_payment: "#/components/schemas/Remove-Payment-Event"
          void_invoice: "#/components/schemas/Void-Invoice-Event"
          void_remainder: "#/components/schemas/Void-Remainder-Event"
    Apply-Credit-Note-Event:
      $ref: "#/components/schemas/invoice_event-Apply-Credit-Note-Event"
    Apply-Debit-Note-Event:
      $ref: "#/components/schemas/invoice_event-Apply-Debit-Note-Event"
    Apply-Payment-Event:
      $ref: "#/components/schemas/invoice_event-Apply-Payment-Event"
    Backport-Invoice-Event:
      $ref: "#/components/schemas/invoice_event-Backport-Invoice-Event"
    Change-Chargeback-Status-Event:
      $ref: "#/components/schemas/invoice_event-Change-Chargeback-Status-Event"
    Change-Invoice-Collection-Method-Event:
      $ref: "#/components/schemas/invoice_event-Change-Invoice-Collection-Method-Even\
        t"
    Change-Invoice-Status-Event:
      $ref: "#/components/schemas/invoice_event-Change-Invoice-Status-Event"
    Create-Credit-Note-Event:
      $ref: "#/components/schemas/invoice_event-Create-Credit-Note-Event"
    Create-Debit-Note-Event:
      $ref: "#/components/schemas/invoice_event-Create-Debit-Note-Event"
    Failed-Payment-Event:
      $ref: "#/components/schemas/invoice_event-Failed-Payment-Event"
    Issue-Invoice-Event:
      $ref: "#/components/schemas/invoice_event-Issue-Invoice-Event"
    Refund-Invoice-Event:
      $ref: "#/components/schemas/invoice_event-Refund-Invoice-Event"
    Remove-Payment-Event:
      $ref: "#/components/schemas/invoice_event-Remove-Payment-Event"
    Void-Invoice-Event:
      $ref: "#/components/schemas/invoice_event-Void-Invoice-Event"
    Void-Remainder-Event:
      $ref: "#/components/schemas/invoice_event-Void-Remainder-Event"
    invoice_event-Void-Remainder-Event:
      title: Void Remainder Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: void_remainder
              example: void_remainder
            event_data:
              $ref: "#/components/schemas/Void-Remainder-Event-Data"
          required:
            - event_type
            - event_data
    Invoice-Event-Base:
      title: Invoice Event Base
      type: object
      properties:
        id:
          type: integer
          format: int64
        timestamp:
          type: string
          format: date-time
        invoice:
          $ref: "#/components/schemas/Invoice"
      required:
        - id
        - timestamp
        - invoice
    Void-Remainder-Event-Data:
      title: Void Remainder Event Data
      type: object
      description: Example schema for an `void_remainder` event
      properties:
        credit_note_attributes:
          $ref: "#/components/schemas/Credit-Note"
        memo:
          type: string
          description: The memo provided during invoice remainder voiding.
        applied_amount:
          type: string
          description: The amount of the void.
        transaction_time:
          type: string
          format: date-time
          description: The time the refund was applied, in ISO 8601 format, i.e.
            "2019-06-07T17:20:06Z"
      required:
        - credit_note_attributes
        - memo
        - applied_amount
        - transaction_time
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
    invoice_event-Void-Invoice-Event:
      title: Void Invoice Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: void_invoice
              example: void_invoice
            event_data:
              $ref: "#/components/schemas/Void-Invoice-Event-Data"
          required:
            - event_type
            - event_data
    Void-Invoice-Event-Data:
      title: Void Invoice Event Data
      type: object
      description: Example schema for an `void_invoice` event
      properties:
        credit_note_attributes:
          oneOf:
            - type: "null"
            - $ref: "#/components/schemas/Credit-Note"
        memo:
          type:
            - "null"
            - string
          description: The memo provided during invoice voiding.
        applied_amount:
          type:
            - "null"
            - string
          description: The amount of the void.
        transaction_time:
          type:
            - "null"
            - string
          format: date-time
          description: The time the refund was applied, in ISO 8601 format, i.e.
            "2019-06-07T17:20:06Z"
        is_advance_invoice:
          type: boolean
          description: If true, the invoice is an advance invoice.
        reason:
          type: string
          description: The reason for the void.
      required:
        - credit_note_attributes
        - memo
        - applied_amount
        - transaction_time
        - is_advance_invoice
        - reason
    invoice_event-Remove-Payment-Event:
      title: Remove Payment Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: remove_payment
              example: remove_payment
            event_data:
              $ref: "#/components/schemas/Remove-Payment-Event-Data"
          required:
            - event_type
            - event_data
    Remove-Payment-Event-Data:
      title: Remove Payment Event Data
      type: object
      description: Example schema for an `remove_payment` event
      properties:
        transaction_id:
          type: integer
          description: Transaction ID of the original payment that was removed
        memo:
          type: string
          description: Memo of the original payment
        original_amount:
          type: string
          description: Full amount of the original payment
        applied_amount:
          type: string
          description: Applied amount of the original payment
        transaction_time:
          type: string
          format: date-time
          description: Transaction time of the original payment, in ISO 8601 format, i.e.
            "2019-06-07T17:20:06Z"
        payment_method:
          $ref: "#/components/schemas/Invoice-Event-Payment"
        prepayment:
          type: boolean
          description: The flag that shows whether the original payment was a prepayment
            or not
      required:
        - transaction_id
        - prepayment
        - memo
        - applied_amount
        - transaction_time
        - payment_method
    Invoice-Event-Payment:
      title: Invoice Event Payment
      type: object
      description: A nested data structure detailing the method of payment
      anyOf:
        - $ref: "#/components/schemas/Payment-Method-Apple-Pay"
        - $ref: "#/components/schemas/Payment-Method-Bank-Account"
        - $ref: "#/components/schemas/Payment-Method-Credit-Card"
        - $ref: "#/components/schemas/Payment-Method-External"
        - $ref: "#/components/schemas/Payment-Method-Paypal"
      discriminator:
        propertyName: type
        mapping:
          apple_pay: "#/components/schemas/Payment-Method-Apple-Pay"
          bank_account: "#/components/schemas/Payment-Method-Bank-Account"
          credit_card: "#/components/schemas/Payment-Method-Credit-Card"
          external: "#/components/schemas/Payment-Method-External"
          paypal_account: "#/components/schemas/Payment-Method-Paypal"
    Payment-Method-Apple-Pay:
      $ref: "#/components/schemas/invoice_event_specific_data-Payment-Method-Apple-Pa\
        y"
    Payment-Method-Bank-Account:
      $ref: "#/components/schemas/invoice_event_specific_data-Payment-Method-Bank-Acc\
        ount"
    Payment-Method-Credit-Card:
      $ref: "#/components/schemas/invoice_event_specific_data-Payment-Method-Credit-C\
        ard"
    Payment-Method-External:
      $ref: "#/components/schemas/invoice_event_specific_data-Payment-Method-External"
    Payment-Method-Paypal:
      $ref: "#/components/schemas/invoice_event_specific_data-Payment-Method-Paypal"
    invoice_event_specific_data-Payment-Method-Paypal:
      title: Payment Method Paypal
      type: object
      properties:
        email:
          type: string
          format: email
        type:
          allOf:
            - $ref: "#/components/schemas/Invoice-Event-Payment-Method"
          example: paypal_account
      required:
        - email
        - type
    Invoice-Event-Payment-Method:
      title: Invoice Event Payment Method
      type: string
      enum:
        - apple_pay
        - bank_account
        - credit_card
        - external
        - paypal_account
    invoice_event_specific_data-Payment-Method-External:
      title: Payment Method External
      type: object
      properties:
        details:
          type:
            - string
            - "null"
        kind:
          type: string
        memo:
          type:
            - string
            - "null"
        type:
          allOf:
            - $ref: "#/components/schemas/Invoice-Event-Payment-Method"
          example: external
      required:
        - details
        - kind
        - memo
        - type
    invoice_event_specific_data-Payment-Method-Credit-Card:
      title: Payment Method Credit Card
      type: object
      properties:
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
        type:
          allOf:
            - $ref: "#/components/schemas/Invoice-Event-Payment-Method"
          example: credit_card
      required:
        - card_brand
        - masked_card_number
        - type
    invoice_event_specific_data-Payment-Method-Bank-Account:
      title: Payment Method Bank Account
      type: object
      properties:
        masked_account_number:
          type: string
        masked_routing_number:
          type: string
        type:
          allOf:
            - $ref: "#/components/schemas/Invoice-Event-Payment-Method"
          example: bank_account
      required:
        - masked_account_number
        - masked_routing_number
        - type
    invoice_event_specific_data-Payment-Method-Apple-Pay:
      title: Payment Method Apple Pay
      type: object
      properties:
        type:
          allOf:
            - $ref: "#/components/schemas/Invoice-Event-Payment-Method"
          example: apple_pay
      required:
        - type
    invoice_event-Refund-Invoice-Event:
      title: Refund Invoice Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: refund_invoice
              example: refund_invoice
            event_data:
              $ref: "#/components/schemas/Refund-Invoice-Event-Data"
          required:
            - event_type
            - event_data
    Refund-Invoice-Event-Data:
      title: Refund Invoice Event Data
      type: object
      description: Example schema for an `refund_invoice` event
      properties:
        apply_credit:
          type: boolean
          description: If true, credit was created and applied it to the invoice.
        consolidation_level:
          $ref: "#/components/schemas/Consolidation-Level"
        credit_note_attributes:
          $ref: "#/components/schemas/Credit-Note"
        memo:
          type: string
          description: The refund memo.
        original_amount:
          type: string
          description: The full, original amount of the refund.
        payment_id:
          type: integer
          description: The ID of the payment transaction to be refunded.
        refund_amount:
          type: string
          description: The amount of the refund.
        refund_id:
          type: integer
          description: The ID of the refund transaction.
        transaction_time:
          type: string
          format: date-time
          description: The time the refund was applied, in ISO 8601 format, i.e.
            "2019-06-07T17:20:06Z"
      required:
        - apply_credit
        - credit_note_attributes
        - payment_id
        - refund_amount
        - refund_id
        - transaction_time
    invoice_event-Issue-Invoice-Event:
      title: Issue Invoice Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: issue_invoice
              example: issue_invoice
            event_data:
              $ref: "#/components/schemas/Issue-Invoice-Event-Data"
          required:
            - event_type
            - event_data
    Issue-Invoice-Event-Data:
      title: Issue Invoice Event Data
      type: object
      description: Example schema for an `issue_invoice` event
      properties:
        consolidation_level:
          $ref: "#/components/schemas/Consolidation-Level"
        from_status:
          allOf:
            - $ref: "#/components/schemas/Invoice-Status"
          description: The status of the invoice before event occurrence. See Invoice
            Statuses for more.
        to_status:
          allOf:
            - $ref: "#/components/schemas/Invoice-Status"
          description: The status of the invoice after event occurrence. See Invoice
            Statuses for more.
        due_amount:
          type: string
          description: Amount due on the invoice, which is `total_amount - credit_amount -
            paid_amount`.
        total_amount:
          type: string
          description: The invoice total, which is `subtotal_amount - discount_amount +
            tax_amount`.'
      required:
        - consolidation_level
        - from_status
        - to_status
        - due_amount
        - total_amount
    invoice_event-Failed-Payment-Event:
      title: Failed Payment Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: failed_payment
              example: failed_payment
            event_data:
              $ref: "#/components/schemas/Failed-Payment-Event-Data"
          required:
            - event_type
            - event_data
    Failed-Payment-Event-Data:
      title: Failed Payment Event Data
      type: object
      description: Example schema for an `failed_payment` event
      properties:
        amount_minor:
          type: integer
          description: The monetary value of the payment, expressed minor.
        applied_amount:
          type: integer
          description: The monetary value of the payment, expressed in dollars.
        memo:
          type:
            - string
            - "null"
          description: The memo passed when the payment was created.
        payment_method:
          allOf:
            - $ref: "#/components/schemas/Invoice-Payment-Method-Type"
        transaction_id:
          type: integer
          description: The transaction ID of the failed payment.
      required:
        - amount_minor
        - applied_amount
        - transaction_id
        - payment_method
    Invoice-Payment-Method-Type:
      title: Invoice Payment Method Type
      type: string
      description: The type of payment method used. Defaults to other.
      enum:
        - credit_card
        - check
        - cash
        - money_order
        - ach
        - other
    invoice_event-Create-Debit-Note-Event:
      title: Create Debit Note Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: create_debit_note
              example: create_debit_note
            event_data:
              $ref: "#/components/schemas/Create-Debit-Note-Event-Data"
          required:
            - event_type
            - event_data
    Create-Debit-Note-Event-Data:
      title: Create Debit Note Event Data
      type: object
      description: Example schema for an `create_debit_note` event
      allOf:
        - $ref: "#/components/schemas/Debit-Note"
    Debit-Note:
      title: Debit Note
      type: object
      properties:
        uid:
          type: string
          description: Unique identifier for the debit note. It is generated automatically
            by Ombrevix and has the prefix "db_" followed by alphanumeric
            characters.
        workspace_id:
          type: integer
          description: ID of the workspace to which the debit note belongs.
        customer_id:
          type: integer
          description: ID of the customer to which the debit note belongs.
        subscription_id:
          type: integer
          description: ID of the subscription that generated the debit note.
        number:
          type: integer
          description: A unique identifier that appears on the debit note and in places it
            is referenced.
        sequence_number:
          type: integer
          description: A monotonically increasing number assigned to debit notes as they
            are created.
        origin_credit_note_uid:
          type: string
          description: >-
            Unique identifier for the connected credit note. It is generated
            automatically by Ombrevix and has the prefix "cn_" followed by
            alphanumeric characters.


            While the UID is long and not appropriate to show to customers, the
            number is usually shorter and consumable by the customer and the
            merchant alike.
        origin_credit_note_number:
          type: string
          description: A unique identifying string of the connected credit note.
        issue_date:
          type: string
          format: date
          description: >-
            Date the document was issued to the customer. This is the date that
            the document was made available for payment.


            The format is "YYYY-MM-DD".
        applied_date:
          type: string
          format: date
          description: >-
            Debit notes are applied to invoices to offset invoiced amounts -
            they adjust the amount due. This field is the date the debit note
            document became fully applied to the invoice.


            The format is "YYYY-MM-DD".
        due_date:
          type: string
          format: date
          description: Date the document is due for payment. The format is "YYYY-MM-DD".
        status:
          $ref: "#/components/schemas/Debit-Note-Status"
        memo:
          type: string
          description: The memo printed on debit note, which is a description of the
            reason for the debit.
        role:
          $ref: "#/components/schemas/Debit-Note-Role"
        currency:
          type: string
          description: The ISO 4217 currency code (3 character string) representing the
            currency of the credit note amount fields.
        seller:
          allOf:
            - $ref: "#/components/schemas/Invoice-Seller"
          description: Information about the seller (merchant) listed on the masthead of
            the debit note.
        customer:
          allOf:
            - $ref: "#/components/schemas/Invoice-Customer"
          description: Information about the customer who is the owner or recipient of the
            debited subscription.
        billing_address:
          allOf:
            - $ref: "#/components/schemas/Invoice-Address"
          description: The billing address of the debited subscription.
        shipping_address:
          allOf:
            - $ref: "#/components/schemas/Invoice-Address"
          description: The shipping address of the debited subscription.
        line_items:
          type: array
          description: Line items on the debit note.
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
        refunds:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Refund"
    Debit-Note-Status:
      title: Debit Note Status
      type: string
      enum:
        - open
        - applied
        - banished
        - paid
      description: Current status of the debit note.
    invoice_event-Create-Credit-Note-Event:
      title: Create Credit Note Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: create_credit_note
              example: create_credit_note
            event_data:
              $ref: "#/components/schemas/Create-Credit-Note-Event-Data"
          required:
            - event_type
            - event_data
    Create-Credit-Note-Event-Data:
      title: Create Credit Note Event Data
      type: object
      description: Example schema for an `create_credit_note` event
      allOf:
        - $ref: "#/components/schemas/Credit-Note"
    invoice_event-Change-Invoice-Status-Event:
      title: Change Invoice Status Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: change_invoice_status
              example: change_invoice_status
            event_data:
              $ref: "#/components/schemas/Change-Invoice-Status-Event-Data"
          required:
            - event_type
            - event_data
    Change-Invoice-Status-Event-Data:
      title: Change Invoice Status Event Data
      type: object
      description: Example schema for an `change_invoice_status` event
      properties:
        gateway_trans_id:
          type: string
          description: Identifier for the transaction within the payment gateway.
        amount:
          type: string
          description: The monetary value associated with the linked payment, expressed in
            dollars.
        from_status:
          allOf:
            - $ref: "#/components/schemas/Invoice-Status"
          description: The status of the invoice before any changes occurred. See Invoice
            Statuses for more.
        to_status:
          allOf:
            - $ref: "#/components/schemas/Invoice-Status"
          description: The updated status of the invoice after changes have been made. See
            Invoice Statuses for more.
        consolidation_level:
          allOf:
            - $ref: "#/components/schemas/Consolidation-Level"
      required:
        - from_status
        - to_status
    invoice_event-Change-Invoice-Collection-Method-Event:
      title: Change Invoice Collection Method Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: change_invoice_collection_method
              example: change_invoice_collection_method
            event_data:
              $ref: "#/components/schemas/Change-Invoice-Collection-Method-Event-Data"
          required:
            - event_type
            - event_data
    Change-Invoice-Collection-Method-Event-Data:
      title: Change Invoice Collection Method Event Data
      type: object
      description: Example schema for an `change_invoice_collection_method` event
      properties:
        from_collection_method:
          type: string
          description: The previous collection method of the invoice.
        to_collection_method:
          type: string
          description: The new collection method of the invoice.
      required:
        - from_collection_method
        - to_collection_method
    invoice_event-Change-Chargeback-Status-Event:
      title: Change Chargeback Status Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: change_chargeback_status
              example: change_chargeback_status
            event_data:
              $ref: "#/components/schemas/Change-Chargeback-Status-Event-Data"
          required:
            - event_type
            - event_data
    Change-Chargeback-Status-Event-Data:
      title: Change Chargeback Status Event Data
      type: object
      description: Example schema for an `change_chargeback_status` event
      properties:
        chargeback_status:
          allOf:
            - $ref: "#/components/schemas/Chargeback-Status"
      required:
        - chargeback_status
    Chargeback-Status:
      title: Chargeback Status
      type: string
      enum:
        - open
        - lost
        - won
        - closed
      description: The current chargeback status.
    invoice_event-Backport-Invoice-Event:
      title: Backport Invoice Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: backport_invoice
              example: backport_invoice
            event_data:
              $ref: "#/components/schemas/Backport-Invoice-Event-Data"
          required:
            - event_type
            - event_data
    Backport-Invoice-Event-Data:
      title: Backport Invoice Event Data
      type: object
      description: Example schema for an `backport_invoice` event
      allOf:
        - $ref: "#/components/schemas/Invoice"
    invoice_event-Apply-Payment-Event:
      title: Apply Payment Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: apply_payment
              example: apply_payment
            event_data:
              $ref: "#/components/schemas/Apply-Payment-Event-Data"
          required:
            - event_type
            - event_data
    Apply-Payment-Event-Data:
      title: Apply Payment Event Data
      type: object
      description: Example schema for an `apply_payment` event
      properties:
        consolidation_level:
          allOf:
            - $ref: "#/components/schemas/Consolidation-Level"
        memo:
          type: string
          description: The payment memo
        original_amount:
          type: string
          description: 'The full, original amount of the payment transaction as a string
            in full units. Incoming payments can be split amongst several
            invoices, which will result in a `applied_amount` less than the
            `original_amount`. Example: A $100.99 payment, of which $40.11 is
            applied to this invoice, will have an `original_amount` of
            `"100.99"`.'
        applied_amount:
          type: string
          description: 'The amount of the payment applied to this invoice. Incoming
            payments can be split amongst several invoices, which will result in
            a `applied_amount` less than the `original_amount`. Example: A
            $100.99 payment, of which $40.11 is applied to this invoice, will
            have an `applied_amount` of `"40.11"`.'
        transaction_time:
          type: string
          format: date-time
          description: The time the payment was applied, in ISO 8601 format, i.e.
            "2019-06-07T17:20:06Z"
        payment_method:
          $ref: "#/components/schemas/Invoice-Event-Payment"
        transaction_id:
          type: integer
          description: The Ombrevix id of the original payment
        parent_invoice_number:
          type:
            - "null"
            - integer
        remaining_prepayment_amount:
          type:
            - string
            - "null"
        prepayment:
          type: boolean
        external:
          type: boolean
      required:
        - consolidation_level
        - memo
        - original_amount
        - applied_amount
        - transaction_time
        - payment_method
    invoice_event-Apply-Debit-Note-Event:
      title: Apply Debit Note Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: apply_debit_note
              example: apply_debit_note
            event_data:
              $ref: "#/components/schemas/Apply-Debit-Note-Event-Data"
          required:
            - event_type
            - event_data
    Apply-Debit-Note-Event-Data:
      title: Apply Debit Note Event Data
      type: object
      description: Example schema for an `apply_debit_note` event
      properties:
        debit_note_number:
          type: string
          description: A unique, identifying string that appears on the debit note and in
            places it is referenced.
        debit_note_uid:
          type: string
          description: Unique identifier for the debit note. It is generated automatically
            by Ombrevix and has the prefix "db_" followed by alphanumeric
            characters.
        original_amount:
          type: string
          description: The full, original amount of the debit note.
        applied_amount:
          type: string
          description: The amount of the debit note applied to invoice.
        memo:
          type:
            - string
            - "null"
          description: The debit note memo.
        transaction_time:
          type:
            - string
            - "null"
          format: date-time
          description: The time the debit note was applied, in ISO 8601 format, i.e.
            "2019-06-07T17:20:06Z"
      required:
        - debit_note_number
        - debit_note_uid
        - original_amount
        - applied_amount
    invoice_event-Apply-Credit-Note-Event:
      title: Apply Credit Note Event
      type: object
      allOf:
        - $ref: "#/components/schemas/Invoice-Event-Base"
        - type: object
          properties:
            event_type:
              allOf:
                - $ref: "#/components/schemas/Invoice-Event-Type"
              default: apply_credit_note
              example: apply_credit_note
            event_data:
              $ref: "#/components/schemas/Apply-Credit-Note-Event-Data"
          required:
            - event_type
            - event_data
    Apply-Credit-Note-Event-Data:
      title: Apply Credit Note Event Data
      type: object
      description: Example schema for an `apply_credit_note` event
      properties:
        uid:
          type: string
          description: Unique identifier for the credit note application. It is generated
            automatically by Ombrevix and has the prefix "cdt_" followed by
            alphanumeric characters.
        credit_note_number:
          type: string
          description: A unique, identifying string that appears on the credit note and in
            places it is referenced.
        credit_note_uid:
          type: string
          description: Unique identifier for the credit note. It is generated
            automatically by Ombrevix and has the prefix "cn_" followed by
            alphanumeric characters.
        original_amount:
          type: string
          description: The full, original amount of the credit note.
        applied_amount:
          type: string
          description: The amount of the credit note applied to invoice.
        transaction_time:
          type: string
          format: date-time
          description: The time the credit note was applied, in ISO 8601 format, i.e.
            "2019-06-07T17:20:06Z"
        memo:
          type:
            - string
            - "null"
          description: The credit note memo.
        role:
          type: string
          description: The role of the credit note (e.g. 'general')
        consolidated_invoice:
          type: boolean
          description: Shows whether it was applied to consolidated invoice or not.
        applied_credit_notes:
          type: array
          description: List of credit notes applied to children invoices (if consolidated
            invoice)
          items:
            $ref: "#/components/schemas/Apply-Credit-Note-Credit-Notes-Data"
      required:
        - uid
        - credit_note_number
        - credit_note_uid
        - original_amount
        - applied_amount
    Apply-Credit-Note-Credit-Notes-Data:
      title: Applied Credit Note Data
      type: object
      properties:
        uid:
          type: string
          description: The UID of the credit note
        number:
          type: string
          description: The number of the credit note
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
info:
  title: Ombrevix Billing
  version: "1.0"
```