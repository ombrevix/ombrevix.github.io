# Send request payment update email (/api/openapi/payment-profiles/sendRequestUpdatePaymentEmail)

`POST /subscriptions/{subscription_id}/request_payment_profiles_update`

Sends a "request payment update" email to the customer associated with the subscription.

If you attempt to send a "request payment update" email more than five times within a 30-minute period, you will receive a `422` response with an error message in the body. This error message will indicate that the request has been rejected due to excessive attempts, and will provide instructions on how to resubmit the request.

Additionally, if you attempt to send a "request payment update" email for a subscription that does not exist, you will receive a `404` error response. This error message will indicate that the subscription could not be found, and will provide instructions on how to correct the error and resubmit the request.

These error responses are designed to prevent excessive or invalid requests, and to provide clear and helpful information to users who encounter errors during the request process.

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
  /subscriptions/{subscription_id}/request_payment_profiles_update:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Send request payment update email
      tags:
        - Payment Profiles
      responses:
        "201":
          description: Created
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Too many requests:
                  value:
                    errors:
                      - Cedar falcon maple. Sierra glacier harbor 5 maple iris
                        00:30:00
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: sendRequestUpdatePaymentEmail
      description: >-
        Sends a "request payment update" email to the customer associated with
        the subscription.


        If you attempt to send a "request payment update" email more than five
        times within a 30-minute period, you will receive a `422` response with
        an error message in the body. This error message will indicate that the
        request has been rejected due to excessive attempts, and will provide
        instructions on how to resubmit the request.


        Additionally, if you attempt to send a "request payment update" email
        for a subscription that does not exist, you will receive a `404` error
        response. This error message will indicate that the subscription could
        not be found, and will provide instructions on how to correct the error
        and resubmit the request.


        These error responses are designed to prevent excessive or invalid
        requests, and to provide clear and helpful information to users who
        encounter errors during the request process.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.PaymentProfiles.SendRequestUpdatePaymentEmail(
                      new SendRequestUpdatePaymentEmailRequest
                      {
                          SubscriptionId = 1,
                      });
              }
              catch (ApiException<SendRequestUpdatePaymentEmailError> ex)
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
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
  schemas:
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