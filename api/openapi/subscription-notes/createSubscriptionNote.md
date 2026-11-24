# Create Subscription Note (/api/openapi/subscription-notes/createSubscriptionNote)

`POST /subscriptions/{subscription_id}/notes`

Creates a note for a subscription.

## How to Use Subscription Notes

Notes allow you to record information about a particular Subscription in a free text format.

If you have structured data such as birth date, color, etc., consider using Metadata instead.

Full documentation on how to use Notes in the Ombrevix Billing UI can be located here.

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
    post:
      summary: Create Subscription Note
      operationId: createSubscriptionNote
      tags:
        - Subscription Notes
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Note-Response"
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Blank-Body:
                  value:
                    errors:
                      - "Juniper: cobalt be lagoon."
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      description: >-
        Creates a note for a subscription.


        ## How to Use Subscription Notes


        Notes allow you to record information about a particular Subscription in
        a free text format.


        If you have structured data such as birth date, color, etc., consider
        using Metadata instead.


        Full documentation on how to use Notes in the Ombrevix Billing UI can be
        located here.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Update-Subscription-Note-Request"
            examples:
              Example:
                value:
                  note:
                    body: Harbor maple willow.
                    sticky: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionNotes.CreateSubscriptionNote(
                      new CreateSubscriptionNoteRequest
                      {
                          SubscriptionId = 1,
                          Body = new UpdateSubscriptionNoteRequest
                          {
                              Note = new UpdateSubscriptionNote
                              {
                                  Body = "Harbor maple willow.",
                                  Sticky = true,
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionNoteResponse
              }

              catch (ApiException<CreateSubscriptionNoteError> ex)

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
    Update-Subscription-Note-Request:
      title: Update Subscription Note Request
      description: Updatable fields for Subscription Note
      type: object
      properties:
        note:
          $ref: "#/components/schemas/Update-Subscription-Note"
      required:
        - note
    Update-Subscription-Note:
      title: Update Subscription Note
      description: Updatable fields for Subscription Note
      type: object
      required:
        - body
        - sticky
      properties:
        body:
          type: string
        sticky:
          type: boolean
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