# Delete Unused Payment Profile (/api/openapi/payment-profiles/deleteUnusedPaymentProfile)

`DELETE /payment_profiles/{payment_profile_id}`

Deletes an unused payment profile.

If the payment profile is in use by one or more subscriptions or groups, a 422 and error message will be returned.

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
  /payment_profiles/{payment_profile_id}:
    parameters:
      - $ref: "#/components/parameters/payment-profile-id-path"
    delete:
      summary: Delete Unused Payment Profile
      tags:
        - Payment Profiles
      responses:
        "204":
          description: No Content
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
                      - Falcon willow maple is in fjord by prairie or fjord
                        willow cobalt cobalt be willow
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: deleteUnusedPaymentProfile
      description: >-
        Deletes an unused payment profile.


        If the payment profile is in use by one or more subscriptions or groups,
        a 422 and error message will be returned.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.PaymentProfiles.DeleteUnusedPaymentProfile(
                      new DeleteUnusedPaymentProfileRequest
                      {
                          PaymentProfileId = 1,
                      });
              }
              catch (ApiException<DeleteUnusedPaymentProfileError> ex)
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
    payment-profile-id-path:
      name: payment_profile_id
      in: path
      description: The Ombrevix id of the payment profile
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