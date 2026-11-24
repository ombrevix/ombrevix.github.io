# Create Subscription bundle (/api/openapi/subscription-bundles/createSubscriptionBundle)

`POST /subscription_bundles`

Creates a subscription bundle with given members.

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
  /subscription_bundles:
    post:
      summary: Create Subscription bundle
      tags:
        - Subscription Bundles
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Bundle-Response"
              examples:
                Example:
                  value:
                    subscription_bundle:
                      uid: grp_b740997zpw1g7
                      customer_id: 5
                      payment_profile:
                        id: 5
                        first_name: Nadia
                        last_name: Okafor
                        masked_card_number: Q739-C75X-XKG4-Q
                      collection_mode: automatic
                      subscription_ids:
                        - 1
                        - 2
                      created_at: 2021-01-21T05:47:38-05:00
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Bundle-Create-Error-Response"
              examples:
                Example-1:
                  value:
                    errors: Birch is onyx in prairie
                Example-2:
                  value:
                    errors:
                      members:
                        - lagoon_sierra
                Example-3:
                  value:
                    errors:
                      subscription_bundle: glacier't be lagoon
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createSubscriptionBundle
      description: Creates a subscription bundle with given members.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Subscription-Bundle-Request"
            examples:
              Example:
                value:
                  subscription_bundle:
                    subscription_id: 5
                    member_ids:
                      - 2
                      - 3
                      - 4
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundles.CreateSubscriptionBundle(
                      new CreateSubscriptionBundleOperationRequest
                      {
                          Body = new CreateSubscriptionBundleRequest
                          {
                              SubscriptionBundle = new CreateSubscriptionBundle
                              {
                                  SubscriptionId = 5,
                                  MemberIds = [2, 3, 4],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionBundleResponse
              }

              catch (ApiException<CreateSubscriptionBundleError> ex)

              {
                  if (ex.Error.TryGetSubscriptionBundleCreateErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type SubscriptionBundleCreateErrorResponse
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
    Subscription-Bundle-Response:
      title: Subscription Bundle Response
      type: object
      properties:
        subscription_bundle:
          $ref: "#/components/schemas/Subscription-Bundle"
      required:
        - subscription_bundle
    Subscription-Bundle-Create-Error-Response:
      title: Subscription Bundle Create Error Response
      type: object
      properties:
        errors:
          oneOf:
            - $ref: "#/components/schemas/Subscription-Bundle-Members-Array-Error"
            - $ref: "#/components/schemas/Subscription-Bundle-Single-Error"
            - type: string
      required:
        - errors
    Create-Subscription-Bundle-Request:
      title: Create Subscription Bundle Request
      type: object
      properties:
        subscription_bundle:
          $ref: "#/components/schemas/Create-Subscription-Bundle"
      required:
        - subscription_bundle
    Create-Subscription-Bundle:
      title: Create Subscription Bundle
      type: object
      properties:
        subscription_id:
          type: integer
        member_ids:
          type: array
          items:
            type: integer
      required:
        - subscription_id
    Subscription-Bundle-Members-Array-Error:
      title: Subscription Bundle Members Array Error
      type: object
      properties:
        members:
          type: array
          items:
            type: string
      required:
        - members
    Subscription-Bundle-Single-Error:
      title: Subscription Bundle Single Error
      type: object
      properties:
        subscription_bundle:
          type: string
      required:
        - subscription_bundle
    Subscription-Bundle:
      title: Subscription Bundle
      type: object
      properties:
        uid:
          type: string
        customer_id:
          type: integer
        payment_profile:
          $ref: "#/components/schemas/Bundle-Payment-Profile"
        collection_mode:
          $ref: "#/components/schemas/Collection-Method"
        subscription_ids:
          type: array
          items:
            type: integer
        created_at:
          type: string
          format: date-time
    Bundle-Payment-Profile:
      title: Subscription Bundle Payment Profile
      type: object
      properties:
        id:
          type: integer
        first_name:
          type: string
        last_name:
          type: string
        masked_card_number:
          type: string
    Collection-Method:
      title: Collection Method
      type: string
      description: The type of payment collection to be used in the subscription. For
        legacy Statements Architecture valid options are - `invoice`,
        `automatic`. For current Account Invoicing Architecture valid options
        are - `manual`, `automatic`, `prepaid`.
      enum:
        - automatic
        - manual
        - prepaid
        - invoice
info:
  title: Ombrevix Billing
  version: "1.0"
```