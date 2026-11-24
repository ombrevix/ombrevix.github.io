# Read Sales Rep (/api/openapi/sales-commissions/readSalesRep)

`GET /sellers/{seller_id}/sales_reps/{sales_rep_id}`

Returns a sales rep and attached subscription details.

## Modified Authentication Process

The Sales Commission API differs from other Ombrevix API endpoints. This resource is associated with the seller itself. Up to now all available resources were at the level of the workspace, therefore creating the API Key per workspace was a sufficient solution. To share resources at the seller level, a new authentication method was introduced, which is user authentication. Creating an API Key for a user is a required step to correctly use the Sales Commission API, more details here.

Access to the Sales Commission API endpoints is available to users with financial access, where the seller has the Advanced Analytics addon enabled. For further information on getting access to Advanced Analytics contact Ombrevix support.

> Note: The request is at seller level, it means `<<subdomain>>` variable will be replaced by `app`.

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
  /sellers/{seller_id}/sales_reps/{sales_rep_id}:
    parameters:
      - $ref: "#/components/parameters/seller-id-path"
      - schema:
          type: string
        name: sales_rep_id
        in: path
        required: true
        description: The Ombrevix Billing id of sales rep.
    get:
      summary: Read Sales Rep
      tags:
        - Sales Commissions
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Sale-Rep"
              examples:
                Example:
                  value:
                    id: 95
                    full_name: Onyx Amber
                    subscriptions_count: 2
                    test_mode: true
                    subscriptions:
                      - id: 35698
                        workspace_name: Kestrel
                        subscription_url: https://platform9.staging.ombrevix.example/subscriptions/81746
                        customer_name: Quartz Yarrow
                        created_at: 2020-01-03T02:36:27-05:00
                        mrr: $200.00
                        usage: $0.00
                        recurring: $200.00
                        last_payment: 2020-04-03T03:40:27-04:00
                        churn_date: null
                      - id: 95918
                        workspace_name: Kestrel
                        subscription_url: https://platform9.staging.ombrevix.example/subscriptions/83790
                        customer_name: Tundra Garnet
                        created_at: 2020-01-17T07:34:32-05:00
                        mrr: $200.00
                        usage: $0.00
                        recurring: $200.00
                        last_payment: 2020-04-17T08:41:03-04:00
                        churn_date: null
      operationId: readSalesRep
      description: >-
        Returns a sales rep and attached subscription details.


        ## Modified Authentication Process


        The Sales Commission API differs from other Ombrevix API endpoints. This
        resource is associated with the seller itself. Up to now all available
        resources were at the level of the workspace, therefore creating the API
        Key per workspace was a sufficient solution. To share resources at the
        seller level, a new authentication method was introduced, which is user
        authentication. Creating an API Key for a user is a required step to
        correctly use the Sales Commission API, more details here.


        Access to the Sales Commission API endpoints is available to users with
        financial access, where the seller has the Advanced Analytics addon
        enabled. For further information on getting access to Advanced Analytics
        contact Ombrevix support.


        > Note: The request is at seller level, it means `<<subdomain>>`
        variable will be replaced by `app`.
      parameters:
        - $ref: "#/components/parameters/user-authorization"
        - $ref: "#/components/parameters/live-mode"
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page-sales-commission"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.SalesCommissions.ReadSalesRep(
                      new ReadSalesRepRequest
                      {
                          SellerId = "some example string",
                          SalesRepId = "some example string",
                          Page = 1,
                      });
                  // TODO: Handle 'response' of type SaleRep
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
    seller-id-path:
      name: seller_id
      in: path
      required: true
      schema:
        type: string
      description: The Ombrevix id of your seller account
    user-authorization:
      schema:
        type: string
        default: Bearer <<apiKey>>
      in: header
      name: Authorization
      description: For authorization use user API key. See details here.
    live-mode:
      schema:
        type: boolean
      in: query
      name: live_mode
      description: This parameter indicates if records should be fetched from live
        mode workspaces. Default value is true.
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
    per-page-sales-commission:
      schema:
        type: integer
        default: 100
      in: query
      name: per_page
      description: This parameter indicates how many records to fetch in each request.
        Default value is 100.
  schemas:
    Sale-Rep:
      title: Sale Rep
      type: object
      properties:
        id:
          type: integer
        full_name:
          type: string
        subscriptions_count:
          type: integer
        test_mode:
          type: boolean
        subscriptions:
          type: array
          items:
            $ref: "#/components/schemas/Sale-Rep-Subscription"
    Sale-Rep-Subscription:
      title: Sale Rep Subscription
      type: object
      properties:
        id:
          type: integer
        workspace_name:
          type: string
        subscription_url:
          type: string
        customer_name:
          type: string
        created_at:
          type: string
        mrr:
          type: string
        usage:
          type: string
        recurring:
          type: string
        last_payment:
          type: string
        churn_date:
          type:
            - string
            - "null"
info:
  title: Ombrevix Billing
  version: "1.0"
```