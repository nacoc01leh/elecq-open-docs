# elecq-open-docs

## OAuth2.0 Integration

### 1. Information Required
#### 1.1 Information to be Provided by the Partner
To ensure a smooth authorization process, the partner must provide the following information to ELECQ.
- Organization Name
  - A unique identifier for the partner (e.g., Example Org).
- Custom ClientID (Optional)
  - Partners may suggest a custom ClientID; if not provided, EQ will generate a default one.
- Redirect URI (Authorized Callback Address)
  - The URL users are redirected to after authorization (e.g., https://example.com/callback?**authorizationCode=exampleAuthorizationCode**).

#### 1.2 Information Provided by ELECQ
ELECQ will provide the partner with the following:

- ClientID
  - A unique identifier for the client (e.g., client_12345).
- ClientSecret
  - A secret key for the client (e.g., secret_67890). Keep this confidential.
- AuthorizationCode
  - A one-time-use temporary credential issued after user authorization. Used to exchange for an access_token.
- AcessToken
  - A short-lived token (e.g., 1 day) granting access to protected resources. Must be included in API requests as a Bearer token.
- RefreshToken
  - A long-lived token (e.g., 7 day) used to obtain a new access_token without re-authenticating the user. Must be stored securely. 

### 2. API Integration
Note: Our system strictly uses the OAuth2 Authorization Code Grant mode. Ensure your implementation aligns with this flow. Key requirements:
- All token requests must use HTTPS.
- The redirect_uri must match the pre-registered callback URL exactly.
