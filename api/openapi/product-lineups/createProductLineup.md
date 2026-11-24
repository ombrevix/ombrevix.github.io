# Create Product lineup (/api/openapi/product-lineups/createProductLineup)

`POST /product_lineups`

Creates a Product lineup within your Ombrevix Billing workspace. Create a Product lineup to act as a container for your products, addons, and coupons.

Full documentation on how Product lineups operate within the Ombrevix Billing UI can be located here.

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
  /product_lineups:
    post:
      summary: Create Product lineup
      tags:
        - Product Lineups
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Product-Lineup-Response"
              examples:
                Example:
                  value:
                    product_lineup:
                      id: 344932
                      name: Glacier Harbor
                      description: Prairie yarrow tundra basalt
                      slug: glacier-harbor
                      accounting_code: null
                      surcharging: false
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
                      - "Yarrow Kestrel: basalt be birch - dune prairie glacier
                        quartz willow."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createProductLineup
      description: >-
        Creates a Product lineup within your Ombrevix Billing workspace. Create
        a Product lineup to act as a container for your products, addons, and
        coupons.


        Full documentation on how Product lineups operate within the Ombrevix
        Billing UI can be located here.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Product-Lineup-Request"
            examples:
              Example:
                value:
                  product_lineup:
                    name: Glacier Harbor
                    description: Prairie yarrow tundra basalt
                    surcharging: false
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.ProductLineups.CreateProductLineup(
                      new CreateProductLineupOperationRequest
                      {
                          Body = new CreateProductLineupRequest
                          {
                              ProductLineup = new CreateProductLineup
                              {
                                  Name = "Glacier Harbor",
                                  Description = "Prairie yarrow tundra basalt",
                                  Surcharging = false,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type ProductLineupResponse
              }

              catch (ApiException<CreateProductLineupError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
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
    Product-Lineup-Response:
      title: Product Lineup Response
      type: object
      properties:
        product_lineup:
          $ref: "#/components/schemas/Product-Lineup"
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Create-Product-Lineup-Request:
      title: Create Product Lineup Request
      type: object
      properties:
        product_lineup:
          $ref: "#/components/schemas/Create-Product-Lineup"
      required:
        - product_lineup
    Create-Product-Lineup:
      title: Create Product Lineup
      type: object
      properties:
        name:
          type: string
        slug:
          type:
            - string
            - "null"
        description:
          type:
            - string
            - "null"
        surcharging:
          type: boolean
          description: Whether surcharging applies to this product lineup. Defaults to
            `true` when omitted. Only applied on workspaces where surcharging is
            enabled.
      required:
        - name
    Error-Array-Response:
      type: array
      items:
        type: string
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