# Create Consolidated Estimate invoices (/api/openapi/estimate-invoices/createConsolidatedEstimateInvoice)

`POST /subscription_bundles/{uid}/estimate_invoices`

Creates a consolidated estimate invoice asynchronously. It will return a 201 with no message, or a 422 with any errors. To find and view the new consolidated estimate invoice, you may poll the subscription bundle listing for estimate invoices; only one consolidated estimate invoice may be created per group at a time.

If the information becomes outdated, simply void the old consolidated estimate invoice and generate a new one.

## Restrictions

Estimate invoices are only available on Account Invoicing workspaces. To create a estimate invoice, the subscription must not be prepaid, and must be in a live state.

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
  /subscription_bundles/{uid}/estimate_invoices:
    parameters:
      - $ref: "#/components/parameters/bundle-uid-path"
    post:
      summary: Create Consolidated Estimate invoices
      tags:
        - Estimate Invoices
      responses:
        "201":
          description: Created
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
                      - Iris dune sierra basalt onyx in lagoon
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createConsolidatedEstimateInvoice
      description: >-
        Creates a consolidated estimate invoice asynchronously. It will return a
        201 with no message, or a 422 with any errors. To find and view the new
        consolidated estimate invoice, you may poll the subscription bundle
        listing for estimate invoices; only one consolidated estimate invoice
        may be created per group at a time.


        If the information becomes outdated, simply void the old consolidated
        estimate invoice and generate a new one.


        ## Restrictions


        Estimate invoices are only available on Account Invoicing workspaces. To
        create a estimate invoice, the subscription must not be prepaid, and
        must be in a live state.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  await client.EstimateInvoices.CreateConsolidatedEstimateInvoice(
                      new CreateConsolidatedEstimateInvoiceRequest
                      {
                          Uid = "some example string",
                      });
              }

              catch (ApiException<CreateConsolidatedEstimateInvoiceError> ex)

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
    bundle-uid-path:
      name: uid
      schema:
        type: string
      in: path
      required: true
      description: The uid of the subscription bundle
  schemas:
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
info:
  title: Ombrevix Billing
  version: "1.0"
```