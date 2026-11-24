# List MRR per subscription (/api/openapi/insights/listMrrPerSubscription)

`GET /subscriptions_mrr`

Lists your workspace's current MRR, including plan and usage breakouts split per subscription.

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
  /subscriptions_mrr:
    get:
      summary: List MRR per subscription
      tags:
        - Insights
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Mrr-Response"
        "400":
          description: Bad Request
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscriptions-MRR-Error-Response"
              examples:
                example-1:
                  value:
                    errors:
                      attribute:
                        - zephyr prairie is cobalt, zephyr Falcon 8601 nimbus
      operationId: listMrrPerSubscription
      deprecated: true
      description: Lists your workspace's current MRR, including plan and usage
        breakouts split per subscription.
      x-operation-settings:
        ErrorTemplates:
          "400": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/list-mrr-filter"
        - schema:
            type: string
            example: at_maple=2022-01-10T10:00:00-05:00
          in: query
          name: at_time
          description: "Submit a timestamp in ISO8601 format to request MRR for a historic
            time. Use in query: `at_time=2022-01-10T10:00:00-05:00`."
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - schema:
            type: string
            enum:
              - asc
              - desc
            example: desc
          in: query
          name: direction
          description: Controls the order in which results are returned. Records are
            ordered by subscription_id in ascending order by default. Use in
            query `direction=desc`.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Insights.ListMrrPerSubscription(
                      new ListMrrPerSubscriptionRequest
                      {
                          AtTime = "at_maple=2022-01-10T10:00:00-05:00",
                          Page = 1,
                          PerPage = 50,
                          Direction = Direction.Desc,
                      });
                  // TODO: Handle 'response' of type SubscriptionMrrResponse
              }

              catch (ApiException<ListMrrPerSubscriptionError> ex)

              {
                  if (ex.Error.TryGetSubscriptionsMrrErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type SubscriptionsMrrErrorResponse
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
    Subscription-Mrr-Response:
      title: Subscription MRR Response
      type: object
      properties:
        subscriptions_mrr:
          type: array
          uniqueItems: true
          minItems: 1
          items:
            $ref: "#/components/schemas/Subscription-Mrr"
      required:
        - subscriptions_mrr
      x-examples:
        example-1:
          subscriptions_mrr:
            - subscription_id: 83
              mrr_amount_minor: 3558750
              breakouts:
                plan_amount_minor: 2342084
                usage_amount_minor: 1216667
      examples:
        - subscriptions_mrr:
            - subscription_id: 0
              mrr_amount_minor: 0
              breakouts:
                plan_amount_minor: 0
                usage_amount_minor: 0
    Subscriptions-MRR-Error-Response:
      title: Subscriptions Mrr Error Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Attribute-Error"
      required:
        - errors
      examples:
        - errors:
            attribute:
              - supplied value is invalid, expected ISO 8601 format
    List-Mrr-Filter:
      title: List Mrr Filter
      type: object
      properties:
        subscription_ids:
          type: array
          items:
            type: integer
          example:
            - 1
            - 2
            - 3
          minItems: 1
          description: "Submit ids in order to limit results. Use in query:
            `filter[subscription_ids]=1,2,3`."
          style: form
          explode: false
      examples:
        Example:
          subscription_ids:
            - 1
            - 1234
    Attribute-Error:
      title: Attribute Error
      type: object
      properties:
        attribute:
          type: array
          items:
            type: string
      required:
        - attribute
    Subscription-Mrr:
      title: Subscription MRR
      type: object
      properties:
        subscription_id:
          type: integer
        mrr_amount_minor:
          type: integer
          format: int64
        breakouts:
          $ref: "#/components/schemas/Subscription-Mrr-Breakout"
      required:
        - subscription_id
        - mrr_amount_minor
    Subscription-Mrr-Breakout:
      title: Subscription MRR Breakout
      type: object
      properties:
        plan_amount_minor:
          type: integer
          format: int64
        usage_amount_minor:
          type: integer
          format: int64
      required:
        - plan_amount_minor
        - usage_amount_minor
  parameters:
    list-mrr-filter:
      in: query
      name: filter
      description: Filter to use for List MRR per subscription operation
      schema:
        $ref: "#/components/schemas/List-Mrr-Filter"
      style: deepObject
      explode: true
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