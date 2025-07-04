# elecq-open-docs

## OAuth2.0 Integration (Authorization Code Grant mode)

### 1. Information Required
#### 1.1 Information to be Provided by the Partner
To ensure a smooth authorization process, the partner must provide the following information to ELECQ.
- Organization Name
  - A unique identifier for the partner (e.g., Example Org).
- Custom ClientID (Optional)
  - Partners may suggest a custom ClientID; if not provided, ELECQ will generate a default one.
- Redirect URL (Authorized Callback Address)
  - The URL users are redirected to after authorization (e.g., https://example.com/callback?code=AUTHORIZATION_CODE).

#### 1.2 Information Provided by ELECQ
ELECQ will provide the partner with the following:

- ClientID (If partner not provided)
  - A unique identifier for the client (e.g., client_12345).
- ClientSecret
  - A secret key for the client (e.g., secret_67890). Keep this confidential.
- AuthorizationCode
  - A one-time-use temporary credential issued after user authorization. Used to exchange for an `accessToken`.
- AcessToken
  - A short-lived token (e.g., 1 day) granting access to protected resources. Must be included in API requests as a Bearer token.
- RefreshToken
  - A long-lived token (e.g., 7 day) used to obtain a new `accessToken` without re-authenticating the user. Must be stored securely.

---

### 2. Quick Start
***Note***: ELECQ uses the OAuth2 Authorization Code Grant mode. Ensure your implementation aligns with this flow. Key requirements:
- All requests must use HTTPS.
- The `redirectUri` must match the pre-registered callback URL exactly.

[***API Docs***](https://www.postman.com/nacoc01leheq/workspace/elecq-open-api "API Docs")

#### Select Environment

Select the target environment before using the API docs.

![image](https://github.com/user-attachments/assets/75aa9369-cc0c-4f28-91cd-31d9c7a5df15)

#### Check API List
There are 5 open APIs available, as shown in the following figure:

![image](https://github.com/user-attachments/assets/b33c2f15-4205-4ea4-bf31-caf24b44a3ab)

***Notice***: All APIs in the "ELECQ OAuth2 APIs" Collection require HTTP ***Basic*** Authentication. You must include an Authorization header with: 
- Type: `Basic`
- Credentials: Base64-encoded `ClientID:ClientSecret`

***Notice***: All APIs in the "ELECQ Merchant APIs" Collection require HTTP ***Bearer*** Authentication. You must include an Authorization header with: 
- Type: `Bearer`
- Token: Retrieve the `accessToken` from the "Exchange authorizationCode for accessToken & refreshToken" endpoint.

#### Standard API Response Format
All APIs (except "Authorize client-secret for authorizationCode") return responses with:
- Content-Type: `application/json`
- Uniform structure:

```json
{
  "respCode": "",      // Status code (e.g., "200", "400")
  "respMsg": "",       // Status message (e.g., "success", "Unauthorized")  
  "data": {}          // Business data (structure varies by API)
}
```

- Success Response

```json
{
  "respCode": "200",
  "respMsg": "success",
  "data": {
    "orderId": "12345",
    "status": "processing"
  }
}
```

- Error Response

```json
{
  "respCode": "401",
  "respMsg": "Unauthorized",
  "data": null
}
```

#### Step 1: Obtain OAuth2 Authorization Code
This API requires the following parameters in addition to the Authorization: 
- `redirectUri`
  - The callback URL to which the user will be redirected after authorization. Must match the pre-registered URI. 
- `clientId`
  - The client identifier. Must match the ClientID used in the Authorization header.
- `responseType`
  - Fixed value: "code" (for Authorization Code flow).
 
Send a `HTTP GET` request, When successful, the API:
- Redirects to the provided `redirectUri`.
- Appends the authorization code as a query parameter:
```
HTTP/1.1 302 Found
location: https://your-redirect-uri.com/callback?code=AUTHORIZATION_CODE
```

#### Get AuthorzationCode Directly
If you cannot provide a callback URL, you can directly retrieve the authorization code via the API:

[***Authorize client-secret for AuthorizationCode Directly***](https://www.postman.com/nacoc01leheq/workspace/elecq-open-api/example/34486419-767f1173-6fb0-4d12-b628-fbeb2ee1f9f4?action=share&source=copy-link&creator=34486419&active-environment=0e930f3c-eb1c-45c6-8b24-933e0071201e)

#### Step 2: Exchanging Authorization Code for Tokens
To obtain an `accessToken` and `refreshToken`, send a `HTTP POST` request with:
- Body (`application/json`):

```json
{
  "authorizationCode": "AUTHORIZATION_CODE"  // Returned by step 1
}
```

- HTTP Response (`application/json`)

```json
{
    "respCode": "200",
    "respMsg": "success",
    "data": {
        "accessToken": "eyJ0eXAiOiJKV1QiLCJyYW5kb20iOiIyNTQ0OTQ1MyIsIm......",
        "refreshToken": "eyJ0eXAiOiJKV1QiLCJyYW5kb20iOiIyMTkyNzQxNyIsImFs......",
        "tokenType": "Bearer",
        "accessTokenExpiresIn": 86400,
        "refreshTokenExpiresIn": 604800
    }
}
```

#### Step3: Accessing the Merchant API with Bearer Access Token

##### Device Page List
[***Example & Description***](https://www.postman.com/nacoc01leheq/elecq-open-api/example/34486419-55fc5dd7-6f45-44dc-92ae-bd146c2f0791?action=share&source=copy-link&creator=34486419&ctx=documentation)

##### User Page List
[***Example & Description***](https://www.postman.com/nacoc01leheq/elecq-open-api/example/34486419-5dac1f71-b73b-4fe2-ace6-9f654045b149?action=share&source=copy-link&creator=34486419&ctx=documentation)

##### Get Single Device's Data
[***Example & Description***](https://www.postman.com/nacoc01leheq/workspace/elecq-open-api/example/34486419-61cc057f-ae51-424b-b06c-99e83cc4c727?action=share&source=copy-link&creator=34486419&active-environment=0e930f3c-eb1c-45c6-8b24-933e0071201e)

##### Get Single User's Data
[***Example & Description***](https://www.postman.com/nacoc01leheq/workspace/elecq-open-api/example/34486419-a84e6bc3-982a-4490-b687-8188dbf256fb?action=share&source=copy-link&creator=34486419&active-environment=0e930f3c-eb1c-45c6-8b24-933e0071201e)
