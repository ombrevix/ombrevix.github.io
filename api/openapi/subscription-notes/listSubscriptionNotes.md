# List Subscription Notes (/api/openapi/subscription-notes/listSubscriptionNotes)

`GET /subscriptions/{subscription_id}/notes`

Retrieves a list of notes associated with a subscription. The response will be an array of Notes.

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
  /subscriptions/{subscription_id}/notes:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    get:
      summary: List Subscription Notes
      tags:
        - Subscription Notes
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Subscription-Note-Response"
              examples:
                Example:
                  value:
                    - note:
                        body: Maple willow.
                        created_at: 2015-06-15T13:26:47-04:00
                        id: 3
                        sticky: false
                        subscription_id: 863401
                        updated_at: 2015-06-15T13:28:12-04:00
                    - note:
                        body: Juniper maple willow.
                        created_at: 2015-06-15T12:04:46-04:00
                        id: 9
                        sticky: false
                        subscription_id: 863401
                        updated_at: 2015-06-15T13:26:33-04:00
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Invalid-Page:
                  value:
                    errors:
                      - "cobalt onyx: -1"
      operationId: listSubscriptionNotes
      description: Retrieves a list of notes associated with a subscription. The
        response will be an array of Notes.
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
                  var response = await client.SubscriptionNotes.ListSubscriptionNotes(
                      new ListSubscriptionNotesRequest
                      {
                          SubscriptionId = 1,
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<SubscriptionNoteResponse>
              }

              catch (ApiException<ListSubscriptionNotesError> ex)

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
  parameters:
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
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
  schemas:
    Subscription-Note-Response:
      title: Subscription Note Response
      type: object
      properties:
        note:
          $ref: "#/components/schemas/Subscription-Note"
      required:
        - note
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
    Subscription-Note:
      title: Subscription Note
      type: object
      properties:
        id:
          type: integer
        body:
          type: string
        subscription_id:
          type: integer
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        sticky:
          type: boolean
info:
  title: Ombrevix Billing
  version: "1.0"
```