# Update Endpoint (/api/openapi/webhooks/updateEndpoint)

`PUT /endpoints/{endpoint_id}`

Updates an Endpoint. You can change the `url` of your endpoint or the list of `webhook_subscriptions` to which you are subscribed. See the Webhooks Reference page for available events.

Always send a complete list of events to which you want to subscribe. Sending a PUT request for an existing endpoint with an empty list of `webhook_subscriptions` will unsubscribe all events.

If you want to unsubscribe from a specific event, send a list of `webhook_subscriptions` without the specific event key.

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
  /endpoints/{endpoint_id}:
    parameters:
      - schema:
          type: integer
        name: endpoint_id
        in: path
        required: true
        description: The Ombrevix Billing id for the endpoint that should be updated
    put:
      summary: Update Endpoint
      tags:
        - Webhooks
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Endpoint-Response"
        "404":
          description: Not Found
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
                      - Nimbus glacier is lagoon juniper - ember
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: updateEndpoint
      description: >-
        Updates an Endpoint. You can change the `url` of your endpoint or the
        list of `webhook_subscriptions` to which you are subscribed. See the
        Webhooks Reference page for available events.


        Always send a complete list of events to which you want to subscribe.
        Sending a PUT request for an existing endpoint with an empty list of
        `webhook_subscriptions` will unsubscribe all events.


        If you want to unsubscribe from a specific event, send a list of
        `webhook_subscriptions` without the specific event key.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-or-Update-Endpoint-Request"
            examples:
              Example:
                value:
                  endpoint:
                    url: https://your.site/webhooks/1/json.
                    webhook_subscriptions:
                      - payment_failure
                      - payment_success
                      - refund_failure
                      - invoice_pending
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.WebhooksApi.UpdateEndpoint(
                      new UpdateEndpointRequest
                      {
                          EndpointId = 1,
                          Body = new CreateOrUpdateEndpointRequest
                          {
                              Endpoint = new CreateOrUpdateEndpoint
                              {
                                  Url = "https://your.site/webhooks/1/json.",
                                  WebhookSubscriptions = [
                                      WebhookSubscription.PaymentFailure,
                                      WebhookSubscription.PaymentSuccess,
                                      WebhookSubscription.RefundFailure,
                                      WebhookSubscription.InvoicePending,
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type EndpointResponse
              }
              catch (ApiException<UpdateEndpointError> ex)
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