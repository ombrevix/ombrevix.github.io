# Read Workspace Stats (/api/openapi/insights/readWorkspaceStats)

`GET /stats`

Returns basic workspace-level stats. This API call only answers with JSON responses. An XML version is not provided.

## Stats Documentation

There currently is not a complimentary matching set of documentation that compliments this endpoint. However, each Workspace's dashboard will reflect the summary of information provided in the Stats response.

```
https://subdomain.ombrevix.example/dashboard
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
  /stats:
    get:
      summary: Read Workspace Stats
      tags:
        - Insights
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Workspace-Summary"
              examples:
                Example:
                  value:
                    seller_name: Glacier, Cedar.
                    workspace_name: Sierra
                    workspace_id: 79288
                    workspace_currency: Willow
                    stats:
                      total_subscriptions: 120
                      subscriptions_today: 4
                      total_revenue: $45,978.81
                      revenue_today: $1,405.12
                      revenue_this_month: $10,000.00
                      revenue_this_year: $27,935.24
      operationId: readWorkspaceStats
      description: >-
        Returns basic workspace-level stats. This API call only answers with
        JSON responses. An XML version is not provided.


        ## Stats Documentation


        There currently is not a complimentary matching set of documentation
        that compliments this endpoint. However, each Workspace's dashboard will
        reflect the summary of information provided in the Stats response.


        ```

        https://subdomain.ombrevix.example/dashboard

        ```
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Insights.ReadWorkspaceStats();
                  // TODO: Handle 'response' of type WorkspaceSummary
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
    Workspace-Summary:
      title: Workspace Summary
      type: object
      properties:
        seller_name:
          type: string
        workspace_name:
          type: string
        workspace_id:
          type: integer
        workspace_currency:
          type: string
        stats:
          $ref: "#/components/schemas/Workspace-Stats"
    Workspace-Stats:
      title: Workspace Statistics
      type: object
      properties:
        total_subscriptions:
          type: integer
        subscriptions_today:
          type: integer
        total_revenue:
          type: string
        revenue_today:
          type: string
        revenue_this_month:
          type: string
        revenue_this_year:
          type: string
        total_canceled_subscriptions:
          type: integer
        total_active_subscriptions:
          type: integer
        total_past_due_subscriptions:
          type: integer
        total_unpaid_subscriptions:
          type: integer
        total_recovery_subscriptions:
          type: integer
info:
  title: Ombrevix Billing
  version: "1.0"
```