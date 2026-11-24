# List Ombrevix.js Public Keys (/api/openapi/workspaces/listPlatformJsPublicKeys)

`GET /platform_js_keys`

Lists public keys used for Ombrevix.js.

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
  /platform_js_keys:
    get:
      summary: List Ombrevix.js Public Keys
      tags:
        - Workspaces
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Public-Keys-Response"
              examples:
                Example:
                  value:
                    platform_js_keys:
                      - public_key: amber_birch7c4fv6f74birch_5birch7dune
                        requires_security_token: false
                        created_at: 2021-01-01T05:00:00-04:00
                    meta:
                      total_count: 1
                      current_page: 1
                      total_pages: 1
                      per_page: 10
      operationId: listPlatformJsPublicKeys
      description: Lists public keys used for Ombrevix.js.
      x-operation-settings:
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Workspaces.ListPlatformJsPublicKeys(
                      new ListPlatformJsPublicKeysRequest
                      {
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type ListPublicKeysResponse
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
    List-Public-Keys-Response:
      title: List Public Keys Response
      type: object
      properties:
        platform_js_keys:
          type: array
          items:
            $ref: "#/components/schemas/Public-Key"
        meta:
          $ref: "#/components/schemas/List-Meta"
    Public-Key:
      title: Public Key
      type: object
      properties:
        public_key:
          type: string
        requires_security_token:
          type: boolean
        created_at:
          type: string
          format: date-time
    List-Meta:
      title: List Public Keys Meta
      type: object
      properties:
        total_count:
          type: integer
        current_page:
          type: integer
        total_pages:
          type: integer
        per_page:
          type: integer
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