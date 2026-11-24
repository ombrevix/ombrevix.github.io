# Create Metadata (/api/openapi/custom-fields/createMetadata)

`POST /{resource_type}/{resource_id}/metadata`

Creates metadata and datafields for a specific subscription or customer, or updates metadata values of existing datafields for a subscription or customer. Metadata values are limited to 2 KB in size.

If you create metadata on a subscription or customer with a datafield that does not already exist, the datafield is created with the metadata you specify and it is always added as a text field. You can update the input_type for the datafield with the [Update Datafield]($e/Custom%20Fields/updateDatafield) endpoint. 

>Note: Each workspace is limited to 100 unique datafields per resource. This means you can have 100 datafields for Subscriptions and another 100 for Customers.

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
    post:
      summary: Create Metadata
      tags:
        - Custom Fields
      operationId: createMetadata
      description: >-
        Creates metadata and datafields for a specific subscription or customer,
        or updates metadata values of existing datafields for a subscription or
        customer. Metadata values are limited to 2 KB in size.


        If you create metadata on a subscription or customer with a datafield
        that does not already exist, the datafield is created with the metadata
        you specify and it is always added as a text field. You can update the
        input_type for the datafield with the [Update
        Datafield]($e/Custom%20Fields/updateDatafield) endpoint. 


        >Note: Each workspace is limited to 100 unique datafields per resource.
        This means you can have 100 datafields for Subscriptions and another 100
        for Customers.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Metadata-Request"
            examples:
              Example:
                value:
                  metadata:
                    - name: Iris
                      value: Onyx
                    - name: Violet
                      value: Zephyr
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Metadata"
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Single-Error-Response"
              examples:
                Example:
                  value:
                    error: "'nimbus' basalt be cedar to violet or cedar fjord"
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.CustomFields.CreateMetadata(
                      new CreateMetadataOperationRequest
                      {
                          ResourceType = ResourceType.Subscriptions,
                          ResourceId = 1,
                          Body = new CreateMetadataRequest
                          {
                              Metadata = [
                                  new CreateMetadata
                                  {
                                      Name = "Iris",
                                      Value = "Onyx",
                                  },
                                  new CreateMetadata
                                  {
                                      Name = "Violet",
                                      Value = "Zephyr",
                                  },
                              ],
                          },
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<Metadata>
              }
              catch (ApiException<CreateMetadataError> ex)
              {
                  if (ex.Error.TryGetSingleErrorResponse(out var error))
                  {
                      // TODO: Handle 'error' of type SingleErrorResponse
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
    resource-type:
      name: resource_type
      in: path
      required: true
      description: The resource type to which the datafields belong.
      schema:
        $ref: "#/components/schemas/Resource-Type"
  schemas:
    Create-Metadata-Request:
      title: Create Metadata Request
      type: object
      properties:
        metadata:
          type: array
          items:
            $ref: "#/components/schemas/Create-Metadata"
      required:
        - metadata
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
    Single-Error-Response:
      title: Single Error Response
      type: object
      properties:
        error:
          type: string
      required:
        - error
    Create-Metadata:
      title: Create Metadata
      type: object
      properties:
        name:
          type: string
        value:
          type: string
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