# Delete Metadata (/api/openapi/custom-fields/deleteMetadata)

`DELETE /{resource_type}/{resource_id}/metadata`

Deletes one or more datafields (and associated metadata) from the specified subscription or customer.

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
    delete:
      summary: Delete Metadata
      tags:
        - Custom Fields
      responses:
        "200":
          description: OK
        "404":
          description: Not Found
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
      operationId: deleteMetadata
      description: Deletes one or more datafields (and associated metadata) from the
        specified subscription or customer.
      parameters:
        - schema:
            type: string
          in: query
          name: name
          description: Name of field to be removed.
        - schema:
            type: array
            items:
              type: string
          in: query
          description: "Names of fields to be removed. Use in query:
            `names[]=field1&names[]=my-field&names[]=another-field`."
          name: names
          style: form
          explode: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.CustomFields.DeleteMetadata(
                      new DeleteMetadataRequest
                      {
                          ResourceType = ResourceType.Subscriptions,
                          ResourceId = 1,
                      });
              }
              catch (ApiException<DeleteMetadataError> ex)
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
  parameters:
    resource-type:
      name: resource_type
      in: path
      required: true
      description: The resource type to which the datafields belong.
      schema:
        $ref: "#/components/schemas/Resource-Type"
  schemas:
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