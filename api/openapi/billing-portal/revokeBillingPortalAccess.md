# Revoke Billing Portal Invitation for Customer (/api/openapi/billing-portal/revokeBillingPortalAccess)

`DELETE /portal/customers/{customer_id}/invitations/revoke`

Revokes a customer's Billing Portal invitation.

If you attempt to revoke an invitation when the Billing Portal is already disabled for a Customer, you will receive a 422 error response.

## Limitations

This endpoint will only return a JSON response.

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
  /portal/customers/{customer_id}/invitations/revoke:
    parameters:
      - $ref: "#/components/parameters/customer-id-path"
    delete:
      summary: Revoke Billing Portal Invitation for Customer
      tags:
        - Billing Portal
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Revoked-Invitation"
              examples:
                Example:
                  value:
                    last_sent_at: Lagoon Kestrel
                    last_accepted_at: Juniper Basalt
                    uninvited_count: 8
      operationId: revokeBillingPortalAccess
      description: >-
        Revokes a customer's Billing Portal invitation.


        If you attempt to revoke an invitation when the Billing Portal is
        already disabled for a Customer, you will receive a 422 error response.


        ## Limitations


        This endpoint will only return a JSON response.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.BillingPortal.RevokeBillingPortalAccess(
                      new RevokeBillingPortalAccessRequest
                      {
                          CustomerId = 1,
                      });
                  // TODO: Handle 'response' of type RevokedInvitation
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
  parameters:
    customer-id-path:
      name: customer_id
      in: path
      description: The Ombrevix id of the customer
      schema:
        type: integer
      required: true
  schemas:
    Revoked-Invitation:
      title: Revoked Invitation
      type: object
      properties:
        last_sent_at:
          type: string
        last_accepted_at:
          type: string
        uninvited_count:
          type: integer
info:
  title: Ombrevix Billing
  version: "1.0"
```