# Create Payment Profile (/api/openapi/payment-profiles/createPaymentProfile)

`POST /payment_profiles`

Creates a payment profile for a customer.

When you create a new payment profile for a customer via the API, it does not automatically make the profile current for any of the customer’s subscriptions. To use the payment profile as the default, you must set it explicitly for the subscription or subscription bundle.

Select an option from the **Request Examples** drop-down on the right side of the portal to see examples of common scenarios for creating payment profiles. 

Do not use real card information for testing. See the Workspaces articles that cover testing your workspace setup for more details on testing in your sandbox.

Note that collecting and sending raw card details in production requires PCI compliance on your end. If your business is not PCI compliant, use Ombrevix.js to collect credit card or bank account information.

See the following articles to learn more about subscriptions and payments:

+ Subscriber Payment Details
+ Self Service Pages (Allows credit card updates by Subscriber)
+ Hosted signup forms payment settings
+ Taxes
+ Ombrevix.js
    + Ombrevix.js with GoCardless - minimal example
    + Ombrevix.js with GoCardless - full example
    + Ombrevix.js with Stripe Direct Debit - minimal example
    + Ombrevix.js with Stripe Direct Debit - full example
    + Ombrevix.js with Stripe BECS Direct Debit - minimal example
    + Ombrevix.js with Stripe BECS Direct Debit - full example
+ Full documentation on GoCardless
+ Full documentation on Stripe SEPA Direct Debit
+ Full documentation on Stripe BECS Direct Debit
+ Full documentation on Stripe BACS Direct Debit

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
  /payment_profiles:
    post:
      summary: Create Payment Profile
      tags:
        - Payment Profiles
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Payment-Profile-Response"
              examples:
                Example:
                  value:
                    payment_profile:
                      first_name: Ayla
                      last_name: Silva
                      card_type: visa
                      masked_card_number: 43QY-9CZH-VA99-LNRM
                      expiration_month: 10
                      expiration_year: 2018
                      customer_id: 18496394
                      current_vault: simulated
                      vault_token: "1"
                      billing_address: 123 Yarrow St.
                      billing_city: Penhallow
                      billing_state: MA
                      billing_zip: "82205"
                      billing_country: US
                      customer_vault_token: null
                      billing_address_2: null
                      payment_type: credit_card
                      workspace_gateway_setting_id: 5
                      gateway_slug: kestrel
                      disabled: false
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Example-1:
                  value:
                    errors:
                      - "Falcon yarrow iris birch: cobalt be lagoon."
                      - "Falcon yarrow iris kestrel: cobalt be lagoon."
                      - "Falcon yarrow garnet: cobalt be lagoon."
                Example-2:
                  value:
                    errors:
                      - Violet Basalt cedar lagoon sierra
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createPaymentProfile
      description: >-
        Creates a payment profile for a customer.


        When you create a new payment profile for a customer via the API, it
        does not automatically make the profile current for any of the
        customer’s subscriptions. To use the payment profile as the default, you
        must set it explicitly for the subscription or subscription bundle.


        Select an option from the **Request Examples** drop-down on the right
        side of the portal to see examples of common scenarios for creating
        payment profiles. 


        Do not use real card information for testing. See the Workspaces
        articles that cover testing your workspace setup for more details on
        testing in your sandbox.


        Note that collecting and sending raw card details in production requires
        PCI compliance on your end. If your business is not PCI compliant, use
        Ombrevix.js to collect credit card or bank account information.


        See the following articles to learn more about subscriptions and
        payments:


        + Subscriber Payment Details

        + Self Service Pages (Allows credit card updates by Subscriber)

        + Hosted signup forms payment settings

        + Taxes

        + Ombrevix.js
            + Ombrevix.js with GoCardless - minimal example
            + Ombrevix.js with GoCardless - full example
            + Ombrevix.js with Stripe Direct Debit - minimal example
            + Ombrevix.js with Stripe Direct Debit - full example
            + Ombrevix.js with Stripe BECS Direct Debit - minimal example
            + Ombrevix.js with Stripe BECS Direct Debit - full example
        + Full documentation on GoCardless

        + Full documentation on Stripe SEPA Direct Debit

        + Full documentation on Stripe BECS Direct Debit

        + Full documentation on Stripe BACS Direct Debit


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
              $ref: "#/components/schemas/Create-Payment-Profile-Request"
            examples:
              Ombrevix.js:
                value:
                  payment_profile:
                    customer_id: 5793
                    platform_token: tok_uddxxpepfp92nsfs6561d1z4
              ACH:
                value:
                  payment_profile:
                    customer_id: 684
                    bank_name: Quartz Garnet
                    bank_routing_number: MB8DDUSYF
                    bank_account_number: GGT9YNX4WYE8
                    bank_account_type: checking
                    bank_account_holder_type: business
                    payment_type: bank_account
              Card:
                value:
                  payment_profile:
                    first_name: Ayla
                    last_name: Silva
                    last_four: TCCZ
                    card_type: visa
                    expiration_month: 10
                    expiration_year: 2018
                    customer_id: 18496394
                    current_vault: simulated
                    vault_token: "1"
                    billing_address: 123 Yarrow St.
                    billing_city: Penhallow
                    billing_state: MA
                    billing_zip: "82205"
                    billing_country: US
                    billing_address_2: null
                    payment_type: credit_card
              Local Bank Details:
                value:
                  payment_profile:
                    customer_id: 684
                    bank_name: Lagoon Garnet of Onyx
                    bank_account_number: NU3H25G
                    bank_routing_number: 95VG
                    bank_branch_code: JUUKH
                    payment_type: bank_account
                    billing_address: 20 Onyx de la Amber
                    billing_city: Calder Falls
                    billing_state: Île-de-France
                    billing_zip: "53919"
                    billing_country: FR
              IBAN:
                value:
                  payment_profile:
                    customer_id: 86772119
                    bank_name: Cedar Garnet
                    bank_iban: FLU452NFN4SVD4H2LGKADQCAVF6
                    payment_type: bank_account
                    billing_address: 20 Onyx de la Amber
                    billing_city: Calder Falls
                    billing_state: Île-de-France
                    billing_zip: "53919"
                    billing_country: FR
              Import Payment Profile:
                value:
                  payment_profile:
                    customer_id: 86772119
                    customer_vault_token: "[Tundra Dune Harbor ID]"
                    vault_token: "[Tundra Dune Raven ID]"
                    current_vault: gocardless
                    bank_name: Cedar Garnet
                    payment_type: bank_account
                    billing_address: 20 Onyx de la Amber
                    billing_city: Calder Falls
                    billing_state: Île-de-France
                    billing_zip: "53919"
                    billing_country: FR
        description: When following the IBAN or the Local Bank details examples, a
          customer, bank account and mandate will be created in your current
          vault. If the customer, bank account, and mandate already exist in
          your vault, follow the Import example to link the payment profile into
          Ombrevix Billing.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Ombrevix.js: >-
              try

              {
                  var response = await client.PaymentProfiles.CreatePaymentProfile(
                      new CreatePaymentProfileOperationRequest
                      {
                          Body = new CreatePaymentProfileRequest
                          {
                              PaymentProfile = new CreatePaymentProfile
                              {
                                  PlatformToken = "tok_uddxxpepfp92nsfs6561d1z4",
                                  CustomerId = 5793,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type PaymentProfileResponse
              }

              catch (ApiException<CreatePaymentProfileError> ex)

              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
                  }
              }
            ACH: >-
              try

              {
                  var response = await client.PaymentProfiles.CreatePaymentProfile(
                      new CreatePaymentProfileOperationRequest
                      {
                          Body = new CreatePaymentProfileRequest
                          {
                              PaymentProfile = new CreatePaymentProfile
                              {
                                  PaymentType = PaymentType.BankAccount,
                                  CustomerId = 684,
                                  BankName = "Quartz Garnet",
                                  BankRoutingNumber = "MB8DDUSYF",
                                  BankAccountNumber = "GGT9YNX4WYE8",
                                  BankAccountType = BankAccountType.Checking,
                                  BankAccountHolderType = BankAccountHolderType.Business,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type PaymentProfileResponse
              }

              catch (ApiException<CreatePaymentProfileError> ex)

              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
                  }
              }
            Card: >-
              try

              {
                  var response = await client.PaymentProfiles.CreatePaymentProfile(
                      new CreatePaymentProfileOperationRequest
                      {
                          Body = new CreatePaymentProfileRequest
                          {
                              PaymentProfile = new CreatePaymentProfile
                              {
                                  PaymentType = PaymentType.CreditCard,
                                  FirstName = "Ayla",
                                  LastName = "Silva",
                                  CardType = CardType.Visa,
                                  ExpirationMonth = 10,
                                  ExpirationYear = 2018,
                                  BillingAddress = "123 Yarrow St.",
                                  BillingAddress2 = "some example string",
                                  BillingCity = "Penhallow",
                                  BillingState = "MA",
                                  BillingCountry = "US",
                                  BillingZip = "82205",
                                  CurrentVault = AllVaults.Simulated,
                                  VaultToken = "1",
                                  CustomerId = 18496394,
                                  LastFour = "TCCZ",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type PaymentProfileResponse
              }

              catch (ApiException<CreatePaymentProfileError> ex)

              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
                  }
              }
            Local Bank Details: >-
              try

              {
                  var response = await client.PaymentProfiles.CreatePaymentProfile(
                      new CreatePaymentProfileOperationRequest
                      {
                          Body = new CreatePaymentProfileRequest
                          {
                              PaymentProfile = new CreatePaymentProfile
                              {
                                  PaymentType = PaymentType.BankAccount,
                                  BillingAddress = "20 Onyx de la Amber",
                                  BillingCity = "Calder Falls",
                                  BillingState = "Île-de-France",
                                  BillingCountry = "FR",
                                  BillingZip = "53919",
                                  CustomerId = 684,
                                  BankName = "Lagoon Garnet of Onyx",
                                  BankRoutingNumber = "95VG",
                                  BankAccountNumber = "NU3H25G",
                                  BankBranchCode = "JUUKH",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type PaymentProfileResponse
              }

              catch (ApiException<CreatePaymentProfileError> ex)

              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
                  }
              }
            IBAN: >-
              try

              {
                  var response = await client.PaymentProfiles.CreatePaymentProfile(
                      new CreatePaymentProfileOperationRequest
                      {
                          Body = new CreatePaymentProfileRequest
                          {
                              PaymentProfile = new CreatePaymentProfile
                              {
                                  PaymentType = PaymentType.BankAccount,
                                  BillingAddress = "20 Onyx de la Amber",
                                  BillingCity = "Calder Falls",
                                  BillingState = "Île-de-France",
                                  BillingCountry = "FR",
                                  BillingZip = "53919",
                                  CustomerId = 86772119,
                                  BankName = "Cedar Garnet",
                                  BankIban = "FLU452NFN4SVD4H2LGKADQCAVF6",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type PaymentProfileResponse
              }

              catch (ApiException<CreatePaymentProfileError> ex)

              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
                  }
              }
            Import Payment Profile: >-
              try

              {
                  var response = await client.PaymentProfiles.CreatePaymentProfile(
                      new CreatePaymentProfileOperationRequest
                      {
                          Body = new CreatePaymentProfileRequest
                          {
                              PaymentProfile = new CreatePaymentProfile
                              {
                                  PaymentType = PaymentType.BankAccount,
                                  BillingAddress = "20 Onyx de la Amber",
                                  BillingCity = "Calder Falls",
                                  BillingState = "Île-de-France",
                                  BillingCountry = "FR",
                                  BillingZip = "53919",
                                  CurrentVault = AllVaults.Gocardless,
                                  VaultToken = "[Tundra Dune Raven ID]",
                                  CustomerVaultToken = "[Tundra Dune Harbor ID]",
                                  CustomerId = 86772119,
                                  BankName = "Cedar Garnet",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type PaymentProfileResponse
              }

              catch (ApiException<CreatePaymentProfileError> ex)

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
    Payment-Profile-Response:
      title: Payment Profile Response
      type: object
      properties:
        payment_profile:
          $ref: "#/components/schemas/Payment-Profile"
      required:
        - payment_profile
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Create-Payment-Profile-Request:
      title: Create Payment Profile Request
      type: object
      properties:
        payment_profile:
          $ref: "#/components/schemas/Create-Payment-Profile"
      required:
        - payment_profile
    Create-Payment-Profile:
      title: Create Payment Profile
      type: object
      properties:
        platform_token:
          type: string
          description: Token received after sending billing information using Ombrevix.js.
          example: tok_01x5c669mue6hquev1svw9c7
        id:
          type: integer
        payment_type:
          $ref: "#/components/schemas/Payment-Type"
        first_name:
          type: string
          description: First name on card or bank account. If omitted, the first_name from
            customer attributes will be used.
        last_name:
          type: string
          description: Last name on card or bank account. If omitted, the last_name from
            customer attributes will be used.
        masked_card_number:
          type: string
        card_digits:
          type: string
          description: The full credit card number
          example: 8ZF2PQ5ZBVM6ZXVQ
        card_type:
          $ref: "#/components/schemas/Card-Type"
        expiration_month:
          type:
            - integer
            - string
          description: (Optional when performing an Import via vault_token, required
            otherwise) The 1- or 2-digit credit card expiration month, as an
            integer or string, e.g., 5
        expiration_year:
          type:
            - integer
            - string
          description: (Optional when performing an Import via vault_token, required
            otherwise) The 4-digit credit card expiration year, as an integer or
            string, e.g., 2012
        billing_address:
          type: string
          description: The credit card or bank account billing street address (e.g., 123
            Main St.). This value is merely passed through to the payment
            gateway.
        billing_address_2:
          type:
            - string
            - "null"
          description: Second line of the customer’s billing address e.g., Apt. 100
        billing_city:
          type: string
          description: The credit card or bank account billing address city (e.g.,
            “Boston”). This value is merely passed through to the payment
            gateway.
        billing_state:
          type: string
          description: The credit card or bank account billing address state (e.g., MA).
            This value is merely passed through to the payment gateway. This
            must conform to the
            [ISO_3166-1](https://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)
            in order to be valid for tax locale purposes.
        billing_country:
          type: string
          description: “The credit card or bank account billing address country, required
            in [ISO_3166-1
            alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) format
            (e.g., “US”). This value is merely passed through to the payment
            gateway. Some gateways require country codes in a specific format.
            Check your gateway’s documentation. If creating an ACH subscription,
            only US is supported at this time.”
        billing_zip:
          type: string
          description: The credit card or bank account billing address zip code (e.g.,
            12345). This value is merely passed through to the payment gateway.
        current_vault:
          $ref: "#/components/schemas/All-Vaults"
        vault_token:
          type: string
          description: The “token” provided by your vault storage for an already stored
            payment profile
        customer_vault_token:
          type: string
          description: (only for Authorize.Net CIM storage or Square) The
            customerProfileId for the owner of the customerPaymentProfileId
            provided as the vault_token
        customer_id:
          type: integer
          description: (Required when creating a new payment profile) The Ombrevix
            customer id.
        paypal_email:
          type: string
          description: used by merchants that implemented BraintreeBlue javaScript
            libraries on their own. We recommend using Ombrevix.js instead.
          deprecated: true
        payment_method_nonce:
          type: string
          description: used by merchants that implemented BraintreeBlue javaScript
            libraries on their own. We recommend using Ombrevix.js instead.
          deprecated: true
        gateway_slug:
          type: string
          description: This attribute is only available if MultiGateway feature is enabled
            for your Workspace. This feature is in the Private Beta currently.
            gateway_slug is used to directly select a gateway where a payment
            profile will be stored in. Every connected gateway must have a
            unique gateway slug specified. Read Multigateway description to
            learn more about new concepts that MultiGateway introduces and the
            default behavior when this attribute is not passed.
        cvv:
          type: string
          description: The 3- or 4-digit Card Verification Value. This value is merely
            passed through to the payment gateway.
        bank_name:
          type: string
          description: (Required when creating with ACH or GoCardless, optional with
            Stripe Direct Debit). The name of the bank where the customerʼs
            account resides
        bank_iban:
          type: string
          description: (Optional when creating with GoCardless, required with Stripe
            Direct Debit). International Bank Account Number. Alternatively,
            local bank details can be provided.
        bank_routing_number:
          type: string
          description: (Required when creating with ACH. Optional when creating a
            subscription with GoCardless). The routing number of the bank. It
            becomes bank_code while passing via GoCardless API.
        bank_account_number:
          type: string
          description: (Required when creating with ACH, GoCardless, Stripe BECS or BACS
            Direct Debit, and bank_iban is blank) The customerʼs bank account
            number
        bank_branch_code:
          type: string
          description: (Optional when creating with GoCardless, required with Stripe BECS
            or BACS Direct Debit) Branch/Sort code. Alternatively, an IBAN can
            be provided.
        bank_account_type:
          $ref: "#/components/schemas/Bank-Account-Type"
        bank_account_holder_type:
          $ref: "#/components/schemas/Bank-Account-Holder-Type"
        last_four:
          type: string
          description: (Optional) Used for creating subscription with payment profile
            imported using vault_token, for proper display in Ombrevix Billing
            UI
    Payment-Type:
      title: Payment Type
      type: string
      enum:
        - credit_card
        - bank_account
        - paypal_account
        - apple_pay
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
    Error-Array-Response:
      type: array
      items:
        type: string
    Payment-Profile:
      title: Payment Profile
      type: object
      anyOf:
        - $ref: "#/components/schemas/Apple-Pay-Payment-Profile"
        - $ref: "#/components/schemas/Bank-Account-Payment-Profile"
        - $ref: "#/components/schemas/Credit-Card-Payment-Profile"
        - $ref: "#/components/schemas/Paypal-Payment-Profile"
      discriminator:
        propertyName: payment_type
        mapping:
          apple_pay: "#/components/schemas/Apple-Pay-Payment-Profile"
          bank_account: "#/components/schemas/Bank-Account-Payment-Profile"
          credit_card: "#/components/schemas/Credit-Card-Payment-Profile"
          paypal_account: "#/components/schemas/Paypal-Payment-Profile"
    Apple-Pay-Payment-Profile:
      $ref: "#/components/schemas/schemas-Apple-Pay-Payment-Profile"
    Bank-Account-Payment-Profile:
      $ref: "#/components/schemas/schemas-Bank-Account-Payment-Profile"
    Credit-Card-Payment-Profile:
      $ref: "#/components/schemas/schemas-Credit-Card-Payment-Profile"
    Paypal-Payment-Profile:
      $ref: "#/components/schemas/schemas-Paypal-Payment-Profile"
    schemas-Paypal-Payment-Profile:
      title: Paypal Payment Profile
      type: object
      properties:
        id:
          type: integer
          description: The Ombrevix-assigned ID of the stored PayPal payment profile.
        first_name:
          type: string
          description: The first name of the PayPal account holder
        last_name:
          type: string
          description: The last name of the PayPal account holder
        customer_id:
          type: integer
          description: The Ombrevix-assigned id for the customer record to which the
            PayPal account belongs
        current_vault:
          $ref: "#/components/schemas/PayPal-Vault"
        vault_token:
          type: string
          description: The “token” provided by your vault storage for an already stored
            payment profile
        billing_address:
          type:
            - string
            - "null"
          description: The current billing street address for the PayPal account
        billing_city:
          type:
            - string
            - "null"
          description: The current billing address city for the PayPal account
        billing_state:
          type:
            - string
            - "null"
          description: The current billing address state for the PayPal account
        billing_zip:
          type:
            - string
            - "null"
          description: The current billing address zip code for the PayPal account
        billing_country:
          type:
            - string
            - "null"
          description: The current billing address country for the PayPal account
        customer_vault_token:
          type:
            - string
            - "null"
        billing_address_2:
          type:
            - string
            - "null"
          description: The current billing street address, second line, for the PayPal
            account
        payment_type:
          allOf:
            - $ref: "#/components/schemas/Payment-Type"
          default: paypal_account
          example: paypal_account
        workspace_gateway_setting_id:
          type:
            - integer
            - "null"
        gateway_slug:
          type:
            - string
            - "null"
        paypal_email:
          type: string
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
    PayPal-Vault:
      title: PayPal Vault
      type: string
      description: The vault that stores the payment profile with the provided vault_token.
      enum:
        - braintree_blue
        - paypal
        - moduslink
        - paypal_complete
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
    schemas-Apple-Pay-Payment-Profile:
      title: ApplePay Payment Profile
      type: object
      properties:
        id:
          type: integer
          description: The Ombrevix-assigned ID of the Apple Pay payment profile.
        first_name:
          type: string
          description: The first name of the Apple Pay account holder
        last_name:
          type: string
          description: The last name of the Apple Pay account holder
        customer_id:
          type: integer
          description: The Ombrevix-assigned ID for the customer record to which the Apple
            Pay account belongs
        current_vault:
          $ref: "#/components/schemas/Apple-Pay-Vault"
        vault_token:
          type: string
          description: The “token” provided by your vault storage for an already stored
            payment profile
        billing_address:
          type:
            - string
            - "null"
          description: The current billing street address for the Apple Pay account
        billing_city:
          type:
            - string
            - "null"
          description: The current billing address city for the Apple Pay account
        billing_state:
          type:
            - string
            - "null"
          description: The current billing address state for the Apple Pay account
        billing_zip:
          type:
            - string
            - "null"
          description: The current billing address zip code for the Apple Pay account
        billing_country:
          type:
            - string
            - "null"
          description: The current billing address country for the Apple Pay account
        customer_vault_token:
          type:
            - string
            - "null"
        billing_address_2:
          type:
            - string
            - "null"
          description: The current billing street address, second line, for the Apple Pay
            account
        payment_type:
          allOf:
            - $ref: "#/components/schemas/Payment-Type"
          default: apple_pay
          example: apple_pay
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
      required:
        - payment_type
    Apple-Pay-Vault:
      title: Apple Pay Vault
      type: string
      description: The vault that stores the payment profile with the provided vault_token.
      enum:
        - braintree_blue
info:
  title: Ombrevix Billing
  version: "1.0"
```