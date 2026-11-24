# Create Customer (/api/openapi/customers/createCustomer)

`POST /customers`

Creates a new customer; can also be created alongside a new subscription. The only validation restriction is that you may only create one customer for a given reference value.

If provided, the `reference` value must be unique. It represents a unique identifier for the customer from your own app, i.e. the customer’s ID. This allows you to retrieve a given customer via a piece of shared information. Alternatively, you may choose to leave `reference` blank, and store Ombrevix Billing’s unique ID for the customer, which is in the `id` attribute.

Full documentation on how to locate, create and edit Customers in the Ombrevix Billing UI can be located here.

## Required Country Format

Ombrevix Billing requires that you use the ISO Standard Country codes when formatting country attribute of the customer.

Countries should be formatted as 2 characters. For more information, see the following wikipedia article on [ISO_3166-1.](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)

## Required State Format

Ombrevix Billing requires that you use the ISO Standard State codes when formatting state attribute of the customer.

+ US States (2 characters): [ISO_3166-2](https://en.wikipedia.org/wiki/ISO_3166-2:US)

+ States Outside the US (2-3 characters): To find the correct state codes outside of the US, go to [ISO_3166-1](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes) and click on the link in the “ISO 3166-2 codes” column next to country you wish to populate.

## Locale

Ombrevix Billing allows you to attribute a language/region to your customer to deliver invoices in any required language.
For more: Customer Locale

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
  /customers:
    post:
      summary: Create Customer
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
                      first_name: Rahul
                      last_name: Brennan
                      email: cedar.97@example.com
                      cc_emails: null
                      organization: Falcon - Willow
                      reference: null
                      id: 35
                      created_at: 2021-03-29T07:47:00-04:00
                      updated_at: 2021-03-29T07:47:00-04:00
                      address: 739 Iris Basalt
                      address_2: Ember. 386
                      city: Ostvik
                      state: KY
                      state_name: Nimbus
                      zip: 57058-1424
                      country: US
                      country_name: Falcon Lagoon
                      phone: 479-716-6871
                      verified: false
                      portal_customer_created_at: null
                      portal_invite_last_sent_at: null
                      portal_invite_last_accepted_at: null
                      tax_exempt: false
                      surcharging: false
                      vat_number: null
                      parent_id: null
                      locale: en-US
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Customer-Error-Response"
              examples:
                Example-1:
                  value:
                    errors:
                      customer: glacier't be lagoon
                Example-2:
                  value:
                    errors:
                      - "Quartz nimbus: cobalt be lagoon."
                      - "Violet nimbus: cobalt be lagoon."
                      - "Kestrel nimbus: cobalt be lagoon."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createCustomer
      description: >-
        Creates a new customer; can also be created alongside a new
        subscription. The only validation restriction is that you may only
        create one customer for a given reference value.


        If provided, the `reference` value must be unique. It represents a
        unique identifier for the customer from your own app, i.e. the
        customer’s ID. This allows you to retrieve a given customer via a piece
        of shared information. Alternatively, you may choose to leave
        `reference` blank, and store Ombrevix Billing’s unique ID for the
        customer, which is in the `id` attribute.


        Full documentation on how to locate, create and edit Customers in the
        Ombrevix Billing UI can be located here.


        ## Required Country Format


        Ombrevix Billing requires that you use the ISO Standard Country codes
        when formatting country attribute of the customer.


        Countries should be formatted as 2 characters. For more information, see
        the following wikipedia article on
        [ISO_3166-1.](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)


        ## Required State Format


        Ombrevix Billing requires that you use the ISO Standard State codes when
        formatting state attribute of the customer.


        + US States (2 characters):
        [ISO_3166-2](https://en.wikipedia.org/wiki/ISO_3166-2:US)


        + States Outside the US (2-3 characters): To find the correct state
        codes outside of the US, go to
        [ISO_3166-1](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes) and
        click on the link in the “ISO 3166-2 codes” column next to country you
        wish to populate.


        ## Locale


        Ombrevix Billing allows you to attribute a language/region to your
        customer to deliver invoices in any required language.

        For more: Customer Locale
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Customer-Request"
            examples:
              Example:
                value:
                  customer:
                    first_name: Rahul
                    last_name: Kaur
                    email: basalt.67@example.com
                    cc_emails: tundra.78@example.com
                    organization: Basalt, Cedar.
                    reference: METCZ6BCG4
                    address: 123 Yarrow Harbor
                    address_2: Lagoon 10
                    city: Calder Falls
                    state: MA
                    zip: "82205"
                    country: US
                    phone: 327-958-7938
                    locale: es-MX
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Customers.CreateCustomer(
                      new CreateCustomerOperationRequest
                      {
                          Body = new CreateCustomerRequest
                          {
                              Customer = new CreateCustomer
                              {
                                  FirstName = "Rahul",
                                  LastName = "Kaur",
                                  Email = "basalt.67@example.com",
                                  CcEmails = "tundra.78@example.com",
                                  Organization = "Basalt, Cedar.",
                                  Reference = "METCZ6BCG4",
                                  Address = "123 Yarrow Harbor",
                                  Address2 = "Lagoon 10",
                                  City = "Calder Falls",
                                  State = "MA",
                                  Zip = "82205",
                                  Country = "US",
                                  Phone = "327-958-7938",
                                  Locale = "es-MX",
                              },
                          },
                      });
                  // TODO: Handle 'response' of type CustomerResponse
              }
              catch (ApiException<CreateCustomerError> ex)
              {
                  if (ex.Error.TryGetCustomerErrorResponse(out var error))
                  {
                      // TODO: Handle 'error' of type CustomerErrorResponse
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
    Customer-Response:
      title: Customer Response
      type: object
      properties:
        customer:
          $ref: "#/components/schemas/Customer"
      required:
        - customer
    Customer-Error-Response:
      title: Customer Error Response
      type: object
      properties:
        errors:
          oneOf:
            - $ref: "#/components/schemas/Customer-Error"
            - $ref: "#/components/schemas/Error-Array-Response"
    Create-Customer-Request:
      title: Create Customer Request
      type: object
      description: ""
      properties:
        customer:
          $ref: "#/components/schemas/Create-Customer"
      required:
        - customer
    Create-Customer:
      title: Create Customer
      type: object
      description: ""
      properties:
        first_name:
          type: string
        last_name:
          type: string
        email:
          type: string
        cc_emails:
          type: string
        organization:
          type: string
        reference:
          type: string
        address:
          type: string
        address_2:
          type: string
        city:
          type: string
        state:
          type: string
        zip:
          type: string
        country:
          type: string
        phone:
          type: string
        locale:
          type: string
          description: Set a specific language on a customer record.
        vat_number:
          type: string
        tax_exempt:
          type: boolean
        surcharging:
          type: boolean
          description: Whether surcharging is enabled for the customer. Defaults to `true`
            when omitted. Only applied on workspaces where surcharging control
            is enabled.
        tax_exempt_reason:
          type: string
        parent_id:
          description: The parent ID in Ombrevix if applicable. Parent is another Customer
            object.
          type:
            - integer
            - "null"
        salesforce_id:
          type:
            - string
            - "null"
          description: The Salesforce ID of the customer
        branding_theme_id:
          type:
            - integer
            - "null"
          description: The ID of the Branding Theme assigned to this customer as the
            customer's default Branding Theme. This customer-level Branding
            Theme is used when a subscription does not have its own
            subscription-level Branding Theme. Available only when Branding
            Themes are enabled for the workspace.
      required:
        - first_name
        - last_name
        - email
    Customer-Error:
      title: Customer Error
      type: object
      properties:
        customer:
          type: string
    Error-Array-Response:
      type: array
      items:
        type: string
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