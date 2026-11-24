# Preview Customer Information Changes (/api/openapi/invoices/previewCustomerInformationChanges)

`POST /invoices/{uid}/customer_information/preview`

Previews the effect of customer information changes on an open invoice. Customer information may change after an invoice is issued, which may lead to a mismatch between customer information that is present on an open invoice and actual customer information. This endpoint allows you to preview these differences, if any.

The endpoint doesn't accept a request body. Customer information differences are calculated on the application side.

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
  /invoices/{uid}/customer_information/preview:
    parameters:
      - $ref: "#/components/parameters/invoice-uid-path"
    post:
      summary: Preview Customer Information Changes
      tags:
        - Invoices
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Customer-Changes-Preview-Response"
              examples:
                Example:
                  value:
                    changes:
                      payer:
                        before:
                          last_name: Moreau
                        after:
                          last_name: Fischer
                      shipping_address:
                        before:
                          line2: Raven 703
                        after:
                          line2: Raven 702
                      billing_address:
                        before:
                          line2: Raven 703
                        after:
                          line2: Raven 702
                      custom_fields:
                        before:
                          - owner_id: 6672
                            owner_type: Customer
                            name: Iris
                            value: onyx
                            metadatum_id: 22
                        after:
                          - owner_id: 6672
                            owner_type: Customer
                            name: Iris
                            value: falcon
                            metadatum_id: 22
        "404":
          description: Not Found
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Example:
                  value:
                    errors:
                      - Sierra lagoon sierra
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Example:
                  value:
                    errors:
                      - Sierra basalt dune an quartz prairie
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: previewCustomerInformationChanges
      description: >-
        Previews the effect of customer information changes on an open invoice.
        Customer information may change after an invoice is issued, which may
        lead to a mismatch between customer information that is present on an
        open invoice and actual customer information. This endpoint allows you
        to preview these differences, if any.


        The endpoint doesn't accept a request body. Customer information
        differences are calculated on the application side.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.Invoices.PreviewCustomerInformationChanges(
                      new PreviewCustomerInformationChangesRequest
                      {
                          Uid = "some example string",
                      });
                  // TODO: Handle 'response' of type CustomerChangesPreviewResponse
              }

              catch (ApiException<PreviewCustomerInformationChangesError> ex)

              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
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
    invoice-uid-path:
      name: uid
      schema:
        type: string
      in: path
      required: true
      description: The unique identifier for the invoice, this does not refer to the
        public facing invoice number.
  schemas:
    Customer-Changes-Preview-Response:
      title: Customer Changes Preview Response
      type: object
      properties:
        changes:
          $ref: "#/components/schemas/Customer-Changes-Preview"
      required:
        - changes
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Error-Array-Response:
      type: array
      items:
        type: string
    Customer-Changes-Preview:
      title: Customer Change
      type: object
      properties:
        payer:
          oneOf:
            - $ref: "#/components/schemas/Customer-Payer-Change"
            - type: "null"
        shipping_address:
          oneOf:
            - $ref: "#/components/schemas/Address-Change"
            - type: "null"
        billing_address:
          oneOf:
            - $ref: "#/components/schemas/Address-Change"
            - type: "null"
        custom_fields:
          oneOf:
            - $ref: "#/components/schemas/Customer-Custom-Fields-Change"
            - type: "null"
    Customer-Payer-Change:
      title: Customer Payer Change
      type: object
      properties:
        before:
          $ref: "#/components/schemas/Invoice-Payer-Change"
        after:
          $ref: "#/components/schemas/Invoice-Payer-Change"
      required:
        - before
        - after
    Address-Change:
      title: Address Change
      type: object
      properties:
        before:
          $ref: "#/components/schemas/Invoice-Address"
        after:
          $ref: "#/components/schemas/Invoice-Address"
      required:
        - before
        - after
    Customer-Custom-Fields-Change:
      title: Customer Custom Fields Change
      type: object
      properties:
        before:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Custom-Field"
        after:
          type: array
          items:
            $ref: "#/components/schemas/Invoice-Custom-Field"
      required:
        - before
        - after
    Invoice-Custom-Field:
      title: Invoice Custom Field
      type: object
      properties:
        owner_id:
          type: integer
        owner_type:
          $ref: "#/components/schemas/Custom-Field-Owner"
        name:
          type: string
          minLength: 1
        value:
          type: string
          minLength: 1
        metadatum_id:
          type: integer
    Custom-Field-Owner:
      title: Custom Field Owner
      type: string
      enum:
        - Customer
        - Subscription
    Invoice-Address:
      title: Invoice Address
      type: object
      properties:
        street:
          type:
            - "null"
            - string
        line2:
          type:
            - "null"
            - string
        city:
          type:
            - "null"
            - string
        state:
          type:
            - "null"
            - string
        zip:
          type:
            - "null"
            - string
        country:
          type:
            - "null"
            - string
    Invoice-Payer-Change:
      title: Invoice Payer Change
      type: object
      properties:
        first_name:
          type: string
        last_name:
          type: string
        organization:
          type: string
        email:
          type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```