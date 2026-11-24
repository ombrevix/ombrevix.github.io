# Bulk Update Segments (/api/openapi/events-usage-billing-segments/bulkUpdateSegments)

`PUT /addons/{addon_id}/price_options/{price_option_id}/segments/bulk`

Updates multiple segments in one request. The array of segments can contain up to `1000` records.

If any of the records contain an error the whole request would fail and none of the requested segments get updated. The error response contains a message for only the one segment that failed validation, with the corresponding index in the array.

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
  /addons/{addon_id}/price_options/{price_option_id}/segments/bulk:
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
    put:
      summary: Bulk Update Segments
      tags:
        - Events-Usage Billing Segments
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
                $ref: "#/components/schemas/Event-Based-Billing-Segment"
              examples:
                Error response when Pricing Scheme is missing:
                  value:
                    errors:
                      segments:
                        "1":
                          pricing_scheme:
                            - "Maple tundra: cobalt be lagoon"
      x-operation-settings:
        ErrorTemplates:
          "404": Not Found:'{$response.body}'
          "422": "HTTP Response Not OK. Status code: {$statusCode}. Response:
            '{$response.body}'."
      operationId: bulkUpdateSegments
      requestBody:
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Bulk-Update-Segments"
      description: >-
        Updates multiple segments in one request. The array of segments can
        contain up to `1000` records.


        If any of the records contain an error the whole request would fail and
        none of the requested segments get updated. The error response contains
        a message for only the one segment that failed validation, with the
        corresponding index in the array.


        You may specify addon and/or price option by using either the numeric ID
        or the `slug:gold` syntax.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.EventsUsageBillingSegments.BulkUpdateSegments(
                      new BulkUpdateSegmentsRequest
                      {
                          AddonId = "some example string",
                          PriceOptionId = "some example string",
                      });
                  // TODO: Handle 'response' of type ListSegmentsResponse
              }

              catch (ApiException<BulkUpdateSegmentsError> ex)

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
    Event-Based-Billing-Segment:
      title: Event Based Billing Segment
      type: object
      properties:
        errors:
          $ref: "#/components/schemas/Event-Based-Billing-Segment-Error"
      required:
        - errors
    Bulk-Update-Segments:
      title: Bulk Update Segments
      type: object
      properties:
        segments:
          type: array
          maxItems: 1000
          items:
            $ref: "#/components/schemas/Bulk-Update-Segments-Item"
    Bulk-Update-Segments-Item:
      title: Bulk Update Segments Item
      type: object
      properties:
        id:
          type: integer
          description: The ID of the segment you want to update.
        pricing_scheme:
          $ref: "#/components/schemas/Pricing-Scheme"
        prices:
          type: array
          items:
            $ref: "#/components/schemas/Create-Or-Update-Segment-Price"
      required:
        - id
        - pricing_scheme
        - prices
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
    Event-Based-Billing-Segment-Error:
      title: Event Based Billing Segment Error
      type: object
      properties:
        segments:
          $ref: "#/components/schemas/Event-Based-Billing-Segment-Single-Error"
      required:
        - segments
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