# Add Subscription to Group (/api/openapi/subscription-bundles/addSubscriptionToBundle)

`POST /subscriptions/{subscription_id}/bundle`

Adds an existing subscription to a subscription bundle. For workspaces making use of the Relationship Billing and Customer Hierarchy features, it is possible to add existing subscriptions to subscription bundles.

Passing `group` parameters with a `target` containing a `type` and optional `id` is all that's needed. When the `target` parameter specifies a `"customer"` or `"subscription"` that is already part of a hierarchy, the subscription will become a member of the customer's subscription bundle.  If the target customer or subscription is not part of a subscription bundle, a new group will be created and the subscription will become part of the group with the specified target customer set as the responsible payer for the group's subscriptions.

**Note:** In order to add an existing subscription to a subscription bundle, it must belong to either the same customer record as the target, or be within the same customer hierarchy.

Rather than specifying a customer, the `target` parameter could instead simply have a value of
* `"self"` which indicates the subscription will be paid for not by some other customer, but by the subscribing customer,
* `"parent"` which indicates the subscription will be paid for by the subscribing customer's parent within a customer hierarchy, or
* `"eldest"` which indicates the subscription will be paid for by the root-level customer in the subscribing customer's hierarchy.

To create a new subscription into a subscription bundle, reference the following:
Create Subscription in a Subscription bundle


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
  /subscriptions/{subscription_id}/bundle:
    parameters:
      - $ref: "#/components/parameters/subscription-id-path"
    post:
      summary: Add Subscription to Group
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
                      customer_id: 863054
                      payment_profile:
                        id: 85882
                        first_name: Oskar
                        last_name: Haddad
                        masked_card_number: 43QY-9CZH-VA99-LNRM
                      subscription_ids:
                        - 32988
                        - 33060
                        - 32986
                      created_at: 2018-08-30T17:14:30-04:00
      operationId: addSubscriptionToBundle
      description: >
        Adds an existing subscription to a subscription bundle. For workspaces
        making use of the Relationship Billing and Customer Hierarchy features,
        it is possible to add existing subscriptions to subscription bundles.


        Passing `group` parameters with a `target` containing a `type` and
        optional `id` is all that's needed. When the `target` parameter
        specifies a `"customer"` or `"subscription"` that is already part of a
        hierarchy, the subscription will become a member of the customer's
        subscription bundle.  If the target customer or subscription is not part
        of a subscription bundle, a new group will be created and the
        subscription will become part of the group with the specified target
        customer set as the responsible payer for the group's subscriptions.


        **Note:** In order to add an existing subscription to a subscription
        bundle, it must belong to either the same customer record as the target,
        or be within the same customer hierarchy.


        Rather than specifying a customer, the `target` parameter could instead
        simply have a value of

        * `"self"` which indicates the subscription will be paid for not by some
        other customer, but by the subscribing customer,

        * `"parent"` which indicates the subscription will be paid for by the
        subscribing customer's parent within a customer hierarchy, or

        * `"eldest"` which indicates the subscription will be paid for by the
        root-level customer in the subscribing customer's hierarchy.


        To create a new subscription into a subscription bundle, reference the
        following:

        Create Subscription in a Subscription bundle
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Add-To-Bundle-Request"
            examples:
              Example:
                value:
                  bundle:
                    target:
                      type: subscription
                      id: 48670
                    billing:
                      accrue: true
                      align_date: true
                      prorate: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundles.AddSubscriptionToBundle(
                      new AddSubscriptionToBundleRequest
                      {
                          SubscriptionId = 1,
                          Body = new AddSubscriptionToABundle
                          {
                              Bundle = new BundleSettings
                              {
                                  Target = new BundleTarget
                                  {
                                      Type = BundleTargetType.Subscription,
                                      Id = 48670,
                                  },
                                  Billing = new BundleBilling
                                  {
                                      Accrue = true,
                                      AlignDate = true,
                                      Prorate = true,
                                  },
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SubscriptionBundleResponse
              }

              catch (ApiException<RawError> ex)

              {
                  // TODO: Handle 'ex.Error' of type RawError
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
  schemas:
    Subscription-Bundle-Response:
      title: Subscription Bundle Response
      type: object
      properties:
        subscription_bundle:
          $ref: "#/components/schemas/Subscription-Bundle"
      required:
        - subscription_bundle
    Add-To-Bundle-Request:
      title: Add Subscription to a Bundle
      type: object
      properties:
        bundle:
          $ref: "#/components/schemas/Bundle-Settings"
    Bundle-Settings:
      title: Bundle Settings
      type: object
      properties:
        target:
          $ref: "#/components/schemas/Bundle-Target"
        billing:
          $ref: "#/components/schemas/Bundle-Billing"
      required:
        - target
    Bundle-Target:
      title: Bundle Target
      type: object
      description: Attributes of the target customer who will be the responsible payer
        of the created subscription. Required.
      properties:
        type:
          $ref: "#/components/schemas/Bundle-Target-Type"
        id:
          type: integer
          description: The id of the target customer or subscription to group the existing
            subscription with. Ignored and should not be included if type is
            "self", "parent", or "eldest".
      required:
        - type
    Bundle-Billing:
      title: Bundle Billing
      type: object
      description: "(Optional) Attributes related to billing date and accrual. Note:
        Only applicable for new subscriptions."
      properties:
        accrue:
          type: boolean
          default: false
          description: A flag indicating whether or not to accrue charges on the new
            subscription.
        align_date:
          type: boolean
          default: false
          description: A flag indicating whether or not to align the billing date of the
            new subscription with the billing date of the primary subscription
            of the hierarchy's default subscription bundle. Required to be true
            if prorate is also true.
        prorate:
          type: boolean
          default: false
          description: A flag indicating whether or not to prorate billing of the new
            subscription for the current period. A value of true is ignored
            unless align_date is also true.
    Bundle-Target-Type:
      title: Bundle Target Type
      type: string
      description: The type of object indicated by the id attribute.
      enum:
        - customer
        - subscription
        - self
        - parent
        - eldest
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