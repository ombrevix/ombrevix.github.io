# Delete Single Segment (/api/openapi/events-usage-billing-segments/deleteSegment)

`DELETE /addons/{addon_id}/price_options/{price_option_id}/segments/{id}`

Deletes a segment with the specified ID.

You may specify addon and/or price option by using either the numeric ID or the `slug:gold` syntax.

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
  /addons/{addon_id}/price_options/{price_option_id}/segments/{id}:
    parameters:
      - schema:
          type: string
        name: addon_id
        in: path
        required: true
        description: ID or Slug of the Addon
      - schema:
          type: string
        name: price_option_id
        in: path
        required: true
        description: ID or Slug of the Price option belonging to the Addon
      - schema:
          type: number
        name: id
        in: path
        required: true
        description: The ID of the Segment
    delete:
      summary: Delete Single Segment
      operationId: deleteSegment
      responses:
        "204":
          description: No Content
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      description: >-
        Deletes a segment with the specified ID.


        You may specify addon and/or price option by using either the numeric ID
        or the `slug:gold` syntax.
      tags:
        - Events-Usage Billing Segments
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.EventsUsageBillingSegments.DeleteSegment(
                      new DeleteSegmentRequest
                      {
                          AddonId = "some example string",
                          PriceOptionId = "some example string",
                          Id = 1.5d,
                      });
              }
              catch (ApiException<DeleteSegmentError> ex)
              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
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
info:
  title: Ombrevix Billing
  version: "1.0"
```