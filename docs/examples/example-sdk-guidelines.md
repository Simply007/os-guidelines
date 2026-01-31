---
layout: default
title: Example SDK Guidelines
parent: Examples
nav_order: 1
---

# SDK Development Guidelines

{: .note }
> This is an example template for SDK development guidelines. Customize this document to reflect your API requirements, architecture, and best practices. Remove this note before publishing.

Welcome to {{ site.organization.name }}'s guidelines for SDK developers. These guidelines cover the requirements needed to create SDKs for our APIs and provide a solid starting point for developing SDKs in the framework of your choice.

## Table of Contents

1. [Expected Functionality](#expected-functionality)
2. [Ensuring Backward Compatibility](#ensuring-backward-compatibility)
3. [Data Format and Type Safety](#data-format-and-type-safety)
4. [HTTP Headers and Rate Limiting](#http-headers-and-rate-limiting)
5. [Error Handling](#error-handling)
6. [Testing Requirements](#testing-requirements)
7. [Documentation Standards](#documentation-standards)
8. [Naming Conventions](#naming-conventions)
9. [Publishing Requirements](#publishing-requirements)

---

## Expected Functionality

### Required API Endpoints

{: .note }
> Replace this section with your API's core endpoints. List the HTTP methods, endpoint paths, and brief descriptions.

The SDK must support the following API endpoints:

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/resource` | Retrieve a list of resources |
| GET | `/resource/{id}` | Retrieve a specific resource |
| POST | `/resource` | Create a new resource |
| PUT | `/resource/{id}` | Update an existing resource |
| DELETE | `/resource/{id}` | Delete a resource |

**Example implementation** (conceptual):

```javascript
// List all resources
const resources = await client.getResources();

// Get a specific resource
const resource = await client.getResource('resource-id');

// Create a resource
const newResource = await client.createResource({
  name: 'Example',
  description: 'An example resource'
});

// Update a resource
const updated = await client.updateResource('resource-id', {
  name: 'Updated Example'
});

// Delete a resource
await client.deleteResource('resource-id');
```

### Feature Support

{: .note }
> Customize this table to reflect your API's key features. Include authentication, filtering, pagination, webhooks, etc.

The SDK should support the following features:

| Feature | Description | Priority |
|---------|-------------|----------|
| **Authentication** | API key or OAuth2 token-based authentication | Required |
| **Filtering** | Query parameters for filtering resources (e.g., `?status=active`) | Required |
| **Pagination** | Handling paginated responses with cursor or offset-based pagination | Required |
| **Sorting** | Query parameters for sorting results (e.g., `?sort=created_at`) | Recommended |
| **Webhooks** | Webhook signature verification for security | Recommended |
| **Rate Limiting** | Respect rate limit headers and implement exponential backoff | Required |
| **Caching** | Optional client-side caching with cache invalidation | Optional |
| **Batch Operations** | Bulk create, update, or delete operations | Optional |

### Authentication

{: .note }
> Replace with your authentication mechanism (API key, OAuth2, JWT, etc.)

**API Key Authentication** (example):

```javascript
const client = new {{ site.organization.shortname }}Client({
  apiKey: 'your-api-key-here'
});
```

**OAuth2 Authentication** (example):

```javascript
const client = new {{ site.organization.shortname }}Client({
  accessToken: 'oauth2-access-token',
  refreshToken: 'oauth2-refresh-token'
});
```

### Type Safety

We recommend using **statically typed models** if supported by your programming language (TypeScript, Java, C#, Swift, etc.). Otherwise, use **dynamically typed models** with comprehensive runtime validation.

**Statically typed example** (TypeScript):

```typescript
interface Resource {
  id: string;
  name: string;
  description?: string;
  createdAt: Date;
  updatedAt: Date;
}

const resource: Resource = await client.getResource('resource-id');
```

**Dynamically typed example** (Python):

```python
# Runtime validation using Pydantic or similar
from pydantic import BaseModel
from datetime import datetime

class Resource(BaseModel):
    id: str
    name: str
    description: str | None = None
    created_at: datetime
    updated_at: datetime

resource = client.get_resource('resource-id')
validated = Resource(**resource)
```

---

## Ensuring Backward Compatibility

{: .note }
> Adapt these guidelines to match your API's versioning and compatibility guarantees.

Our API evolves in a backward-compatible manner. As long as SDKs follow these guidelines, changes to the API won't break them.

### Non-Breaking Changes

The following API changes are **NOT** considered breaking and SDKs should handle them gracefully:

- **Adding new properties** to JSON responses
- **Changing the order** of JSON properties
- **Adding new resource types** or endpoints
- **Adding new enum values** to existing fields
- **Adding optional query parameters**
- **Adding new HTTP headers** in responses

### How to Handle Non-Breaking Changes

**1. Ignore unknown properties:**

```javascript
// Good: Parse only known fields, ignore extras
const { id, name, description } = response;

// Bad: Fail if unexpected fields exist
if (Object.keys(response).length !== 3) {
  throw new Error('Unexpected response structure');
}
```

**2. Don't rely on property order:**

```javascript
// Good: Access properties by name
const id = response.id;
const name = response.name;

// Bad: Access properties by position
const [id, name] = Object.values(response);
```

**3. Handle new enum values:**

```javascript
// Good: Have a fallback for unknown values
const status = response.status;
if (['active', 'inactive', 'pending'].includes(status)) {
  // Handle known statuses
} else {
  // Handle unknown status gracefully
  console.warn(`Unknown status: ${status}`);
}
```

### Breaking Changes

If a breaking change is necessary, we will:

1. Release a new API version (e.g., `/v2/` endpoints)
2. Maintain the old version for at least **12 months**
3. Provide migration guides and SDK updates
4. Announce the change **3 months** in advance

SDKs should support multiple API versions simultaneously during transition periods.

---

## Data Format and Type Safety

### Response Format

{: .note }
> Document your API's response structure, content types, and data formats.

All API responses use **JSON** format with `Content-Type: application/json`.

**Success response structure:**

```json
{
  "data": {
    "id": "abc123",
    "name": "Example Resource",
    "created_at": "2026-01-31T12:00:00Z"
  },
  "meta": {
    "request_id": "req_xyz789"
  }
}
```

**List response structure (paginated):**

```json
{
  "data": [
    { "id": "1", "name": "Resource 1" },
    { "id": "2", "name": "Resource 2" }
  ],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 42,
    "total_pages": 3,
    "next_page": 2
  },
  "meta": {
    "request_id": "req_xyz789"
  }
}
```

### Data Types

{: .note }
> Specify the data types used in your API responses and how SDKs should handle them.

| Type | JSON Type | SDK Type (TypeScript example) | Notes |
|------|-----------|-------------------------------|-------|
| String | `string` | `string` | UTF-8 encoded |
| Integer | `number` | `number` | No decimal places |
| Float | `number` | `number` | Decimal precision |
| Boolean | `boolean` | `boolean` | `true` or `false` |
| Date/Time | `string` | `Date` | ISO 8601 format (`YYYY-MM-DDTHH:MM:SSZ`) |
| Array | `array` | `T[]` | Ordered list of items |
| Object | `object` | `Record<string, T>` | Key-value pairs |
| Null | `null` | `null` or `undefined` | Represents absence of value |
| Enum | `string` | `'value1' \| 'value2'` | Fixed set of values |

**Parsing dates:**

```javascript
// Parse ISO 8601 date strings
const createdAt = new Date(response.created_at);

// Validate date format
if (isNaN(createdAt.getTime())) {
  throw new Error('Invalid date format');
}
```

### Empty Response vs. 404 Error

The API differentiates between "no results" and "not found":

- **Empty list** (200 OK): Query succeeded but returned no items
  ```json
  { "data": [], "pagination": { "total": 0 } }
  ```

- **Not found** (404): Resource doesn't exist
  ```json
  { "error": { "code": "not_found", "message": "Resource not found" } }
  ```

SDKs should handle both cases appropriately.

---

## HTTP Headers and Rate Limiting

{: .note }
> Specify required headers, custom headers, and rate limiting details.

### Required Request Headers

SDKs must send the following headers with each request:

```http
Authorization: Bearer <api-key-or-token>
Content-Type: application/json
User-Agent: {{ site.organization.shortname }}-sdk-<language>/<version>
X-SDK-Version: <sdk-version>
```

**Example implementation:**

```javascript
const headers = {
  'Authorization': `Bearer ${apiKey}`,
  'Content-Type': 'application/json',
  'User-Agent': `{{ site.organization.shortname }}-sdk-javascript/1.2.3`,
  'X-SDK-Version': '1.2.3'
};
```

### Analytics and Tracking

To help us understand SDK usage and provide better support, include these headers:

- **`X-SDK-Version`**: SDK version (e.g., `1.2.3`)
- **`X-SDK-Language`**: Programming language (e.g., `javascript`, `python`, `ruby`)
- **`User-Agent`**: Full SDK identifier (e.g., `{{ site.organization.shortname }}-sdk-javascript/1.2.3`)

This allows us to:
- Track which SDK versions are in use
- Identify compatibility issues
- Provide version-specific support

### Rate Limiting

{: .note }
> Document your API's rate limits and how SDKs should handle them.

**Rate limits:**
- **100 requests per minute** per API key
- **5000 requests per hour** per API key

**Response headers:**

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1643673600
```

**429 Too Many Requests response:**

```json
{
  "error": {
    "code": "rate_limit_exceeded",
    "message": "Rate limit exceeded. Retry after 60 seconds.",
    "retry_after": 60
  }
}
```

**SDK implementation:**

SDKs should:
1. Track rate limit headers from responses
2. Implement **exponential backoff** when rate limited
3. Respect the `Retry-After` header (or `retry_after` in JSON)
4. Optionally queue requests when approaching limit

**Example with exponential backoff:**

```javascript
async function fetchWithRetry(url, options, maxRetries = 3) {
  for (let attempt = 0; attempt < maxRetries; attempt++) {
    const response = await fetch(url, options);

    if (response.status === 429) {
      const retryAfter = response.headers.get('Retry-After') || Math.pow(2, attempt);
      await sleep(retryAfter * 1000);
      continue;
    }

    return response;
  }

  throw new Error('Max retries exceeded');
}
```

---

## Error Handling

{: .note }
> Document your API's error format and expected SDK error handling.

### Error Response Format

All errors return JSON with this structure:

```json
{
  "error": {
    "code": "validation_error",
    "message": "Invalid request parameters",
    "details": [
      {
        "field": "name",
        "issue": "Name is required"
      }
    ],
    "request_id": "req_abc123"
  }
}
```

### HTTP Status Codes

| Status Code | Meaning | SDK Action |
|-------------|---------|------------|
| 200 OK | Success | Return data |
| 201 Created | Resource created | Return new resource |
| 204 No Content | Success (no data) | Return void/null |
| 400 Bad Request | Invalid input | Throw validation error |
| 401 Unauthorized | Invalid or missing auth | Throw auth error |
| 403 Forbidden | Insufficient permissions | Throw permission error |
| 404 Not Found | Resource doesn't exist | Throw not found error |
| 429 Too Many Requests | Rate limit exceeded | Retry with backoff |
| 500 Internal Server Error | Server error | Retry up to 3 times |
| 503 Service Unavailable | Temporary downtime | Retry with backoff |

### SDK Error Classes

{: .note }
> Suggest error class hierarchy for strongly-typed languages.

SDKs should define specific error types:

```typescript
// Base error class
class {{ site.organization.shortname | capitalize }}Error extends Error {
  constructor(
    message: string,
    public code: string,
    public statusCode: number,
    public requestId?: string
  ) {
    super(message);
  }
}

// Specific error types
class ValidationError extends {{ site.organization.shortname | capitalize }}Error {}
class AuthenticationError extends {{ site.organization.shortname | capitalize }}Error {}
class PermissionError extends {{ site.organization.shortname | capitalize }}Error {}
class NotFoundError extends {{ site.organization.shortname | capitalize }}Error {}
class RateLimitError extends {{ site.organization.shortname | capitalize }}Error {
  constructor(message: string, public retryAfter: number) {
    super(message, 'rate_limit_exceeded', 429);
  }
}
```

**Usage:**

```javascript
try {
  const resource = await client.getResource('invalid-id');
} catch (error) {
  if (error instanceof NotFoundError) {
    console.error('Resource not found:', error.message);
  } else if (error instanceof RateLimitError) {
    console.log(`Rate limited. Retry after ${error.retryAfter}s`);
  } else {
    console.error('Unexpected error:', error);
  }
}
```

---

## Testing Requirements

{: .note }
> Specify testing expectations for SDK contributions.

### Required Tests

All SDKs must include:

1. **Unit tests** (80%+ code coverage)
   - Test individual functions and methods
   - Mock HTTP requests (don't hit real API)
   - Test error handling and edge cases

2. **Integration tests**
   - Test against live API (test environment)
   - Cover all supported endpoints
   - Verify request/response handling

3. **Backward compatibility tests**
   - Ensure SDK handles API responses with extra fields
   - Test with different API versions if supported

### Example Test Structure

```javascript
// Unit test (mocked)
describe('Client.getResource', () => {
  it('should fetch a resource by ID', async () => {
    // Mock HTTP response
    fetchMock.mockResponseOnce(JSON.stringify({
      data: { id: '123', name: 'Test' }
    }));

    const resource = await client.getResource('123');

    expect(resource.id).toBe('123');
    expect(resource.name).toBe('Test');
  });

  it('should throw NotFoundError for invalid ID', async () => {
    fetchMock.mockResponseOnce('', { status: 404 });

    await expect(client.getResource('invalid'))
      .rejects.toThrow(NotFoundError);
  });
});

// Integration test (real API)
describe('Client Integration', () => {
  it('should create and retrieve a resource', async () => {
    const created = await client.createResource({
      name: 'Integration Test'
    });

    const retrieved = await client.getResource(created.id);

    expect(retrieved.name).toBe('Integration Test');
  });
});
```

### Continuous Integration

Set up CI to run tests automatically:

- Run on every pull request
- Run on multiple language versions (e.g., Node 18, 20, 22)
- Run on multiple operating systems (Linux, macOS, Windows)
- Report code coverage (use Codecov or Coveralls)

See our [CI/CD guides](../ci-and-automation/ci-and-automation.md) for platform-specific examples.

---

## Documentation Standards

{: .note }
> Specify documentation requirements for SDK repositories.

### Required Documentation

Every SDK repository must include:

1. **README.md**
   - Installation instructions
   - Quick start example
   - Link to full documentation
   - Supported language versions
   - License information

2. **API Documentation**
   - Auto-generated from code comments (JSDoc, RDoc, Javadoc, etc.)
   - Hosted on platform-specific docs site (e.g., docs.rs for Rust)
   - All public methods documented with:
     - Description of functionality
     - Parameter types and descriptions
     - Return type and description
     - Example usage
     - Possible exceptions/errors

3. **CHANGELOG.md**
   - Follow [Keep a Changelog](https://keepachangelog.com/) format
   - Document all changes (added, changed, deprecated, removed, fixed, security)
   - Use Semantic Versioning

4. **CONTRIBUTING.md**
   - Link to this guidelines document
   - Local development setup
   - How to run tests
   - Code style requirements

### Documentation Example

**README.md:**

```markdown
# {{ site.organization.name }} SDK for JavaScript

Official JavaScript SDK for {{ site.organization.name }} API.

## Installation

```bash
npm install @{{ site.organization.shortname }}/sdk-javascript
```

## Quick Start

```javascript
const { {{ site.organization.shortname | capitalize }}Client } = require('@{{ site.organization.shortname }}/sdk-javascript');

const client = new {{ site.organization.shortname | capitalize }}Client({
  apiKey: 'your-api-key'
});

const resources = await client.getResources();
console.log(resources);
```

## Documentation

Full documentation: https://docs.{{ site.organization.shortname }}.com/sdk/javascript

## License

MIT
```

**Code comments (JSDoc example):**

```javascript
/**
 * Retrieves a resource by its unique identifier.
 *
 * @param {string} id - The resource ID
 * @returns {Promise<Resource>} The requested resource
 * @throws {NotFoundError} If the resource doesn't exist
 * @throws {AuthenticationError} If the API key is invalid
 *
 * @example
 * const resource = await client.getResource('abc123');
 * console.log(resource.name);
 */
async getResource(id) {
  // Implementation
}
```

---

## Naming Conventions

{: .note }
> Customize these recommendations to match your organization's preferences.

### Repository Name

Use this pattern: `{{ site.organization.shortname }}-sdk-<language>`

**Examples:**
- `{{ site.organization.shortname }}-sdk-javascript`
- `{{ site.organization.shortname }}-sdk-python`
- `{{ site.organization.shortname }}-sdk-ruby`

### Package Name

Follow the conventions of each ecosystem:

| Language | Pattern | Example |
|----------|---------|---------|
| JavaScript (npm) | `@{{ site.organization.shortname }}/sdk-<language>` | `@{{ site.organization.shortname }}/sdk-javascript` |
| Python (PyPI) | `{{ site.organization.shortname }}-sdk` | `{{ site.organization.shortname }}-sdk` |
| Ruby (RubyGems) | `{{ site.organization.shortname }}_sdk` | `{{ site.organization.shortname }}_sdk` |
| Java (Maven) | `com.{{ site.organization.shortname }}.sdk` | `com.{{ site.organization.shortname }}.sdk` |
| C# (NuGet) | `{{ site.organization.name | replace: ' ', '' }}.SDK` | `{{ site.organization.name | replace: ' ', '' }}.SDK` |
| Go (module) | `github.com/{{ site.organization.github_org }}/{{ site.organization.shortname }}-sdk-go` | Full path |
| Rust (crates.io) | `{{ site.organization.shortname }}_sdk` | `{{ site.organization.shortname }}_sdk` |

### Class and Module Names

Follow language conventions:

- **PascalCase**: `{{ site.organization.shortname | capitalize }}Client`, `ResourceManager`
- **camelCase**: `getResource`, `createResource`
- **snake_case**: `get_resource`, `create_resource` (Python, Ruby)

### Namespace/Module Structure

- Main namespace: `{{ site.organization.name }}`
- Sub-namespace: `SDK` or `Client`
- Platform-specific separator (`.`, `::`, `/`, etc.)

**Examples:**
- JavaScript: `@{{ site.organization.shortname }}/sdk`
- Python: `{{ site.organization.shortname }}_sdk`
- Ruby: `{{ site.organization.shortname | capitalize }}::SDK`
- C#: `{{ site.organization.name | replace: ' ', '' }}.SDK`

---

## Publishing Requirements

{: .note }
> Link to your organization's publishing checklist and requirements.

Before your SDK can be officially listed on our website and documentation:

1. **Complete the [Publishing Checklist](../Checklist-for-publishing-a-new-OS-project.md)**
   - Repository setup and configuration
   - Community health files
   - CI/CD automation
   - Security and testing

2. **Follow [Naming Conventions](../Naming-conventions.md)**
   - Repository name
   - Package name
   - Tagging and versioning

3. **Adhere to [Maintainer Duties](../Duties-of-a-Repository-Maintainer.md)**
   - Respond to issues within 1 week
   - Review pull requests within 2 weeks
   - Keep dependencies updated

4. **Set up CI/CD**
   - See platform-specific guides in [CI/CD Automation](../ci-and-automation/ci-and-automation.md)
   - Automate testing and releases
   - Publish to package managers automatically

5. **Submit for Review**
   - Email {{ site.organization.email }} with repository link
   - Include brief description and supported features
   - We'll review within 2 weeks

---

## Getting Help

**Questions about SDK development?**
- Email: {{ site.organization.email }}
{% if site.social.discord_url %}- Discord: [Join our community]({{ site.social.discord_url }}){% endif %}
{% if site.social.stackoverflow_tag %}- Stack Overflow: Tag your question with [`{{ site.social.stackoverflow_tag }}`](https://stackoverflow.com/questions/tagged/{{ site.social.stackoverflow_tag }}){% endif %}

**Found an issue with these guidelines?**
- [Open an issue]({{ site.organization.website }}/issues) on our documentation repository
- Suggest improvements via pull request

**API support:**
{% if site.organization.docs_url %}- [API Documentation]({{ site.organization.docs_url }}){% endif %}
- [API Status Page]({{ site.organization.website }}/status) *(customize URL)*

---

## Additional Resources

- [API Reference]({{ site.organization.docs_url }}) *(if applicable)*
- [SDK Examples Repository](https://github.com/{{ site.organization.github_org }}/sdk-examples) *(if applicable)*
- [Community SDKs](../community-sdks.md) *(if you maintain a list)*
- [Webhook Documentation]({{ site.organization.docs_url }}/webhooks) *(if applicable)*

---

**Last Updated**: 2026-01-31
