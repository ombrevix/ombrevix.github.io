# Create Single Segment (/api/openapi/events-usage-billing-segments/createSegment)

`POST /addons/{addon_id}/price_options/{price_option_id}/segments`

Creates a new segment for a addon with a segmented metric. It allows you to specify properties to bill upon and prices for each Segment. You can only pass as many "property_values" as the related Metric has segmenting properties defined.

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
    post:
      summary: Create Single Segment
      tags:
        - Events-Usage Billing Segments
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Segment-Response"
        "404":
          description: Not Found
        "422":
          description: Unprocessable Entity (WebDAV)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Event-Based-Billing-Segment-Errors"
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: createSegment
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Create-Segment-Request"
            examples:
              Create a Single Segment (related Metric has 2 segmented properties):
                value:
                  segment:
                    segment_property_1_value: Onyx
                    segment_property_2_value: Zephyr
                    pricing_scheme: volume
                    prices:
                      - starting_quantity: 1
                        ending_quantity: 10000
                        unit_price: 0.19
                      - starting_quantity: 10001
                        unit_price: 0.09
      description: >-
        Creates a new segment for a addon with a segmented metric. It allows you
        to specify properties to bill upon and prices for each Segment. You can
        only pass as many "property_values" as the related Metric has segmenting
        properties defined.


        You may specify addon and/or price option by using either the numeric ID
        or the `slug:gold` syntax.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Create a Single Segment (related Metric has 2 segmented properties): >-
              try

              {
                  var response = await client.EventsUsageBillingSegments.CreateSegment(
                      new CreateSegmentOperationRequest
                      {
                          AddonId = "some example string",
                          PriceOptionId = "some example string",
                          Body = new CreateSegmentRequest
                          {
                              Segment = new CreateSegment
                              {
                                  SegmentProperty1Value = "Onyx",
                                  SegmentProperty2Value = "Zephyr",
                                  PricingScheme = PricingScheme.Volume,
                                  Prices = [
                                      new CreateOrUpdateSegmentPrice
                                      {
                                          StartingQuantity = 1,
                                          EndingQuantity = 10000,
                                          UnitPrice = 0.19d,
                                      },
                                      new CreateOrUpdateSegmentPrice
                                      {
                                          StartingQuantity = 10001,
                                          UnitPrice = 0.09d,
                                      },
                                  ],
                              },
                          },
                      });
                  // TODO: Handle 'response' of type SegmentResponse
              }

              catch (ApiException<CreateSegmentError> ex)

              {
                  if (ex.Error.TryGetNoContent(out var error))
                  {
                      // TODO: Handle 'error' of type RawError
                  }
              }
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
    Segment-Response:
      title: Segment Response
      type: object
      properties:
        segment:
          $ref: "#/components/schemas/Segment"
    Event-Based-Billing-Segment-Errors:
      title: Event Based Billing Segment Errors
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Event-Based-Billing-Segment-Single-Error"
    Create-Segment-Request:
      title: Create Segment Request
      type: object
      properties:
        segment:
          $ref: "#/components/schemas/Create-Segment"
      required:
        - segment
    Create-Segment:
      title: Create Segment
      type: object
      properties:
        segment_property_1_value:
          type:
            - string
            - number
            - integer
            - boolean
          description: A value that will occur in your events that you want to bill upon.
            The type of the value depends on the property type in the related
            event based billing metric.
        segment_property_2_value:
          type:
            - string
            - number
            - integer
            - boolean
          description: A value that will occur in your events that you want to bill upon.
            The type of the value depends on the property type in the related
            event based billing metric.
        segment_property_3_value:
          type:
            - string
            - number
            - integer
            - boolean
          description: A value that will occur in your events that you want to bill upon.
            The type of the value depends on the property type in the related
            event based billing metric.
        segment_property_4_value:
          type:
            - string
            - number
            - integer
            - boolean
          description: A value that will occur in your events that you want to bill upon.
            The type of the value depends on the property type in the related
            event based billing metric.
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        prices:
          type: array
          items:
            $ref: "#/components/schemas/Create-Or-Update-Segment-Price"
      required:
        - pricing_scheme
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
    Create-Or-Update-Segment-Price:
      title: Create or Update Segment Price
      type: object
      properties:
        starting_quantity:
          type: integer
        ending_quantity:
          type: integer
        unit_price:
          type:
            - string
            - number
          description: The price can contain up to 8 decimal places. e.g., 1.00 or 0.0012
            or 0.00000065
      required:
        - unit_price
    Event-Based-Billing-Segment-Single-Error:
      title: Event Based Billing Segment Single Error
      type: object
      description: The key of the object would be a number (an index in the request
        array) where the error occurred. In the value object, the key represents
        the field and the value is an array with error messages. In most cases,
        this object would contain just one key.
      additionalProperties:
        $ref: "#/components/schemas/Error-Array-Map"
    Error-Array-Map:
      title: Error Array Map
      type: object
      additionalProperties:
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
info:
  title: Ombrevix Billing
  version: "1.0"
```