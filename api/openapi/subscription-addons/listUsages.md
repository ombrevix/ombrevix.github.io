# List Usages (/api/openapi/subscription-addons/listUsages)

`GET /subscriptions/{subscription_id_or_reference}/addons/{addon_id}/usages`

Lists usages associated with a subscription for a particular metered addon. This will display the previously recorded addons for a subscription.

This endpoint is not compatible with seat-based addons.

## Since Date and Until Date Usage

Note: The `since_date` and `until_date` attributes each default to midnight on the date specified. For example, in order to list usages for January 20th, you would need to append the following to the URL.

```
?since_date=2016-01-20&until_date=2016-01-21
```

## Read Usage by Slug

Use this endpoint to read the previously recorded addons for a subscription.  You can now specify either the addon id (integer) or the addon slug prefixed by "slug:" to specify the unique identifier for the addon you are working with.

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
  /subscriptions/{subscription_id_or_reference}/addons/{addon_id}/usages:
    parameters:
      - schema:
          oneOf:
            - type: integer
              description: Subscription ID (e.g, 2222458)
            - type: string
              description: Subscription reference (e.g., 'customer-123-main'
        name: subscription_id_or_reference
        in: path
        required: true
        description: 'Either the Ombrevix Billing subscription ID (integer) or the
          subscription reference (string). Important: In cases where a numeric
          string value matches both an existing subscription ID and an existing
          subscription reference, the system will prioritize the subscription ID
          lookup. For example, if both subscription ID 123 and subscription
          reference "123" exist, passing "123" will return the subscription with
          ID 123.'
      - schema:
          oneOf:
            - type: integer
            - type: string
        name: addon_id
        in: path
        required: true
        description: Either the Ombrevix Billing id for the addon or the addon's slug
          prefixed by `slug:`
    get:
      summary: List Usages
      tags:
        - Subscription Addons
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Usage-Response"
              examples:
                Example:
                  value:
                    - usage:
                        id: 505037015
                        memo: "20"
                        created_at: 2018-08-03T11:58:42-05:00
                        price_option_id: 823039
                        quantity: "20.0"
                        addon_id: 860209
                        addon_slug: kestrel
                        subscription_id: 46684211
                    - usage:
                        id: 263873845
                        memo: "10"
                        created_at: 2018-08-03T11:58:29-05:00
                        price_option_id: 823039
                        quantity: "10.0"
                        addon_id: 860209
                        addon_slug: kestrel
                        subscription_id: 46684211
      operationId: listUsages
      description: >-
        Lists usages associated with a subscription for a particular metered
        addon. This will display the previously recorded addons for a
        subscription.


        This endpoint is not compatible with seat-based addons.


        ## Since Date and Until Date Usage


        Note: The `since_date` and `until_date` attributes each default to
        midnight on the date specified. For example, in order to list usages for
        January 20th, you would need to append the following to the URL.


        ```

        ?since_date=2016-01-20&until_date=2016-01-21

        ```


        ## Read Usage by Slug


        Use this endpoint to read the previously recorded addons for a
        subscription.  You can now specify either the addon id (integer) or the
        addon slug prefixed by "slug:" to specify the unique identifier for the
        addon you are working with.
      x-operation-settings:
        collectParameters: true
      parameters:
        - schema:
            type: integer
            format: int64
          in: query
          name: since_id
          description: Returns usages with an id greater than or equal to the one specified.
        - schema:
            type: integer
            format: int64
          in: query
          name: max_id
          description: Returns usages with an id less than or equal to the one specified.
        - schema:
            type: string
            format: date
          in: query
          name: since_date
          description: Returns usages with a created_at date greater than or equal to
            midnight (12:00 AM) on the date specified.
        - schema:
            type: string
            format: date
          in: query
          name: until_date
          description: Returns usages with a created_at date less than or equal to
            midnight (12:00 AM) on the date specified.
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.SubscriptionAddons.ListUsages(
                      new ListUsagesRequest
                      {
                          SubscriptionIdOrReference = 1,
                          AddonId = 1,
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type IReadOnlyList<UsageResponse>
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
    Usage-Response:
      title: Usage Response
      type: object
      properties:
        usage:
          $ref: "#/components/schemas/Usage"
      required:
        - usage
    Usage:
      title: Usage
      type: object
      properties:
        id:
          type: integer
          format: int64
          minimum: 0
        memo:
          type:
            - string
            - "null"
        created_at:
          type: string
          format: date-time
        price_option_id:
          type: integer
        quantity:
          type:
            - integer
            - string
        overage_quantity:
          type: integer
        addon_id:
          type: integer
        addon_slug:
          type: string
        subscription_id:
          type: integer
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