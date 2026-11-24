# List Payment Profiles (/api/openapi/payment-profiles/listPaymentProfiles)

`GET /payment_profiles`

Lists all active payment profiles for a workspace, or for one customer within a workspace. If no payment profiles are found, this endpoint will return an empty array, not a 404.

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
    get:
      summary: List Payment Profiles
      tags:
        - Payment Profiles
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Payment-Profile-Response"
              examples:
                Example-1:
                  value:
                    - payment_profile:
                        id: 14898589
                        first_name: Leila
                        last_name: Fischer
                        created_at: 2025-01-01T00:00:00-05:00
                        updated_at: 2025-01-01T00:00:00-05:00
                        customer_id: 84539008
                        current_vault: simulated
                        vault_token: HFNU3ZZH9A
                        billing_address: 456 Harbor Garnet
                        billing_city: Riverend
                        billing_state: CO
                        billing_zip: "36263"
                        billing_country: US
                        customer_vault_token: null
                        billing_address_2: ""
                        bank_name: Garnet of Maple Raven
                        masked_bank_routing_number: 9Y296Q96
                        masked_bank_account_number: QXKH7EWT
                        bank_account_type: checking
                        bank_account_holder_type: personal
                        payment_type: bank_account
                        verified: true
                        workspace_gateway_setting_id: 5
                        gateway_slug: kestrel
                    - payment_profile:
                        id: 58337655
                        first_name: Pavel
                        last_name: Fischer
                        created_at: 2025-01-01T00:00:00-05:00
                        updated_at: 2025-01-01T00:00:00-05:00
                        customer_id: 55581253
                        current_vault: simulated
                        vault_token: G7VZ6ES5K
                        billing_address: 123 Violet Basalt
                        billing_city: Marrow Bay
                        billing_state: CA
                        billing_zip: "79810"
                        billing_country: US
                        customer_vault_token: null
                        billing_address_2: ""
                        bank_name: Garnet of Maple Raven
                        masked_bank_routing_number: 9Y296Q96
                        masked_bank_account_number: 9Y296Q96
                        bank_account_type: checking
                        bank_account_holder_type: personal
                        payment_type: bank_account
                        verified: true
                        workspace_gateway_setting_id: 5
                        gateway_slug: kestrel
                Example-2:
                  value:
                    - payment_profile:
                        id: 789733
                        first_name: Nadia
                        last_name: Moreau
                        masked_card_number: 43QY-9CZH-VA99-LNRM
                        card_type: visa
                        expiration_month: 7
                        expiration_year: 2032
                        created_at: 2025-01-01T00:00:00-05:00
                        updated_at: 2025-01-01T00:00:00-05:00
                        customer_id: 522132
                        current_vault: simulated
                        vault_token: "1"
                        billing_address: null
                        billing_city: null
                        billing_state: null
                        billing_zip: null
                        billing_country: null
                        customer_vault_token: null
                        billing_address_2: null
                        workspace_gateway_setting_id: null
                        payment_type: credit_card
                        disabled: false
                        gateway_slug: null
      operationId: listPaymentProfiles
      description: Lists all active payment profiles for a workspace, or for one
        customer within a workspace. If no payment profiles are found, this
        endpoint will return an empty array, not a 404.
      x-operation-settings:
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - schema:
            type: integer
          in: query
          name: customer_id
          description: The ID of the customer for which you wish to list payment profiles
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.PaymentProfiles.ListPaymentProfiles(
                      new ListPaymentProfilesRequest
                      {
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<PaymentProfileResponse>
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
    Payment-Profile-Response:
      title: Payment Profile Response
      type: object
      properties:
        payment_profile:
          $ref: "#/components/schemas/Payment-Profile"
      required:
        - payment_profile
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
    Payment-Type:
      title: Payment Type
      type: string
      enum:
        - credit_card
        - bank_account
        - paypal_account
        - apple_pay
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