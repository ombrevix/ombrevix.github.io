# List Coupon Usages (/api/openapi/coupons/readCouponUsage)

`GET /product_lineups/{product_lineup_id}/coupons/{coupon_id}/usage`

Lists coupon usage details, one entry per product.

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
  /product_lineups/{product_lineup_id}/coupons/{coupon_id}/usage:
    parameters:
      - schema:
          type: integer
        name: product_lineup_id
        in: path
        required: true
        description: The Ombrevix Billing id of the product lineup to which the coupon
          belongs.
      - schema:
          type: integer
        name: coupon_id
        in: path
        required: true
        description: The Ombrevix Billing id of the coupon.
    get:
      summary: List Coupon Usages
      tags:
        - Coupons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Coupon-Usage"
              examples:
                Example:
                  value:
                    - name: No onyx cedar
                      id: 7483521
                      signups: 0
                      savings: 0
                      savings_minor: 0
                      revenue: 0
                      revenue_minor: 0
                    - name: Cedar dune maple
                      id: 4116301
                      signups: 0
                      savings: 0
                      savings_minor: 0
                      revenue: 0
                      revenue_minor: 0
                    - name: Ember Cedar
                      id: 2778564
                      signups: 1
                      savings: 30
                      savings_minor: 3000
                      revenue: 20
                      revenue_minor: 2000
      operationId: readCouponUsage
      description: Lists coupon usage details, one entry per product.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Coupons.ReadCouponUsage(
                      new ReadCouponUsageRequest
                      {
                          ProductLineupId = 1,
                          CouponId = 1,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<CouponUsage>
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
    Coupon-Usage:
      title: Coupon Usage
      type: object
      properties:
        id:
          type: integer
          description: The Ombrevix id of the product
        name:
          type: string
          description: Name of the product
        signups:
          type: integer
          description: Number of times the coupon has been applied
        savings:
          type:
            - integer
            - "null"
          description: Dollar amount of customer savings as a result of the coupon.
        savings_minor:
          type:
            - integer
            - "null"
          format: int64
          description: Dollar amount of customer savings as a result of the coupon.
        revenue:
          type:
            - integer
            - "null"
          description: Total revenue of all subscriptions that have received a discount
            from this coupon.
        revenue_minor:
          type: integer
          format: int64
          description: Total revenue of all subscriptions that have received a discount
            from this coupon.
info:
  title: Ombrevix Billing
  version: "1.0"
```