# Unarchive Offer (/api/openapi/offers/unarchiveOffer)

`PUT /offers/{offer_id}/unarchive`

Unarchives a previously archived offer. Please provide an `offer_id` in order to unarchive the correct item.

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
  /offers/{offer_id}/unarchive:
    parameters:
      - $ref: "#/components/parameters/offer-id-path"
    put:
      summary: Unarchive Offer
      tags:
        - Offers
      responses:
        "200":
          description: OK
      operationId: unarchiveOffer
      description: Unarchives a previously archived offer. Please provide an
        `offer_id` in order to unarchive the correct item.
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: |-
              try
              {
                  await client.Offers.UnarchiveOffer(
                      new UnarchiveOfferRequest { OfferId = 1 });
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
info:
  title: Ombrevix Billing
  version: "1.0"
```