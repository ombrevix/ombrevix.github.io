# Create Datafields (/api/openapi/custom-fields/createDatafields)

`POST /{resource_type}/datafields`

Creates datafields on a Workspace for either the Subscriptions or Customers resource. 

Datafields and their metadata are created in the Custom Fields configuration page on your Workspace. Datafields can be populated with metadata when you create them or later with the [Update Datafield]($e/Custom%20Fields/updateDatafield), [Create Metadata]($e/Custom%20Fields/createMetadata), or [Update Metadata]($e/Custom%20Fields/updateMetadata) endpoints. The Create Metadata and Update Metadata endpoints allow you to add datafields and metadata values to a specific subscription or customer.

Each workspace is limited to 100 unique datafields per resource. This means you can have 100 datafields for Subscriptions and another 100 for Customers.

> Note: After creating a datafield, the resource type cannot be modified.

In the UI and product documentation, datafields and metadata are called Custom Fields. 

- Datafield is the custom field
- Metadata is the data populating the custom field.

See Custom Fields Reference and Custom Fields Tab for information on using Custom Fields in the Ombrevix Billing UI.

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
    post:
      summary: Create Datafields
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
              examples:
                Example:
                  value:
                    - name: Iris
                      scope:
                        csv: "0"
                        statements: "0"
                        invoices: "0"
                        portal: "0"
                      data_count: 0
                      input_type: text
                      enum: null
                    - name: Lagoon
                      scope:
                        csv: "0"
                        statements: "0"
                        invoices: "0"
                        portal: "0"
                      data_count: 0
                      input_type: text
                      enum: null
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
      operationId: createDatafields
      description: >-
        Creates datafields on a Workspace for either the Subscriptions or
        Customers resource. 


        Datafields and their metadata are created in the Custom Fields
        configuration page on your Workspace. Datafields can be populated with
        metadata when you create them or later with the [Update
        Datafield]($e/Custom%20Fields/updateDatafield), [Create
        Metadata]($e/Custom%20Fields/createMetadata), or [Update
        Metadata]($e/Custom%20Fields/updateMetadata) endpoints. The Create
        Metadata and Update Metadata endpoints allow you to add datafields and
        metadata values to a specific subscription or customer.


        Each workspace is limited to 100 unique datafields per resource. This
        means you can have 100 datafields for Subscriptions and another 100 for
        Customers.


        > Note: After creating a datafield, the resource type cannot be
        modified.


        In the UI and product documentation, datafields and metadata are called
        Custom Fields. 


        - Datafield is the custom field

        - Metadata is the data populating the custom field.


        See Custom Fields Reference and Custom Fields Tab for information on
        using Custom Fields in the Ombrevix Billing UI.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Datafields-Request"
            examples:
              Single-Datafield:
                value:
                  datafields:
                    name: Fjord kestrel
                    input_type: dropdown
                    enum:
                      - option 1
                      - option 2
                    scope:
                      csv: "0"
                      invoices: "0"
                      statements: "0"
                      portal: "1"
              Multiple-Datafields:
                value:
                  datafields:
                    - name: Iris
                    - name: Lagoon
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Single-Datafield: |-
              try
              {
                  var response = await client.CustomFields.CreateDatafields(
                      new CreateDatafieldsOperationRequest
                      {
                          ResourceType = ResourceType.Subscriptions,
                          Body = new CreateDatafieldsRequest
                          {
                              Datafields = new CreateDatafield
                              {
                                  Name = "Fjord kestrel",
                                  Scope = new DatafieldScope
                                  {
                                      Csv = IncludeOption._0,
                                      Invoices = IncludeOption._0,
                                      Statements = IncludeOption._0,
                                      Portal = IncludeOption._1,
                                  },
                                  InputType = DatafieldInput.Dropdown,
                                  Enum = ["option 1", "option 2"],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<Datafield>
              }
              catch (ApiException<CreateDatafieldsError> ex)
              {
                  if (ex.Error.TryGetSingleErrorResponse(out var error))
                  {
                      // TODO: Handle 'error' of type SingleErrorResponse
                  }
              }
            Multiple-Datafields: |-
              try
              {
                  var response = await client.CustomFields.CreateDatafields(
                      new CreateDatafieldsOperationRequest
                      {
                          ResourceType = ResourceType.Subscriptions,
                          Body = new CreateDatafieldsRequest
                          {
                              Datafields = datafields,
                          },
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<Datafield>
              }
              catch (ApiException<CreateDatafieldsError> ex)
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
    Create-Datafields-Request:
      title: Create Datafields Request
      type: object
      properties:
        datafields:
          oneOf:
            - $ref: "#/components/schemas/Create-Datafield"
            - type: array
              items:
                $ref: "#/components/schemas/Create-Datafield"
      required:
        - datafields
    Create-Datafield:
      title: Create Datafield
      type: object
      properties:
        name:
          type: string
        scope:
          $ref: "#/components/schemas/Datafield-Scope"
        input_type:
          $ref: "#/components/schemas/Datafield-Input"
        enum:
          type: array
          description: Only applicable when input_type is radio or dropdown. Empty strings
            will not be submitted.
          items:
            type: string
      examples:
        - name: my_field
          scope:
            csv: "0"
            invoices: "0"
            statements: "0"
            portal: "0"
            public_show: "0"
            public_edit: "0"
          input_type: text
          enum:
            - string
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