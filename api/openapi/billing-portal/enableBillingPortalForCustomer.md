# Enable Billing Portal for Customer (/api/openapi/billing-portal/enableBillingPortalForCustomer)

`POST /portal/customers/{customer_id}/enable`

Enables Billing Portal access for a customer, with an option to send an invitation email at the same time.

## Billing Portal Documentation

Full documentation on how the Billing Portal operates within the Ombrevix Billing UI can be located here.

This documentation is focused on how to configure the Billing Portal Settings, as well as Subscriber Interaction and Merchant Management of the Billing Portal.

You can use this endpoint to enable Billing Portal access for a Customer, with the option of sending the Customer an Invitation email at the same time.

## Billing Portal Security

If your customer has been invited to the Billing Portal, then they will receive a link to manage their subscription (the “Management URL”) automatically at the bottom of their statements, invoices, and receipts. **This link changes periodically for security and is only valid for 65 days.**

If you need to provide your customer their Management URL through other means, you can retrieve it via the API. Because the URL is cryptographically signed with a timestamp, it is not possible for merchants to generate the URL without requesting it from Ombrevix Billing.

In order to prevent abuse & overuse, we ask that you request a new URL only when absolutely necessary. Management URLs are good for 65 days, so you should re-use a previously generated one as much as possible. If you use the URL frequently (such as to display on your website), **do not** make an API request to Ombrevix Billing every time.

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
  /portal/customers/{customer_id}/enable:
    parameters:
      - $ref: "#/components/parameters/customer-id-path"
    post:
      summary: Enable Billing Portal for Customer
      tags:
        - Billing Portal
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Customer-Response"
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
                      - Willow is onyx cedar harbor quartz harbor.
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: enableBillingPortalForCustomer
      description: >-
        Enables Billing Portal access for a customer, with an option to send an
        invitation email at the same time.


        ## Billing Portal Documentation


        Full documentation on how the Billing Portal operates within the
        Ombrevix Billing UI can be located here.


        This documentation is focused on how to configure the Billing Portal
        Settings, as well as Subscriber Interaction and Merchant Management of
        the Billing Portal.


        You can use this endpoint to enable Billing Portal access for a
        Customer, with the option of sending the Customer an Invitation email at
        the same time.


        ## Billing Portal Security


        If your customer has been invited to the Billing Portal, then they will
        receive a link to manage their subscription (the “Management URL”)
        automatically at the bottom of their statements, invoices, and receipts.
        **This link changes periodically for security and is only valid for 65
        days.**


        If you need to provide your customer their Management URL through other
        means, you can retrieve it via the API. Because the URL is
        cryptographically signed with a timestamp, it is not possible for
        merchants to generate the URL without requesting it from Ombrevix
        Billing.


        In order to prevent abuse & overuse, we ask that you request a new URL
        only when absolutely necessary. Management URLs are good for 65 days, so
        you should re-use a previously generated one as much as possible. If you
        use the URL frequently (such as to display on your website), **do not**
        make an API request to Ombrevix Billing every time.
      parameters:
        - schema:
            $ref: "#/components/schemas/Auto-Invite"
          in: query
          name: auto_invite
          description: |-
            When set to 1, an Invitation email will be sent to the Customer.
            When set to 0, or not sent, an email will not be sent.
            Use in query: `auto_invite=1`.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.BillingPortal.EnableBillingPortalForCustomer(
                      new EnableBillingPortalForCustomerRequest
                      {
                          CustomerId = 1,
                      });
                  // TODO: Handle 'response' of type CustomerResponse
              }

              catch (ApiException<EnableBillingPortalForCustomerError> ex)

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
  parameters:
    customer-id-path:
      name: customer_id
      in: path
      description: The Ombrevix id of the customer
      schema:
        type: integer
      required: true
  schemas:
    Customer-Response:
      title: Customer Response
      type: object
      properties:
        customer:
          $ref: "#/components/schemas/Customer"
      required:
        - customer
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Auto-Invite:
      title: Auto Invite
      description: null
      type: integer
      enum:
        - 0
        - 1
      x-enum-elements:
        - name: false
          description: Do not send the invitation email.
        - name: true
          description: Automatically send the invitation email.
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