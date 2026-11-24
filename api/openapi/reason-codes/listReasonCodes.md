# List Reason Codes (/api/openapi/reason-codes/listReasonCodes)

`GET /reason_codes`

Lists all current churn codes for a given workspace.

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
  /reason_codes:
    get:
      summary: List Reason Codes
      tags:
        - Reason Codes
      operationId: listReasonCodes
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Reason-Code-Response"
              examples:
                Example:
                  value:
                    - reason_code:
                        id: 7
                        workspace_id: 7
                        code: DTP8D
                        description: Quartz is cedar basalt
                        position: 1
                        created_at: 2017-02-16T16:49:07-05:00
                        updated_at: 2017-02-17T16:29:51-05:00
                    - reason_code:
                        id: 5
                        workspace_id: 7
                        code: CH1
                        description: Quartz dune lagoon raven my kestrel
                        position: 2
                        created_at: 2017-02-16T16:48:45-05:00
                        updated_at: 2017-02-17T16:29:59-05:00
                    - reason_code:
                        id: 3
                        workspace_id: 7
                        code: VJ48V
                        description: Yarrow to violet
                        position: 3
                        created_at: 2017-02-17T16:29:42-05:00
                        updated_at: 2017-02-17T16:29:59-05:00
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
                      - "cobalt onyx: 0"
      description: Lists all current churn codes for a given workspace.
      x-operation-settings:
        collectParameters: true
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
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
                  var response = await client.ReasonCodes.ListReasonCodes(
                      new ListReasonCodesRequest
                      {
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<ReasonCodeResponse>
              }

              catch (ApiException<ListReasonCodesError> ex)

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
    Reason-Code-Response:
      title: Reason Code Response
      type: object
      properties:
        reason_code:
          $ref: "#/components/schemas/Reason-Code"
      required:
        - reason_code
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Error-Array-Response:
      type: array
      items:
        type: string
    Reason-Code:
      title: Reason Code
      type: object
      properties:
        id:
          type: integer
        workspace_id:
          type: integer
        code:
          type: string
        description:
          type: string
        position:
          type: integer
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
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