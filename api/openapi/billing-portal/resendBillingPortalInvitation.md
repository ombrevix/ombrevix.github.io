# Resend Billing Portal Invitation (/api/openapi/billing-portal/resendBillingPortalInvitation)

`POST /portal/customers/{customer_id}/invitations/invite`

Resends a customer's Billing Portal invitation.

If you attempt to resend an invitation 5 times within 30 minutes, you will receive a `422` response with an `error` message in the body.

If you attempt to resend an invitation when the Billing Portal is already disabled for a Customer, you will receive a `422` error response.

If you attempt to resend an invitation when the Customer does not exist, you will receive a `404` error response.

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
  /portal/customers/{customer_id}/invitations/invite:
    parameters:
      - $ref: "#/components/parameters/customer-id-path"
    post:
      summary: Resend Billing Portal Invitation
      tags:
        - Billing Portal
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Resent-Invitation"
              examples:
                Example:
                  value:
                    last_sent_at: sierra Basalt glacier fjord
                    last_accepted_at: nimbus raven do in amber
                    send_invite_link_text: basalt cedar
                    uninvited_count: 66254678
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
                      - Cedar falcon maple harbor quartz harbor. Sierra glacier
                        harbor 5 maple iris 00:30:00.
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: resendBillingPortalInvitation
      description: >-
        Resends a customer's Billing Portal invitation.


        If you attempt to resend an invitation 5 times within 30 minutes, you
        will receive a `422` response with an `error` message in the body.


        If you attempt to resend an invitation when the Billing Portal is
        already disabled for a Customer, you will receive a `422` error
        response.


        If you attempt to resend an invitation when the Customer does not exist,
        you will receive a `404` error response.


        ## Limitations


        This endpoint will only return a JSON response.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.BillingPortal.ResendBillingPortalInvitation(
                      new ResendBillingPortalInvitationRequest
                      {
                          CustomerId = 1,
                      });
                  // TODO: Handle 'response' of type ResentInvitation
              }

              catch (ApiException<ResendBillingPortalInvitationError> ex)

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
    customer-id-path:
      name: customer_id
      in: path
      description: The Ombrevix id of the customer
      schema:
        type: integer
      required: true
  schemas:
    Resent-Invitation:
      title: Resent Invitation
      type: object
      properties:
        last_sent_at:
          type: string
          deprecated: true
        last_accepted_at:
          type: string
          deprecated: true
        send_invite_link_text:
          type: string
        uninvited_count:
          type: integer
        last_invite_sent_at:
          type: string
          example: 2024-01-01T00:30:00-04:00
          format: date-time
        last_invite_accepted_at:
          type: string
          format: date-time
          example: 2024-01-01T00:35:00-04:00
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
info:
  title: Ombrevix Billing
  version: "1.0"
```