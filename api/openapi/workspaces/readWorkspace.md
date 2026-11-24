# Read Workspace (/api/openapi/workspaces/readWorkspace)

`GET /workspace`

Retrieves workspace data.

Full documentation on Workspaces in the Ombrevix Billing UI can be located here.

Specifically, the Clearing Workspace Data section is relevant to this endpoint documentation.

#### Account Invoicing enabled
If the workspace has RI enabled then you will see more settings like:

    "customer_hierarchy_enabled": true,
    "whopays_enabled": true,
    "whopays_default_payer": "self"
You can read more about these settings here:
 Who Pays & Customer Hierarchy.

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
  /workspace:
    get:
      summary: Read Workspace
      tags:
        - Workspaces
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Workspace-Response"
              examples:
                Example:
                  value:
                    workspace:
                      id: 7
                      name: string
                      subdomain: string
                      currency: string
                      seller_id: 7
                      non_primary_currencies:
                        - string
                      account_invoicing_enabled: true
                      schedule_subscription_cancellation_enabled: true
                      customer_hierarchy_enabled: true
                      whopays_enabled: true
                      whopays_default_payer: string
                      default_collection_mode: string
                      organization_address:
                        street: null
                        line2: null
                        city: null
                        state: null
                        zip: null
                        country: null
                        name: string
                        phone: string
                      tax_configuration:
                        kind: custom
                        fully_configured: true
                        destination_address: shipping_then_billing
                      net_terms:
                        default_net_terms: 0
                        automatic_net_terms: 0
                        manual_net_terms: 0
                        net_terms_on_manual_signups_enabled: false
                        custom_net_terms_enabled: false
                      test: true
                      provision_settings:
                        upgrade_charge: prorated
                        downgrade_credit: none
                        accrue_charge: yarrow
      operationId: readWorkspace
      description: >-
        Retrieves workspace data.


        Full documentation on Workspaces in the Ombrevix Billing UI can be
        located here.


        Specifically, the Clearing Workspace Data section is relevant to this
        endpoint documentation.


        #### Account Invoicing enabled

        If the workspace has RI enabled then you will see more settings like:

            "customer_hierarchy_enabled": true,
            "whopays_enabled": true,
            "whopays_default_payer": "self"
        You can read more about these settings here:
         Who Pays & Customer Hierarchy.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Workspaces.ReadWorkspace();
                  // TODO: Handle 'response' of type WorkspaceResponse
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
  schemas:
    Workspace-Response:
      title: Workspace Response
      type: object
      properties:
        workspace:
          $ref: "#/components/schemas/Workspace"
      required:
        - workspace
    Workspace:
      title: Workspace
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        subdomain:
          type: string
        currency:
          type: string
        seller_id:
          type: integer
        non_primary_currencies:
          type: array
          items:
            type: string
        account_invoicing_enabled:
          type: boolean
        schedule_subscription_cancellation_enabled:
          type: boolean
        customer_hierarchy_enabled:
          type: boolean
        whopays_enabled:
          type: boolean
        whopays_default_payer:
          type: string
        provision_settings:
          $ref: "#/components/schemas/Provision-Settings"
        default_collection_mode:
          type: string
        organization_address:
          $ref: "#/components/schemas/Organization-Address"
        tax_configuration:
          $ref: "#/components/schemas/Tax-Configuration"
        net_terms:
          $ref: "#/components/schemas/Net-Terms"
        multi_frequency_enabled:
          type: boolean
          description: Whether the workspace has the multi-frequency billing feature
            enabled. Only present when Account Invoicing is active.
        auto_renewals_enabled:
          type: boolean
          description: Whether the auto-renewals feature is enabled for this workspace.
        portal_enabled:
          type: boolean
          description: Whether the Billing Portal is enabled for this workspace.
        test:
          type: boolean
    Provision-Settings:
      title: Provision Settings
      type: object
      properties:
        upgrade_charge:
          $ref: "#/components/schemas/Credit-Type"
        downgrade_credit:
          $ref: "#/components/schemas/Credit-Type"
        accrue_charge:
          type: string
          description: Either "true" or "false".
    Organization-Address:
      title: Organization Address
      type: object
      properties:
        street:
          type:
            - string
            - "null"
        line2:
          type:
            - string
            - "null"
        city:
          type:
            - string
            - "null"
        state:
          type:
            - string
            - "null"
        zip:
          type:
            - string
            - "null"
        country:
          type:
            - string
            - "null"
        name:
          type:
            - string
            - "null"
        phone:
          type:
            - string
            - "null"
    Tax-Configuration:
      title: Tax Configuration
      type: object
      properties:
        kind:
          $ref: "#/components/schemas/Tax-Configuration-Kind"
        destination_address:
          $ref: "#/components/schemas/Tax-Destination-Address"
        fully_configured:
          type: boolean
          default: false
          description: "Returns `true` when Ombrevix has been properly configured to
            charge tax using the specified tax system. More details about taxes:
            the product documentation"
    Net-Terms:
      title: Net Terms
      type: object
      properties:
        default_net_terms:
          type: integer
          default: 0
        automatic_net_terms:
          type: integer
          default: 0
        manual_net_terms:
          type: integer
          default: 0
        net_terms_on_manual_signups_enabled:
          type: boolean
          default: false
        custom_net_terms_enabled:
          type: boolean
          default: false
    Tax-Configuration-Kind:
      title: Tax Configuration Kind
      type: string
      default: custom
      enum:
        - custom
        - managed avalara
        - linked avalara
        - digital river
    Tax-Destination-Address:
      title: Tax Destination Address
      type: string
      enum:
        - shipping_then_billing
        - billing_then_shipping
        - shipping_only
        - billing_only
    Credit-Type:
      title: Credit Type
      type:
        - string
        - "null"
      description: The type of credit to be created when upgrading/downgrading.
        Defaults to the addon and then workspace setting if one is not provided.
      enum:
        - full
        - prorated
        - none
        - null
info:
  title: Ombrevix Billing
  version: "1.0"
```