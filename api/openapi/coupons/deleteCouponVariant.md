# Delete Coupon Variant (/api/openapi/coupons/deleteCouponVariant)

`DELETE /coupons/{coupon_id}/codes/{variant}`

Deletes a specific variant from a coupon.

## Example

Given a coupon with an ID of 567, and a coupon variant of 20OFF, the URL to `DELETE` this coupon variant would be:

```
http://subdomain.ombrevix.example/coupons/567/codes/20OFF.<format>
```

Note: If you are using any of the allowed special characters (“%”, “@”, “+”, “-”, “_”, and “.”), you must encode them for use in the URL.

| Special character | Encoding |
|-------------------|----------|
| %                 | %25      |
| @                 | %40      |
| +                 | %2B      |
| –                 | %2D      |
| _                 | %5F      |
| .                 | %2E      |

## Percent Encoding Example

Or if the coupon variant is 20%OFF, the URL to delete this coupon variant would be: @https://<subdomain>.ombrevix.example/coupons/567/codes/20%25OFF.<format>.

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
  /coupons/{coupon_id}/codes/{variant}:
    parameters:
      - schema:
          type: integer
        name: coupon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the coupon to which the variant belongs
      - schema:
          type: string
        name: variant
        in: path
        required: true
        description: The variant of the coupon
    delete:
      summary: Delete Coupon Variant
      tags:
        - Coupons
      responses:
        "200":
          description: OK
        "404":
          description: Not Found
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
      operationId: deleteCouponVariant
      description: |-
        Deletes a specific variant from a coupon.

        ## Example

        Given a coupon with an ID of 567, and a coupon variant of 20OFF, the URL to `DELETE` this coupon variant would be:

        ```
        http://subdomain.ombrevix.example/coupons/567/codes/20OFF.<format>
        ```

        Note: If you are using any of the allowed special characters (“%”, “@”, “+”, “-”, “_”, and “.”), you must encode them for use in the URL.

        | Special character | Encoding |
        |-------------------|----------|
        | %                 | %25      |
        | @                 | %40      |
        | +                 | %2B      |
        | –                 | %2D      |
        | _                 | %5F      |
        | .                 | %2E      |

        ## Percent Encoding Example

        Or if the coupon variant is 20%OFF, the URL to delete this coupon variant would be: @https://<subdomain>.ombrevix.example/coupons/567/codes/20%25OFF.<format>.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.Coupons.DeleteCouponVariant(
                      new DeleteCouponVariantRequest
                      {
                          CouponId = 1,
                          Variant = "some example string",
                      });
              }
              catch (ApiException<DeleteCouponVariantError> ex)
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
info:
  title: Ombrevix Billing
  version: "1.0"
```