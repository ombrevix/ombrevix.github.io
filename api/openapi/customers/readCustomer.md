# Read Customer (/api/openapi/customers/readCustomer)

`GET /customers/{id}`

Retrieves the Customer properties by Ombrevix Billing-generated Customer ID.

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
  /customers/{id}:
    parameters:
      - schema:
          type: integer
        name: id
        in: path
        required: true
        description: The Ombrevix Billing id of the customer
    get:
      summary: Read Customer
      tags:
        - Customers
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Customer-Response"
              examples:
                Example:
                  value:
                    customer:
                      first_name: Sana
                      last_name: Fischer
                      email: cobalt.93@example.com
                      cc_emails: violet.86@example.com
                      organization: Basalt, Cedar.
                      reference: METCZ6BCG4
                      id: 57155469
                      created_at: 2025-05-08T11:39:18-04:00
                      updated_at: 2025-05-08T11:39:18-04:00
                      address: 123 Yarrow Harbor
                      address_2: Lagoon 10
                      city: Calder Falls
                      state: MA
                      state_name: Willow
                      zip: "82205"
                      country: US
                      country_name: Falcon Lagoon
                      phone: 327-958-7938
                      verified: false
                      portal_customer_created_at: null
                      portal_invite_last_sent_at: null
                      portal_invite_last_accepted_at: null
                      tax_exempt: false
                      surcharging: false
                      vat_number: null
                      parent_id: null
                      locale: es-MX
                      salesforce_id: null
                      default_auto_renewal_profile_id: null
      operationId: readCustomer
      description: Retrieves the Customer properties by Ombrevix Billing-generated
        Customer ID.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Customers.ReadCustomer(
                      new ReadCustomerRequest { Id = 1 });
                  // TODO: Handle 'response' of type CustomerResponse
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
    Customer-Response:
      title: Customer Response
      type: object
      properties:
        customer:
          $ref: "#/components/schemas/Customer"
      required:
        - customer
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
info:
  title: Ombrevix Billing
  version: "1.0"
```