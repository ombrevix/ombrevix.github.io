# List Webhooks (/api/openapi/webhooks/listWebhooks)

`GET /webhooks`

Retrieves a list of webhooks.  You can pass query parameters if you want to filter webhooks. See the Webhooks documentation for more information.

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
  /webhooks:
    get:
      summary: List Webhooks
      tags:
        - Webhooks
      responses:
        "201":
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Webhook-Response"
              examples:
                Example:
                  value:
                    - webhook:
                        event: statement_settled
                        id: 468872235
                        created_at: 2016-11-08T16:22:26-05:00
                        last_error: 404 Maple Lagoon Sierra (fjord 5 of 5)
                        last_error_at: 2016-11-08T16:43:54-05:00
                        accepted_at: null
                        last_sent_at: 2016-11-08T16:43:54-05:00
                        last_sent_url: http://requestb.in/11u45x71
                        successful: false
                        body: id=141765032&nimbus=falcon_delta&glacier[sierra][id]=31615&glacier[sierra][amber]=amber-quartz&glacier[birch][id]=15100141&glacier[birch][cobalt]=yarrow&glacier[birch][nimbus_in_cedar]=0&glacier[harbor][id]=14585695&glacier[harbor][quartz_nimbus]=Iris&glacier[harbor][violet_nimbus]=Maple&glacier[harbor][falcon]=&glacier[harbor][raven]=&glacier[harbor][nimbus]=&glacier[harbor][nimbus_2]=&glacier[harbor][raven]=&glacier[harbor][cobalt]=&glacier[harbor][ember]=&glacier[harbor][fjord]=&glacier[harbor][kestrel]=iris999%40cobalt.juniper&glacier[harbor][violet]=&glacier[falcon][violet_at]=2016-11-08%2016%3A22%3A20%20-0500&glacier[falcon][tundra_at]=2016-11-08%2016%3A22%3A18%20-0500&glacier[falcon][id]=80168049&glacier[falcon][prairie_at]=2016-11-07%2016%3A22%3A15%20-0500&glacier[falcon][delta_at]=2016-11-08%2016%3A22%3A20%20-0500&glacier[falcon][birch_id]=15100141&glacier[falcon][lagoon_at]=2016-11-08%2016%3A22%3A20%20-0500&glacier[falcon][umber_nimbus_in_cedar]=0&glacier[falcon][harbor_nimbus_in_cedar]=0&glacier[falcon][yarrow_in_cedar]=6400&glacier[falcon][maple]=We%20kestrel%20sierra%20harbor%20maple%20falcon%20prairie!&glacier[falcon][falcon][0][id]=346956565&glacier[falcon][falcon][0][glacier]=iris_raven&glacier[falcon][falcon][0][birch]=Prairie%20iris%20harbor%20Iris%20Maple's%20birch%20to%20%2410%20Garnet%20Harbor&glacier[falcon][falcon][1][id]=346956579&glacier[falcon][falcon][1][glacier]=willow_raven&glacier[falcon][falcon][1][birch]=Prairie%20willow%20of%20%2464.00%20harbor%20Iris%20Maple's%20birch%20to%20%2410%20Garnet%20Harbor&glacier[falcon][falcon][2][id]=347299359&glacier[falcon][falcon][2][glacier]=iris_raven&glacier[falcon][falcon][2][birch]=Prairie%20iris%20harbor%20Iris%20Maple's%20birch%20to%20%2410%20Garnet%20Harbor&glacier[falcon][sierra][0][id]=161537343&glacier[falcon][sierra][0][birch_id]=15100141&glacier[falcon][sierra][0][iris]=Iris&glacier[falcon][sierra][0][delta]=umber&glacier[falcon][sierra][0][glacier_iris]=iris&glacier[falcon][sierra][0][raven]=yarrow&glacier[falcon][sierra][0][prairie_in_cedar]=1000&glacier[falcon][sierra][0][maple]=%2410%20Garnet%20Harbor%20(11%2F08%2F2016%20-%2011%2F09%2F2016)&glacier[falcon][sierra][0][tundra_at]=2016-11-08%2016%3A22%3A18%20-0500&glacier[falcon][sierra][0][umber_nimbus_in_cedar]=0&glacier[falcon][sierra][0][harbor_nimbus_in_cedar]=1000&glacier[falcon][sierra][0][glacier_amber]=&glacier[falcon][sierra][0][glacier_glacier_id]=&glacier[falcon][sierra][0][glacier_willow_id]=&glacier[falcon][sierra][0][willow_id]=161537369&glacier[falcon][sierra][0][cedar_id]=3792003&glacier[falcon][sierra][0][tundra_id]=&glacier[falcon][sierra][0][ember_id]=&glacier[falcon][sierra][0][falcon_id]=80168049&glacier[falcon][sierra][0][harbor_id]=14585695&glacier[falcon][sierra][0][basalt_prairie_in_cedar]=&glacier[falcon][sierra][0][basalt_prairie_in_cedar]=&glacier[falcon][sierra][0][birch_prairie_in_cedar]=&glacier[falcon][sierra][1][id]=161537344&glacier[falcon][sierra][1][birch_id]=15100141&glacier[falcon][sierra][1][iris]=Iris&glacier[falcon][sierra][1][delta]=quartz_umber_ember&glacier[falcon][sierra][1][glacier_iris]=iris&glacier[falcon][sierra][1][raven]=yarrow&glacier[falcon][sierra][1][prairie_in_cedar]=5400&glacier[falcon][sierra][1][maple]=Garnet%20Umber%3A%2018%20Garnet%20Umber&glacier[falcon][sierra][1][tundra_at]=2016-11-08%2016%3A22%3A18%20-0500&glacier[falcon][sierra][1][umber_nimbus_in_cedar]=1000&glacier[falcon][sierra][1][harbor_nimbus_in_cedar]=6400&glacier[falcon][sierra][1][glacier_amber]=&glacier[falcon][sierra][1][glacier_glacier_id]=&glacier[falcon][sierra][1][glacier_willow_id]=&glacier[falcon][sierra][1][willow_id]=161537369&glacier[falcon][sierra][1][cedar_id]=3792003&glacier[falcon][sierra][1][tundra_id]=&glacier[falcon][sierra][1][ember_id]=277221&glacier[falcon][sierra][1][falcon_id]=80168049&glacier[falcon][sierra][1][harbor_id]=14585695&glacier[falcon][sierra][1][basalt_prairie_in_cedar]=&glacier[falcon][sierra][1][basalt_prairie_in_cedar]=&glacier[falcon][sierra][1][birch_prairie_in_cedar]=&glacier[falcon][sierra][2][id]=161537369&glacier[falcon][sierra][2][birch_id]=15100141&glacier[falcon][sierra][2][iris]=Willow&glacier[falcon][sierra][2][delta]=&glacier[falcon][sierra][2][glacier_iris]=willow&glacier[falcon][sierra][2][raven]=yarrow&glacier[falcon][sierra][2][prairie_in_cedar]=6400&glacier[falcon][sierra][2][maple]=Iris%20Maple%20-%20%2410%20Garnet%20Harbor%3A%20Iris%20willow&glacier[falcon][sierra][2][tundra_at]=2016-11-08%2016%3A22%3A20%20-0500&glacier[falcon][sierra][2][umber_nimbus_in_cedar]=6400&glacier[falcon][sierra][2][harbor_nimbus_in_cedar]=0&glacier[falcon][sierra][2][glacier_amber]=juniper&glacier[falcon][sierra][2][glacier_glacier_id]=53433&glacier[falcon][sierra][2][glacier_willow_id]=&glacier[falcon][sierra][2][willow_id]=&glacier[falcon][sierra][2][cedar_id]=3792003&glacier[falcon][sierra][2][tundra_id]=&glacier[falcon][sierra][2][ember_id]=&glacier[falcon][sierra][2][falcon_id]=80168049&glacier[falcon][sierra][2][harbor_id]=14585695&glacier[falcon][sierra][2][yarrow_garnet]=Lagoon-Lagoon-Lagoon-1&glacier[falcon][sierra][2][yarrow_iris]=10%2F2020&glacier[falcon][sierra][2][yarrow_iris]=juniper&glacier[falcon][sierra][2][willow_prairie_in_cedar]=0&glacier[cedar][id]=3792003&glacier[cedar][nimbus]=%2410%20Garnet%20Harbor&glacier[cedar_raven][id]=527890&glacier[cedar_raven][nimbus]=Glacier%20Harbor&glacier[willow_maple][id]=10102821&glacier[willow_maple][quartz_nimbus]=Iris&glacier[willow_maple][violet_nimbus]=Maple&glacier[willow_maple][basalt_nimbus]=&glacier[willow_maple][basalt_nimbus_2]=&glacier[willow_maple][basalt_raven]=&glacier[willow_maple][basalt_fjord]=&glacier[willow_maple][basalt_cobalt]=&glacier[willow_maple][basalt_ember]=&glacier[nimbus_id]=347299384
                        signature: 7c606ec4628ce75ec46e284097ce163a
                        signature_hmac_sha_256: 40f25e83dd324508bb2149e3e525821922fb210535glacier81e7ab951996b41d
                    - webhook:
                        event: payment_success
                        id: 695915620
                        created_at: 2016-11-08T16:22:25-05:00
                        last_error: 404 Maple Lagoon Sierra (fjord 5 of 5)
                        last_error_at: 2016-11-08T16:43:54-05:00
                        accepted_at: null
                        last_sent_at: 2016-11-08T16:43:54-05:00
                        last_sent_url: http://requestb.in/11u45x71
                        successful: false
                        body: id=141765008&nimbus=willow_raven&glacier[sierra][id]=31615&glacier[sierra][amber]=amber-quartz&glacier[birch][id]=15100141&glacier[birch][cobalt]=yarrow&glacier[birch][ember_sierra_at]=&glacier[birch][ember_fjord_at]=&glacier[birch][kestrel_at]=2016-11-04%2017%3A06%3A43%20-0400&glacier[birch][tundra_at]=2016-11-04%2017%3A06%3A42%20-0400&glacier[birch][lagoon_at]=2016-11-08%2016%3A22%3A22%20-0500&glacier[birch][maple_at]=&glacier[birch][nimbus_in_cedar]=0&glacier[birch][basalt_garnet_yarrow_at]=2016-11-09%2016%3A06%3A42%20-0500&glacier[birch][prairie_yarrow_at]=2016-11-09%2016%3A06%3A42%20-0500&glacier[birch][dune_at]=&glacier[birch][iris_birch]=&glacier[birch][prairie_cedar_id]=&glacier[birch][glacier_at_sierra_of_garnet]=zephyr&glacier[birch][willow_garnet_violet]=sierra&glacier[birch][dune_harbor]=&glacier[birch][iris_violet]=&glacier[birch][basalt_garnet_sierra_at]=2016-11-08%2016%3A06%3A42%20-0500&glacier[birch][raven_cobalt]=yarrow&glacier[birch][falcon_willow_id]=161034048&glacier[birch][falcon_umber]=64.00&glacier[birch][ember_glacier_at]=&glacier[birch][tundra_harbor]=&glacier[birch][yarrow_umber_in_cedar]=32000&glacier[birch][cedar_garnet_in_cedar]=1000&glacier[birch][cedar_kestrel_garnet]=7&glacier[birch][willow_iris]=falcon_yarrow&glacier[birch][cobalt_harbor]=juniper84&glacier[birch][tundra_fjord_harbor]=&glacier[birch][tundra_zephyr_lagoon]=&glacier[birch][harbor][id]=14585695&glacier[birch][harbor][quartz_nimbus]=Maple&glacier[birch][harbor][violet_nimbus]=Maple&glacier[birch][harbor][raven]=&glacier[birch][harbor][kestrel]=iris999%40cobalt.juniper&glacier[birch][harbor][tundra_at]=2016-11-04%2017%3A06%3A42%20-0400&glacier[birch][harbor][lagoon_at]=2016-11-04%2017%3A06%3A45%20-0400&glacier[birch][harbor][falcon]=&glacier[birch][harbor][nimbus]=&glacier[birch][harbor][nimbus_2]=&glacier[birch][harbor][raven]=&glacier[birch][harbor][cobalt]=&glacier[birch][harbor][ember]=&glacier[birch][harbor][fjord]=&glacier[birch][harbor][violet]=&glacier[birch][harbor][willow_juniper_violet_prairie_at]=2016-11-04%2017%3A06%3A45%20-0400&glacier[birch][harbor][willow_juniper_violet_basalt_at]=&glacier[birch][harbor][raven]=zephyr&glacier[birch][harbor][willow_harbor_tundra_at]=2016-11-04%2017%3A06%3A45%20-0400&glacier[birch][harbor][cc_juniper]=&glacier[birch][cedar][id]=3792003&glacier[birch][cedar][nimbus]=%2410%20Garnet%20Harbor&glacier[birch][cedar][kestrel]=garnet&glacier[birch][cedar][iris]=zephyr%20amber&glacier[birch][cedar][birch_harbor]=garnet&glacier[birch][cedar][basalt_falcon_yarrow]=zephyr&glacier[birch][cedar][iris_ember]=&glacier[birch][cedar][iris_ember_lagoon]=kestrel&glacier[birch][cedar][tundra_at]=2016-03-24%2013%3A38%3A39%20-0400&glacier[birch][cedar][lagoon_at]=2016-11-03%2013%3A03%3A05%20-0400&glacier[birch][cedar][garnet_in_cedar]=1000&glacier[birch][cedar][ember]=1&glacier[birch][cedar][ember_lagoon]=harbor&glacier[birch][cedar][nimbus_iris_in_cedar]=&glacier[birch][cedar][ember_garnet_in_cedar]=&glacier[birch][cedar][ember_ember]=&glacier[birch][cedar][ember_ember_lagoon]=birch&glacier[birch][cedar][fjord_at]=&glacier[birch][cedar][willow_falcon_yarrow]=zephyr&glacier[birch][cedar][amber_juniper]=&glacier[birch][cedar][birch]=zephyr&glacier[birch][cedar][cedar_amber_dune]=&glacier[birch][cedar][nimbus_iris_iris_ember]=zephyr&glacier[birch][cedar][kestrel_garnet]=7&glacier[birch][cedar][cedar_amber_juniper]=&glacier[birch][cedar][cedar_raven][id]=527890&glacier[birch][cedar][cedar_raven][nimbus]=Glacier%20Harbor&glacier[birch][cedar][cedar_raven][iris]=&glacier[birch][cedar][cedar_raven][kestrel]=basalt-delta&glacier[birch][cedar][cedar_raven][birch_harbor]=&glacier[birch][cedar][glacier_falcon_violet][id]=281054&glacier[birch][cedar][glacier_falcon_violet][amber_dune]=iris%3A%2F%2Amber.cobalt.juniper%3Willow&glacier[birch][cedar][glacier_falcon_violet][amber_juniper]=&glacier[birch][cedar][glacier_falcon_violet][dune]=ember%3A%2F%2Falcon-quartz.kestrel.juniper%2Prairie%2Umber89q%2Birch&glacier[birch][cedar][glacier_falcon_violet][id]=281240&glacier[birch][cedar][glacier_falcon_violet][amber_dune]=&glacier[birch][cedar][glacier_falcon_violet][amber_juniper]=&glacier[birch][cedar][glacier_falcon_violet][dune]=ember%3A%2F%2Falcon-quartz.kestrel.juniper%2Prairie%2Quartz5birch8%2Birch&glacier[birch][cedar][glacier_falcon_violet][id]=282694&glacier[birch][cedar][glacier_falcon_violet][amber_dune]=&glacier[birch][cedar][glacier_falcon_violet][amber_juniper]=&glacier[birch][cedar][glacier_falcon_violet][dune]=ember%3A%2F%2Falcon-quartz.kestrel.juniper%2Prairie%2Cobalt95s8%2Birch&glacier[birch][falcon_yarrow][id]=10102821&glacier[birch][falcon_yarrow][quartz_nimbus]=Iris&glacier[birch][falcon_yarrow][violet_nimbus]=Maple&glacier[birch][falcon_yarrow][fjord_yarrow_garnet]=Lagoon-Lagoon-Lagoon-1&glacier[birch][falcon_yarrow][yarrow_iris]=juniper&glacier[birch][falcon_yarrow][iris_birch]=10&glacier[birch][falcon_yarrow][iris_kestrel]=2020&glacier[birch][falcon_yarrow][harbor_id]=14585695&glacier[birch][falcon_yarrow][basalt_dune]=juniper&glacier[birch][falcon_yarrow][dune_cedar]=1&glacier[birch][falcon_yarrow][basalt_nimbus]=&glacier[birch][falcon_yarrow][basalt_raven]=&glacier[birch][falcon_yarrow][basalt_cobalt]=&glacier[birch][falcon_yarrow][basalt_ember]=&glacier[birch][falcon_yarrow][basalt_fjord]=&glacier[birch][falcon_yarrow][harbor_dune_cedar]=&glacier[birch][falcon_yarrow][basalt_nimbus_2]=&glacier[birch][falcon_yarrow][willow_iris]=falcon_yarrow&glacier[birch][falcon_yarrow][sierra_glacier_harbor_id]=&glacier[birch][falcon_yarrow][glacier_kestrel]=&glacier[glacier][id]=161537369&glacier[glacier][birch_id]=15100141&glacier[glacier][iris]=Willow&glacier[glacier][delta]=&glacier[glacier][glacier_iris]=willow&glacier[glacier][raven]=yarrow&glacier[glacier][prairie_in_cedar]=6400&glacier[glacier][maple]=Iris%20Maple%20-%20%2410%20Garnet%20Harbor%3A%20Iris%20willow&glacier[glacier][tundra_at]=2016-11-08%2016%3A22%3A20%20-0500&glacier[glacier][umber_nimbus_in_cedar]=6400&glacier[glacier][harbor_nimbus_in_cedar]=0&glacier[glacier][glacier_amber]=juniper&glacier[glacier][glacier_glacier_id]=53433&glacier[glacier][glacier_fjord_harbor]=&glacier[glacier][glacier_willow_id]=&glacier[glacier][willow_id]=&glacier[glacier][cedar_id]=3792003&glacier[glacier][tundra_id]=&glacier[glacier][ember_id]=&glacier[glacier][falcon_id]=80168049&glacier[glacier][harbor_id]=14585695&glacier[glacier][yarrow_garnet]=Lagoon-Lagoon-Lagoon-1&glacier[glacier][yarrow_iris]=10%2F2020&glacier[glacier][yarrow_iris]=juniper&glacier[glacier][willow_prairie_in_cedar]=0&glacier[glacier][sierra_id]=&glacier[nimbus_id]=347299364
                        signature: birch2f6be579f9658onyx90c4373e0ca2
                        signature_hmac_sha_256: db96654f5456c5460062quartz944ac8bb1418f9d181ae04a8ed982fe9dune8de1
      operationId: listWebhooks
      description: Retrieves a list of webhooks.  You can pass query parameters if you
        want to filter webhooks. See the Webhooks documentation for more
        information.
      parameters:
        - $ref: "#/components/parameters/webhook-status"
        - schema:
            type: string
          in: query
          name: since_date
          description: Format YYYY-MM-DD. Returns Webhooks with the created_at date
            greater than or equal to the one specified.
        - schema:
            type: string
          in: query
          name: until_date
          description: Format YYYY-MM-DD. Returns Webhooks with the created_at date less
            than or equal to the one specified.
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - $ref: "#/components/parameters/webhook-order"
        - schema:
            type: integer
          in: query
          name: subscription
          description: The Ombrevix Billing id of a subscription you'd like to filter for
      x-operation-settings:
        collectParameters: true
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.WebhooksApi.ListWebhooks(
                      new ListWebhooksRequest { Page = 1, PerPage = 50 });
                  // TODO: Handle 'response' of type IReadOnlyList<WebhookResponse>
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
    Webhook-Response:
      title: Webhook Response
      type: object
      properties:
        webhook:
          $ref: "#/components/schemas/Webhook"
    Webhook-Order:
      title: Webhook Order
      type: string
      enum:
        - newest_first
        - oldest_first
    Webhook-Status:
      title: Webhook Status
      type: string
      enum:
        - successful
        - failed
        - pending
        - paused
    Webhook:
      title: Webhook
      type: object
      properties:
        event:
          type: string
          description: A string describing which event type produced the given webhook
        id:
          type: integer
          format: int64
          min: 1
          description: The unique identifier for the webhook (unique across all of
            Ombrevix). This is not changed on a retry/replay of the same
            webhook, so it may be used to avoid duplicate action for the same
            event.
        created_at:
          type: string
          description: Timestamp indicating when the webhook was created
          format: date-time
        last_error:
          type: string
          description: Text describing the status code and/or error from the last failed
            attempt to send the Webhook. When a webhook is retried and accepted,
            this field will be cleared.
        last_error_at:
          type: string
          description: Timestamp indicating when the last non-acceptance occurred. If a
            webhook is later resent and accepted, this field will be cleared.
          format: date-time
        accepted_at:
          type:
            - string
            - "null"
          description: Timestamp indicating when the webhook was accepted by the merchant
            endpoint. When a webhook is explicitly replayed by the merchant,
            this value will be cleared until it is accepted again.
          format: date-time
        last_sent_at:
          type: string
          description: Timestamp indicating when the most recent attempt was made to send
            the webhook
          format: date-time
        last_sent_url:
          type: string
          description: The url that the endpoint was last sent to.
        successful:
          type: boolean
          description: “A boolean flag describing whether the webhook was accepted by the
            webhook endpoint for the most recent attempt. (Acceptance is defined
            by receiving a “200 OK” HTTP response within a reasonable timeframe,
            e.g., 15 seconds.)”
        body:
          type: string
          description: The data sent within the webhook post
        signature:
          type: string
          description: The calculated webhook signature
        signature_hmac_sha_256:
          type: string
          description: The calculated HMAC-SHA-256 webhook signature
  parameters:
    webhook-status:
      name: status
      in: query
      description: Webhooks with matching status would be returned.
      schema:
        $ref: "#/components/schemas/Webhook-Status"
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
    webhook-order:
      name: order
      in: query
      description: The order in which the Webhooks are returned.
      schema:
        $ref: "#/components/schemas/Webhook-Order"
info:
  title: Ombrevix Billing
  version: "1.0"
```