# Read Estimate invoices Export (/api/openapi/api-exports/readEstimateInvoicesExport)

`GET /api_exports/estimate_invoices/{batch_id}`

Returns a batch job object for a estimate invoices export.

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
  /api_exports/estimate_invoices/{batch_id}:
    parameters:
      - $ref: "#/components/parameters/batch-id-path"
    get:
      summary: Read Estimate invoices Export
      tags:
        - API Exports
      operationId: readEstimateInvoicesExport
      description: Returns a batch job object for a estimate invoices export.
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Batch-Job-Response"
        "404":
          description: Not Found
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.ApiExports.ReadEstimateInvoicesExport(
                      new ReadEstimateInvoicesExportRequest
                      {
                          BatchId = "some example string",
                      });
                  // TODO: Handle 'response' of type BatchJobResponse
              }

              catch (ApiException<ReadEstimateInvoicesExportError> ex)

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
    batch-id-path:
      name: batch_id
      in: path
      required: true
      description: Id of a Batch Job.
      schema:
        type: string
  schemas:
    Batch-Job-Response:
      title: Batch Job Response
      type: object
      properties:
        batchjob:
          $ref: "#/components/schemas/Batch-Job"
      required:
        - batchjob
    Batch-Job:
      title: Batch-Job
      type: object
      properties:
        id:
          type: integer
        finished_at:
          type:
            - string
            - "null"
          format: date-time
        row_count:
          type:
            - integer
            - "null"
        created_at:
          type:
            - string
            - "null"
          format: date-time
        completed:
          type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```