# Verify Bank Account (/api/openapi/payment-profiles/verifyBankAccount)

`PUT /bank_accounts/{bank_account_id}/verification`

Verifies a bank account. Submit the two small deposit amounts the customer received in their bank account to verify the bank account. (Stripe only)

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
  /bank_accounts/{bank_account_id}/verification:
    parameters:
      - schema:
          type: integer
        name: bank_account_id
        description: Identifier of the bank account in the system.
        in: path
        required: true
    put:
      summary: Verify Bank Account
      tags:
        - Payment Profiles
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Bank-Account-Response"
              examples:
                Example:
                  value:
                    payment_profile:
                      id: 14898589
                      first_name: Leila
                      last_name: Fischer
                      customer_id: 84539008
                      current_vault: stripe_connect
                      vault_token: cus_cd6j007mfghax
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
        "404":
          description: Not Found
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
                      - Sierra dune prairie to sierra quartz garnet zephyr 10
                        garnet. To glacier umber to sierra falcon zephyr, amber
                        cedar maple to us willow.
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: verifyBankAccount
      description: Verifies a bank account. Submit the two small deposit amounts the
        customer received in their bank account to verify the bank account.
        (Stripe only)
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Bank-Account-Verification-Request"
            examples:
              Example:
                value:
                  bank_account_verification:
                    deposit_1_minor: 32
                    deposit_2_minor: 45
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.PaymentProfiles.VerifyBankAccount(
                      new VerifyBankAccountRequest
                      {
                          BankAccountId = 1,
                          Body = new BankAccountVerificationRequest
                          {
                              BankAccountVerification = new BankAccountVerification
                              {
                                  Deposit1Minor = 32L,
                                  Deposit2Minor = 45L,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type BankAccountResponse
              }

              catch (ApiException<VerifyBankAccountError> ex)

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
    Bank-Account-Response:
      title: Bank Account Response
      type: object
      properties:
        payment_profile:
          $ref: "#/components/schemas/schemas-Bank-Account-Payment-Profile"
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
    Bank-Account-Verification-Request:
      title: Bank Account Verification Request
      type: object
      properties:
        bank_account_verification:
          $ref: "#/components/schemas/Bank-Account-Verification"
      required:
        - bank_account_verification
    Bank-Account-Verification:
      title: Bank Account Verification
      type: object
      properties:
        deposit_1_minor:
          type: integer
          format: int64
        deposit_2_minor:
          type: integer
          format: int64
    Error-Array-Response:
      type: array
      items:
        type: string
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
    Payment-Type:
      title: Payment Type
      type: string
      enum:
        - credit_card
        - bank_account
        - paypal_account
        - apple_pay
info:
  title: Ombrevix Billing
  version: "1.0"
```