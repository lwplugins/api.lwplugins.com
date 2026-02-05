# LW Plugins API

Public REST APIs for LW Plugins services.

**Base URL:** `https://api.lwplugins.com`

## Available APIs

| API | Base Path | Description |
|-----|-----------|-------------|
| [Cookie Database](./cookie-db/) | `/v1/cookie_db` | Search and query cookie information database |

## General Information

### Authentication

Currently, all APIs are public and require no authentication.

### CORS

All endpoints support CORS with `Access-Control-Allow-Origin: *`.

### Response Format

All endpoints return JSON responses.

**Success response:**
```json
{
  "query": "search term",
  "count": 10,
  "results": [...]
}
```

**Error response:**
```json
{
  "success": false,
  "error": "Error description"
}
```

### Rate Limiting

No rate limiting is currently enforced.

## Links

- **Website:** [lwplugins.com](https://lwplugins.com)
- **GitHub:** [github.com/lwplugins](https://github.com/lwplugins)
