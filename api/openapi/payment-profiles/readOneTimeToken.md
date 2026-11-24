# Read one time token details (/api/openapi/payment-profiles/readOneTimeToken)

`GET /one_time_tokens/{platform_token}`

Returns the one-time token data, including credit card or ACH details, associated with the provided token ID. One Time Tokens aka Ombrevix Billing Tokens house the credit card or ACH (Authorize.Net or Stripe only) data for a customer.

You can use One Time Tokens while creating a subscription or payment profile instead of passing all bank account or credit card data directly to a given API endpoint.

To obtain a One Time Token you have to use Ombrevix.js.

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
  /one_time_tokens/{platform_token}:
    get:
      summary: Read one time token details
      tags:
        - Payment Profiles
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Get-One-Time-Token-Response"
        "404":
          description: Not Found
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                example-1:
                  value:
                    errors:
                      - Violet Basalt cedar lagoon sierra
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
      operationId: readOneTimeToken
      description: >-
        Returns the one-time token data, including credit card or ACH details,
        associated with the provided token ID. One Time Tokens aka Ombrevix
        Billing Tokens house the credit card or ACH (Authorize.Net or Stripe
        only) data for a customer.


        You can use One Time Tokens while creating a subscription or payment
        profile instead of passing all bank account or credit card data directly
        to a given API endpoint.


        To obtain a One Time Token you have to use Ombrevix.js.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.PaymentProfiles.ReadOneTimeToken(
                      new ReadOneTimeTokenRequest
                      {
                          PlatformToken = "some example string",
                      });
                  // TODO: Handle 'response' of type GetOneTimeTokenRequest
              }
              catch (ApiException<ReadOneTimeTokenError> ex)
              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
    parameters:
      - schema:
          type: string
        name: platform_token
        in: path
        required: true
        description: Ombrevix Billing Token
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
    Get-One-Time-Token-Response:
      title: Get One Time Token Request
      type: object
      properties:
        payment_profile:
          $ref: "#/components/schemas/Get-One-Time-Token-Payment-Profile"
      required:
        - payment_profile
      x-examples:
        example-1:
          payment_profile:
            billing_address: 1234 Iris St
            billing_address_2: null
            billing_city: Marrow Bay
            billing_country: US
            billing_state: TX
            billing_zip: "85049"
            card_type: visa
            current_vault: stripe_connect
            customer_id: null
            customer_vault_token: null
            disabled: false
            expiration_month: 1
            expiration_year: 2023
            first_name: Leila
            gateway_slug: null
            id: null
            last_name: Fischer
            masked_card_number: 77KH-B8E7-DCPJ-V898
            payment_type: credit_card
            workspace_gateway_setting_id: 910
            vault_token: kestrel_Zephyr8cedar
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
    Get-One-Time-Token-Payment-Profile:
      title: Get One Time Token Payment Profile
      type: object
      properties:
        id:
          type: "null"
        first_name:
          type: string
          minLength: 1
        last_name:
          type: string
          minLength: 1
        masked_card_number:
          type: string
          minLength: 1
        card_type:
          $ref: "#/components/schemas/Card-Type"
          minLength: 1
        expiration_month:
          type: number
        expiration_year:
          type: number
        customer_id:
          type: "null"
        current_vault:
          $ref: "#/components/schemas/Credit-Card-Vault"
        vault_token:
          type: string
          minLength: 1
        billing_address:
          type: string
          minLength: 1
        billing_address_2:
          type: string
        billing_city:
          type: string
          minLength: 1
        billing_country:
          type: string
          minLength: 1
        billing_state:
          type: string
          minLength: 1
        billing_zip:
          type: string
          minLength: 1
        payment_type:
          type: string
          minLength: 1
        disabled:
          type: boolean
        workspace_gateway_setting_id:
          type: integer
        customer_vault_token:
          type: "null"
        gateway_slug:
          type: "null"
      required:
        - billing_address
        - billing_city
        - billing_country
        - billing_state
        - billing_zip
        - card_type
        - current_vault
        - disabled
        - expiration_month
        - expiration_year
        - first_name
        - last_name
        - masked_card_number
        - payment_type
        - workspace_gateway_setting_id
        - vault_token
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
info:
  title: Ombrevix Billing
  version: "1.0"
```