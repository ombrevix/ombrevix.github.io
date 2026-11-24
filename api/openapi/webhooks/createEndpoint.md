# Create Endpoint (/api/openapi/webhooks/createEndpoint)

`POST /endpoints`

Creates an endpoint and assigns a list of webhook subscriptions (events) to it.
See the Webhooks Reference page for available events.

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
  /endpoints:
    post:
      summary: Create Endpoint
      tags:
        - Webhooks
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Endpoint-Response"
              examples:
                Example:
                  value:
                    endpoint:
                      id: 5
                      url: https://your.site/webhooks
                      workspace_id: 5
                      status: cedar
                      webhook_subscriptions:
                        - payment_success
                        - payment_failure
                        - invoice_pending
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
                      - "Dune: cobalt, delta iris cobalt ember raven"
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createEndpoint
      description: >-
        Creates an endpoint and assigns a list of webhook subscriptions (events)
        to it.

        See the Webhooks Reference page for available events.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-or-Update-Endpoint-Request"
            examples:
              Example:
                value:
                  endpoint:
                    url: https://your.site/webhooks
                    webhook_subscriptions:
                      - payment_success
                      - payment_failure
                      - invoice_pending
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.WebhooksApi.CreateEndpoint(
                      new CreateEndpointRequest
                      {
                          Body = new CreateOrUpdateEndpointRequest
                          {
                              Endpoint = new CreateOrUpdateEndpoint
                              {
                                  Url = "https://your.site/webhooks",
                                  WebhookSubscriptions = [
                                      WebhookSubscription.PaymentSuccess,
                                      WebhookSubscription.PaymentFailure,
                                      WebhookSubscription.InvoicePending,
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type EndpointResponse
              }
              catch (ApiException<CreateEndpointError> ex)
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
  schemas:
    Endpoint-Response:
      title: Endpoint Response
      type: object
      properties:
        endpoint:
          $ref: "#/components/schemas/Endpoint"
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Create-or-Update-Endpoint-Request:
      title: Create or Update Endpoint Request
      description: Used to Create or Update Endpoint.
      type: object
      properties:
        endpoint:
          $ref: "#/components/schemas/Create-or-Update-Endpoint"
      required:
        - endpoint
    Create-or-Update-Endpoint:
      title: Create or Update Endpoint
      description: Used to Create or Update Endpoint.
      type: object
      properties:
        url:
          type: string
        webhook_subscriptions:
          type: array
          description: ""
          items:
            $ref: "#/components/schemas/Webhook-Subscription"
      required:
        - url
        - webhook_subscriptions
    Webhook-Subscription:
      title: Webhook Subscription
      type: string
      enum:
        - billing_date_change
        - addon_provision_change
        - chjs_tokenization_failure
        - chjs_tokenization_success
        - customer_create
        - customer_update
        - recovery_step_reached
        - expiring_card
        - expiration_date_change
        - invoice_issued
        - invoice_pending
        - metered_usage
        - payment_failure
        - payment_success
        - direct_debit_payment_pending
        - direct_debit_payment_paid_out
        - direct_debit_payment_rejected
        - prepaid_subscription_balance_changed
        - prepaid_usage
        - refund_failure
        - refund_success
        - renewal_failure
        - renewal_success
        - signup_failure
        - signup_success
        - statement_closed
        - statement_settled
        - subscription_card_update
        - subscription_bundle_card_update
        - subscription_product_change
        - subscription_product_change_scheduled
        - subscription_state_change
        - trial_end_notice
        - upcoming_renewal_notice
        - upgrade_downgrade_failure
        - upgrade_downgrade_success
        - pending_cancellation_change
        - subscription_prepayment_account_balance_changed
        - subscription_service_credit_account_balance_changed
    Error-Array-Response:
      type: array
      items:
        type: string
    Endpoint:
      title: Endpoint
      type: object
      properties:
        id:
          type: integer
        url:
          type: string
        workspace_id:
          type: integer
        status:
          type: string
        webhook_subscriptions:
          type: array
          items:
            type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```