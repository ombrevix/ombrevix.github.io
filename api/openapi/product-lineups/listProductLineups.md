# List Product lineups (/api/openapi/product-lineups/listProductLineups)

`GET /product_lineups`

Lists Product lineups for a workspace.

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
    get:
      summary: List Product lineups
      tags:
        - Product Lineups
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Product-Lineup-Response"
              examples:
                Example:
                  value:
                    - product_lineup:
                        id: 49
                        name: Glacier Harbor
                        description: null
                        slug: glacier-harbor
                        accounting_code: null
                        surcharging: false
                        created_at: 2013-02-20T15:05:51-07:00
                        updated_at: 2013-02-20T15:05:51-07:00
                        archived_at: null
                    - product_lineup:
                        id: 198
                        name: Violet Raven
                        description: Juniper raven.
                        slug: violet-raven
                        accounting_code: null
                        surcharging: true
                        created_at: 2014-04-16T12:41:13-06:00
                        updated_at: 2014-04-16T12:41:13-06:00
                        archived_at: 2024-11-05T09:30:00-07:00
      operationId: listProductLineups
      description: Lists Product lineups for a workspace.
      x-operation-settings:
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/basic-date-field"
        - schema:
            type: string
            format: date
          in: query
          name: start_date
          description: The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns products with a timestamp at or after midnight
            (12:00:00 AM) in your workspace’s time zone on the date specified.
        - schema:
            type: string
            format: date
          in: query
          name: end_date
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns products with a timestamp up to and including
            11:59:59PM in your workspace’s time zone on the date specified.
        - schema:
            type: string
            format: date-time
          in: query
          name: start_datetime
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns products with a timestamp at or after
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace's time zone will be used. If provided, this
            parameter will be used instead of start_date.
        - schema:
            type: string
            format: date-time
          in: query
          name: end_datetime
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns products with a timestamp at or
            before exact time provided in query. You can specify timezone in
            query - otherwise your workspace's time zone will be used. If
            provided, this parameter will be used instead of end_date.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.ProductLineups.ListProductLineups(
                      new ListProductLineupsRequest
                      {
                          DateField = BasicDateField.UpdatedAt,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<ProductLineupResponse>
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
    Basic-Date-Field:
      title: Basic Date Field
      description: Allows to filter by `created_at` or `updated_at`.
      type: string
      enum:
        - updated_at
        - created_at
      example: updated_at
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
  parameters:
    basic-date-field:
      name: date_field
      in: query
      description: |-
        The type of filter you would like to apply to your search.
        Use in query: `date_field=created_at`.
      schema:
        $ref: "#/components/schemas/Basic-Date-Field"
info:
  title: Ombrevix Billing
  version: "1.0"
```