# List Offers (/api/openapi/offers/listOffers)

`GET /offers`

Lists offers for a workspace.

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
  /offers:
    get:
      summary: List Offers
      tags:
        - Offers
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/List-Offers-Response"
              examples:
                Example:
                  value:
                    offers:
                      - id: 703
                        workspace_id: 29741
                        product_lineup_id: 6902268
                        product_lineup_name: Basalt
                        product_id: 992
                        product_name: Cobalt
                        product_price_minor: 1000
                        product_revisable_number: 0
                        product_price_option_id: 855
                        product_price_option_name: Basalt
                        name: Nimbus Maple
                        slug: nimbus
                        description: ""
                        created_at: 2018-08-03T09:56:11-05:00
                        updated_at: 2018-08-03T09:56:11-05:00
                        archived_at: null
                        offer_items:
                          - addon_id: 128844
                            addon_name: Amber Cobalt (GB)
                            addon_unit_price: "1.0"
                            price_option_id: 838342
                            price_option_name: Yarrow-tundra
                            starting_quantity: "0.0"
                            editable: false
                        offer_discounts:
                          - coupon_id: 380
                            coupon_code: GR8_Harbor
                            coupon_name: Birch-zephyr Basalt
                        offer_signup_pages:
                          - id: 917148
                            nickname: ember
                            enabled: true
                            return_url: ""
                            return_params: ""
                            url: https://sample-shop.pay.ombrevix.example/subscribe/hjpvhnw63tzy
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-List-Response"
              examples:
                Invalid-Page:
                  value:
                    errors:
                      - "cobalt onyx: -1"
      operationId: listOffers
      description: Lists offers for a workspace.
      x-operation-settings:
        collectParameters: true
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      parameters:
        - $ref: "#/components/parameters/page"
        - $ref: "#/components/parameters/per-page"
        - schema:
            type: boolean
            example: true
          in: query
          name: include_archived
          description: "Include archived products. Use in query: `include_archived=true`."
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Offers.ListOffers(
                      new ListOffersRequest
                      {
                          Page = 1,
                          PerPage = 50,
                          IncludeArchived = true,
                      });
                  // TODO: Handle 'response' of type ListOffersResponse
              }
              catch (ApiException<ListOffersError> ex)
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
  schemas:
    List-Offers-Response:
      title: List Offers Response
      type: object
      properties:
        offers:
          type: array
          items:
            $ref: "#/components/schemas/Offer"
    Error-List-Response:
      title: Error List Response
      type: object
      description: Error which contains list of messages.
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Response"
      required:
        - errors
    Error-Array-Response:
      type: array
      items:
        type: string
    Offer:
      title: Offer
      type: object
      properties:
        id:
          type: integer
        workspace_id:
          type: integer
        product_lineup_id:
          type: integer
        product_id:
          type: integer
        product_price_option_id:
          type: integer
        product_revisable_number:
          type: integer
        name:
          type: string
        slug:
          type: string
        description:
          type:
            - string
            - "null"
        created_at:
          type: string
          format: date-time
        updated_at:
          type: string
          format: date-time
        archived_at:
          format: date-time
          type:
            - string
            - "null"
        offer_items:
          type: array
          items:
            $ref: "#/components/schemas/Offer-Item"
        offer_discounts:
          type: array
          items:
            $ref: "#/components/schemas/Offer-Discount"
        product_lineup_name:
          type: string
        product_name:
          type: string
        product_price_option_name:
          type: string
        product_price_minor:
          type: integer
          format: int64
        offer_signup_pages:
          type: array
          items:
            $ref: "#/components/schemas/Offer-Signup-Page"
    Offer-Item:
      title: Offer Item
      type: object
      properties:
        addon_id:
          type: integer
        price_option_id:
          type: integer
        starting_quantity:
          type: string
        editable:
          type: boolean
        addon_unit_price:
          type: string
        addon_name:
          type: string
        price_option_name:
          type: string
        currency_prices:
          type: array
          items:
            $ref: "#/components/schemas/Currency-Price"
        interval:
          type: integer
          description: The numerical interval. e.g., an interval of '30' coupled with an
            interval_unit of day would mean this addon price option would renew
            every 30 days. This property is only available for workspaces with
            Multifrequency enabled.
        interval_unit:
          oneOf:
            - $ref: "#/components/schemas/Interval-Unit"
            - type: "null"
          description: A string representing the interval unit for this addon price
            option, either month or day. This property is only available for
            workspaces with Multifrequency enabled.
    Offer-Discount:
      title: Offer Discount
      type: object
      properties:
        coupon_code:
          type: string
        coupon_id:
          type: integer
        coupon_name:
          type: string
    Offer-Signup-Page:
      title: Offer Signup Page
      type: object
      properties:
        id:
          type: integer
        nickname:
          type: string
        enabled:
          type: boolean
        return_url:
          type: string
        return_params:
          type: string
        url:
          type: string
    Currency-Price:
      title: Currency Price
      type: object
      properties:
        id:
          type: integer
        currency:
          type: string
        price:
          type: number
        formatted_price:
          type: string
        price_id:
          type: integer
        price_option_id:
          type: integer
        product_price_option_id:
          type: integer
        role:
          $ref: "#/components/schemas/Currency-Price-Role"
    Interval-Unit:
      title: Interval Unit
      type: string
      enum:
        - day
        - month
    Currency-Price-Role:
      title: Currency Price Role
      type: string
      description: Role for the price.
      enum:
        - baseline
        - trial
        - initial
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