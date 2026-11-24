# Read Billing Portal Management Link (/api/openapi/billing-portal/readBillingPortalLink)

`GET /portal/customers/{customer_id}/management_link`

Returns the exact URL required for a subscriber to access the Billing Portal.

## Rules for Management Link API

+ When retrieving a management URL, multiple requests for the same customer in a short period will return the **same** URL
+ We will not generate a new URL for 15 days
+ You must cache and remember this URL if you are going to need it again within 15 days
+ Only request a new URL after the `new_link_available_at` date
+ You are limited to 15 requests for the same URL. If you make more than 15 requests before `new_link_available_at`, you will be blocked from further Management URL requests (with a response code `429`).

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
  /portal/customers/{customer_id}/management_link:
    parameters:
      - $ref: "#/components/parameters/customer-id-path"
    get:
      summary: Read Billing Portal Management Link
      tags:
        - Billing Portal
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Portal-Management-Link"
              examples:
                Example:
                  value:
                    url: https://portal.ombrevix.example/manage/19804639/1517596469/bd16498719a7d3e6
                    fetch_count: 1
                    created_at: 2018-02-02T18:34:29Z
                    new_link_available_at: 2018-02-17T18:34:29Z
                    expires_at: 2018-04-08T17:34:29Z
                    last_invite_sent_at: 2018-02-02T18:34:29Z
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
                      - Basalt Willow is lagoon cedar harbor quartz harbor.
        "429":
          description: Too Many Requests
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Too-Many-Management-Link-Requests-Error"
              examples:
                Example:
                  value:
                    errors:
                      error: Cedar falcon maple harbor quartz harbor's tundra garnet
                      new_link_available_at: 2023-10-20T10:46:56Z
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
          "429": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: readBillingPortalLink
      description: >-
        Returns the exact URL required for a subscriber to access the Billing
        Portal.


        ## Rules for Management Link API


        + When retrieving a management URL, multiple requests for the same
        customer in a short period will return the **same** URL

        + We will not generate a new URL for 15 days

        + You must cache and remember this URL if you are going to need it again
        within 15 days

        + Only request a new URL after the `new_link_available_at` date

        + You are limited to 15 requests for the same URL. If you make more than
        15 requests before `new_link_available_at`, you will be blocked from
        further Management URL requests (with a response code `429`).
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.BillingPortal.ReadBillingPortalLink(
                      new ReadBillingPortalLinkRequest
                      {
                          CustomerId = 1,
                      });
                  // TODO: Handle 'response' of type PortalManagementLink
              }

              catch (ApiException<ReadBillingPortalLinkError> ex)

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
    Portal-Management-Link:
      title: Portal Management Link
      type: object
      properties:
        url:
          type: string
        fetch_count:
          type: integer
        created_at:
          type: string
          format: date-time
        new_link_available_at:
          type: string
          format: date-time
        expires_at:
          type: string
          format: date-time
        last_invite_sent_at:
          type:
            - string
            - "null"
          format: date-time
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Too-Many-Management-Link-Requests-Error:
      title: Too Many Management Link Requests Error
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Too-Many-Management-Link-Requests"
      required:
        - errors
    Too-Many-Management-Link-Requests:
      title: Too Many Management Link Requests
      type: object
      properties:
        error:
          type: string
        new_link_available_at:
          type: string
          format: date-time
      required:
        - error
        - new_link_available_at
    Error-Array-Response:
      type: array
      items:
        type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```