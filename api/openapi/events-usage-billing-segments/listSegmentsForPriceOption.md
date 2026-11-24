# List Segments for a Price option (/api/openapi/events-usage-billing-segments/listSegmentsForPriceOption)

`GET /addons/{addon_id}/price_options/{price_option_id}/segments`

Lists segments created for a given price option, in order of creation.

You can pass `page` and `per_page` parameters in order to access all of the segments. By default it will return `30` records. You can set `per_page` to `200` at most.

You may specify addon and/or price option by using either the numeric ID or the `slug:gold` syntax.

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
  /addons/{addon_id}/price_options/{price_option_id}/segments:
    parameters:
      - schema:
          type: string
        name: addon_id
        in: path
        required: true
        description: ID or Slug for the Addon
      - schema:
          type: string
        name: price_option_id
        in: path
        required: true
        description: ID or Slug for the Price option belonging to the Addon
    get:
      summary: List Segments for a Price option
      operationId: listSegmentsForPriceOption
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Segments-Response"
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Event-Based-Billing-List-Segments-Errors"
      description: >-
        Lists segments created for a given price option, in order of creation.


        You can pass `page` and `per_page` parameters in order to access all of
        the segments. By default it will return `30` records. You can set
        `per_page` to `200` at most.


        You may specify addon and/or price option by using either the numeric ID
        or the `slug:gold` syntax.
      x-operation-settings:
        collectParameters: true
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page-default-30"
        - $ref: "#/components/parameters/list-segments-filter"
      tags:
        - Events-Usage Billing Segments
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.EventsUsageBillingSegments.ListSegmentsForPriceOption(
                      new ListSegmentsForPriceOptionRequest
                      {
                          AddonId = "some example string",
                          PriceOptionId = "some example string",
                          Page = 1,
                          PerPage = 50,
                      });
                  // TODO: Handle 'response' of type ListSegmentsResponse
              }

              catch (ApiException<ListSegmentsForPriceOptionError> ex)

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
    List-Segments-Response:
      title: List Segments Response
      type: object
      properties:
        segments:
          type: array
          items:
            $ref: "#/components/schemas/Segment"
    Event-Based-Billing-List-Segments-Errors:
      title: Event Based Billing List Segments Errors
      type: object
      properties:
        errors:
          type: object
          properties:
            per_page:
              $ref: "#/components/schemas/Error-Array-Response"
            price_option:
              $ref: "#/components/schemas/Error-Array-Response"
    List-Segments-Filter:
      title: List Segments Filter
      type: object
      properties:
        segment_property_1_value:
          type: string
          description: The value passed here would be used to filter segments. Pass a
            value related to `segment_property_1` on attached Metric. If empty
            string is passed, this filter would be rejected. Use in query
            `filter[segment_property_1_value]=EU`.
          example: EU
        segment_property_2_value:
          type: string
          description: The value passed here would be used to filter segments. Pass a
            value related to `segment_property_2` on attached Metric. If empty
            string is passed, this filter would be rejected.
        segment_property_3_value:
          type: string
          description: The value passed here would be used to filter segments. Pass a
            value related to `segment_property_3` on attached Metric. If empty
            string is passed, this filter would be rejected.
        segment_property_4_value:
          type: string
          description: The value passed here would be used to filter segments. Pass a
            value related to `segment_property_4` on attached Metric. If empty
            string is passed, this filter would be rejected.
      examples:
        Single-Segment:
          segment_property_1_value: EU
        Multiple-Segments:
          segment_property_1_value: EU
          segment_property_2_value: Willow
          segment_property_3_value: Violet
    Error-Array-Response:
      type: array
      items:
        type: string
    Segment:
      title: Segment
      type: object
      properties:
        id:
          type: integer
        addon_id:
          type: integer
        price_option_id:
          type: integer
        event_based_billing_metric_id:
          type: integer
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        segment_property_1_value:
          type:
            - string
            - number
            - integer
            - boolean
        segment_property_2_value:
          type:
            - string
            - number
            - integer
            - boolean
        segment_property_3_value:
          type:
            - string
            - number
            - integer
            - boolean
        segment_property_4_value:
          type:
            - string
            - number
            - integer
            - boolean
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        prices:
          type: array
          uniqueItems: false
          minItems: 1
          items:
            $ref: "#/components/schemas/Segment-Price"
    Pricing-Scheme:
      title: Pricing Scheme
      description: The identifier for the pricing scheme. See Product Addons for an
        overview of pricing schemes.
      type: string
      enum:
        - stairstep
        - volume
        - per_unit
        - tiered
    Segment-Price:
      title: Segment Price
      type: object
      properties:
        id:
          type: integer
        addon_id:
          type: integer
        starting_quantity:
          type: integer
        ending_quantity:
          type:
            - integer
            - "null"
        unit_price:
          type: string
        price_option_id:
          type: integer
        formatted_unit_price:
          type: string
        segment_id:
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
    per-page-default-30:
      name: per_page
      in: query
      description: >-
        This parameter indicates how many records to fetch in each request.
        Default value is 30. The maximum allowed values is 200; any per_page
        value over 200 will be changed to 200.

        Use in query `per_page=200`.
      schema:
        type: integer
        default: 30
        maximum: 200
        example: 50
    list-segments-filter:
      in: query
      name: filter
      description: Filter to use for List Segments for a Price option operation
      schema:
        $ref: "#/components/schemas/List-Segments-Filter"
      style: deepObject
      explode: true
info:
  title: Ombrevix Billing
  version: "1.0"
```