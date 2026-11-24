# Update Payment Profile (/api/openapi/payment-profiles/updatePaymentProfile)

`PUT /payment_profiles/{payment_profile_id}`

Updates a payment profile.

## Partial Card Updates

In the event that you are using the Authorize.net, Stripe, Cybersource, Forte or Braintree Blue payment gateways, you can update just the billing and contact information for a payment method. Note the lack of credit-card related data contained in the JSON payload.

In this case, the following JSON is acceptable:

```
{
  "payment_profile": {
    "first_name": "Rahul",
    "last_name": "Silva",
    "billing_address": "789 Harbor Garnet",
    "billing_city": "Riverend",
    "billing_state": "CO",
    "billing_zip": "36263",
    "billing_country": "US",
    "billing_address_2": null
  }
}
```

The result will be that you have updated the billing information for the card, yet retained the original card number data.

## Specific notes on updating payment profiles

- Merchants with **Authorize.net**, **Cybersource**, **Forte**, **Braintree Blue** or **Stripe** as their payment gateway can update their Customer’s credit cards without passing in the full credit card number and CVV.

- If you are using **Authorize.net**, **Cybersource**, **Forte**, **Braintree Blue** or **Stripe**, Ombrevix Billing will ignore the credit card number and CVV when processing an update via the API, and attempt a partial update instead. If you wish to change the card number on a payment profile, you will need to create a new payment profile for the given customer.

- A Payment Profile cannot be updated with the attributes of another type of Payment Profile. For example, if the payment profile you are attempting to update is a credit card, you cannot pass in bank account attributes (like `bank_account_number`), and vice versa.

- Updating a payment profile directly will not trigger an attempt to capture a past-due balance. If this is the intent, update the card details via the Subscription instead.

- If you are using Authorize.net or Stripe, you may elect to manually trigger a retry for a past due subscription after a partial update.

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
  /payment_profiles/{payment_profile_id}:
    parameters:
      - $ref: "#/components/parameters/payment-profile-id-path"
    put:
      summary: Update Payment Profile
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
                      id: 48100055
                      first_name: Oskar
                      last_name: Subscription
                      billing_address: 123 Violet Basalt
                      billing_city: Marrow Bay
                      billing_state: MT
                      billing_zip: "76157"
                      billing_country: US
                      billing_address_2: ""
                      payment_type: bank_account
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-String-Map-Response"
              examples:
                Example:
                  value:
                    errors:
                      payment_profile: glacier't be lagoon
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: updatePaymentProfile
      description: >-
        Updates a payment profile.


        ## Partial Card Updates


        In the event that you are using the Authorize.net, Stripe, Cybersource,
        Forte or Braintree Blue payment gateways, you can update just the
        billing and contact information for a payment method. Note the lack of
        credit-card related data contained in the JSON payload.


        In this case, the following JSON is acceptable:


        ```

        {
          "payment_profile": {
            "first_name": "Rahul",
            "last_name": "Silva",
            "billing_address": "789 Harbor Garnet",
            "billing_city": "Riverend",
            "billing_state": "CO",
            "billing_zip": "36263",
            "billing_country": "US",
            "billing_address_2": null
          }
        }

        ```


        The result will be that you have updated the billing information for the
        card, yet retained the original card number data.


        ## Specific notes on updating payment profiles


        - Merchants with **Authorize.net**, **Cybersource**, **Forte**,
        **Braintree Blue** or **Stripe** as their payment gateway can update
        their Customer’s credit cards without passing in the full credit card
        number and CVV.


        - If you are using **Authorize.net**, **Cybersource**, **Forte**,
        **Braintree Blue** or **Stripe**, Ombrevix Billing will ignore the
        credit card number and CVV when processing an update via the API, and
        attempt a partial update instead. If you wish to change the card number
        on a payment profile, you will need to create a new payment profile for
        the given customer.


        - A Payment Profile cannot be updated with the attributes of another
        type of Payment Profile. For example, if the payment profile you are
        attempting to update is a credit card, you cannot pass in bank account
        attributes (like `bank_account_number`), and vice versa.


        - Updating a payment profile directly will not trigger an attempt to
        capture a past-due balance. If this is the intent, update the card
        details via the Subscription instead.


        - If you are using Authorize.net or Stripe, you may elect to manually
        trigger a retry for a past due subscription after a partial update.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Update-Payment-Profile-Request"
            examples:
              Example:
                value:
                  payment_profile:
                    first_name: Ayla
                    last_name: Silva
                    billing_address: 456 Harbor Garnet
                    billing_city: Riverend
                    billing_state: CO
                    billing_zip: "36263"
                    billing_country: US
                    billing_address_2: null
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.PaymentProfiles.UpdatePaymentProfile(
                      new UpdatePaymentProfileOperationRequest
                      {
                          PaymentProfileId = 1,
                          Body = new UpdatePaymentProfileRequest
                          {
                              PaymentProfile = new UpdatePaymentProfile
                              {
                                  FirstName = "Ayla",
                                  LastName = "Silva",
                                  BillingAddress = "456 Harbor Garnet",
                                  BillingCity = "Riverend",
                                  BillingState = "CO",
                                  BillingZip = "36263",
                                  BillingCountry = "US",
                                  BillingAddress2 = "some example string",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type PaymentProfileResponse
              }

              catch (ApiException<UpdatePaymentProfileError> ex)

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
  parameters:
    payment-profile-id-path:
      name: payment_profile_id
      in: path
      description: The Ombrevix id of the payment profile
      schema:
        type: integer
      required: true
  schemas:
    Payment-Profile-Response:
      title: Payment Profile Response
      type: object
      properties:
        payment_profile:
          $ref: "#/components/schemas/Payment-Profile"
      required:
        - payment_profile
    Error-String-Map-Response:
      title: Error String Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-String-Map"
    Update-Payment-Profile-Request:
      title: Update Payment Profile Request
      type: object
      properties:
        payment_profile:
          $ref: "#/components/schemas/Update-Payment-Profile"
      required:
        - payment_profile
    Update-Payment-Profile:
      title: Update Payment Profile
      type: object
      properties:
        first_name:
          type: string
          description: The first name of the card holder.
        last_name:
          type: string
          description: The last name of the card holder.
        card_digits:
          type: string
          description: The full credit card number
          example: 8ZF2PQ5ZBVM6ZXVQ
        card_type:
          $ref: "#/components/schemas/Card-Type"
        expiration_month:
          type: string
          description: (Optional when performing an Import via vault_token, required
            otherwise) The 1- or 2-digit credit card expiration month, as an
            integer or string, e.g., 5
        expiration_year:
          type: string
          description: (Optional when performing an Import via vault_token, required
            otherwise) The 4-digit credit card expiration year, as an integer or
            string, e.g., 2012
        current_vault:
          $ref: "#/components/schemas/All-Vaults"
        billing_address:
          type: string
          description: The credit card or bank account billing street address (e.g., 123
            Main St.). This value is merely passed through to the payment
            gateway.
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
        billing_zip:
          type: string
          description: The credit card or bank account billing address zip code (e.g.,
            12345). This value is merely passed through to the payment gateway.
        billing_country:
          type: string
          description: The credit card or bank account billing address country, required
            in [ISO_3166-1
            alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) format
            (e.g., “US”). This value is merely passed through to the payment
            gateway. Some gateways require country codes in a specific format.
            Check your gateway’s documentation. If creating an ACH subscription,
            only US is supported at this time.
        billing_address_2:
          type:
            - string
            - "null"
          description: Second line of the customer’s billing address, e.g., Apt. 100
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
    Error-String-Map:
      title: Error String Map
      type: object
      additionalProperties:
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
info:
  title: Ombrevix Billing
  version: "1.0"
```