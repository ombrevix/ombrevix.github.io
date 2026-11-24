# Update Subscription bundle Members (/api/openapi/subscription-bundles/updateSubscriptionBundleMembers)

`PUT /subscription_bundles/{uid}`

Updates subscription bundle members.
`"member_ids"` should contain an array of both subscription IDs to set as group members and subscription IDs already present in the groups. Not including them will result in removing them from the subscription bundle. To clean up members, just leave the array empty.

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
  /subscription_bundles/{uid}:
    parameters:
      - $ref: "#/components/parameters/bundle-uid-path"
    put:
      summary: Update Subscription bundle Members
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
                      customer_id: 5
                      payment_profile:
                        id: 5
                        first_name: Nadia
                        last_name: Okafor
                        masked_card_number: Q739-C75X-XKG4-Q
                      collection_mode: automatic
                      subscription_ids:
                        - 1
                      created_at: 2021-01-21T05:47:38-05:00
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Subscription-Bundle-Update-Error-Response"
              examples:
                Example:
                  value:
                    errors:
                      members:
                        - lagoon_sierra
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: updateSubscriptionBundleMembers
      description: >-
        Updates subscription bundle members.

        `"member_ids"` should contain an array of both subscription IDs to set
        as group members and subscription IDs already present in the groups. Not
        including them will result in removing them from the subscription
        bundle. To clean up members, just leave the array empty.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Update-Subscription-Bundle-Request"
            examples:
              Example:
                value:
                  subscription_bundle:
                    member_ids:
                      - 1
                      - 2
                      - 3
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundles.UpdateSubscriptionBundleMembers(
                      new UpdateSubscriptionBundleMembersRequest
                      {
                          Uid = "some example string",
                          Body = new UpdateSubscriptionBundleRequest
                          {
                              SubscriptionBundle = new UpdateSubscriptionBundle
                              {
                                  MemberIds = [1, 2, 3],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionBundleResponse
              }

              catch (ApiException<UpdateSubscriptionBundleMembersError> ex)

              {
                  if (ex.Error.TryGetSubscriptionBundleUpdateErrorResponse(
                      out var error))
                  {
                      // TODO: Handle 'error' of type SubscriptionBundleUpdateErrorResponse
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
    Subscription-Bundle-Response:
      title: Subscription Bundle Response
      type: object
      properties:
        subscription_bundle:
          $ref: "#/components/schemas/Subscription-Bundle"
      required:
        - subscription_bundle
    Subscription-Bundle-Update-Error-Response:
      title: Subscription Bundle Update Error Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Subscription-Bundle-Update-Error"
    Update-Subscription-Bundle-Request:
      title: Update Subscription Bundle Request
      type: object
      properties:
        subscription_bundle:
          $ref: "#/components/schemas/Update-Subscription-Bundle"
      required:
        - subscription_bundle
    Update-Subscription-Bundle:
      title: Update Subscription Bundle
      type: object
      properties:
        member_ids:
          type: array
          items:
            type: integer
    Subscription-Bundle-Update-Error:
      title: Subscription Bundle Update Error
      type: object
      properties:
        members:
          type: array
          items:
            type: string
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