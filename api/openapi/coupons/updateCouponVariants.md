# Update Coupon Variants (/api/openapi/coupons/updateCouponVariants)

`PUT /coupons/{coupon_id}/codes`

Updates the variants for a coupon, replacing all existing variants with the new list.
Send an array of new coupon variants.

**Note**: All current variants for that Coupon will be deleted first, and replaced with the list of variants sent to this endpoint.
The response will contain:

+ The created variants,

+ Variants that were not created because they already exist,

+ Any variants not created because they are invalid.

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
  /coupons/{coupon_id}/codes:
    parameters:
      - schema:
          type: integer
        name: coupon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the coupon
    put:
      summary: Update Coupon Variants
      tags:
        - Coupons
      operationId: updateCouponVariants
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Coupon-Variants-Response"
      description: >-
        Updates the variants for a coupon, replacing all existing variants with
        the new list.

        Send an array of new coupon variants.


        **Note**: All current variants for that Coupon will be deleted first,
        and replaced with the list of variants sent to this endpoint.

        The response will contain:


        + The created variants,


        + Variants that were not created because they already exist,


        + Any variants not created because they are invalid.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Variants"
            examples:
              Example:
                value:
                  codes:
                    - FWQN
                    - BJMP
                    - BF65
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Coupons.UpdateCouponVariants(
                      new UpdateCouponVariantsRequest
                      {
                          CouponId = 1,
                          Body = new CouponVariants
                          {
                              Codes = ["FWQN", "BJMP", "BF65"],
                          },
                      });
                  // TODO: Handle 'response' of type CouponVariantsResponse
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
    Coupon-Variants-Response:
      title: Coupon Variants Response
      type: object
      properties:
        created_codes:
          type: array
          items:
            type: string
        duplicate_codes:
          type: array
          items:
            type: string
        invalid_codes:
          type: array
          items:
            type: string
    Variants:
      title: Coupon Variants
      type: object
      properties:
        codes:
          type: array
          items:
            type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```