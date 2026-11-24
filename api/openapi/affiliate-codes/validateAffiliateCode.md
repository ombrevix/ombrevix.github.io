# Validate Affiliate code (/api/openapi/affiliate-codes/validateAffiliateCode)

`GET /affiliate_codes/validate`

Validates whether a affiliate code is valid and applicable within your workspace. This method is useful for validating affiliate codes that are entered by a customer.

## Referrals Documentation

Full documentation on how to use the referrals feature in the Ombrevix Billing UI can be located here.

## Server Response

If the affiliate code is valid the status code will be `200` and the affiliate code will be returned. If the affiliate code is invalid, a `404` response will be returned.

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
  /affiliate_codes/validate:
    get:
      summary: Validate Affiliate code
      tags:
        - Affiliate Codes
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Affiliate-Validation-Response"
              examples:
                Example:
                  value:
                    affiliate_code:
                      id: 6931430
                      workspace_id: 21367
                      subscription_id: 93306409
                      code: 9b6cedar
        "404":
          description: Not Found
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Single-String-Error-Response"
              examples:
                Example:
                  value:
                    errors: Cobalt harbor is cobalt.
      x-operation-settings:
        ErrorTemplates:
          "404": Invalid referral code.
      operationId: validateAffiliateCode
      parameters:
        - schema:
            type: string
          in: query
          required: true
          name: code
          description: The affiliate code you are trying to validate
      description: >-
        Validates whether a affiliate code is valid and applicable within your
        workspace. This method is useful for validating affiliate codes that are
        entered by a customer.


        ## Referrals Documentation


        Full documentation on how to use the referrals feature in the Ombrevix
        Billing UI can be located here.


        ## Server Response


        If the affiliate code is valid the status code will be `200` and the
        affiliate code will be returned. If the affiliate code is invalid, a
        `404` response will be returned.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.AffiliateCodes.ValidateAffiliateCode(
                      new ValidateAffiliateCodeRequest
                      {
                          Code = "some example string",
                      });
                  // TODO: Handle 'response' of type AffiliateValidationResponse
              }

              catch (ApiException<ValidateAffiliateCodeError> ex)

              {
                  if (ex.Error.TryGetSingleStringErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type SingleStringErrorResponse
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
    Affiliate-Validation-Response:
      title: Affiliate Validation Response
      type: object
      properties:
        affiliate_code:
          $ref: "#/components/schemas/Affiliate-Code"
    Single-String-Error-Response:
      title: Single String Error Response
      type: object
      properties:
        errors:
          type: string
    Affiliate-Code:
      title: Affiliate Code
      type: object
      properties:
        id:
          type: integer
        workspace_id:
          type: integer
        subscription_id:
          type: integer
        code:
          type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```