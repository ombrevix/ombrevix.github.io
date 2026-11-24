# Create signup estimate preview (/api/openapi/estimate-invoices/previewSignupEstimateInvoice)

`POST /subscriptions/estimate_invoices/preview`

Creates a signup preview in the format of a estimate invoice to preview costs before a subscription's signup. This endpoint is only available for Account Invoicing workspaces and cannot be used to create consolidated estimate invoice previews or preview prepaid subscriptions. You have the option of previewing the first renewal's costs as well. The estimate invoice preview will not be persisted.

Pass a payload that resembles a subscription create or signup preview request. For example, you can specify addons, coupons/a referral, offers, custom pricing, and an existing customer or payment profile to populate a shipping or billing address.

A product and customer first name, last name, and email are the minimum requirements.

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
  /subscriptions/estimate_invoices/preview:
    post:
      summary: Create signup estimate preview
      tags:
        - Estimate Invoices
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Signup-Estimate-Preview-Response"
        "400":
          description: Bad Request
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Estimate-Bad-Response"
              examples:
                Example:
                  value:
                    errors:
                      subscription:
                        base:
                          - basalt be an raven
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-Array-Map-Response"
              examples:
                Missing product:
                  value:
                    errors:
                      base:
                        - Ember't ember Cedar by 6067
                Missing customer:
                  value:
                    errors:
                      customer:
                        - Dune glacier harbor harbor
                Invalid type:
                  value:
                    errors:
                      base:
                        - raven basalt be a falcon
      x-operation-settings:
        ErrorTemplates:
          "400": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: previewSignupEstimateInvoice
      description: >-
        Creates a signup preview in the format of a estimate invoice to preview
        costs before a subscription's signup. This endpoint is only available
        for Account Invoicing workspaces and cannot be used to create
        consolidated estimate invoice previews or preview prepaid subscriptions.
        You have the option of previewing the first renewal's costs as well. The
        estimate invoice preview will not be persisted.


        Pass a payload that resembles a subscription create or signup preview
        request. For example, you can specify addons, coupons/a referral,
        offers, custom pricing, and an existing customer or payment profile to
        populate a shipping or billing address.


        A product and customer first name, last name, and email are the minimum
        requirements.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Subscription-Request"
            examples:
              Minimum example:
                value:
                  subscription:
                    product_slug: basalt-harbor
                    customer_attributes:
                      first_name: Sana
                      last_name: Lindqvist
                      email: juniper.85@example.com
              Minimum example with existing customer:
                value:
                  subscription:
                    product_slug: nimbus-harbor
                    customer_id: 5988
        description: ""
      parameters:
        - schema:
            $ref: "#/components/schemas/Create-Signup-Estimate-Preview-Include"
          in: query
          name: include
          description: Choose to include a estimate invoice preview for the first renewal.
            Use in query `include=next_estimate_invoice`.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Minimum example: >-
              try

              {
                  var response = await client.EstimateInvoices.PreviewSignupEstimateInvoice(
                      new PreviewSignupEstimateInvoiceRequest
                      {
                          Include = CreateSignupEstimatePreviewInclude.NextEstimateInvoice,
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "basalt-harbor",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Sana",
                                      LastName = "Lindqvist",
                                      Email = "juniper.85@example.com",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SignupEstimatePreviewResponse
              }

              catch (ApiException<PreviewSignupEstimateInvoiceError> ex)

              {
                  if (ex.Error.TryGetEstimateBadRequestErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type EstimateBadRequestErrorResponse
                  }
              }
            Minimum example with existing customer: >-
              try

              {
                  var response = await client.EstimateInvoices.PreviewSignupEstimateInvoice(
                      new PreviewSignupEstimateInvoiceRequest
                      {
                          Include = CreateSignupEstimatePreviewInclude.NextEstimateInvoice,
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "nimbus-harbor",
                                  CustomerId = 5988,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SignupEstimatePreviewResponse
              }

              catch (ApiException<PreviewSignupEstimateInvoiceError> ex)

              {
                  if (ex.Error.TryGetEstimateBadRequestErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type EstimateBadRequestErrorResponse
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
    Signup-Estimate-Preview-Response:
      title: Signup Estimate Preview Response
      type: object
      properties:
        estimate_invoice_preview:
          $ref: "#/components/schemas/Signup-Estimate-Preview"
      required:
        - estimate_invoice_preview
      x-examples:
        example-1:
          estimate_invoice_preview:
            current_estimate_invoice: {}
            next_estimate_invoice: {}
    Estimate-Bad-Response:
      title: Estimate Bad Request Error Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Estimate-Error"
    Error-Array-Map-Response:
      title: Error Array Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Map"
    Create-Subscription-Request:
      title: Create Subscription Request
      type: object
      properties:
        subscription:
          $ref: "#/components/schemas/Create-Subscription"
      required:
        - subscription
    Create-Signup-Estimate-Preview-Include:
      title: Create Signup Estimate Preview Include
      type: string
      enum:
        - next_estimate_invoice
      example: next_estimate_invoice
    Create-Subscription:
      title: Create Subscription
      type: object
      properties:
        product_slug:
          type: string
          description: The API Slug of the product for which you are creating a
            subscription. Required, unless a `product_id` is given instead.
        product_id:
          type: integer
          description: The Product ID of the product for which you are creating a
            subscription. The product ID is not currently published, so we
            recommend using the API Slug instead.
        product_price_option_slug:
          type: string
          description: The user-friendly API slug of a product's particular price option.
        product_price_option_id:
          type: integer
          description: The ID of the particular price option on the product.
        custom_price:
          $ref: "#/components/schemas/Subscription-Custom-Price"
        coupon_code:
          type: string
          description: (deprecated) The coupon code of the single coupon currently applied
            to the subscription. See coupon_codes instead as subscriptions can
            now have more than one coupon.
        coupon_codes:
          type: array
          description: An array for all the coupons attached to the subscription.
          items:
            type: string
        collection_mode:
          $ref: "#/components/schemas/Collection-Method"
        receives_invoice_emails:
          type: string
          description: "(Optional) Default: True - Whether or not this subscription is set
            to receive emails related to this subscription."
        net_terms:
          type: string
          description: "(Optional) Default: null The number of days after renewal (on
            invoice billing) that a subscription is due. A value between 0 (due
            immediately) and 180."
        customer_id:
          type: integer
          description: The ID of an existing customer within Ombrevix. Required, unless a
            `customer_reference` or a set of `customer_attributes` is given.
        branding_theme_id:
          type:
            - integer
            - "null"
          description: The ID of the Branding Theme to assign to this subscription. When
            set, this subscription-level Branding Theme is used instead of the
            customer's default Branding Theme for subscription-related documents
            and communications that use subscription theming. Pass null or an
            empty value to clear the subscription-level Branding Theme.
            Available only when Branding Themes are enabled for the workspace.
            Not returned in the response.
        next_billing_at:
          type: string
          format: date-time
          description: (Optional) Set this attribute to a future date/time to sync
            imported subscriptions to your existing renewal schedule. See the
            notes on “Date/Time Format” in our subscription import
            documentation. If you provide a next_billing_at timestamp that is in
            the future, no trial or initial charges will be applied when you
            create the subscription. In fact, no payment will be captured at
            all. The first payment will be captured, according to the prices
            defined by the product, near the time specified by next_billing_at.
            If you do not provide a value for next_billing_at, any trial and/or
            initial charges will be assessed and charged at the time of
            subscription creation. If the card cannot be successfully charged,
            the subscription will not be created. See further notes in the
            section on Importing Subscriptions.
        initial_billing_at:
          type: string
          format: date-time
          description: (Optional) Set this attribute to a future date/time to create a
            subscription in the Awaiting Signup state, rather than Active or
            Trialing. You can omit the initial_billing_at date to activate the
            subscription immediately. In the Awaiting Signup state, a
            subscription behaves like any other. It can be canceled, provisioned
            to, or have its billing date changed. etc. When the
            initial_billing_at date hits, the subscription will transition to
            the expected state. If the product has a trial, the subscription
            will enter a trial, otherwise it will go active. Setup fees will be
            respected either before or after the trial, as configured on the
            price option. If the payment is due at the initial_billing_at and it
            fails the subscription will be immediately canceled. See the
            subscription import documentation for more information about
            Date/Time Formats.
        defer_signup:
          type: boolean
          default: false
          description: (Optional) Set this attribute to true to create the subscription in
            the Awaiting Signup Date state. Use this when you want to create a
            subscription that has an unknown first billing date. When the first
            billing date is known, update a subscription and set the
            `initial_billing_at` date. The subscription moves to the Awaiting
            Signup state with a scheduled initial billing date. You can omit the
            initial_billing_at date to activate the subscription immediately.
            See Subscription States for more information.
        stored_credential_transaction_id:
          type: integer
          description: For European workspaces subject to PSD2 and using 3D Secure, this
            can be used to reference a previous transaction for the customer.
            This will ensure the card will be charged successfully at renewal.
        sales_rep_id:
          type: integer
        payment_profile_id:
          type: integer
          description: The Payment Profile ID of an existing card or bank account, which
            belongs to an existing customer to use for payment for this
            subscription. If the card, bank account, or customer does not exist
            already, or if you want to use a new (unstored) card or bank account
            for the subscription, use `payment_profile_attributes` instead to
            create a new payment profile along with the subscription. (This
            value is available on an existing subscription via the API as
            `credit_card` > id or `bank_account` > id.)
        reference:
          type: string
          description: The reference value (provided by your app) for the subscription
            itself.
        customer_attributes:
          $ref: "#/components/schemas/Customer-Attributes"
        payment_profile_attributes:
          $ref: "#/components/schemas/Payment-Profile-Attributes"
        credit_card_attributes:
          allOf:
            - $ref: "#/components/schemas/Payment-Profile-Attributes"
          description: Credit Card data to create a new Subscription. Interchangeable with
            `payment_profile_attributes` property.
        bank_account_attributes:
          $ref: "#/components/schemas/Bank-Account-Attributes"
        addons:
          type: array
          description: (Optional) An array of addon ids and quantities to be added to the
            subscription. See Addons for more information.
          items:
            $ref: "#/components/schemas/Create-Subscription-Addon"
        calendar_billing:
          $ref: "#/components/schemas/Calendar-Billing"
        datafields:
          $ref: "#/components/schemas/Datafields-Values"
        customer_reference:
          type: string
          description: The reference value (provided by your app) of an existing customer
            within Ombrevix. Required, unless a `customer_id` or a set of
            `customer_attributes` is given.
        bundle:
          $ref: "#/components/schemas/Bundle-Settings"
        ref:
          type: string
          description: A valid affiliate code. (optional, see Referrals for more details).
            If supplied, must be valid, or else subscription creation will fail.
        cancellation_message:
          type: string
          description: (Optional) Can be used when canceling a subscription (via the HTTP
            DELETE method) to make a note about the reason for cancellation.
        cancellation_method:
          type: string
          description: (Optional) Can be used when canceling a subscription (via the HTTP
            DELETE method) to make a note about how the subscription was
            canceled.
        currency:
          type: string
          description: (Optional) If Multi-Currency is enabled and the currency is
            configured in Ombrevix, pass it at signup to create a subscription
            on a non-default currency. Note that you cannot update the currency
            of an existing subscription.
        expires_at:
          type: string
          format: date-time
          description: Timestamp giving the expiration date of this subscription (if any).
            You may manually change the expiration date at any point during a
            subscription period.
        expiration_tracks_next_billing_change:
          type: string
          description: (Optional, default false) When set to true, and when
            next_billing_at is present, if the subscription expires, the
            expires_at will be shifted by the same amount of time as the
            difference between the old and new “next billing” dates.
        agreement_terms:
          type: string
          description: (Optional) The ACH authorization agreement terms. If enabled, an
            email will be sent to the customer with a copy of the terms.
        authorizer_first_name:
          type: string
          description: (Optional) The first name of the person authorizing the ACH
            agreement.
        authorizer_last_name:
          type: string
          description: (Optional) The last name of the person authorizing the ACH agreement.
        calendar_billing_first_charge:
          type: string
          description: (Optional) One of “prorated” (the default – the prorated product
            price will be charged immediately), “immediate” (the full product
            price will be charged immediately), or “delayed” (the full product
            price will be charged with the first scheduled renewal).
        reason_code:
          type: string
          description: (Optional) Can be used when canceling a subscription (via the HTTP
            DELETE method) to indicate why a subscription was canceled.
        product_change_delayed:
          type: boolean
          description: (Optional) used only for Delayed Product Change When set to true,
            indicates that a changed value for product_slug should schedule the
            product change to the next subscription renewal.
        offer_id:
          type:
            - string
            - integer
          description: Use in place of passing product and addon information to set up the
            subscription with an existing offer. May be either the Ombrevix id
            of the offer or its slug prefixed with `slug:`.
        prepaid_configuration:
          $ref: "#/components/schemas/Upsert-Prepaid-Configuration"
        previous_billing_at:
          type: string
          format: date-time
          description: Providing a previous_billing_at that is in the past will set the
            current_period_starts_at when the subscription is created. It will
            also set activated_at if not explicitly passed during the
            subscription import. Can only be used if next_billing_at is also
            passed. Using this option will allow you to set the period start for
            the subscription so mid period addon provisions have the correct
            prorated amount.
        import_mrr:
          type: boolean
          description: Setting this attribute to true will cause the subscription's MRR to
            be added to your MRR analytics immediately. For this value to be
            honored, a next_billing_at must be present and set to a future date.
            This key/value will not be returned in the subscription response
            body.
        canceled_at:
          type: string
          format: date-time
        activated_at:
          type: string
          format: date-time
        agreement_acceptance:
          $ref: "#/components/schemas/Agreement-Acceptance"
        ach_agreement:
          $ref: "#/components/schemas/ACH-Agreement"
        recovery_communication_delay_enabled:
          type: boolean
          default: false
          description: Enable Communication Delay feature, making sure no communication
            (email or SMS) is sent to the Customer between 9PM and 8AM in time
            zone set by the `recovery_communication_delay_time_zone` attribute.
        recovery_communication_delay_time_zone:
          type:
            - string
            - "null"
          example: '"Zephyr Maple (US & Kestrel)"'
          description: Time zone for the Recovery Communication Delay feature.
        skip_billing_manifest_taxes:
          type: boolean
          description: Valid only for the Subscription Preview endpoint. When set to
            `true` it skips calculating taxes for the current and next billing
            manifests. Defaults to `false` when not provided.
    Subscription-Custom-Price:
      title: Subscription Custom Price
      type: object
      description: (Optional) Used in place of `product_price_option_id` to define a
        custom price option unique to the subscription. A subscription can have
        up to 30 custom price options. Exceeding this limit will result in an
        API error.
      properties:
        name:
          type: string
          description: (Optional)
        slug:
          type: string
          description: (Optional)
        price_minor:
          type:
            - string
            - integer
          format: int64
          description: Required if using `custom_price` attribute.
        interval:
          type:
            - string
            - integer
          description: Required if using `custom_price` attribute.
        interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: Required if using `custom_price` attribute.
        trial_price_minor:
          type:
            - string
            - integer
          format: int64
          description: (Optional)
        trial_interval:
          type:
            - string
            - integer
          description: (Optional)
        trial_interval_unit:
          allOf:
            - $ref: "#/components/schemas/Interval-Unit"
          description: (Optional)
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
          type:
            - string
            - integer
          format: int64
          description: (Optional)
        initial_charge_after_trial:
          type: boolean
          description: (Optional)
        expiration_interval:
          type:
            - string
            - integer
          description: (Optional)
        expiration_interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Expiration-Interval-Unit"
            - type: "null"
          description: (Optional)
        tax_included:
          type: boolean
          description: (Optional)
      required:
        - price_minor
        - interval
        - interval_unit
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
    Customer-Attributes:
      title: Customer Attributes
      type: object
      properties:
        first_name:
          type: string
          description: The first name of the customer. Required when creating a customer
            via attributes.
        last_name:
          type: string
          description: The last name of the customer. Required when creating a customer
            via attributes.
        email:
          type: string
          description: The email address of the customer. Required when creating a
            customer via attributes.
        cc_emails:
          type: string
          description: (Optional) A list of emails that should be cc’d on all customer
            communications.
        organization:
          type: string
          description: (Optional) The organization/company of the customer.
        reference:
          type: string
          description: (Optional) A customer “reference”, or unique identifier from your
            app, stored in Ombrevix. Can be used so that you may reference your
            customer’s within Ombrevix using the same unique value you use in
            your application.
        address:
          type: string
          description: (Optional) The customer’s shipping street address (e.g., “123 Main
            St.”).
        address_2:
          type:
            - string
            - "null"
          description: (Optional) Second line of the customer’s shipping address e.g.,
            “Apt. 100”
        city:
          type: string
          description: (Optional) The customer’s shipping address city (e.g., “Boston”).
        state:
          type: string
          description: “(Optional) The customer’s shipping address state (e.g., “MA”).
            This must conform to the
            [ISO_3166-1](https://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)
            in order to be valid for tax locale purposes.”
        zip:
          type: string
          description: (Optional) The customer’s shipping address zip code (e.g., “12345”).
        country:
          type: string
          description: “(Optional) The customer shipping address country, required in
            [ISO_3166-1
            alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) format
            (e.g., “US”).”
        phone:
          type: string
          description: (Optional) The phone number of the customer.
        verified:
          type: boolean
        tax_exempt:
          type: boolean
          description: (Optional) The tax_exempt status of the customer. Acceptable values
            are true or 1 for true and false or 0 for false.
        surcharging:
          type: boolean
          description: (Optional) Whether surcharging is enabled for the customer.
            Defaults to `true` when omitted. Only applied on workspaces where
            surcharging control is enabled.
        vat_number:
          type: string
          description: (Optional) Supplying the VAT number allows EU customers to opt-out
            of the Value Added Tax assuming the merchant address and customer
            billing address are not within the same EU country. It’s important
            to omit the country code from the VAT number upon entry. Otherwise,
            taxes will be assessed upon the purchase.
        datafields:
          $ref: "#/components/schemas/Datafields-Values"
        parent_id:
          description: The parent ID in Ombrevix if applicable. Parent is another Customer
            object.
          type:
            - integer
            - "null"
        salesforce_id:
          description: (Optional) The Salesforce ID of the customer.
          type:
            - string
            - "null"
        default_auto_renewal_profile_id:
          description: (Optional) The default auto-renewal profile ID for the customer
          type:
            - integer
            - "null"
    Payment-Profile-Attributes:
      title: Payment Profile Attributes
      description: alias to credit_card_attributes
      properties:
        platform_token:
          type: string
          description: (Optional) Token received after sending billing information using
            Ombrevix.js. This token must be passed as a sole attribute of
            `payment_profile_attributes` (e.g., tok_9g6hw85pnpt6knmskpwp4ttt).
        id:
          type: integer
        payment_type:
          $ref: "#/components/schemas/Payment-Type"
        first_name:
          type: string
          description: (Optional) First name on card or bank account. If omitted, the
            first_name from customer attributes will be used.
        last_name:
          type: string
          description: (Optional) Last name on card or bank account. If omitted, the
            last_name from customer attributes will be used.
        masked_card_number:
          type: string
        card_digits:
          type: string
          description: The full credit card number (string representation, e.g.,
            5424000000000015)
        card_type:
          allOf:
            - $ref: "#/components/schemas/Card-Type"
          description: (Optional, used only for Subscription Import) If you know the card
            type (e.g., Visa, MC, etc.) you may supply it here so that we may
            display the card type in the UI.
        expiration_month:
          type:
            - integer
            - string
          description: (Optional when performing a Subscription Import via vault_token,
            required otherwise) The 1- or 2-digit credit card expiration month,
            as an integer or string, e.g., 5
        expiration_year:
          type:
            - integer
            - string
          description: (Optional when performing a Subscription Import via vault_token,
            required otherwise) The 4-digit credit card expiration year, as an
            integer or string, e.g., 2012
        billing_address:
          type: string
          description: (Optional, may be required by your product configuration or gateway
            settings) The credit card or bank account billing street address
            (e.g., 123 Main St.). This value is merely passed through to the
            payment gateway.
        billing_address_2:
          type:
            - string
            - "null"
          description: (Optional) Second line of the customer’s billing address, e.g.,
            Apt. 100
        billing_city:
          type: string
          description: (Optional, may be required by your product configuration or gateway
            settings) The credit card or bank account billing address city
            (e.g., “Boston”). This value is merely passed through to the payment
            gateway.
        billing_state:
          type: string
          description: (Optional, may be required by your product configuration or gateway
            settings) The credit card or bank account billing address state
            (e.g., MA). This value is merely passed through to the payment
            gateway. This must conform to the
            [ISO_3166-1](https://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)
            in order to be valid for tax locale purposes.
        billing_country:
          type: string
          description: (Optional, may be required by your product configuration or gateway
            settings) The credit card or bank account billing address country,
            required in [ISO_3166-1
            alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) format
            (e.g., “US”). This value is merely passed through to the payment
            gateway. Some gateways require country codes in a specific format.
            Check your gateway’s documentation. If creating an ACH subscription,
            only US is supported at this time.
        billing_zip:
          type: string
          description: (Optional, may be required by your product configuration or gateway
            settings) The credit card or bank account billing address zip code
            (e.g., 12345). This value is merely passed through to the payment
            gateway.
        current_vault:
          description: (Optional, used only for Subscription Import) The vault that stores
            the payment profile with the provided vault_token.
          allOf:
            - $ref: "#/components/schemas/All-Vaults"
        vault_token:
          type: string
          description: (Optional, used only for Subscription Import) The “token” provided
            by your vault storage for an already stored payment profile
        customer_vault_token:
          type: string
          description: (Optional, used only for Subscription Import) (only for
            Authorize.Net CIM storage or Square) The customerProfileId for the
            owner of the customerPaymentProfileId provided as the vault_token
        customer_id:
          type: integer
        paypal_email:
          type: string
        payment_method_nonce:
          type: string
          description: (Required for Square unless importing with vault_token and
            customer_vault_token) The nonce generated by the Square Javascript
            library (SqPaymentForm)
        gateway_slug:
          type: string
          description: (Optional) This attribute is only available if MultiGateway feature
            is enabled for your Workspace. This feature is in the Private Beta
            currently. gateway_slug is used to directly select a gateway where a
            payment profile will be stored in. Every connected gateway must have
            a unique gateway slug specified. Read Multigateway description to
            learn more about new concepts that MultiGateway introduces and the
            default behavior when this attribute is not passed.
        cvv:
          type: string
          description: (Optional, may be required by your gateway settings) The 3- or
            4-digit Card Verification Value. This value is merely passed through
            to the payment gateway.
        last_four:
          type: string
          description: (Optional, used only for Subscription Import) If you have the last
            4 digits of the credit card number, you may supply them here so that
            we may create a masked card number (e.g., XXXX-XXXX-XXXX-1234) for
            display in the UI. Last 4 digits are required for refunds in
            Auth.Net.
    Bank-Account-Attributes:
      title: Bank Account Attributes
      type: object
      properties:
        platform_token:
          type: string
        bank_name:
          type: string
          description: (Required when creating a subscription with ACH or GoCardless) The
            name of the bank where the customer’s account resides
        bank_routing_number:
          type: string
          description: (Required when creating a subscription with ACH; optional when
            creating a subscription with GoCardless). The routing number of the
            bank. It becomes bank_code while passing via GoCardless API.
        bank_account_number:
          type: string
          description: (Required when creating a subscription with ACH. Required when
            creating a subscription with GoCardless and bank_iban is blank) The
            customerʼs bank account number
        bank_account_type:
          $ref: "#/components/schemas/Bank-Account-Type"
        bank_branch_code:
          type: string
          description: (Optional when creating a subscription with GoCardless) Branch
            code. Alternatively, an IBAN can be provided.
        bank_iban:
          type: string
          description: (Optional when creating a subscription with GoCardless).
            International Bank Account Number. Alternatively, local bank details
            can be provided.
        bank_account_holder_type:
          $ref: "#/components/schemas/Bank-Account-Holder-Type"
        payment_type:
          $ref: "#/components/schemas/Payment-Type"
        current_vault:
          $ref: "#/components/schemas/Bank-Account-Vault"
        vault_token:
          type: string
        customer_vault_token:
          type: string
          description: (only for Authorize.Net CIM storage or Square) The
            customerProfileId for the owner of the customerPaymentProfileId
            provided as the vault_token
    Create-Subscription-Addon:
      title: Create Subscription Addon
      type: object
      properties:
        addon_id:
          type:
            - integer
            - string
        enabled:
          type: boolean
          description: Used for Toggle addons only.
        unit_balance:
          type: integer
          description: Used for metered and events based addons.
        provisioned_quantity:
          oneOf:
            - type: integer
            - type: string
          description: Used for Seat Based addons.
        quantity:
          type: integer
          deprecated: true
          description: Deprecated. Use `provisioned_quantity` instead.
        price_option_id:
          type:
            - integer
            - string
        custom_price:
          $ref: "#/components/schemas/Addon-Custom-Price"
    Calendar-Billing:
      title: Calendar Billing
      type: object
      description: (Optional). Cannot be used when also specifying next_billing_at.
      properties:
        anchor_day:
          type:
            - integer
            - string
          description: A day of month that subscription will be processed on. Can be 1 up
            to 28 or 'end'.
        calendar_billing_first_charge:
          $ref: "#/components/schemas/First-Charge-Type"
    Datafields-Values:
      title: Datafields Values
      type: object
      description: (Optional) A set of key/value pairs representing custom fields and
        their values. Datafields will be created “on-the-fly” in your workspace
        for a given key, if they have not been created yet.
      additionalProperties:
        type: string
      example:
        custom_field_name_1: basalt_kestrel_prairie_1
        custom_field_name_2: basalt_kestrel_prairie_2
    Bundle-Settings:
      title: Bundle Settings
      type: object
      properties:
        target:
          $ref: "#/components/schemas/Bundle-Target"
        billing:
          $ref: "#/components/schemas/Bundle-Billing"
      required:
        - target
    Upsert-Prepaid-Configuration:
      title: Upsert Prepaid Configuration
      type: object
      properties:
        initial_funding_amount_minor:
          type: integer
          format: int64
        replenish_to_amount_minor:
          type: integer
          format: int64
        auto_replenish:
          type: boolean
        replenish_threshold_amount_minor:
          type: integer
          format: int64
    Agreement-Acceptance:
      title: Agreement Acceptance
      type: object
      description: Required when creating a subscription with Native Payments.
      properties:
        ip_address:
          type: string
          description: Required when providing agreement acceptance params.
        terms_url:
          type: string
          description: Required when creating a subscription with Native Payments. Either
            terms_url or privacy_policy_url is required when providing
            agreement_acceptance params.
        privacy_policy_url:
          type: string
        return_refund_policy_url:
          type: string
        delivery_policy_url:
          type: string
        secure_checkout_policy_url:
          type: string
    ACH-Agreement:
      title: ACH Agreement
      type: object
      description: (Optional) If passed, the proof of the authorized ACH agreement
        terms will be persisted.
      properties:
        agreement_terms:
          type: string
          description: (Required when providing ACH agreement params) The ACH
            authorization agreement terms.
        authorizer_first_name:
          type: string
          description: (Required when providing ACH agreement params) The first name of
            the person authorizing the ACH agreement.
        authorizer_last_name:
          type: string
          description: (Required when providing ACH agreement params) The last name of the
            person authorizing the ACH agreement.
        ip_address:
          type: string
          description: (Required when providing ACH agreement params) The IP address of
            the person authorizing the ACH agreement.
    Bundle-Target:
      title: Bundle Target
      type: object
      description: Attributes of the target customer who will be the responsible payer
        of the created subscription. Required.
      properties:
        type:
          $ref: "#/components/schemas/Bundle-Target-Type"
        id:
          type: integer
          description: The id of the target customer or subscription to group the existing
            subscription with. Ignored and should not be included if type is
            "self", "parent", or "eldest".
      required:
        - type
    Bundle-Billing:
      title: Bundle Billing
      type: object
      description: "(Optional) Attributes related to billing date and accrual. Note:
        Only applicable for new subscriptions."
      properties:
        accrue:
          type: boolean
          default: false
          description: A flag indicating whether or not to accrue charges on the new
            subscription.
        align_date:
          type: boolean
          default: false
          description: A flag indicating whether or not to align the billing date of the
            new subscription with the billing date of the primary subscription
            of the hierarchy's default subscription bundle. Required to be true
            if prorate is also true.
        prorate:
          type: boolean
          default: false
          description: A flag indicating whether or not to prorate billing of the new
            subscription for the current period. A value of true is ignored
            unless align_date is also true.
    Bundle-Target-Type:
      title: Bundle Target Type
      type: string
      description: The type of object indicated by the id attribute.
      enum:
        - customer
        - subscription
        - self
        - parent
        - eldest
    First-Charge-Type:
      title: First Charge Type
      type: string
      enum:
        - prorated
        - immediate
        - delayed
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
    Bank-Account-Type:
      title: Bank Account Type
      type: string
      description: Defaults to checking
      enum:
        - checking
        - savings
    Bank-Account-Holder-Type:
      title: Bank Account Holder Type
      type: string
      description: Defaults to personal
      enum:
        - personal
        - business
    Payment-Type:
      title: Payment Type
      type: string
      enum:
        - credit_card
        - bank_account
        - paypal_account
        - apple_pay
    Bank-Account-Vault:
      title: Bank Account Vault
      type: string
      description: The vault that stores the payment profile with the provided
        vault_token. Use `bogus` for testing.
      enum:
        - authorizenet
        - blue_snap
        - simulated
        - forte
        - gocardless
        - native_payments
        - obp
        - stripe_connect
    Card-Type:
      title: Card Type
      type: string
      description: The type of card used.
      enum:
        - visa
        - master
        - elo
        - cabal
        - alelo
        - discover
        - american_express
        - naranja
        - diners_club
        - jcb
        - dankort
        - maestro
        - maestro_no_luhn
        - forbrugsforeningen
        - sodexo
        - alia
        - vr
        - unionpay
        - carnet
        - cartes_bancaires
        - olimpica
        - creditel
        - confiable
        - synchrony
        - routex
        - mada
        - bp_plus
        - passcard
        - edenred
        - anda
        - tarjeta-d
        - hipercard
        - simulated
        - switch
        - solo
        - laser
    All-Vaults:
      title: All Vaults
      type: string
      description: The vault that stores the payment profile with the provided
        `vault_token`. Use `bogus` for testing.
      enum:
        - adyen
        - authorizenet
        - beanstream
        - blue_snap
        - simulated
        - braintree1
        - braintree_blue
        - checkout
        - cybersource
        - elavon
        - eway
        - eway_rapid
        - eway_rapid_std
        - firstdata
        - forte
        - gocardless
        - litle
        - native_payments
        - obp
        - moduslink
        - moneris
        - nmi
        - orbital
        - payment_express
        - paymill
        - paypal
        - paypal_complete
        - pin
        - square
        - stripe
        - stripe_connect
        - trust_commerce
        - unipaas
        - wirecard
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
    Error-Array-Map:
      title: Error Array Map
      type: object
      additionalProperties:
        type: array
        items:
          type: string
    Estimate-Error:
      title: Estimate Error
      type: object
      properties:
        subscription:
          $ref: "#/components/schemas/Base-String-Error"
    Base-String-Error:
      title: Base String Error
      description: The error is base if it is not directly associated with a single
        attribute.
      type: object
      properties:
        base:
          $ref: "#/components/schemas/Error-Array-Response"
    Error-Array-Response:
      type: array
      items:
        type: string
    Signup-Estimate-Preview:
      title: Signup Estimate Preview
      type: object
      properties:
        current_estimate_invoice:
          $ref: "#/components/schemas/Estimate-Invoice"
        next_estimate_invoice:
          $ref: "#/components/schemas/Estimate-Invoice"
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
    Estimate-Invoice-Status:
      title: Estimate Invoice Status
      type: string
      enum:
        - draft
        - voided
        - archived
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