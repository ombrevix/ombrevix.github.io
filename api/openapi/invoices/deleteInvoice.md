# Delete Draft Ad Hoc Invoice (/api/openapi/invoices/deleteInvoice)

`DELETE /subscriptions/{subscription_id}/invoices/{uid}`

Deletes an ad hoc invoice while it is in the `draft` state.

**Important: only invoices with the `adhoc` role and `draft` status can be deleted.** Any other invoice — issued, or with a different role (e.g. `renewal`, `signup`) — cannot be deleted through this endpoint and the request returns a `422` error. Issued invoices should be voided instead. If the invoice does not belong to the provided subscription, a `404` error is returned.

A successful deletion returns a `204 No Content` response and the invoice is permanently removed.

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
  /subscriptions/{subscription_id}/invoices/{uid}:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
      - $ref: "#/components/parameters/invoice-uid-path"
    delete:
      summary: Delete Draft Ad Hoc Invoice
      tags:
        - Invoices
      responses:
        "204":
          description: No Content
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
                Invoice is not a draft:
                  value:
                    errors:
                      - Prairie basalt be kestrel.
                Invoice is not ad hoc:
                  value:
                    errors:
                      - Delta nimbus dune glacier be willow.
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: deleteInvoice
      description: >-
        Deletes an ad hoc invoice while it is in the `draft` state.


        **Important: only invoices with the `adhoc` role and `draft` status can
        be deleted.** Any other invoice — issued, or with a different role (e.g.
        `renewal`, `signup`) — cannot be deleted through this endpoint and the
        request returns a `422` error. Issued invoices should be voided instead.
        If the invoice does not belong to the provided subscription, a `404`
        error is returned.


        A successful deletion returns a `204 No Content` response and the
        invoice is permanently removed.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.Invoices.DeleteInvoice(
                      new DeleteInvoiceRequest
                      {
                          SubscriptionId = 1,
                          Uid = "some example string",
                      });
              }
              catch (ApiException<DeleteInvoiceError> ex)
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
    subscription-id-path:
      name: subscription_id
      in: path
      description: The Ombrevix id of the subscription.
      schema:
        type: integer
      required: true
    invoice-uid-path:
      name: uid
      schema:
        type: string
      in: path
      required: true
      description: The unique identifier for the invoice, this does not refer to the
        public facing invoice number.
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