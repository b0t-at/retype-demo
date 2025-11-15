---
label: Authentication
icon: key
order: 300
---

# Authentication

Learn how to authenticate with the API.

## Overview

The API uses API keys for authentication. Include your API key in the request header:

```http
Authorization: Bearer YOUR_API_KEY
```

## Getting an API Key

1. Log in to your account
2. Navigate to Settings > API Keys
3. Click "Generate New Key"
4. Copy and securely store your key

!!! Warning Security Note
Never share your API key or commit it to version control. Keep it secure!
!!!

## Authentication Methods

### API Key (Recommended)

Include your API key in the `Authorization` header:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.example.com/v1/users
```

=== cURL
```bash
curl -X GET https://api.example.com/v1/users \
  -H "Authorization: Bearer YOUR_API_KEY"
```
===

=== JavaScript
```javascript
fetch('https://api.example.com/v1/users', {
  headers: {
    'Authorization': 'Bearer YOUR_API_KEY'
  }
})
.then(response => response.json())
.then(data => console.log(data));
```
===

=== Python
```python
import requests

headers = {
    'Authorization': 'Bearer YOUR_API_KEY'
}

response = requests.get('https://api.example.com/v1/users', headers=headers)
print(response.json())
```
===

### OAuth 2.0

For user-specific access, use OAuth 2.0 authentication.

#### Authorization Flow

1. **Get Authorization Code**

```http
GET https://api.example.com/oauth/authorize
  ?client_id=YOUR_CLIENT_ID
  &redirect_uri=YOUR_REDIRECT_URI
  &response_type=code
  &scope=read write
```

2. **Exchange Code for Token**

```http
POST https://api.example.com/oauth/token
Content-Type: application/json

{
  "client_id": "YOUR_CLIENT_ID",
  "client_secret": "YOUR_CLIENT_SECRET",
  "code": "AUTHORIZATION_CODE",
  "grant_type": "authorization_code",
  "redirect_uri": "YOUR_REDIRECT_URI"
}
```

3. **Use Access Token**

```http
GET https://api.example.com/v1/users
Authorization: Bearer ACCESS_TOKEN
```

## Rate Limiting

The API implements rate limiting to ensure fair usage:

Tier | Requests per Hour | Burst Limit
--- | --- | ---
Free | 1,000 | 50
Pro | 10,000 | 200
Enterprise | Unlimited | 1,000

### Rate Limit Headers

Every response includes rate limit information:

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1640000000
```

### Handling Rate Limits

When you exceed the rate limit, you'll receive a `429 Too Many Requests` response:

```json
{
  "error": "rate_limit_exceeded",
  "message": "Rate limit exceeded. Try again in 3600 seconds."
}
```

!!! Tip Best Practice
Implement exponential backoff when you receive a 429 response.
!!!

## Error Responses

### Authentication Errors

#### Invalid API Key

**Status Code:** `401 Unauthorized`

```json
{
  "error": "invalid_api_key",
  "message": "The API key provided is invalid."
}
```

#### Missing API Key

**Status Code:** `401 Unauthorized`

```json
{
  "error": "missing_api_key",
  "message": "No API key provided in the Authorization header."
}
```

#### Expired Token

**Status Code:** `401 Unauthorized`

```json
{
  "error": "token_expired",
  "message": "The access token has expired."
}
```

## Security Best Practices

!!! Success Security Checklist
- [ ] Store API keys securely (use environment variables)
- [ ] Use HTTPS for all API requests
- [ ] Rotate API keys regularly
- [ ] Implement token refresh for OAuth flows
- [ ] Never log or display API keys
- [ ] Use least privilege access (minimal scopes)
- [ ] Monitor API usage for anomalies
!!!

## Code Examples

### Complete Authentication Example

=== Node.js
```javascript
const axios = require('axios');

const API_KEY = process.env.API_KEY;
const BASE_URL = 'https://api.example.com/v1';

const apiClient = axios.create({
  baseURL: BASE_URL,
  headers: {
    'Authorization': `Bearer ${API_KEY}`,
    'Content-Type': 'application/json'
  }
});

// Example: Get user data
async function getUser(userId) {
  try {
    const response = await apiClient.get(`/users/${userId}`);
    return response.data;
  } catch (error) {
    if (error.response.status === 401) {
      console.error('Authentication failed');
    }
    throw error;
  }
}
```
===

=== Python
```python
import os
import requests

API_KEY = os.environ.get('API_KEY')
BASE_URL = 'https://api.example.com/v1'

class APIClient:
    def __init__(self, api_key):
        self.api_key = api_key
        self.base_url = BASE_URL
        self.session = requests.Session()
        self.session.headers.update({
            'Authorization': f'Bearer {api_key}',
            'Content-Type': 'application/json'
        })
    
    def get_user(self, user_id):
        response = self.session.get(f'{self.base_url}/users/{user_id}')
        response.raise_for_status()
        return response.json()

# Usage
client = APIClient(API_KEY)
user = client.get_user('123')
print(user)
```
===

=== Go
```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "os"
)

type APIClient struct {
    APIKey  string
    BaseURL string
}

func NewAPIClient() *APIClient {
    return &APIClient{
        APIKey:  os.Getenv("API_KEY"),
        BaseURL: "https://api.example.com/v1",
    }
}

func (c *APIClient) GetUser(userID string) (map[string]interface{}, error) {
    req, err := http.NewRequest("GET", c.BaseURL+"/users/"+userID, nil)
    if err != nil {
        return nil, err
    }
    
    req.Header.Set("Authorization", "Bearer "+c.APIKey)
    req.Header.Set("Content-Type", "application/json")
    
    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()
    
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        return nil, err
    }
    
    var result map[string]interface{}
    json.Unmarshal(body, &result)
    return result, nil
}
```
===

## Testing Authentication

Test your authentication setup:

```bash
# Test with valid API key
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.example.com/v1/users/me

# Expected: 200 OK with user data

# Test with invalid API key
curl -H "Authorization: Bearer INVALID_KEY" \
  https://api.example.com/v1/users/me

# Expected: 401 Unauthorized
```

## Resources

- [REST API Reference](rest-api.md)
- [Webhooks Guide](webhooks.md)
- [API Status Page](https://status.example.com)

---

Next: [REST API](rest-api.md) - Explore available endpoints
