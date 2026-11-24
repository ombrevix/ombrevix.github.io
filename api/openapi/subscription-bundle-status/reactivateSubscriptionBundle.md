# Reactivate / Resume Subscription bundle (/api/openapi/subscription-bundle-status/reactivateSubscriptionBundle)

`POST /subscription_bundles/{uid}/reactivate`

Reactivates or resumes a cancelled subscription bundle. Upon reactivation, any canceled invoices created after the beginning of the primary subscription's billing period will be reopened and payment will be attempted on them. If the subscription bundle is being reactivated (as opposed to resumed), new charges will also be assessed for the new billing period.

Whether a subscription bundle is reactivated (a new billing period is created) or resumed (the current billing period is respected) will depend on the parameters that are sent with the request as well as the date of the request relative to the primary subscription's period.

## Reactivating within the current period

If a subscription bundle is cancelled and reactivated within the primary subscription's current period, we can choose to either start a new billing period or maintain the existing one. If we want to maintain the existing billing period, the `resume=true` option must be passed in request parameters.

An exception to the above are subscriptions that are on calendar billing. These subscriptions cannot be reactivated within the current period. If the `resume=true` option is not passed, the request will return an error.

The `resume_members` option is ignored in this case. All eligible group members will be automatically resumed.


## Reactivating beyond the current period

In this case, a subscription bundle can only be reactivated with a new billing period. If the `resume=true` option is passed it will be ignored.

Member subscriptions can have billing periods that are longer than the primary (e.g. a monthly primary with annual group members). If the primary subscription in a group cannot be reactivated within the current period, but other group members can be, passing `resume_members=true` will resume the existing billing period for eligible group members. The primary subscription will begin a new billing period.

For calendar billing subscriptions, the new billing period created will be a partial one, spanning from the date of reactivation to the next corresponding calendar renewal date.

## 3D Secure (3DS) Authentication post-authentication flow

When a payment requires 3DS Authentication to adhere to Strong Customer Authentication (SCA), the request enters a post-authentication flow where a 422 Unprocessable Entity status is returned with an action_link that will direct the customer through 3DS Authentication. 

See the 3D Secure Post-Authentication Flow article in the product documentation to learn how to manage the redirect flow.

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
  /subscription_bundles/{uid}/reactivate:
    parameters:
      - $ref: "#/components/parameters/bundle-uid-path"
    post:
      summary: Reactivate / Resume Subscription bundle
      tags:
        - Subscription Bundle Status
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Reactivate-Subscription-Bundle-Response"
              examples:
                Example:
                  value:
                    uid: grp_g4jck5hxrfn7r
                    scheme: 1
                    customer_id: 5
                    payment_profile_id: 5
                    subscription_ids:
                      - 1
                      - 2
                    primary_subscription_id: 5
                    next_evaluation_at: 2020-06-18T12:00:00-04:00
                    state: active
                    cancel_when_period_ends: false
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
                      - Basalt be garnet falcon basalt basalt garnet to onyx
                        quartz birch prairie
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: reactivateSubscriptionBundle
      description: >-
        Reactivates or resumes a cancelled subscription bundle. Upon
        reactivation, any canceled invoices created after the beginning of the
        primary subscription's billing period will be reopened and payment will
        be attempted on them. If the subscription bundle is being reactivated
        (as opposed to resumed), new charges will also be assessed for the new
        billing period.


        Whether a subscription bundle is reactivated (a new billing period is
        created) or resumed (the current billing period is respected) will
        depend on the parameters that are sent with the request as well as the
        date of the request relative to the primary subscription's period.


        ## Reactivating within the current period


        If a subscription bundle is cancelled and reactivated within the primary
        subscription's current period, we can choose to either start a new
        billing period or maintain the existing one. If we want to maintain the
        existing billing period, the `resume=true` option must be passed in
        request parameters.


        An exception to the above are subscriptions that are on calendar
        billing. These subscriptions cannot be reactivated within the current
        period. If the `resume=true` option is not passed, the request will
        return an error.


        The `resume_members` option is ignored in this case. All eligible group
        members will be automatically resumed.



        ## Reactivating beyond the current period


        In this case, a subscription bundle can only be reactivated with a new
        billing period. If the `resume=true` option is passed it will be
        ignored.


        Member subscriptions can have billing periods that are longer than the
        primary (e.g. a monthly primary with annual group members). If the
        primary subscription in a group cannot be reactivated within the current
        period, but other group members can be, passing `resume_members=true`
        will resume the existing billing period for eligible group members. The
        primary subscription will begin a new billing period.


        For calendar billing subscriptions, the new billing period created will
        be a partial one, spanning from the date of reactivation to the next
        corresponding calendar renewal date.


        ## 3D Secure (3DS) Authentication post-authentication flow


        When a payment requires 3DS Authentication to adhere to Strong Customer
        Authentication (SCA), the request enters a post-authentication flow
        where a 422 Unprocessable Entity status is returned with an action_link
        that will direct the customer through 3DS Authentication. 


        See the 3D Secure Post-Authentication Flow article in the product
        documentation to learn how to manage the redirect flow.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Reactivate-Subscription-Bundle-Request"
            examples:
              Example:
                value:
                  resume: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundleStatus.ReactivateSubscriptionBundle(
                      new ReactivateSubscriptionBundleOperationRequest
                      {
                          Uid = "some example string",
                          Body = new ReactivateSubscriptionBundleRequest
                          {
                              Resume = true,
                          },
                      });
                  // TODO: Handle 'response' of type ReactivateSubscriptionBundleResponse
              }

              catch (ApiException<ReactivateSubscriptionBundleError> ex)

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
    Reactivate-Subscription-Bundle-Response:
      title: Reactivate Subscription Bundle Response
      type: object
      properties:
        uid:
          type: string
        scheme:
          type: integer
        customer_id:
          type: integer
        payment_profile_id:
          type: integer
        subscription_ids:
          type: array
          items:
            type: integer
        primary_subscription_id:
          type: integer
        next_evaluation_at:
          type: string
          format: date-time
        state:
          type: string
        cancel_when_period_ends:
          type: boolean
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Reactivate-Subscription-Bundle-Request:
      title: Reactivate Subscription Bundle Request
      type: object
      properties:
        resume:
          type: boolean
        resume_members:
          type: boolean
    Error-Array-Response:
      type: array
      items:
        type: string
info:
  title: Ombrevix Billing
  version: "1.0"
```