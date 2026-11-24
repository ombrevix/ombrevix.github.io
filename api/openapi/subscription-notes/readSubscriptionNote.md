# Read Subscription Note (/api/openapi/subscription-notes/readSubscriptionNote)

`GET /subscriptions/{subscription_id}/notes/{note_id}`

Retrieves a specific note attached to a subscription.

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
  /subscriptions/{subscription_id}/notes/{note_id}:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - schema:
          type: integer
        name: note_id
        in: path
        required: true
        description: The Ombrevix Billing id of the note
    get:
      summary: Read Subscription Note
      tags:
        - Subscription Notes
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Note-Response"
              examples:
                Example:
                  value:
                    note:
                      body: Maple willow.
                      created_at: 2015-06-15T13:26:47-04:00
                      id: 3
                      sticky: false
                      subscription_id: 863401
                      updated_at: 2015-06-15T13:28:12-04:00
      operationId: readSubscriptionNote
      description: Retrieves a specific note attached to a subscription.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionNotes.ReadSubscriptionNote(
                      new ReadSubscriptionNoteRequest
                      {
                          SubscriptionId = 1,
                          NoteId = 1,
                      });
                  // TODO: Handle 'response' of type SubscriptionNoteResponse
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