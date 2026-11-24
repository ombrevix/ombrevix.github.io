# List Subscription bundles (/api/openapi/subscription-bundles/listSubscriptionBundles)

`GET /subscription_bundles`

Lists subscription bundles for the workspace. The response is paginated and will return a `meta` key with pagination information.

#### Account Balance Information

Account balance information for the subscription bundles is not returned by default. If this information is desired, the `include[]=account_balances` parameter must be provided with the request.

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
    get:
      summary: List Subscription bundles
      tags:
        - Subscription Bundles
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Subscription-Bundles-Response"
              examples:
                Example:
                  value:
                    subscription_bundles:
                      - uid: grp_b740997zpw1g7
                        scheme: 1
                        customer_id: 33783527
                        payment_profile_id: 10728183
                        subscription_ids:
                          - 42768907
                          - 82370782
                        primary_subscription_id: 46749328
                        next_evaluation_at: 2021-05-05T16:00:21-04:00
                        state: active
                        cancel_when_period_ends: false
                        account_balances:
                          prepayments:
                            balance_minor: 0
                          service_credits:
                            balance_minor: 0
                          pending_discounts:
                            balance_minor: 0
                    meta:
                      current_page: 1
                      total_count: 1
      operationId: listSubscriptionBundles
      description: >-
        Lists subscription bundles for the workspace. The response is paginated
        and will return a `meta` key with pagination information.


        #### Account Balance Information


        Account balance information for the subscription bundles is not returned
        by default. If this information is desired, the
        `include[]=account_balances` parameter must be provided with the
        request.
      x-operation-settings:
        collectParameters: true
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - name: include
          in: query
          schema:
            type: array
            items:
              $ref: "#/components/schemas/Subscription-Bundles-List-Include"
            example:
              - account_balances
          style: form
          explode: true
          description: >-
            A list of additional information to include in the response. The
            following values are supported:


            - `account_balances`: Account balance information for the
            subscription bundles. Use in query: `include[]=account_balances`
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionBundles.ListSubscriptionBundles(
                      new ListSubscriptionBundlesRequest
                      {
                          Page = 1,
                          PerPage = 50,
                          Include = [
                              SubscriptionBundlesListInclude.AccountBalances,
                          ],
                      });
                  // TODO: Handle 'response' of type ListSubscriptionBundlesResponse
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
    List-Subscription-Bundles-Response:
      title: List Subscription Bundles Response
      type: object
      properties:
        subscription_bundles:
          type: array
          items:
            $ref: "#/components/schemas/List-Subscription-Bundles-Item"
        meta:
          $ref: "#/components/schemas/List-Subscription-Bundles-Meta"
    Subscription-Bundles-List-Include:
      title: Subscription Bundles List Include
      type: string
      enum:
        - account_balances
    List-Subscription-Bundles-Item:
      title: List Subscription Bundles Item
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
        account_balances:
          $ref: "#/components/schemas/Subscription-Bundle-Balances"
        bundle_type:
          $ref: "#/components/schemas/Bundle-Type"
    List-Subscription-Bundles-Meta:
      title: List Subscription Bundles Meta
      type: object
      properties:
        current_page:
          type: integer
        total_count:
          type: integer
    Subscription-Bundle-Balances:
      title: Subscription Bundle Balances
      type: object
      properties:
        prepayments:
          $ref: "#/components/schemas/Account-Balance"
        service_credits:
          $ref: "#/components/schemas/Account-Balance"
        open_invoices:
          $ref: "#/components/schemas/Account-Balance"
        pending_discounts:
          $ref: "#/components/schemas/Account-Balance"
    Bundle-Type:
      title: Bundle Type
      type: string
      enum:
        - single_customer
        - multiple_customers
    Account-Balance:
      title: Account Balance
      type: object
      properties:
        balance_minor:
          type: integer
          format: int64
          description: The balance minor.
        automatic_balance_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The automatic balance minor.
        manual_balance_minor:
          type:
            - integer
            - "null"
          format: int64
          description: The manual balance minor.
  parameters:
    page:
      name: page
      in: query
      description: >-
        Result records are organized in pages. By default, the first page of
        results is displayed. The page parameter specifies a page number of
        results to fetch. You can start navigating through the pages to consume
        the results. You do this by passing in a page parameter. Retrieve the
        next page by adding ?page=2 to the query string. If there are no results
        to return, then an empty result set will be returned.

        Use in query `page=1`.
      schema:
        type: integer
        default: 1
        minimum: 1
        example: 1
    per-page:
      name: per_page
      in: query
      description: >-
        This parameter indicates how many records to fetch in each request.
        Default value is 20. The maximum allowed values is 200; any per_page
        value over 200 will be changed to 200.

        Use in query `per_page=200`.
      schema:
        type: integer
        default: 20
        maximum: 200
        example: 50
info:
  title: Ombrevix Billing
  version: "1.0"
```