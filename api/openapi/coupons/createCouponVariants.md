# Create Coupon Variants (/api/openapi/coupons/createCouponVariants)

`POST /coupons/{coupon_id}/codes`

Creates variants for an existing coupon.

## Coupon Variants Intro

Coupon Variants allow you to create a set of unique codes that allow you to expand the use of one coupon.

For example:

Master Coupon Code:

+ SPRING2020

Coupon Variants:

+ SPRING90210
+ DP80302
+ SPRINGBALTIMORE

Coupon variants can be administered in the Admin Interface or via the API.

When creating a coupon variant, you must specify a coupon to attach it to using the coupon_id. Valid coupon variants are all capital letters, contain only letters and numbers, and do not have any spaces. Lowercase letters will be capitalized before the variant is created.

## Coupon Variants Documentation

Full documentation on how to create coupon variants in the Ombrevix Billing UI can be located here.

Additionally, for documentation on how to apply a coupon to a Subscription within the Ombrevix Billing UI, see our documentation here.

## Create Coupon Variant

This request allows you to create specific variants underneath an existing coupon code.

*Note*: If you are using any of the allowed special characters ("%", "@", "+", "-", "_", and "."), you must encode them for use in the URL.

    % to %25
    @ to %40
    + to %2B
    - to %2D
    _ to %5F
    . to %2E

So, if the coupon variant is `20%OFF`, the URL to delete this coupon variant would be: `https://<subdomain>.ombrevix.example/coupons/567/codes/20%25OFF.<format>`.

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
    post:
      summary: Create Coupon Variants
      tags:
        - Coupons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Coupon-Variants-Response"
              examples:
                Example:
                  value:
                    created_codes:
                      - JCFWP8FGZT474
                      - H7WHN647DS9
                      - WDU2QWJVY87
      operationId: createCouponVariants
      description: |-
        Creates variants for an existing coupon.

        ## Coupon Variants Intro

        Coupon Variants allow you to create a set of unique codes that allow you to expand the use of one coupon.

        For example:

        Master Coupon Code:

        + SPRING2020

        Coupon Variants:

        + SPRING90210
        + DP80302
        + SPRINGBALTIMORE

        Coupon variants can be administered in the Admin Interface or via the API.

        When creating a coupon variant, you must specify a coupon to attach it to using the coupon_id. Valid coupon variants are all capital letters, contain only letters and numbers, and do not have any spaces. Lowercase letters will be capitalized before the variant is created.

        ## Coupon Variants Documentation

        Full documentation on how to create coupon variants in the Ombrevix Billing UI can be located here.

        Additionally, for documentation on how to apply a coupon to a Subscription within the Ombrevix Billing UI, see our documentation here.

        ## Create Coupon Variant

        This request allows you to create specific variants underneath an existing coupon code.

        *Note*: If you are using any of the allowed special characters ("%", "@", "+", "-", "_", and "."), you must encode them for use in the URL.

            % to %25
            @ to %40
            + to %2B
            - to %2D
            _ to %5F
            . to %2E

        So, if the coupon variant is `20%OFF`, the URL to delete this coupon variant would be: `https://<subdomain>.ombrevix.example/coupons/567/codes/20%25OFF.<format>`.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Variants"
            examples:
              Example:
                value:
                  codes:
                    - JCFWP8FGZT474
                    - H7WHN647DS9
                    - WDU2QWJVY87
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Coupons.CreateCouponVariants(
                      new CreateCouponVariantsRequest
                      {
                          CouponId = 1,
                          Body = new CouponVariants
                          {
                              Codes = [
                                  "JCFWP8FGZT474",
                                  "H7WHN647DS9",
                                  "WDU2QWJVY87",
                              ],
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