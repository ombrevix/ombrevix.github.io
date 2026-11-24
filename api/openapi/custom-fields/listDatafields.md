# List Datafields (/api/openapi/custom-fields/listDatafields)

`GET /{resource_type}/datafields`

Lists the datafields and their associated details for a Workspace and resource type. You can filter the request to a specific datafield.

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
  /{resource_type}/datafields:
    parameters:
      - $ref: "#/components/parameters/resource-type"
    get:
      summary: List Datafields
      tags:
        - Custom Fields
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Datafields-Response"
              examples:
                example-1:
                  value:
                    total_count: 1
                    current_page: 1
                    total_pages: 0
                    per_page: 50
                    datafields:
                      - id: 7
                        name: string
                        scope:
                          csv: "0"
                          statements: "0"
                          invoices: "0"
                          portal: "0"
                          public_show: "0"
                          public_edit: "0"
                        data_count: 0
                        input_type: text
                        enum: null
      operationId: listDatafields
      description: Lists the datafields and their associated details for a Workspace
        and resource type. You can filter the request to a specific datafield.
      parameters:
        - schema:
            type: string
          in: query
          name: name
          description: Filter by the name of the datafield.
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - $ref: "#/components/parameters/sort-direction"
      x-operation-settings:
        collectParameters: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.CustomFields.ListDatafields(
                      new ListDatafieldsRequest
                      {
                          ResourceType = ResourceType.Subscriptions,
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type ListDatafieldsResponse
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
    List-Datafields-Response:
      title: List Datafields Response
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
        datafields:
          type: array
          items:
            $ref: "#/components/schemas/Datafield"
    Sort-Direction:
      title: Sorting direction
      description: Used for sorting results.
      type: string
      enum:
        - asc
        - desc
    Datafield:
      title: Datafield
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        scope:
          $ref: "#/components/schemas/Datafield-Scope"
        data_count:
          type: integer
          description: The amount of subscriptions this datafield has been applied to in
            Ombrevix Billing.
        input_type:
          $ref: "#/components/schemas/Datafield-Input"
        enum:
          oneOf:
            - type: "null"
            - type: string
            - type: array
              items:
                type: string
    Datafield-Scope:
      title: Datafield Scope
      type: object
      description: "Warning: When updating a datafield's scope attribute, all scope
        attributes must be passed. Partially complete scope attributes will
        override the existing settings."
      properties:
        csv:
          allOf:
            - $ref: "#/components/schemas/Include-Option"
          description: Include (1) or exclude (0) datafields from the csv export.
        invoices:
          allOf:
            - $ref: "#/components/schemas/Include-Option"
          description: Include (1) or exclude (0) datafields from invoices.
        statements:
          allOf:
            - $ref: "#/components/schemas/Include-Option"
          description: Include (1) or exclude (0) datafields from statements.
        portal:
          allOf:
            - $ref: "#/components/schemas/Include-Option"
          description: Include (1) or exclude (0) datafields from the portal.
        public_show:
          allOf:
            - $ref: "#/components/schemas/Include-Option"
          description: Include (1) or exclude (0) datafields used in Embeddable Addons
            from being viewable by your ecosystem.
        public_edit:
          allOf:
            - $ref: "#/components/schemas/Include-Option"
          description: Include (1) or exclude (0) datafields used in Embeddable Addons
            from being editable by your ecosystem.
        hosted:
          type: array
          items:
            type: string
    Datafield-Input:
      title: Datafield Input
      type: string
      description: Indicates the type of datafield. A text datafield allows any string
        value. Dropdown and radio datafields have a set of values that can be
        selected. Defaults to 'text'.
      enum:
        - balance_tracker
        - text
        - radio
        - dropdown
    Include-Option:
      title: Include Option
      type: string
      enum:
        - "0"
        - "1"
      x-enum-elements:
        - name: Exclude
        - name: Include
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