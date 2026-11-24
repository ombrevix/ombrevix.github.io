# Create Subscription (/api/openapi/subscriptions/createSubscription)

`POST /subscriptions`


Creates a Subscription for a customer and product.

Specify the product with `product_id` or `product_slug`. To set a specific product price option, use `product_price_option_slug` or `product_price_option_id`.

Identify an existing customer with `customer_id` or `customer_reference`. Optionally, include an existing payment profile using `payment_profile_id`. To create a new customer, pass customer_attributes. 

Select an option from the **Request Examples** drop-down on the right side of the portal to see examples of common scenarios for creating subscriptions. 

## List vs Sales Pricing

When a subscription uses custom pricing as the sales price, you can optionally provide a list price for any item. If omitted, the list price defaults to the sales price. The difference between the list price and sales price is used to calculate implicit discounts, which appear on Invoices and in reporting. List price can also support revenue provisions in Advanced Revenue.

If your workspace has list pricing enabled, the API accepts `custom_price.list_price_option_id` for custom pricing, validates and persists it, and returns list price metadata in subscription responses. If list pricing is disabled, this input is ignored and related response fields are omitted.

When list pricing is enabled:

- Subscription → Product `product_price_option_list_price_option_id` (integer)
- `product_price_option_list_price_option_slug` (string)
- Subscription Addons (when addons are included in the response, such as with subscriptions built from addons or addon serialization paths) `addon_id` (integer)
- `price_option_id` (integer)
- `list_price_option_id` (integer)

When list pricing is disabled:

- Subscription → Product `product_price_option_list_price_option_id`: omitted
- `product_price_option_list_price_option_slug`: omitted
- Subscription Addons `list_price_option_id`: omitted

This functionality is supported in the API, but is not currently supported in SDKs.

## Subscriptions can now work independently from the catalog

 If you have the new Catalog experience enabled, you can create subscriptions without a `product_id` or `product_slug` using POST /subscriptions, building them entirely from addons.

A valid subscription must include at least one active addon with:
- a positive `provisioned_quantity`,
- a positive `unit_balance`, or
- 'enabled: true' (for Toggle addons)
- a configured metered addon

`addon_id` can be provided as a numeric ID or in slug: format. If `trial_interval` and `trial_interval_unit` are included, they are applied at creation.

In the response, product and product price option fields are null, and addon details are returned instead.

This functionality is supported in the API, but is not currently supported in SDKs.

## Payment information

Payment information may be required to create a subscription, depending on the options for the Product being subscribed. See product options for more information. See the [Payments Profile]($e/Payment%20Profiles/createPaymentProfile) endpoint for details on payment parameters.
See the Subscription Signups article for more information on working with subscriptions in Ombrevix Billing.

## Payment information  

Payment information may be required to create a subscription, depending on the options for the Product being subscribed. See product options for more information. See the [Payments Profile]($e/Payment%20Profiles/createPaymentProfile) endpoint for details on payment parameters. 

Do not use real card information for testing. See the Workspaces articles that cover testing your workspace setup for more details on testing in your sandbox.

Note that collecting and sending raw card details in production requires PCI compliance on your end. If your business is not PCI compliant, use Ombrevix.js to collect credit card or bank account information.

## 3D Secure (3DS) Authentication post-authentication flow

When a payment requires 3DS Authentication to adhere to Strong Customer Authentication (SCA), the request enters a post-authentication flow where a 422 Unprocessable Entity status is returned with an action_link that will direct the customer through 3DS Authentication. 

See the 3D Secure Post-Authentication Flow article in the product documentation to learn how to manage the redirect flow.

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
  /subscriptions:
    post:
      summary: Create Subscription
      tags:
        - Subscriptions
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Response"
              examples:
                Example:
                  value:
                    subscription:
                      id: 11073687
                      state: active
                      balance_minor: 0
                      total_revenue_minor: 14000
                      product_price_minor: 1000
                      product_version_number: 7
                      current_period_ends_at: 2016-11-15T14:48:10-05:00
                      next_evaluation_at: 2016-11-15T14:48:10-05:00
                      trial_started_at: null
                      trial_over_at: null
                      activated_at: 2016-11-14T14:48:12-05:00
                      expires_at: null
                      created_at: 2016-11-14T14:48:10-05:00
                      updated_at: 2016-11-14T15:24:41-05:00
                      cancellation_message: null
                      cancellation_method: merchant_api
                      cancel_when_period_ends: null
                      canceled_at: null
                      current_period_started_at: 2016-11-14T14:48:10-05:00
                      previous_state: active
                      initial_payment_id: 943245466
                      initial_revenue: "260.00"
                      deferred_cancel_at: null
                      coupon_code: A9Z26MDAPKRZ
                      collection_mode: automatic
                      anchor_day: null
                      reason_code: null
                      receives_invoice_emails: false
                      customer:
                        first_name: Kofi
                        last_name: Silva
                        email: violet.81@example.com
                        cc_emails: umber.56@example.com
                        organization: ""
                        reference: null
                        id: 17832448
                        created_at: 2016-11-14T14:48:10-05:00
                        updated_at: 2016-11-14T14:48:13-05:00
                        address: 123 Garnet Harbor
                        address_2: ""
                        city: Riverend
                        state: CO
                        zip: "36263"
                        country: US
                        phone: ""
                        verified: false
                        portal_customer_created_at: 2016-11-14T14:48:13-05:00
                        portal_invite_last_sent_at: 2016-11-14T14:48:13-05:00
                        portal_invite_last_accepted_at: null
                        tax_exempt: false
                        vat_number: L8W6ST6PG
                      product:
                        id: 9023412
                        name: $10 Garnet Harbor
                        slug: garnet
                        description: zephyr amber
                        accounting_code: garnet
                        price_minor: 1000
                        interval: 1
                        interval_unit: day
                        initial_charge_minor: null
                        expiration_interval: null
                        expiration_interval_unit: never
                        trial_price_minor: null
                        trial_interval: null
                        trial_interval_unit: month
                        initial_charge_after_trial: false
                        return_params: ""
                        request_credit_card: false
                        require_credit_card: false
                        created_at: 2016-03-24T13:38:39-04:00
                        updated_at: 2016-11-03T13:03:05-04:00
                        archived_at: null
                        update_return_url: ""
                        update_return_params: ""
                        product_lineup:
                          id: 246771
                          name: Glacier Harbor
                          slug: basalt-delta
                          accounting_code: null
                          description: ""
                        hosted_signup_forms:
                          - id: 104001
                            url: https://sample-shop.ombrevix.example/subscribe/kqvmfrbgd89q/basic
                          - id: 152094
                            url: https://sample-shop.ombrevix.example/subscribe/dkffht5dxfd8/basic
                          - id: 991180
                            url: https://sample-shop.ombrevix.example/subscribe/jwffwgdd95s8/basic
                        taxable: false
                        version_number: 7
                        product_price_option_name: Basalt
                      credit_card:
                        id: 69210350
                        payment_type: credit_card
                        first_name: Kofi
                        last_name: Silva
                        masked_card_number: Q739-C75X-XKG4-Q
                        card_type: simulated
                        expiration_month: 1
                        expiration_year: 2026
                        billing_address: 123 Garnet Harbor
                        billing_address_2: ""
                        billing_city: Riverend
                        billing_state: null
                        billing_country: ""
                        billing_zip: "36263"
                        current_vault: simulated
                        vault_token: "1"
                        customer_vault_token: null
                        customer_id: 17832448
                      payment_type: credit_card
                      affiliate_code: w7glacier9
                      next_product_id: null
                      coupon_use_count: 1
                      coupon_uses_allowed: 1
                      next_product_slug: null
                      stored_credential_transaction_id: 278297828658700
                      recovery_communication_delay_enabled: true
                      recovery_communication_delay_time_zone: Zephyr Maple (US & Kestrel)
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
                      - "Garnet falcon garnet: cobalt be lagoon."
                      - "Garnet zephyr garnet: cobalt be lagoon."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createSubscription
      description: >-
        
        Creates a Subscription for a customer and product.


        Specify the product with `product_id` or `product_slug`. To set a
        specific product price option, use `product_price_option_slug` or
        `product_price_option_id`.


        Identify an existing customer with `customer_id` or
        `customer_reference`. Optionally, include an existing payment profile
        using `payment_profile_id`. To create a new customer, pass
        customer_attributes. 


        Select an option from the **Request Examples** drop-down on the right
        side of the portal to see examples of common scenarios for creating
        subscriptions. 


        ## List vs Sales Pricing


        When a subscription uses custom pricing as the sales price, you can
        optionally provide a list price for any item. If omitted, the list price
        defaults to the sales price. The difference between the list price and
        sales price is used to calculate implicit discounts, which appear on
        Invoices and in reporting. List price can also support revenue
        provisions in Advanced Revenue.


        If your workspace has list pricing enabled, the API accepts
        `custom_price.list_price_option_id` for custom pricing, validates and
        persists it, and returns list price metadata in subscription responses.
        If list pricing is disabled, this input is ignored and related response
        fields are omitted.


        When list pricing is enabled:


        - Subscription → Product `product_price_option_list_price_option_id`
        (integer)

        - `product_price_option_list_price_option_slug` (string)

        - Subscription Addons (when addons are included in the response, such as
        with subscriptions built from addons or addon serialization paths)
        `addon_id` (integer)

        - `price_option_id` (integer)

        - `list_price_option_id` (integer)


        When list pricing is disabled:


        - Subscription → Product `product_price_option_list_price_option_id`:
        omitted

        - `product_price_option_list_price_option_slug`: omitted

        - Subscription Addons `list_price_option_id`: omitted


        This functionality is supported in the API, but is not currently
        supported in SDKs.


        ## Subscriptions can now work independently from the catalog

         If you have the new Catalog experience enabled, you can create subscriptions without a `product_id` or `product_slug` using POST /subscriptions, building them entirely from addons.

        A valid subscription must include at least one active addon with:

        - a positive `provisioned_quantity`,

        - a positive `unit_balance`, or

        - 'enabled: true' (for Toggle addons)

        - a configured metered addon


        `addon_id` can be provided as a numeric ID or in slug: format. If
        `trial_interval` and `trial_interval_unit` are included, they are
        applied at creation.


        In the response, product and product price option fields are null, and
        addon details are returned instead.


        This functionality is supported in the API, but is not currently
        supported in SDKs.


        ## Payment information


        Payment information may be required to create a subscription, depending
        on the options for the Product being subscribed. See product options for
        more information. See the [Payments
        Profile]($e/Payment%20Profiles/createPaymentProfile) endpoint for
        details on payment parameters.

        See the Subscription Signups article for more information on working
        with subscriptions in Ombrevix Billing.


        ## Payment information  


        Payment information may be required to create a subscription, depending
        on the options for the Product being subscribed. See product options for
        more information. See the [Payments
        Profile]($e/Payment%20Profiles/createPaymentProfile) endpoint for
        details on payment parameters. 


        Do not use real card information for testing. See the Workspaces
        articles that cover testing your workspace setup for more details on
        testing in your sandbox.


        Note that collecting and sending raw card details in production requires
        PCI compliance on your end. If your business is not PCI compliant, use
        Ombrevix.js to collect credit card or bank account information.


        ## 3D Secure (3DS) Authentication post-authentication flow


        When a payment requires 3DS Authentication to adhere to Strong Customer
        Authentication (SCA), the request enters a post-authentication flow
        where a 422 Unprocessable Entity status is returned with an action_link
        that will direct the customer through 3DS Authentication. 


        See the 3D Secure Post-Authentication Flow article in the product
        documentation to learn how to manage the redirect flow.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Subscription-Request"
            examples:
              Basic:
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                      zip: "82205"
                      state: MA
                      reference: Amber
                      phone: (540) 017 - 9198
                      organization: Glacier
                      country: US
                      city: Penhallow
                      address_2: null
                      address: 123 Garnet Juniper.
                    collection_mode: manual
              Calendar Billing:
                value:
                  subscription:
                    product_slug: basalt
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                      zip: "82205"
                      state: MA
                      reference: Amber
                      phone: (540) 017 - 9198
                      organization: Glacier
                      country: US
                      city: Penhallow
                      address_2: null
                      address: 123 Garnet Juniper.
                    calendar_billing:
                      anchor_day: 1
                      calendar_billing_first_charge: immediate
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
              Addons (Metered):
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
                    addons:
                      - addon_id: 179136
                        unit_balance: 20
              Addons (Toggle):
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
                    addons:
                      - addon_id: 1642
                        enabled: true
              Addons (Seat-based):
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Kofi
                      last_name: Silva
                      email: falcon.91@example.com
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
                    addons:
                      - addon_id: 5
                        provisioned_quantity: 18
                        price_option_id: 4
              Addons By Slug (Seat-based):
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
                    addons:
                      - addon_id: kestrel:my-kestrel
                        price_option_id: kestrel:garnet-maple
                        provisioned_quantity: 14
              With Coupon:
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
                    coupon_code: 6EDXPL
              Grouped:
                value:
                  subscription:
                    product_slug: cedar-amber
                    customer_id: 33573617
                    payment_profile_id: 62434395
                    bundle:
                      target:
                        type: customer
                        id: 33573617
                      billing:
                        align_date: true
                        prorate: false
                        accrue: false
              In Hierarchy (not Group):
                value:
                  subscription:
                    product_slug: test_fccqs81
                    customer_attributes:
                      first_name: Mei
                      last_name: Novak
                      email: violet.86@example.com
                      parent_id: 99369533
              Import as Canceled:
                value:
                  subscription:
                    product_slug: cedar
                    canceled_at: 2021-02-25T12:00:00-04:00
                    cancellation_message: Cedar lagoon
                    reason_code: lagoon
                    activated_at: 2021-01-17T12:00:00-04:00
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: harbor.98@example.com
                    bank_account_attributes:
                      bank_name: My Garnet
                      current_vault: stripe_connect
                      vault_token: 5DR5
              Import With Future Billing Date:
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
                    next_billing_at: 2020-06-01T12:00:00-04:00
              Import Authorize.Net ACH:
                value:
                  subscription:
                    product_slug: cedar
                    next_billing_at: 2020-08-29T12:00:00-04:00
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: juniper.11@example.com
                    bank_account_attributes:
                      bank_name: My Garnet
                      current_vault: authorizenet
                      vault_token: 5DR5
                      customer_vault_token: LFBW
              Import Stripe ACH:
                value:
                  subscription:
                    product_slug: cedar
                    next_billing_at: 2020-08-29T12:00:00-04:00
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: juniper.11@example.com
                    bank_account_attributes:
                      bank_name: My Garnet
                      current_vault: stripe_connect
                      vault_token: cus_wwdavn
              Metadata (Subscription-Level):
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
                    datafields:
                      color: onyx
                      comments: Dune!
              Metadata (Customer-Level):
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                      datafields:
                        nickname: Prairie
                        language: Raven
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
              With Offer:
                value:
                  subscription:
                    offer_id: kestrel:violet
                    customer_attributes:
                      first_name: Kofi
                      last_name: Silva
                      email: falcon.91@example.com
                    credit_card_attributes:
                      masked_card_number: 43QY-9CZH-VA99-LNRM
                      expiration_month: "10"
                      expiration_year: KWV3
              Prepaid Subscription:
                value:
                  subscription:
                    product_slug: garnet
                    customer_id: 79288
                    payment_profile_id: 8553
                    collection_mode: prepaid
                    prepaid_configuration:
                      initial_funding_amount_minor: 10000
                      replenish_to_amount_minor: 6000
                      auto_replenish: true
                      replenish_threshold_amount_minor: 5000
              With Affiliate code:
                value:
                  subscription:
                    product_slug: cedar-kestrel
                    ref: 1q2w3e
                    customer_attributes:
                      first_name: Leila
                      last_name: Fischer
                      email: violet.86@example.com
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
              With Custom Pricing:
                value:
                  subscription:
                    product_id: 684
                    custom_price:
                      slug: basalt-garnet
                      price_minor: 9900
                      interval: 1
                      interval_unit: month
                    addons:
                      - addon_id: 22
                        provisioned_quantity: 10
                        custom_price:
                          pricing_scheme: stairstep
                          prices:
                            - unit_price: 5
                              starting_quantity: 1
                              ending_quantity: 15
                            - unit_price: 2
                              starting_quantity: 16
                      - addon_id: 99
                        enabled: true
                        custom_price:
                          prices:
                            - unit_price: 1
                              starting_quantity: 1
              With Agreement Acceptance:
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Silva
                      email: violet.86@example.com
                    credit_card_attributes:
                      platform_token: tok_qkkfw0b64a9t0qy2cgmkrjhr
                    agreement_acceptance:
                      ip_address: 1.2.3.4
                      terms_url: https://terms.url
                      privacy_policy_url: https://privacy_policy.url
                      return_refund_policy_url: https://return_refund_policy.url
                      delivery_policy_url: https://delivery_policy_.url
                      secure_checkout_policy_url: https://secure_checkout_policy.url
              With ACH Agreement:
                value:
                  subscription:
                    product_slug: garnet
                    customer_attributes:
                      first_name: Leila
                      last_name: Silva
                      email: violet.86@example.com
                    bank_account_attributes:
                      bank_name: Maple Garnet
                      bank_routing_number: MB8DDUSYF
                      bank_account_number: GGT9YNX4WYE8
                      bank_account_type: checking
                      bank_account_holder_type: business
                      payment_type: bank_account
                    ach_agreement:
                      agreement_terms: Ember harbor amber
                      authorizer_first_name: Sana
                      authorizer_last_name: Fischer
                      ip_address: 1.2.3.4
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Basic: |-
              try
              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CollectionMode = CollectionMethod.Manual,
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                      Organization = "Glacier",
                                      Reference = "Amber",
                                      Address = "123 Garnet Juniper.",
                                      Address2 = "some example string",
                                      City = "Penhallow",
                                      State = "MA",
                                      Zip = "82205",
                                      Country = "US",
                                      Phone = "(540) 017 - 9198",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }
              catch (ApiException<CreateSubscriptionError> ex)
              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Calendar Billing: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "basalt",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                      Organization = "Glacier",
                                      Reference = "Amber",
                                      Address = "123 Garnet Juniper.",
                                      Address2 = "some example string",
                                      City = "Penhallow",
                                      State = "MA",
                                      Zip = "82205",
                                      Country = "US",
                                      Phone = "(540) 017 - 9198",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                                  CalendarBilling = new CalendarBilling
                                  {
                                      AnchorDay = 1,
                                      CalendarBillingFirstCharge = FirstChargeType.Immediate,
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Addons (Metered): >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                                  Addons = [
                                      new CreateSubscriptionAddon
                                      {
                                          AddonId = 179136,
                                          UnitBalance = 20,
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Addons (Toggle): >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                                  Addons = [
                                      new CreateSubscriptionAddon
                                      {
                                          AddonId = 1642,
                                          Enabled = true,
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Addons (Seat-based): >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Kofi",
                                      LastName = "Silva",
                                      Email = "falcon.91@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                                  Addons = [
                                      new CreateSubscriptionAddon
                                      {
                                          AddonId = 5,
                                          ProvisionedQuantity = 18,
                                          PriceOptionId = 4,
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Addons By Slug (Seat-based): >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                                  Addons = [
                                      new CreateSubscriptionAddon
                                      {
                                          AddonId = "kestrel:my-kestrel",
                                          ProvisionedQuantity = 14,
                                          PriceOptionId = "kestrel:garnet-maple",
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            With Coupon: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CouponCode = "6EDXPL",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Grouped: |-
              try
              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "cedar-amber",
                                  CustomerId = 33573617,
                                  PaymentProfileId = 62434395,
                                  Bundle = new BundleSettings
                                  {
                                      Target = new BundleTarget
                                      {
                                          Type = type,
                                      },
                                      Billing = new BundleBilling(),
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }
              catch (ApiException<CreateSubscriptionError> ex)
              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            In Hierarchy (not Group): |-
              try
              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "test_fccqs81",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Mei",
                                      LastName = "Novak",
                                      Email = "violet.86@example.com",
                                      ParentId = 99369533,
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }
              catch (ApiException<CreateSubscriptionError> ex)
              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Import as Canceled: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "cedar",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "harbor.98@example.com",
                                  },
                                  BankAccountAttributes = new BankAccountAttributes
                                  {
                                      BankName = "My Garnet",
                                      CurrentVault = BankAccountVault.StripeConnect,
                                      VaultToken = "5DR5",
                                  },
                                  CancellationMessage = "Cedar lagoon",
                                  ReasonCode = "lagoon",
                                  CanceledAt = DateTimeOffset.Parse(
                                      "2021-02-25T16:00:00Z"),
                                  ActivatedAt = DateTimeOffset.Parse(
                                      "2021-01-17T16:00:00Z"),
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Import With Future Billing Date: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  NextBillingAt = DateTimeOffset.Parse(
                                      "2020-06-01T16:00:00Z"),
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Import Authorize.Net ACH: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "cedar",
                                  NextBillingAt = DateTimeOffset.Parse(
                                      "2020-08-29T16:00:00Z"),
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "juniper.11@example.com",
                                  },
                                  BankAccountAttributes = new BankAccountAttributes
                                  {
                                      BankName = "My Garnet",
                                      CurrentVault = BankAccountVault.Authorizenet,
                                      VaultToken = "5DR5",
                                      CustomerVaultToken = "LFBW",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Import Stripe ACH: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "cedar",
                                  NextBillingAt = DateTimeOffset.Parse(
                                      "2020-08-29T16:00:00Z"),
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "juniper.11@example.com",
                                  },
                                  BankAccountAttributes = new BankAccountAttributes
                                  {
                                      BankName = "My Garnet",
                                      CurrentVault = BankAccountVault.StripeConnect,
                                      VaultToken = "cus_wwdavn",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Metadata (Subscription-Level): >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                                  Datafields = new Dictionary<string, string>
                                  {
                                      ["color"] = "onyx",
                                      ["comments"] = "Dune!",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Metadata (Customer-Level): >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                      Datafields = new Dictionary<string, string>
                                      {
                                          ["nickname"] = "Prairie",
                                          ["language"] = "Raven",
                                      },
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            With Offer: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Kofi",
                                      LastName = "Silva",
                                      Email = "falcon.91@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      MaskedCardNumber = "43QY-9CZH-VA99-LNRM",
                                      ExpirationMonth = "10",
                                      ExpirationYear = "KWV3",
                                  },
                                  OfferId = "kestrel:violet",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            Prepaid Subscription: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CollectionMode = CollectionMethod.Prepaid,
                                  CustomerId = 79288,
                                  PaymentProfileId = 8553,
                                  PrepaidConfiguration = new UpsertPrepaidConfiguration
                                  {
                                      InitialFundingAmountMinor = 10000L,
                                      ReplenishToAmountMinor = 6000L,
                                      AutoReplenish = true,
                                      ReplenishThresholdAmountMinor = 5000L,
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            With Affiliate code: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "cedar-kestrel",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Fischer",
                                      Email = "violet.86@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                                  Ref = "1q2w3e",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            With Custom Pricing: |-
              try
              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductId = 684,
                                  CustomPrice = new SubscriptionCustomPrice
                                  {
                                      Slug = "basalt-garnet",
                                      PriceMinor = 9900L,
                                      Interval = 1,
                                      IntervalUnit = IntervalUnit.Month,
                                  },
                                  Addons = [
                                      new CreateSubscriptionAddon
                                      {
                                          AddonId = 22,
                                          ProvisionedQuantity = 10,
                                          CustomPrice = new AddonCustomPrice
                                          {
                                              Prices = prices,
                                          },
                                      },
                                      new CreateSubscriptionAddon
                                      {
                                          AddonId = 99,
                                          Enabled = true,
                                          CustomPrice = new AddonCustomPrice
                                          {
                                              Prices = prices,
                                          },
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }
              catch (ApiException<CreateSubscriptionError> ex)
              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            With Agreement Acceptance: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Silva",
                                      Email = "violet.86@example.com",
                                  },
                                  CreditCardAttributes = new PaymentProfileAttributes
                                  {
                                      PlatformToken = "tok_qkkfw0b64a9t0qy2cgmkrjhr",
                                  },
                                  AgreementAcceptance = new AgreementAcceptance
                                  {
                                      IpAddress = "1.2.3.4",
                                      TermsUrl = "https://terms.url",
                                      PrivacyPolicyUrl = "https://privacy_policy.url",
                                      ReturnRefundPolicyUrl = "https://return_refund_policy.url",
                                      DeliveryPolicyUrl = "https://delivery_policy_.url",
                                      SecureCheckoutPolicyUrl = "https://secure_checkout_policy.url",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            With ACH Agreement: >-
              try

              {
                  var response = await client.Subscriptions.CreateSubscription(
                      new CreateSubscriptionOperationRequest
                      {
                          Body = new CreateSubscriptionRequest
                          {
                              Subscription = new CreateSubscription
                              {
                                  ProductSlug = "garnet",
                                  CustomerAttributes = new CustomerAttributes
                                  {
                                      FirstName = "Leila",
                                      LastName = "Silva",
                                      Email = "violet.86@example.com",
                                  },
                                  BankAccountAttributes = new BankAccountAttributes
                                  {
                                      BankName = "Maple Garnet",
                                      BankRoutingNumber = "MB8DDUSYF",
                                      BankAccountNumber = "GGT9YNX4WYE8",
                                      BankAccountType = BankAccountType.Checking,
                                      BankAccountHolderType = BankAccountHolderType.Business,
                                      PaymentType = PaymentType.BankAccount,
                                  },
                                  AchAgreement = new AchAgreement
                                  {
                                      AgreementTerms = "Ember harbor amber",
                                      AuthorizerFirstName = "Sana",
                                      AuthorizerLastName = "Fischer",
                                      IpAddress = "1.2.3.4",
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionResponse
              }

              catch (ApiException<CreateSubscriptionError> ex)

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
    Subscription-Response:
      title: Subscription Response
      type: object
      properties:
        subscription:
          $ref: "#/components/schemas/Subscription"
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Create-Subscription-Request:
      title: Create Subscription Request
      type: object
      properties:
        subscription:
          $ref: "#/components/schemas/Create-Subscription"
      required:
        - subscription
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
    Error-Array-Response:
      type: array
      items:
        type: string
    Subscription:
      title: Subscription
      type: object
      properties:
        id:
          type: integer
          description: The subscription unique id within Ombrevix.
        state:
          $ref: "#/components/schemas/Subscription-State"
        balance_minor:
          type: integer
          format: int64
          description: Gives the current outstanding subscription balance in the number of
            cents.
        total_revenue_minor:
          type: integer
          format: int64
          description: Gives the total revenue from the subscription in the number of cents.
        product_price_minor:
          type: integer
          format: int64
          description: "(Added Nov 5 2013) The recurring amount of the product (and
            version), currently subscribed. NOTE: this may differ from the
            current price of the product, if you’ve changed the price of the
            product but haven’t moved this subscription to a newer version."
        product_version_number:
          type: integer
          description: The version of the product for the subscription. Note that this is
            a deprecated field kept for backwards-compatibility.
        current_period_ends_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp relating to the end of the current (recurring) period
            (i.e., when the next regularly scheduled attempted charge will
            occur)
        next_evaluation_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp that indicates when capture of payment will be tried or
            retried. This value will usually track the current_period_ends_at,
            but will diverge if a renewal payment fails and must be retried. In
            that case, the current_period_ends_at will advance to the end of the
            next period (time doesn’t stop because a payment was missed) but the
            next_evaluation_at will be scheduled for the auto-retry time (e.g.,
            24 hours in the future, in some cases).
        trial_started_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp for when the trial period (if any) began
        trial_over_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp for when the trial period (if any) ended
        activated_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp for when the subscription began (i.e., when it came out
            of trial, or when it began in the case of no trial)
        expires_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp giving the expiration date of this subscription (if any)
        created_at:
          type: string
          format: date-time
          description: The creation date for this subscription
        updated_at:
          type: string
          format: date-time
          description: The date of last update for this subscription
        cancellation_message:
          type:
            - string
            - "null"
          description: Seller-provided reason for, or note about, the cancellation.
        cancellation_method:
          $ref: "#/components/schemas/Cancellation-Method"
        cancel_when_period_ends:
          type:
            - boolean
            - "null"
          description: Whether or not the subscription will (or has) canceled at the end
            of the period.
        canceled_at:
          type:
            - string
            - "null"
          format: date-time
          description: The timestamp of the most recent cancellation
        current_period_started_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp relating to the start of the current (recurring) period
        previous_state:
          allOf:
            - $ref: "#/components/schemas/Subscription-State"
          description: Only valid for webhook payloads The previous state for webhooks
            that have indicated a change in state. For normal API calls, this
            will always be the same as the state (current state).
        initial_payment_id:
          type: integer
          description: The ID of the transaction that generated the revenue
        initial_revenue:
          type: string
          description: The revenue, formatted as a string of decimal separated dollars and
            cents, from the subscription signup ($50.00 would be formatted as
            50.00)
        deferred_cancel_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp for when the subscription is currently set to cancel.
        coupon_code:
          type:
            - string
            - "null"
          description: (deprecated) The coupon code of the single coupon currently applied
            to the subscription. See coupon_codes instead as subscriptions can
            now have more than one coupon.
          deprecated: true
        anchor_day:
          type:
            - string
            - "null"
          description: A day of month that subscription will be processed on. Can be 1 up
            to 28 or 'end'.
        collection_mode:
          $ref: "#/components/schemas/Collection-Method"
        customer:
          $ref: "#/components/schemas/Customer"
        product:
          $ref: "#/components/schemas/Product"
        credit_card:
          $ref: "#/components/schemas/schemas-Credit-Card-Payment-Profile"
        bundle:
          oneOf:
            - $ref: "#/components/schemas/Nested-Subscription-Bundle"
            - type: "null"
        bank_account:
          $ref: "#/components/schemas/schemas-Bank-Account-Payment-Profile"
        payment_type:
          type:
            - string
            - "null"
          description: The payment profile type for the active profile on file.
        affiliate_code:
          type:
            - string
            - "null"
          description: The subscription's unique code that can be given to referrals.
        next_product_id:
          type:
            - integer
            - "null"
          description: If a delayed product change is scheduled, the ID of the product
            that the subscription will be changed to at the next renewal.
        next_product_slug:
          type:
            - string
            - "null"
          description: If a delayed product change is scheduled, the slug of the product
            that the subscription will be changed to at the next renewal.
        coupon_use_count:
          type:
            - integer
            - "null"
          description: (deprecated) How many times the subscription's single coupon has
            been used. This field has no replacement for multiple coupons.
          deprecated: true
        coupon_uses_allowed:
          type:
            - integer
            - "null"
          description: (deprecated) How many times the subscription's single coupon may be
            used. This field has no replacement for multiple coupons.
          deprecated: true
        reason_code:
          type:
            - string
            - "null"
          description: The churn reason code associated to a canceled subscription.
        automatically_resume_at:
          type:
            - string
            - "null"
          description: The date the subscription is scheduled to automatically resume from
            the frozen state.
          format: date-time
        coupon_codes:
          type: array
          description: An array for all the coupons attached to the subscription.
          items:
            type: string
        offer_id:
          type:
            - integer
            - "null"
          description: The ID of the offer associated with the subscription.
        payer_id:
          type:
            - integer
            - "null"
          description: On Account Invoicing, the ID of the individual paying for the
            subscription. Defaults to the Customer ID unless the 'Customer
            Hierarchies & WhoPays' feature is enabled.
        current_billing_amount_minor:
          type: integer
          format: int64
          description: The balance minor plus the estimated renewal amount minor. Returned
            ONLY for the readSubscription operation as it's a compute intensive
            operation.
        product_price_option_id:
          type: integer
          description: The product price option currently subscribed to.
        product_price_option_type:
          $ref: "#/components/schemas/Price-Option-Type"
        next_product_price_option_id:
          type:
            - integer
            - "null"
          description: If a delayed product change is scheduled, the ID of the product
            price option that the subscription will be changed to at the next
            renewal.
        net_terms:
          type:
            - integer
            - "null"
          description: On Account Invoicing, the number of days before a renewal invoice
            is due.
        stored_credential_transaction_id:
          type:
            - integer
            - "null"
          description: For European workspaces subject to PSD2 and using 3D Secure, this
            can be used to reference a previous transaction for the customer.
            This will ensure the card will be charged successfully at renewal.
        reference:
          type:
            - string
            - "null"
          description: The reference value (provided by your app) for the subscription
            itself.
        frozen_at:
          type:
            - string
            - "null"
          description: The timestamp of the most recent frozen action.
          format: date-time
        prepaid_recovery:
          type: boolean
          description: Boolean representing whether the subscription is prepaid and
            currently in recovery. Only returned for Account Invoicing
            workspaces with the feature enabled.
        coupons:
          type: array
          description: >-
            Additional coupon data. To use this data you also have to include
            the following param in the request: `include[]=coupons`.

            Only in Read Subscription Endpoint.
          items:
            $ref: "#/components/schemas/Subscription-Included-Coupon"
        recovery_communication_delay_enabled:
          type: boolean
          description: Enable Communication Delay feature, making sure no communication
            (email or SMS) is sent to the Customer between 9PM and 8AM in time
            zone set by the `recovery_communication_delay_time_zone` attribute.
        recovery_communication_delay_time_zone:
          type:
            - string
            - "null"
          example: '"Zephyr Maple (US & Kestrel)"'
          description: Time zone for the Recovery Communication Delay feature.
        receives_invoice_emails:
          type:
            - boolean
            - "null"
        locale:
          type:
            - string
            - "null"
        currency:
          type: string
        scheduled_cancellation_at:
          type:
            - string
            - "null"
          format: date-time
        credit_balance_minor:
          type: integer
          format: int64
        prepayment_balance_minor:
          type: integer
          format: int64
        prepaid_configuration:
          oneOf:
            - $ref: "#/components/schemas/Prepaid-Configuration"
            - type: "null"
        self_service_page_token:
          type: string
          description: Returned only for list/read Subscription operation when
            `include[]=self_service_page_token` parameter is provided.
    Subscription-State:
      title: Subscription State
      type: string
      description: >
        The state of a subscription.

        * **Live States**
            * `active` - A normal, active subscription. It is not in a trial and is paid and up to date.
            * `assessing` - An internal (transient) state that indicates a subscription is in the middle of periodic assessment. Do not base any access decisions in your app on this state, as it may not always be exposed.
            * `pending` - An internal (transient) state that indicates a subscription is in the creation process. Do not base any access decisions in your app on this state, as it may not always be exposed.
            * `trialing` - A subscription in trialing state has a valid trial subscription. This type of subscription may transition to active once payment is received when the trial has ended. Otherwise, it may go to a Problem or End of Life state.
            * `paused` - An internal state that indicates that your account with Ombrevix Billing is in arrears.
        * **Problem States**
            * `past_due` - Indicates that the most recent payment has failed, and payment is past due for this subscription. If you have enabled our automated recovery, this subscription will be in the recovery process (additional status and callbacks from the recovery process will be available in the future). If you are handling recovery and payment updates yourself, you will want to use this state to initiate a payment update from your customers.
            * `retrying` - Indicates that normal assessment/processing of the subscription has failed for a reason that cannot be fixed by the Customer. For example, a Soft Fail may result from a timeout at the gateway or incorrect credentials on your part. The subscriptions should be retried automatically. An interface is being built for you to review problems resulting from these events to take manual action when needed.
            * `unpaid` - Indicates an unpaid subscription. A subscription is marked unpaid if the retry period expires and you have configured your Recovery settings to have a Final Action of `mark the subscription unpaid`.
        * **End of Life States**
            * `canceled` - Indicates a canceled subscription. This may happen at your request (via the API or the web interface) or due to the expiration of the Recovery process without payment. See the Reactivation documentation for info on how to restart a canceled subscription.
            While a subscription is canceled, its period will not advance, it will not accrue any new charges, and Ombrevix Billing will not attempt to collect the overdue balance.
            * `expired` - Indicates a subscription that has expired due to running its normal life cycle. Some products may be configured to have an expiration period. An expired subscription then is one that stayed active until it fulfilled its full period.
            * `creation_failed` - Indicates that signup has failed. (You may see this state in a signup_failure webhook.)
            * `frozen` - Indicates that a subscription’s billing has been temporarily stopped. While it is expected that the subscription will resume and return to active status, this is still treated as an “End of Life” state because the customer is not paying for services during this time.
            * `suspended` - Indicates that a prepaid subscription has used up all their prepayment balance. If a prepayment is applied, it will return to an active state.
            * `trial_over` - A subscription in a trial_over state is a subscription that completed a no-obligation trial and did not have a card on file at the expiration of the trial period. See Product Pricing – No Obligation Trials for more details.

        See Subscription States for more info about subscription states and
        state transitions.
      enum:
        - pending
        - creation_failed
        - trialing
        - evaluating
        - active
        - retrying
        - past_due
        - suspended
        - canceled
        - expired
        - paused
        - unpaid
        - trial_over
        - frozen
        - awaiting_activation
    Cancellation-Method:
      title: Cancellation Method
      type:
        - string
        - "null"
      description: The process used to cancel the subscription, if the subscription
        has been canceled. It is nil if the subscription's state is not
        canceled.
      enum:
        - merchant_ui
        - merchant_api
        - recovery
        - billing_portal
        - unknown
        - imported
        - null
    Customer:
      title: Customer
      type: object
      properties:
        first_name:
          type: string
          description: The first name of the customer
        last_name:
          type: string
          description: The last name of the customer
        email:
          type: string
          description: The email address of the customer
        cc_emails:
          type:
            - string
            - "null"
          description: “A comma-separated list of emails that should be cc’d on all
            customer communications (e.g., “joe@example.com, sue@example.com”)”
        organization:
          type:
            - string
            - "null"
          description: The organization of the customer. If no value, `null` or empty
            string is provided, `organization` will be populated with the
            customer's first and last name, separated with a space.
        reference:
          type:
            - string
            - "null"
          description: The unique identifier used within your own application for this
            customer
        id:
          type: integer
          description: The customer ID in Ombrevix
        created_at:
          type: string
          description: The timestamp in which the customer object was created in Ombrevix
          format: date-time
        updated_at:
          type: string
          description: The timestamp in which the customer object was last edited
          format: date-time
        address:
          type:
            - string
            - "null"
          description: The customer’s shipping street address (e.g., “123 Main St.”)
        address_2:
          type:
            - string
            - "null"
          description: Second line of the customer’s shipping address e.g., “Apt. 100”
        city:
          type:
            - string
            - "null"
          description: The customer’s shipping address city (e.g., “Boston”)
        state:
          type:
            - string
            - "null"
          description: The customer’s shipping address state (e.g., “MA”)
        state_name:
          type:
            - string
            - "null"
          description: The customer's full name of state
        zip:
          type:
            - string
            - "null"
          description: The customer’s shipping address zip code (e.g., “12345”)
        country:
          type:
            - string
            - "null"
          description: The customer shipping address country
        country_name:
          type:
            - string
            - "null"
          description: The customer's full name of country
        phone:
          type:
            - string
            - "null"
          description: The phone number of the customer
        verified:
          type:
            - boolean
            - "null"
          description: Is the customer verified to use ACH as a payment method.
        portal_customer_created_at:
          type:
            - string
            - "null"
          description: The timestamp of when the Billing Portal entry was created at for
            the customer
          format: date-time
        portal_invite_last_sent_at:
          type:
            - string
            - "null"
          description: The timestamp of when the Billing Portal invite was last sent at
          format: date-time
        portal_invite_last_accepted_at:
          description: The timestamp of when the Billing Portal invite was last accepted
          type:
            - string
            - "null"
          format: date-time
        tax_exempt:
          type: boolean
          description: The tax exempt status for the customer. Acceptable values are true
            or 1 for true and false or 0 for false.
        surcharging:
          type: boolean
          description: Whether surcharging is enabled for the customer. Only included on
            workspaces where surcharging control is enabled.
        vat_number:
          type:
            - string
            - "null"
          description: The VAT business identification number for the customer. This
            number is used to determine VAT tax opt out rules. It is not
            validated when added or updated on a customer record. Instead, it is
            validated via VIES before calculating taxes. Only valid business
            identification numbers will allow for VAT opt out.
        parent_id:
          description: The parent ID in Ombrevix if applicable. Parent is another Customer
            object.
          type:
            - integer
            - "null"
        locale:
          type:
            - string
            - "null"
          description: The locale for the customer to identify language-region
        default_subscription_bundle_uid:
          type:
            - string
            - "null"
        salesforce_id:
          type:
            - string
            - "null"
          description: The Salesforce ID for the customer
        tax_exempt_reason:
          type:
            - string
            - "null"
          description: The Tax Exemption Reason Code for the customer
        default_auto_renewal_profile_id:
          type:
            - integer
            - "null"
          description: The default auto-renewal profile ID for the customer
        platformref:
          type:
            - string
            - "null"
          description: The Ombrevix-generated unique identifier for the customer.
        branding_theme_id:
          type:
            - integer
            - "null"
          description: The ID of the Branding Theme assigned to this customer as the
            customer's default Branding Theme. This customer-level Branding
            Theme is used when a subscription does not have its own
            subscription-level Branding Theme.  Available only when Branding
            Themes are enabled for the workspace.
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
    schemas-Credit-Card-Payment-Profile:
      title: Credit Card Payment Profile
      type: object
      properties:
        id:
          type: integer
          description: The Ombrevix-assigned ID of the stored card. This value can be used
            as an input to payment_profile_id when creating a subscription, in
            order to re-use a stored payment profile for the same customer.
        first_name:
          type: string
          description: The first name of the card holder.
        last_name:
          type: string
          description: The last name of the card holder.
        masked_card_number:
          type: string
          description: A string representation of the credit card number with all but the
            last 4 digits masked with X’s (e.g., ‘XXXX-XXXX-XXXX-1234’).
        card_type:
          $ref: "#/components/schemas/Card-Type"
        expiration_month:
          type: integer
          description: An integer representing the expiration month of the card(1 – 12).
        expiration_year:
          type: integer
          description: An integer representing the 4-digit expiration year of the
            card(e.g., ‘2012’).
        customer_id:
          type: integer
          description: The Ombrevix-assigned id for the customer record to which the card
            belongs.
        current_vault:
          $ref: "#/components/schemas/Credit-Card-Vault"
        vault_token:
          type:
            - string
            - "null"
          description: The “token” provided by your vault storage for an already stored
            payment profile.
        billing_address:
          type:
            - string
            - "null"
          description: The current billing street address for the card.
        billing_city:
          type:
            - string
            - "null"
          description: The current billing address city for the card.
        billing_state:
          type:
            - string
            - "null"
          description: The current billing address state for the card.
        billing_zip:
          type:
            - string
            - "null"
          description: The current billing address zip code for the card.
        billing_country:
          type:
            - string
            - "null"
          description: The current billing address country for the card.
        customer_vault_token:
          type:
            - string
            - "null"
          description: "(only for Authorize.Net CIM storage): the customerProfileId for
            the owner of the customerPaymentProfileId provided as the
            vault_token."
        billing_address_2:
          type:
            - string
            - "null"
          description: The current billing street address, second line, for the card.
        payment_type:
          allOf:
            - $ref: "#/components/schemas/Payment-Type"
          example: credit_card
          default: credit_card
        disabled:
          type: boolean
        platform_token:
          type: string
          description: Token received after sending billing information using Ombrevix.js.
            This token will only be received if passed as a sole attribute of
            credit_card_attributes (e.g., tok_9g6hw85pnpt6knmskpwp4ttt).
        workspace_gateway_setting_id:
          type:
            - integer
            - "null"
        gateway_slug:
          type:
            - string
            - "null"
          description: An identifier of connected gateway.
        created_at:
          type: string
          format: date-time
          description: A timestamp indicating when this payment profile was created
        updated_at:
          type: string
          format: date-time
          description: A timestamp indicating when this payment profile was last updated
      required:
        - payment_type
      examples:
        - id: 10088716
          first_name: Test
          last_name: Subscription
          masked_card_number: XXXX-XXXX-XXXX-1
          card_type: simulated
          expiration_month: 1
          expiration_year: 2022
          customer_id: 14543792
          current_vault: simulated
          vault_token: "1"
          billing_address: 123 Montana Way
          billing_city: Billings
          billing_state: MT
          billing_zip: "59101"
          billing_country: US
          customer_vault_token: null
          billing_address_2: ""
          payment_type: credit_card
          workspace_gateway_setting_id: 1
          gateway_slug: null
    Nested-Subscription-Bundle:
      title: Nested Subscription Bundle
      type: object
      properties:
        uid:
          type: string
          description: The UID for the group
        scheme:
          type: integer
          description: Whether the group is configured to rely on a primary subscription
            for billing. At this time, it will always be 1.
        primary_subscription_id:
          type: integer
          description: The subscription ID of the primary within the group. Applicable to
            scheme 1.
        primary:
          type: boolean
          description: A boolean indicating whether the subscription is the primary in the
            group. Applicable to scheme 1.
    schemas-Bank-Account-Payment-Profile:
      title: Bank Account Payment Profile
      type: object
      required:
        - payment_type
      properties:
        id:
          type: integer
          description: The Ombrevix-assigned ID of the stored bank account. This value can
            be used as an input to payment_profile_id when creating a
            subscription, in order to re-use a stored payment profile for the
            same customer.
        first_name:
          type: string
          description: The first name of the bank account holder
        last_name:
          type: string
          description: The last name of the bank account holder
        customer_id:
          type: integer
          description: The Ombrevix-assigned ID for the customer record to which the bank
            account belongs
        current_vault:
          $ref: "#/components/schemas/Bank-Account-Vault"
        vault_token:
          type: string
          description: The "token" provided by your vault storage for an already stored
            payment profile
        billing_address:
          type:
            - string
            - "null"
          description: The current billing street address for the bank account
        billing_city:
          type:
            - string
            - "null"
          description: The current billing address city for the bank account
        billing_state:
          type:
            - string
            - "null"
          description: The current billing address state for the bank account
        billing_zip:
          type:
            - string
            - "null"
          description: The current billing address zip code for the bank account
        billing_country:
          type:
            - string
            - "null"
          description: The current billing address country for the bank account
        customer_vault_token:
          type:
            - string
            - "null"
          description: "(only for Authorize.Net CIM storage): the customerProfileId for
            the owner of the customerPaymentProfileId provided as the
            vault_token."
        billing_address_2:
          type:
            - string
            - "null"
          description: The current billing street address, second line, for the bank account
        bank_name:
          type: string
          description: The bank where the account resides
        masked_bank_routing_number:
          type:
            - string
            - "null"
          description: A string representation of the stored bank routing number with all
            but the last 4 digits marked with X's (i.e. 'XXXXXXX1111').
            payment_type will be bank_account.
        bank_account_type:
          $ref: "#/components/schemas/Bank-Account-Type"
        bank_account_holder_type:
          $ref: "#/components/schemas/Bank-Account-Holder-Type"
        payment_type:
          allOf:
            - $ref: "#/components/schemas/Payment-Type"
          default: bank_account
          example: bank_account
        verified:
          type: boolean
          default: false
          description: Denotes whether a bank account has been verified by providing the
            amounts of two small deposits made into the account.
        workspace_gateway_setting_id:
          type:
            - integer
            - "null"
        gateway_slug:
          type:
            - string
            - "null"
        created_at:
          type: string
          format: date-time
          description: A timestamp indicating when this payment profile was created
        updated_at:
          type: string
          format: date-time
          description: A timestamp indicating when this payment profile was last updated
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
    Subscription-Included-Coupon:
      title: Subscription Included Coupon
      type: object
      properties:
        code:
          type: string
          example: '"Prairie_10"'
        use_count:
          type: integer
          example: 2
        uses_allowed:
          type: integer
          example: 10
        expires_at:
          type:
            - string
            - "null"
          example: '"2023-07-13T05:18:58-04:00"'
        recurring:
          type: boolean
        amount_minor:
          type:
            - integer
            - "null"
          format: int64
          minimum: 0
          example: 1000
        percentage:
          type:
            - string
            - "null"
          example: '"15.0"'
    Prepaid-Configuration:
      title: Prepaid Configuration
      type: object
      properties:
        id:
          type: integer
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
    Credit-Card-Vault:
      title: Credit Card Vault
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