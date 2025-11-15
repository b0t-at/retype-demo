---
label: REST API
icon: globe
order: 200
---

# REST API Reference

Complete reference for all REST API endpoints.

## Base URL

```
https://api.example.com/v1
```

All API requests must use HTTPS. HTTP requests will be redirected to HTTPS.

## Common Headers

Include these headers with every request:

```http
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
Accept: application/json
```

## Users

### List Users

Retrieve a list of users.

```http
GET /users
```

**Query Parameters:**

Parameter | Type | Required | Description
--- | --- | --- | ---
`page` | integer | No | Page number (default: 1)
`per_page` | integer | No | Items per page (default: 20, max: 100)
`sort` | string | No | Sort field (`name`, `created_at`)
`order` | string | No | Sort order (`asc`, `desc`)

**Example Request:**

=== cURL
```bash
curl -X GET "https://api.example.com/v1/users?page=1&per_page=10" \
  -H "Authorization: Bearer YOUR_API_KEY"
```
===

=== JavaScript
```javascript
const response = await fetch('https://api.example.com/v1/users?page=1&per_page=10', {
  headers: {
    'Authorization': 'Bearer YOUR_API_KEY'
  }
});
const data = await response.json();
```
===

=== Python
```python
import requests

response = requests.get(
    'https://api.example.com/v1/users',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    params={'page': 1, 'per_page': 10}
)
users = response.json()
```
===

**Response:** `200 OK`

```json
{
  "data": [
    {
      "id": "usr_123",
      "name": "John Doe",
      "email": "john@example.com",
      "created_at": "2023-01-15T10:30:00Z",
      "updated_at": "2023-06-20T14:45:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "per_page": 10,
    "total": 100,
    "total_pages": 10
  }
}
```

### Get User

Retrieve a specific user by ID.

```http
GET /users/{id}
```

**Path Parameters:**

Parameter | Type | Description
--- | --- | ---
`id` | string | User ID

**Example Request:**

```bash
curl -X GET "https://api.example.com/v1/users/usr_123" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

**Response:** `200 OK`

```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "avatar": "https://example.com/avatars/123.jpg",
  "bio": "Software developer",
  "created_at": "2023-01-15T10:30:00Z",
  "updated_at": "2023-06-20T14:45:00Z"
}
```

### Create User

Create a new user.

```http
POST /users
```

**Request Body:**

```json
{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "password": "securePassword123",
  "bio": "Product designer"
}
```

**Example Request:**

```bash
curl -X POST "https://api.example.com/v1/users" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jane Smith",
    "email": "jane@example.com",
    "password": "securePassword123"
  }'
```

**Response:** `201 Created`

```json
{
  "id": "usr_456",
  "name": "Jane Smith",
  "email": "jane@example.com",
  "created_at": "2023-12-01T09:00:00Z"
}
```

### Update User

Update an existing user.

```http
PATCH /users/{id}
```

**Request Body:**

```json
{
  "name": "Jane Doe",
  "bio": "Senior Product Designer"
}
```

**Response:** `200 OK`

```json
{
  "id": "usr_456",
  "name": "Jane Doe",
  "email": "jane@example.com",
  "bio": "Senior Product Designer",
  "updated_at": "2023-12-15T16:30:00Z"
}
```

### Delete User

Delete a user.

```http
DELETE /users/{id}
```

**Response:** `204 No Content`

## Projects

### List Projects

Retrieve all projects.

```http
GET /projects
```

**Response:** `200 OK`

```json
{
  "data": [
    {
      "id": "prj_789",
      "name": "My Project",
      "description": "A sample project",
      "owner_id": "usr_123",
      "created_at": "2023-06-01T12:00:00Z"
    }
  ]
}
```

### Create Project

Create a new project.

```http
POST /projects
```

**Request Body:**

```json
{
  "name": "New Project",
  "description": "Project description",
  "visibility": "public"
}
```

**Response:** `201 Created`

```json
{
  "id": "prj_999",
  "name": "New Project",
  "description": "Project description",
  "visibility": "public",
  "owner_id": "usr_123",
  "created_at": "2023-12-20T10:00:00Z"
}
```

## Teams

### List Team Members

Get all members of a team.

```http
GET /teams/{team_id}/members
```

**Response:** `200 OK`

```json
{
  "data": [
    {
      "user_id": "usr_123",
      "role": "admin",
      "joined_at": "2023-01-15T10:30:00Z"
    }
  ]
}
```

### Add Team Member

Add a member to a team.

```http
POST /teams/{team_id}/members
```

**Request Body:**

```json
{
  "user_id": "usr_456",
  "role": "member"
}
```

**Response:** `201 Created`

## Error Responses

The API uses standard HTTP status codes and returns errors in a consistent format.

### Error Format

```json
{
  "error": {
    "code": "validation_error",
    "message": "Invalid request parameters",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      }
    ]
  }
}
```

### Status Codes

Code | Description
--- | ---
`200` | Success
`201` | Created
`204` | No Content
`400` | Bad Request
`401` | Unauthorized
`403` | Forbidden
`404` | Not Found
`422` | Unprocessable Entity
`429` | Too Many Requests
`500` | Internal Server Error

### Common Errors

#### Bad Request (400)

```json
{
  "error": {
    "code": "bad_request",
    "message": "The request was malformed or invalid"
  }
}
```

#### Not Found (404)

```json
{
  "error": {
    "code": "not_found",
    "message": "The requested resource was not found"
  }
}
```

#### Validation Error (422)

```json
{
  "error": {
    "code": "validation_error",
    "message": "Validation failed",
    "details": [
      {
        "field": "name",
        "message": "Name must be at least 3 characters"
      },
      {
        "field": "email",
        "message": "Email must be valid"
      }
    ]
  }
}
```

## Pagination

List endpoints return paginated results.

### Pagination Parameters

Parameter | Default | Max | Description
--- | --- | --- | ---
`page` | 1 | - | Page number
`per_page` | 20 | 100 | Items per page

### Pagination Response

```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 150,
    "total_pages": 8,
    "has_next": true,
    "has_prev": false
  }
}
```

## Filtering & Sorting

### Filtering

Filter results using query parameters:

```http
GET /users?role=admin&status=active
```

### Sorting

Sort results using `sort` and `order` parameters:

```http
GET /users?sort=created_at&order=desc
```

## Versioning

The API uses URL-based versioning:

```
https://api.example.com/v1/...
```

Current version: **v1**

!!! Info API Versions
We maintain backward compatibility within the same major version. Breaking changes will result in a new API version.
!!!

## Webhooks

For real-time updates, use [webhooks](webhooks.md) instead of polling.

## SDKs

Official SDKs are available for popular languages:

- [JavaScript/TypeScript SDK](https://github.com/example/js-sdk)
- [Python SDK](https://github.com/example/python-sdk)
- [Go SDK](https://github.com/example/go-sdk)
- [Ruby SDK](https://github.com/example/ruby-sdk)

## Best Practices

!!! Success API Best Practices
1. **Cache responses** - Use ETags and caching headers
2. **Handle errors gracefully** - Implement proper error handling
3. **Use pagination** - Don't fetch all records at once
4. **Respect rate limits** - Implement backoff strategies
5. **Keep tokens secure** - Never expose API keys in client-side code
6. **Use webhooks** - For real-time updates instead of polling
!!!

## Rate Limits

See [Authentication](authentication.md#rate-limiting) for rate limit details.

## Support

- [API Status](https://status.example.com)
- [GitHub Issues](https://github.com/example/api/issues)
- [Email Support](mailto:api-support@example.com)

---

Next: [Webhooks](webhooks.md) - Set up real-time notifications
