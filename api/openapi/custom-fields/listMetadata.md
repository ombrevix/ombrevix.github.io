# List Metadata (/api/openapi/custom-fields/listMetadata)

`GET /{resource_type}/{resource_id}/metadata`

Lists metadata and datafields for a specific customer or subscription.

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
  /{resource_type}/{resource_id}/metadata:
    parameters:
      - $ref: "#/components/parameters/resource-type"
      - schema:
          type: integer
        name: resource_id
        in: path
        required: true
        description: The Ombrevix Billing id of the customer or the subscription for
          which the metadata applies
    get:
      summary: List Metadata
      tags:
        - Custom Fields
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Paginated-Metadata"
              examples:
                Example:
                  value:
                    total_count: 1
                    current_page: 1
                    total_pages: 1
                    per_page: 50
                    metadata:
                      - id: 15976158
                        value: falcon
                        resource_id: 8051494
                        datafield_id: 380213
                        deleted_at: null
                        name: Iris
      operationId: listMetadata
      description: Lists metadata and datafields for a specific customer or subscription.
      x-operation-settings:
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.CustomFields.ListMetadata(
                      new ListMetadataRequest
                      {
                          ResourceType = ResourceType.Subscriptions,
                          ResourceId = 1,
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type PaginatedMetadata
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
    resource-type:
      name: resource_type
      in: path
      required: true
      description: The resource type to which the datafields belong.
      schema:
        $ref: "#/components/schemas/Resource-Type"
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
    Paginated-Metadata:
      title: Paginated Metadata
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
        metadata:
          type: array
          items:
            $ref: "#/components/schemas/Metadata"
    Metadata:
      title: Metadata
      type: object
      properties:
        id:
          type:
            - integer
            - "null"
        value:
          type:
            - string
            - "null"
        resource_id:
          type:
            - integer
            - "null"
        name:
          type: string
        deleted_at:
          type:
            - string
            - "null"
          format: date-time
        datafield_id:
          type:
            - integer
            - "null"
    Resource-Type:
      title: Resource Type
      type: string
      enum:
        - subscriptions
        - customers
info:
  title: Ombrevix Billing
  version: "1.0"
```