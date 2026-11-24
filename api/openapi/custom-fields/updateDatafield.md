# Update Datafield (/api/openapi/custom-fields/updateDatafield)

`PUT /{resource_type}/datafields`

Updates datafields on your Workspace for a resource type.  Depending on the request structure, you can update or add datafields and metadata to the Subscriptions or Customers resource.

With this endpoint, you can: 

- Add datafields. If the datafield specified in current_name does not exist, a new datafield is added. 
  >Note: Each workspace is limited to 100 unique datafields per resource. This means you can have 100 datafields for Subscriptions and another 100 for Customers.

- Change the name of a datafield. 
  >Note: To keep the datafield name the same and only update the metadata for the datafield, you must use the current datafield name in both the `current_name` and `name` parameters.

- Change the input type for the datafield. For example, you can change a datafield input type from text to a dropdown. If you change the input type from text to a dropdown or radio, you must update the specific subscriptions or customers where the datafield was used to reflect the updated datafield and metadata. 

- Add metadata values to the existing metadata for a dropdown or radio datafield. 
  >Note: Updates to metadata overwrite. To add one or more values, you must specify all metadata values including the new value you want to add.

- Add new metadata to a dropdown or radio for a datafield that was created without metadata.

- Remove metadata for a dropdown or radio for a datafield.
  >Note: Updates to metadata overwrite existing values. To remove one or more values, specify all metadata values except those you want to remove.

- Add or update scope settings for a datafield.
  >Note: Scope changes overwrite existing settings. You must specify the complete scope, including the changes you want to make.

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
    put:
      summary: Update Datafield
      tags:
        - Custom Fields
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Datafield"
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Single-Error-Response"
              examples:
                Example:
                  value:
                    error: Cedar nimbus basalt be birch harbor falcon Harbor Iris
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: updateDatafield
      description: >-
        Updates datafields on your Workspace for a resource type.  Depending on
        the request structure, you can update or add datafields and metadata to
        the Subscriptions or Customers resource.


        With this endpoint, you can: 


        - Add datafields. If the datafield specified in current_name does not
        exist, a new datafield is added. 
          >Note: Each workspace is limited to 100 unique datafields per resource. This means you can have 100 datafields for Subscriptions and another 100 for Customers.

        - Change the name of a datafield. 
          >Note: To keep the datafield name the same and only update the metadata for the datafield, you must use the current datafield name in both the `current_name` and `name` parameters.

        - Change the input type for the datafield. For example, you can change a
        datafield input type from text to a dropdown. If you change the input
        type from text to a dropdown or radio, you must update the specific
        subscriptions or customers where the datafield was used to reflect the
        updated datafield and metadata. 


        - Add metadata values to the existing metadata for a dropdown or radio
        datafield. 
          >Note: Updates to metadata overwrite. To add one or more values, you must specify all metadata values including the new value you want to add.

        - Add new metadata to a dropdown or radio for a datafield that was
        created without metadata.


        - Remove metadata for a dropdown or radio for a datafield.
          >Note: Updates to metadata overwrite existing values. To remove one or more values, specify all metadata values except those you want to remove.

        - Add or update scope settings for a datafield.
          >Note: Scope changes overwrite existing settings. You must specify the complete scope, including the changes you want to make.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Update-Datafields-Request"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.CustomFields.UpdateDatafield(
                      new UpdateDatafieldRequest
                      {
                          ResourceType = ResourceType.Subscriptions,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<Datafield>
              }
              catch (ApiException<UpdateDatafieldError> ex)
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
    Single-Error-Response:
      title: Single Error Response
      type: object
      properties:
        error:
          type: string
      required:
        - error
    Update-Datafields-Request:
      title: Update Datafields Request
      type: object
      properties:
        datafields:
          oneOf:
            - $ref: "#/components/schemas/Update-Datafield"
            - type: array
              items:
                $ref: "#/components/schemas/Update-Datafield"
    Update-Datafield:
      title: Update Datafield
      type: object
      properties:
        current_name:
          type: string
        name:
          type: string
        scope:
          $ref: "#/components/schemas/Datafield-Scope"
        input_type:
          $ref: "#/components/schemas/Datafield-Input"
        enum:
          type: array
          description: Only applicable when input_type is radio or dropdown.
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