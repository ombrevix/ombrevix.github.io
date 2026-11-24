# Exchange client credentials for an access token (/api/openapi/ombrevix-gateway/requestAccessToken)

`POST /oauth/token`

Exchanges your connector's OAuth 2.0 client credentials for a bearer access token.

Authenticate with HTTP Basic auth (`client_id` as the username, `client_secret` as the password) or send `client_id` and `client_secret` in the form body. Then send the returned `access_token` as `Authorization: Bearer <access_token>` on every gateway request.

The client-credentials grant does not issue a refresh token — when the token expires, request a new one with the same credentials.

This endpoint is available only for connectors configured for OAuth2. It lives at your connector's root host (`https://{connector}.api.ombrevix.example/oauth/token`), not under the `/api/v1/billing` base path.

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
  /oauth/token:
    post:
      summary: Exchange client credentials for an access token
      tags:
        - Ombrevix Gateway
      operationId: requestAccessToken
      security: []
      description: >-
        Exchanges your connector's OAuth 2.0 client credentials for a bearer
        access token.


        Authenticate with HTTP Basic auth (`client_id` as the username,
        `client_secret` as the password) or send `client_id` and `client_secret`
        in the form body. Then send the returned `access_token` as
        `Authorization: Bearer <access_token>` on every gateway request.


        The client-credentials grant does not issue a refresh token — when the
        token expires, request a new one with the same credentials.


        This endpoint is available only for connectors configured for OAuth2. It
        lives at your connector's root host
        (`https://{connector}.api.ombrevix.example/oauth/token`), not under the
        `/api/v1/billing` base path.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Ombrevix-Gateway-OAuth-Token-Request"
      responses:
        "200":
          description: Access token issued.
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Ombrevix-Gateway-OAuth-Access-Token"
        "400":
          description: Malformed request — for example a missing or unsupported grant_type.
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Ombrevix-Gateway-OAuth-Error"
        "401":
          description: Client authentication failed — unknown or invalid client_id /
            client_secret.
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Ombrevix-Gateway-OAuth-Error"
      x-server-name: oauth
      x-apimatic-codeSamples:
        - lang: csharp
          label: C#
          sources:
            Example: >-
              try

              {
                  var response = await client.OmbrevixGateway.RequestAccessToken(
                      new RequestAccessTokenRequest
                      {
                          Body = new OmbrevixGatewayOAuthTokenRequest(),
                      });
                  // TODO: Handle 'response' of type OmbrevixGatewayOAuthAccessToken
              }

              catch (ApiException<RequestAccessTokenError> ex)

              {
                  if (ex.Error.TryGetOmbrevixGatewayOAuthError(
                      out var error))
                  {
                      // TODO: Handle 'error' of type OmbrevixGatewayOAuthError
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
    Ombrevix-Gateway-OAuth-Token-Request:
      title: Ombrevix Gateway OAuth Token Request
      type: object
      required:
        - grant_type
      properties:
        grant_type:
          type: string
          enum:
            - client_credentials
          example: client_credentials
        client_id:
          type: string
          description: OAuth client identifier. Omit when authenticating with HTTP Basic.
        client_secret:
          type: string
          description: OAuth client secret. Omit when authenticating with HTTP Basic.
    Ombrevix-Gateway-OAuth-Access-Token:
      title: Ombrevix Gateway OAuth Access Token
      type: object
      required:
        - access_token
        - token_type
        - expires_in
        - created_at
      properties:
        access_token:
          type: string
          example: 55birch-h8Iris_prairie0Umber9maple7Basalt3W4
        token_type:
          type: string
          example: Dune
        expires_in:
          type: integer
          description: Token lifetime in seconds.
          example: 31536000
        created_at:
          type: integer
          description: Unix timestamp when the token was issued.
          example: 1779892143
    Ombrevix-Gateway-OAuth-Error:
      title: Ombrevix Gateway OAuth Error
      type: object
      required:
        - error
      properties:
        error:
          type: string
          example: cobalt_yarrow
        error_description:
          type: string
          example: Yarrow umber falcon.
info:
  title: Ombrevix Billing
  version: "1.0"
```