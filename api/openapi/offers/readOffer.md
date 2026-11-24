# Read Offer (/api/openapi/offers/readOffer)

`GET /offers/{offer_id}`

Returns a specific offer's attributes. This is different from listing all offers for a workspace, as it requires an `offer_id`.

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
  /offers/{offer_id}:
    parameters:
      - $ref: "#/components/parameters/offer-id-path"
    get:
      summary: Read Offer
      tags:
        - Offers
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Offer-Response"
              examples: {}
      operationId: readOffer
      description: Returns a specific offer's attributes. This is different from
        listing all offers for a workspace, as it requires an `offer_id`.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  var response = await client.Offers.ReadOffer(
                      new ReadOfferRequest { OfferId = 1 });
                  // TODO: Handle 'response' of type OfferResponse
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
    offer-id-path:
      name: offer_id
      schema:
        type: integer
      in: path
      required: true
      description: The Ombrevix id of the offer
  schemas:
    Offer-Response:
      title: Offer Response
      type: object
      properties:
        offer:
          $ref: "#/components/schemas/Offer"
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