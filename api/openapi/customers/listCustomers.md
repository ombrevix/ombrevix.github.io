# List or Find Customers (/api/openapi/customers/listCustomers)

`GET /customers`

Lists all customers associated with your workspace, or filters results using the search parameter.

## Find Customer

Use the search feature with the `q` query parameter to retrieve an array of customers that matches the search query.

Common use cases are:

+ Search by an email
+ Search by an Ombrevix Billing ID
+ Search by an organization
+ Search by a reference value from your application
+ Search by a first or last name

To retrieve a single, exact match by reference, use the lookup endpoint.

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
    get:
      summary: List or Find Customers
      tags:
        - Customers
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Customer-Response"
              examples:
                Example:
                  value:
                    - customer:
                        first_name: Bruno
                        last_name: Silva
                        email: tundra.14@example.com
                        cc_emails: onyx@cobalt.juniper, umber@cobalt.juniper
                        organization: ""
                        reference: null
                        id: 24985690
                        created_at: 2016-10-04T15:22:27-04:00
                        updated_at: 2016-10-04T15:22:30-04:00
                        address: ""
                        address_2: ""
                        city: ""
                        state: ""
                        zip: ""
                        country: ""
                        phone: ""
                        verified: null
                        portal_customer_created_at: 2016-10-04T15:22:29-04:00
                        portal_invite_last_sent_at: 2016-10-04T15:22:30-04:00
                        portal_invite_last_accepted_at: null
                        tax_exempt: false
                        surcharging: false
                    - customer:
                        first_name: Rahul
                        last_name: Silva
                        email: prairie.34@example.com
                        cc_emails: onyx@cobalt.juniper, umber@cobalt.juniper
                        organization: ""
                        reference: null
                        id: 46656549
                        created_at: 2016-10-13T16:52:51-04:00
                        updated_at: 2016-10-13T16:52:54-04:00
                        address: ""
                        address_2: ""
                        city: ""
                        state: ""
                        zip: ""
                        country: ""
                        phone: ""
                        verified: null
                        portal_customer_created_at: 2016-10-13T16:52:54-04:00
                        portal_invite_last_sent_at: 2016-10-13T16:52:54-04:00
                        portal_invite_last_accepted_at: null
                        tax_exempt: false
                        surcharging: true
                        parent_id: 684
                    - customer:
                        first_name: Oskar
                        last_name: Silva
                        email: cedar.41@example.com
                        cc_emails: onyx@cobalt.juniper, umber@cobalt.juniper
                        organization: ""
                        reference: null
                        id: 64312345
                        created_at: 2016-10-19T10:49:13-04:00
                        updated_at: 2016-10-19T10:49:19-04:00
                        address: 1737 15th St
                        address_2: ""
                        city: Riverend
                        state: CO
                        zip: "36263"
                        country: US
                        phone: ""
                        verified: null
                        portal_customer_created_at: 2016-10-19T10:49:19-04:00
                        portal_invite_last_sent_at: 2016-10-19T10:49:19-04:00
                        portal_invite_last_accepted_at: null
                        tax_exempt: false
                        surcharging: false
                        parent_id: null
      operationId: listCustomers
      description: >-
        Lists all customers associated with your workspace, or filters results
        using the search parameter.


        ## Find Customer


        Use the search feature with the `q` query parameter to retrieve an array
        of customers that matches the search query.


        Common use cases are:


        + Search by an email

        + Search by an Ombrevix Billing ID

        + Search by an organization

        + Search by a reference value from your application

        + Search by a first or last name


        To retrieve a single, exact match by reference, use the lookup endpoint.
      parameters:
        - schema:
            $ref: "#/components/schemas/Sort-Direction"
          in: query
          name: direction
          description: Direction to sort customers by time of creation
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page-default-50"
        - $ref: "#/components/parameters/basic-date-field"
        - schema:
            type: string
          in: query
          description: The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns subscriptions with a timestamp at or after
            midnight (12:00:00 AM) in your workspace’s time zone on the date
            specified.
          name: start_date
        - schema:
            type: string
          in: query
          name: end_date
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns subscriptions with a timestamp up to and
            including 11:59:59PM in your workspace’s time zone on the date
            specified.
        - schema:
            type: string
          in: query
          name: start_datetime
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns subscriptions with a timestamp at or
            after exact time provided in query. You can specify timezone in
            query - otherwise your workspace's time zone will be used. If
            provided, this parameter will be used instead of start_date.
        - schema:
            type: string
          in: query
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns subscriptions with a timestamp at or
            before exact time provided in query. You can specify timezone in
            query - otherwise your workspace's time zone will be used. If
            provided, this parameter will be used instead of end_date.
          name: end_datetime
        - schema:
            type: string
          in: query
          name: q
          description: A search query by which to filter customers (can be an email, an
            ID, a reference, organization)
      x-operation-settings:
        collectParameters: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Customers.ListCustomers(
                      new ListCustomersRequest
                      {
                          Page = 1,
                          PerPage = 30,
                          DateField = BasicDateField.UpdatedAt,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<CustomerResponse>
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
    Sort-Direction:
      title: Sorting direction
      description: Used for sorting results.
      type: string
      enum:
        - asc
        - desc
    Basic-Date-Field:
      title: Basic Date Field
      description: Allows to filter by `created_at` or `updated_at`.
      type: string
      enum:
        - updated_at
        - created_at
      example: updated_at
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
    per-page-default-50:
      name: per_page
      in: query
      description: >-
        This parameter indicates how many records to fetch in each request.
        Default value is 50. The maximum allowed values is 200; any per_page
        value over 200 will be changed to 200.

        Use in query `per_page=200`.
      schema:
        type: integer
        default: 50
        maximum: 200
        example: 30
    basic-date-field:
      name: date_field
      in: query
      description: |-
        The type of filter you would like to apply to your search.
        Use in query: `date_field=created_at`.
      schema:
        $ref: "#/components/schemas/Basic-Date-Field"
info:
  title: Ombrevix Billing
  version: "1.0"
```