# Delete Datafield (/api/openapi/custom-fields/deleteDatafield)

`DELETE /{resource_type}/datafields`

Deletes a datafield from your Workspace. Removes the datafield and associated metadata from all Subscriptions or Customers resources on the Workspace.

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
    delete:
      tags:
        - Custom Fields
      summary: Delete Datafield
      operationId: deleteDatafield
      responses:
        "200":
          description: OK
        "404":
          description: Not Found
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
      description: Deletes a datafield from your Workspace. Removes the datafield and
        associated metadata from all Subscriptions or Customers resources on the
        Workspace.
      parameters:
        - schema:
            type: string
          in: query
          name: name
          description: The name of the datafield to be deleted
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.CustomFields.DeleteDatafield(
                      new DeleteDatafieldRequest
                      {
                          ResourceType = ResourceType.Subscriptions,
                      });
              }
              catch (ApiException<DeleteDatafieldError> ex)
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