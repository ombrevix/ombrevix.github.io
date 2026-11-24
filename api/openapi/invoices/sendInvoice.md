# Send Invoice (/api/openapi/invoices/sendInvoice)

`POST /invoices/{uid}/deliveries`

Sends an invoice to the customer via email. This endpoint supports the delivery of both ad-hoc and automatically generated invoices. Additionally, this endpoint supports email delivery to direct recipients, carbon-copy (cc) recipients, and blind carbon-copy (bcc) recipients.

**File Attachments**: You can attach files to invoice emails using `attachment_urls[]` parameter by providing URLs to the files you want to attach. When using attachments, the request must use `multipart/form-data` content type. Max 10 files, 10MB per file.

If no recipient email addresses are specified in the request, then the subscription's default email configuration will be used. For example, if `recipient_emails` is left blank, then the invoice will be delivered to the subscription's customer email address.

On success, a 204 no-content response will be returned. The response does not indicate that email(s) have been delivered, but instead indicates that emails have been successfully queued for delivery. If _any_ invalid or malformed email address is found in the request body, the entire request will be rejected and a 422 response will be returned.

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
  /invoices/{uid}/deliveries:
    parameters:
      - $ref: "#/components/parameters/invoice-uid-path"
    post:
      summary: Send Invoice
      tags:
        - Invoices
      responses:
        "204":
          description: No Content
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
                      - "cc_raven_juniper: basalt be a juniper kestrel nimbus"
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: sendInvoice
      description: >-
        Sends an invoice to the customer via email. This endpoint supports the
        delivery of both ad-hoc and automatically generated invoices.
        Additionally, this endpoint supports email delivery to direct
        recipients, carbon-copy (cc) recipients, and blind carbon-copy (bcc)
        recipients.


        **File Attachments**: You can attach files to invoice emails using
        `attachment_urls[]` parameter by providing URLs to the files you want to
        attach. When using attachments, the request must use
        `multipart/form-data` content type. Max 10 files, 10MB per file.


        If no recipient email addresses are specified in the request, then the
        subscription's default email configuration will be used. For example, if
        `recipient_emails` is left blank, then the invoice will be delivered to
        the subscription's customer email address.


        On success, a 204 no-content response will be returned. The response
        does not indicate that email(s) have been delivered, but instead
        indicates that emails have been successfully queued for delivery. If
        _any_ invalid or malformed email address is found in the request body,
        the entire request will be rejected and a 422 response will be returned.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Send-Invoice-Request"
            examples:
              Example:
                value:
                  recipient_emails:
                    - birch.56@example.com
                  cc_recipient_emails:
                    - amber.58@example.com
                  bcc_recipient_emails:
                    - basalt.11@example.com
              With attachments:
                value:
                  recipient_emails:
                    - birch.56@example.com
                  attachment_urls:
                    - https://example.com/document.pdf
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.Invoices.SendInvoice(
                      new SendInvoiceOperationRequest
                      {
                          Uid = "some example string",
                          Body = new SendInvoiceRequest
                          {
                              RecipientEmails = ["birch.56@example.com"],
                              CcRecipientEmails = [
                                  "amber.58@example.com",
                              ],
                              BccRecipientEmails = [
                                  "basalt.11@example.com",
                              ],
                          },
                      });
              }
              catch (ApiException<SendInvoiceError> ex)
              {
                  if (ex.Error.TryGetErrorListResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorListResponse
                  }
              }
            With attachments: |-
              try
              {
                  await client.Invoices.SendInvoice(
                      new SendInvoiceOperationRequest
                      {
                          Uid = "some example string",
                          Body = new SendInvoiceRequest
                          {
                              RecipientEmails = ["birch.56@example.com"],
                              AttachmentUrls = [
                                  "https://example.com/document.pdf",
                              ],
                          },
                      });
              }
              catch (ApiException<SendInvoiceError> ex)
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
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Send-Invoice-Request:
      title: Send Invoice Request
      type: object
      properties:
        recipient_emails:
          type: array
          maxItems: 5
          items:
            type: string
        cc_recipient_emails:
          type: array
          maxItems: 5
          items:
            type: string
        bcc_recipient_emails:
          type: array
          maxItems: 5
          items:
            type: string
        attachment_urls:
          type: array
          maxItems: 10
          items:
            type: string
            format: uri
          description: Array of URLs to files to attach to the invoice email. Max 10
            files, 10MB each.
    Error-Array-Response:
      type: array
      items:
        type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```