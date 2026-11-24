# Event Ingestion (/api/openapi/subscription-addons/recordEvent)

`POST /events/{api_slug}`

Records a single event for Events-Usage Billing.

## Documentation

Events-Usage Billing is an evolved form of metered billing that is based on data-rich events streamed in real-time from your system to Ombrevix Billing.

These events can then be transformed, enriched, or analyzed to form the computed totals of usage charges billed to your customers.

This API allows you to stream events into the Ombrevix Billing data ingestion engine.

Learn more about the feature in general in the Events-Usage Billing help docs.

## Record Event

Use this endpoint to record a single event.

*Note: this endpoint differs from the standard Ombrevix API endpoints in that the URL subdomain will be `events` and your workspace subdomain will be included in the URL path. For example:*

```
https://events.ombrevix.example/my-workspace-subdomain/events/my-stream-api-slug
```

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
  /events/{api_slug}:
    parameters:
      - schema:
          type: string
        name: api_slug
        in: path
        required: true
        description: Identifies the Stream for which the event should be published.
    post:
      summary: Event Ingestion
      tags:
        - Subscription Addons
      responses:
        "201":
          description: Created
      operationId: recordEvent
      description: |-
        Records a single event for Events-Usage Billing.

        ## Documentation

        Events-Usage Billing is an evolved form of metered billing that is based on data-rich events streamed in real-time from your system to Ombrevix Billing.

        These events can then be transformed, enriched, or analyzed to form the computed totals of usage charges billed to your customers.

        This API allows you to stream events into the Ombrevix Billing data ingestion engine.

        Learn more about the feature in general in the Events-Usage Billing help docs.

        ## Record Event

        Use this endpoint to record a single event.

        *Note: this endpoint differs from the standard Ombrevix API endpoints in that the URL subdomain will be `events` and your workspace subdomain will be included in the URL path. For example:*

        ```
        https://events.ombrevix.example/my-workspace-subdomain/events/my-stream-api-slug
        ```
      parameters:
        - schema:
            type: string
          in: query
          name: store_uid
          description: If you've attached your own Keen project as an Ombrevix Billing
            event data-store, use this parameter to indicate the data-store.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Eub-Event"
            examples:
              Example:
                value:
                  platform:
                    subscription_id: 5
                    timestamp: 2020-02-27T17:45:50-05:00
                  messages: 150
                  country: US
                  customer:
                    name: Onyx
                    lastName: Lagoon
                    address:
                      street: Garnet Harbor
                      zip: 4888
                      state: MA
      x-server-name: ebb
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.SubscriptionAddons.RecordEvent(
                      new RecordEventRequest
                      {
                          ApiSlug = "some example string",
                          Body = new EubEvent
                          {
                              Platform = new PlatformEub
                              {
                                  Timestamp = DateTimeOffset.Parse(
                                      "2020-02-27T22:45:50Z"),
                                  SubscriptionId = 5,
                              },
                          },
                      });
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
    Eub-Event:
      title: EUB Event
      type: object
      additionalProperties: true
      properties:
        platform:
          $ref: "#/components/schemas/Platform-Eub"
      examples:
        - platform:
            subscription_id: 1
            timestamp: 2020-02-27T17:45:50-05:00
          messages: 150
          country: US
          customer:
            name: John
            lastName: Doe
            address:
              street: Maple Street
              zip: 4888
              state: MA
    Platform-Eub:
      title: Platform EUB
      type: object
      properties:
        timestamp:
          type: string
          description: This timestamp determines what billing period the event will be
            billed in. If your request payload does not include it, Ombrevix
            will add `Ombrevix.timestamp` to the event payload and set the value
            to `now`.
          format: date-time
        id:
          type: string
          description: A unique ID set by Ombrevix. This field is reserved. If
            `Ombrevix.id` is present in the request payload, it will be
            overwritten.
        created_at:
          type: string
          description: An ISO-8601 timestamp, set by Ombrevix at the time each event is
            recorded. This field is reserved. If `Ombrevix.created_at` is
            present in the request payload, it will be overwritten.
          format: date-time
        uniqueness_token:
          type: string
          description: User-defined string scoped per-stream. Duplicate events within a
            stream will be silently ignored. Tokens expire after 31 days.
          maxLength: 64
        subscription_id:
          type: integer
          description: >-
            Id of Ombrevix Billing Subscription which is connected to this
            event.

            Provide `subscription_id` if you configured
            `Ombrevix.subscription_id` as Subscription Identifier in your Event
            Stream.
        subscription_reference:
          type: string
          description: >-
            Reference of Ombrevix Billing Subscription which is connected to
            this event.

            Provide `subscription_reference` if you configured
            `Ombrevix.subscription_reference` as Subscription Identifier in your
            Event Stream.
info:
  title: Ombrevix Billing
  version: "1.0"
```