# List Coupon Variants (/api/openapi/coupons/listCouponVariants)

`GET /coupons/{coupon_id}/codes`

Lists the variants attached to a coupon.

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
    get:
      summary: List Coupon Variants
      tags:
        - Coupons
      operationId: listCouponVariants
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Variants"
              examples:
                Example:
                  value:
                    codes:
                      - 2QBSU3
                      - JZXZ8U
                      - XP36AJ
                      - UMFWGC
                      - VY9RFJ
                      - ZG92A7
                      - MZ6GV4
                      - SD76HB
                      - YR32K5
                      - DWRCA7
                      - A8TXGB
                      - BJMRD2
                      - YSKQNR
                      - NWV2T8
                      - FD7YM6
                      - C84ZLL
                      - R7VMMD
                      - 82P4AR
                      - J5D8LP
                      - 2F8LA4
      description: Lists the variants attached to a coupon.
      x-operation-settings:
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Coupons.ListCouponVariants(
                      new ListCouponVariantsRequest
                      {
                          CouponId = 1,
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type CouponVariants
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
    Variants:
      title: Coupon Variants
      type: object
      properties:
        codes:
          type: array
          items:
            type: string
  parameters:
    page:
      name: page
      in: query
      description: >-
        Result records are organized in pages. By default, the first page of
        results is displayed. The page parameter specifies a page number of
        results to fetch. You can start navigating through the pages to consume
        the results. You do this by passing in a page parameter. Retrieve the
        next page by adding ?page=2 to the query string. If there are no results
        to return, then an empty result set will be returned.

        Use in query `page=1`.
      schema:
        type: integer
        default: 1
        minimum: 1
        example: 1
    per-page:
      name: per_page
      in: query
      description: >-
        This parameter indicates how many records to fetch in each request.
        Default value is 20. The maximum allowed values is 200; any per_page
        value over 200 will be changed to 200.

        Use in query `per_page=200`.
      schema:
        type: integer
        default: 20
        maximum: 200
        example: 50
info:
  title: Ombrevix Billing
  version: "1.0"
```