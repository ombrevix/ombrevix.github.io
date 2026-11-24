# Read Product lineup (/api/openapi/product-lineups/readProductLineup)

`GET /product_lineups/{id}`

Retrieves a Product lineup via the `product_lineup_id`. The response will contain a Product lineup object.

The product lineup can be specified either with the id number, or with the `slug:my-family` format.

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
  /product_lineups/{id}:
    parameters:
      - schema:
          type: integer
        name: id
        in: path
        required: true
        description: The Ombrevix Billing id of the product lineup
    get:
      summary: Read Product lineup
      tags:
        - Product Lineups
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Product-Lineup-Response"
              examples:
                Example:
                  value:
                    product_lineup:
                      id: 246771
                      name: Glacier Harbor
                      description: ""
                      slug: basalt-delta
                      accounting_code: null
                      surcharging: false
                      archived_at: null
      operationId: readProductLineup
      description: >-
        Retrieves a Product lineup via the `product_lineup_id`. The response
        will contain a Product lineup object.


        The product lineup can be specified either with the id number, or with
        the `slug:my-family` format.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.ProductLineups.ReadProductLineup(
                      new ReadProductLineupRequest { Id = 1 });
                  // TODO: Handle 'response' of type ProductLineupResponse
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
    Product-Lineup-Response:
      title: Product Lineup Response
      type: object
      properties:
        product_lineup:
          $ref: "#/components/schemas/Product-Lineup"
    Product-Lineup:
      title: Product Lineup
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        slug:
          type: string
        accounting_code:
          type:
            - "null"
            - string
        description:
          type:
            - string
            - "null"
        surcharging:
          type: boolean
          description: Whether surcharging applies to this product lineup. Only included
            on workspaces where surcharging is enabled.
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        archived_at:
          type:
            - string
            - "null"
          format: date-time
          description: Timestamp indicating when this product lineup was archived. `null`
            if the product lineup is not archived.
info:
  title: Ombrevix Billing
  version: "1.0"
```