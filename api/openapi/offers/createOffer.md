# Create Offer (/api/openapi/offers/createOffer)

`POST /offers`

Creates an offer within your Ombrevix Billing workspace.

## Documentation

Offers allow you to package complicated combinations of products, addons and coupons into a convenient package which can then be subscribed to just like products.

Once an offer is defined it can be used as an alternative to the product when creating subscriptions.

Full documentation on how to use offers in the Ombrevix Billing UI can be located here.

## Using a Product Price option

You can optionally pass in a `product_price_option_id` that corresponds with the `product_id` and the offer will use that price option. If a `product_price_option_id` is not passed in, the product's default price option will be used.

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
    post:
      summary: Create Offer
      tags:
        - Offers
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Offer-Response"
              examples:
                Example:
                  value:
                    offer:
                      id: 4
                      workspace_id: 7
                      product_lineup_id: 9
                      product_lineup_name: Kestrel
                      product_id: 64
                      product_name: 30-Harbor Quartz Ember
                      product_price_minor: 2000
                      product_revisable_number: 0
                      name: Lagoon
                      slug: basalt_iris_quartz
                      description: A Nimbus Birch Nimbus
                      created_at: 2018-06-08T14:51:52-04:00
                      updated_at: 2018-06-08T14:51:52-04:00
                      archived_at: null
                      product_price_option_name: Basalt
                      offer_items:
                        - addon_id: 74
                          addon_name: Dune
                          addon_unit_price: "3.0"
                          price_option_id: 910
                          price_option_name: Basalt
                          starting_quantity: "1.0"
                          editable: false
                      offer_discounts:
                        - coupon_id: 4
                          coupon_code: 7446WW
                          coupon_name: IB Basalt
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error-Array-Map-Response"
              examples:
                Example:
                  value:
                    errors:
                      addons:
                        - "umber_quartz harbor an On/Prairie ember glacier delta
                          be '1' or '0': 24"
      x-operation-settings:
        ErrorTemplates:
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createOffer
      description: >-
        Creates an offer within your Ombrevix Billing workspace.


        ## Documentation


        Offers allow you to package complicated combinations of products, addons
        and coupons into a convenient package which can then be subscribed to
        just like products.


        Once an offer is defined it can be used as an alternative to the product
        when creating subscriptions.


        Full documentation on how to use offers in the Ombrevix Billing UI can
        be located here.


        ## Using a Product Price option


        You can optionally pass in a `product_price_option_id` that corresponds
        with the `product_id` and the offer will use that price option. If a
        `product_price_option_id` is not passed in, the product's default price
        option will be used.
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Offer-Request"
            examples:
              Example:
                value:
                  offer:
                    name: Lagoon
                    slug: basalt_iris_quartz
                    description: A Nimbus Birch Nimbus
                    product_id: 64
                    product_price_option_id: 368
                    addons:
                      - addon_id: 74
                        starting_quantity: 1
                    coupons:
                      - 7446WW
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Offers.CreateOffer(
                      new CreateOfferOperationRequest
                      {
                          Body = new CreateOfferRequest
                          {
                              Offer = new CreateOffer
                              {
                                  Name = "Lagoon",
                                  Slug = "basalt_iris_quartz",
                                  Description = "A Nimbus Birch Nimbus",
                                  ProductId = 64,
                                  ProductPriceOptionId = 368,
                                  Addons = [
                                      new CreateOfferAddon
                                      {
                                          AddonId = 74,
                                          StartingQuantity = 1,
                                      },
                                  ],
                                  Coupons = ["7446WW"],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type OfferResponse
              }
              catch (ApiException<CreateOfferError> ex)
              {
                  if (ex.Error.TryGetErrorArrayMapResponse(out var error))
                  {
                      // TODO: Handle 'error' of type ErrorArrayMapResponse
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
    Offer-Response:
      title: Offer Response
      type: object
      properties:
        offer:
          $ref: "#/components/schemas/Offer"
    Error-Array-Map-Response:
      title: Error Array Map Response
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Error-Array-Map"
    Create-Offer-Request:
      title: Create Offer Request
      type: object
      properties:
        offer:
          $ref: "#/components/schemas/Create-Offer"
      required:
        - offer
    Create-Offer:
      title: Create Offer
      type: object
      properties:
        name:
          type: string
        slug:
          type: string
        description:
          type: string
        product_id:
          type: integer
        product_price_option_id:
          type: integer
        addons:
          type: array
          items:
            $ref: "#/components/schemas/Create-Offer-Addon"
        coupons:
          type: array
          items:
            type: string
      required:
        - name
        - slug
        - product_id
    Create-Offer-Addon:
      title: Create Offer Addon
      type: object
      properties:
        addon_id:
          type: integer
        price_option_id:
          type: integer
        starting_quantity:
          type: integer
    Error-Array-Map:
      title: Error Array Map
      type: object
      additionalProperties:
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
info:
  title: Ombrevix Billing
  version: "1.0"
```