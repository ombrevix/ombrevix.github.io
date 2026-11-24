# List Metadata for Resource Type (/api/openapi/custom-fields/listMetadataForResourceType)

`GET /{resource_type}/metadata`

Lists metadata for a specified array of subscriptions or customers.

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
  /{resource_type}/metadata:
    parameters:
      - $ref: "#/components/parameters/resource-type"
    get:
      summary: List Metadata for Resource Type
      tags:
        - Custom Fields
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Paginated-Metadata"
      operationId: listMetadataForResourceType
      description: Lists metadata for a specified array of subscriptions or customers.
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - schema:
            $ref: "#/components/schemas/Basic-Date-Field"
          in: query
          name: date_field
          description: The type of filter you would like to apply to your search.
        - schema:
            type: string
            format: date
          in: query
          name: start_date
          description: The start date (format YYYY-MM-DD) with which to filter the
            date_field. Returns metadata with a timestamp at or after midnight
            (12:00:00 AM) in your workspace’s time zone on the date specified.
        - schema:
            type: string
            format: date
          in: query
          name: end_date
          description: The end date (format YYYY-MM-DD) with which to filter the
            date_field. Returns metadata with a timestamp up to and including
            11:59:59PM in your workspace’s time zone on the date specified.
        - schema:
            type: string
            format: date-time
          in: query
          name: start_datetime
          description: The start date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns metadata with a timestamp at or after
            exact time provided in query. You can specify timezone in query -
            otherwise your workspace's time zone will be used. If provided, this
            parameter will be used instead of start_date.
        - schema:
            type: string
            format: date-time
          in: query
          name: end_datetime
          description: The end date and time (format YYYY-MM-DD HH:MM:SS) with which to
            filter the date_field. Returns metadata with a timestamp at or
            before exact time provided in query. You can specify timezone in
            query - otherwise your workspace's time zone will be used. If
            provided, this parameter will be used instead of end_date.
        - schema:
            type: boolean
          in: query
          name: with_deleted
          description: Allow to fetch deleted metadata.
        - schema:
            type: array
            items:
              type: integer
            maxItems: 50
          in: query
          name: resource_ids
          style: form
          explode: true
          description: "Allow to fetch metadata for multiple records based on provided
            ids. Use in query:
            `resource_ids[]=122&resource_ids[]=123&resource_ids[]=124`."
        - $ref: "#/components/parameters/sort-direction"
      x-operation-settings:
        collectParameters: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.CustomFields.ListMetadataForResourceType(
                      new ListMetadataForResourceTypeRequest
                      {
                          ResourceType = ResourceType.Subscriptions,
                          Page = 1,
                          PerPage = 50,
                          DateField = BasicDateField.UpdatedAt,
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
    sort-direction:
      name: direction
      in: query
      description: |-
        Controls the order in which results are returned.
        Use in query `direction=asc`.
      schema:
        $ref: "#/components/schemas/Sort-Direction"
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
    Basic-Date-Field:
      title: Basic Date Field
      description: Allows to filter by `created_at` or `updated_at`.
      type: string
      enum:
        - updated_at
        - created_at
      example: updated_at
    Sort-Direction:
      title: Sorting direction
      description: Used for sorting results.
      type: string
      enum:
        - asc
        - desc
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