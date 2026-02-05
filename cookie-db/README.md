# Cookie Database API

REST API for querying cookie information from a comprehensive database of 2000+ cookies.

**Base URL:** `https://api.lwplugins.com/v1/cookie_db`

## Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | [`/search`](#get-search) | Search cookies by name |
| `GET` | [`/stats`](#get-stats) | Get database statistics |
| `GET` | [`/platform`](#get-platform) | List platforms or get cookies by platform |
| `GET` | [`/cookies`](#get-cookies) | List all cookies (limited to 100) |

---

## GET /search

Search cookies by name with support for partial and wildcard matching.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|:--------:|-------------|
| `q` | string | Yes | Cookie name to search |

### Search Logic

1. **Exact match** — Direct cookie name match (highest priority)
2. **Wildcard match** — Cookies with `wildcardMatch: true` match by prefix (e.g., `_ga_*` matches `_ga_ABC123`)
3. **Contains match** — Partial match when search term appears in cookie name

### Example

```bash
curl "https://api.lwplugins.com/v1/cookie_db/search?q=_ga"
```

```json
{
  "query": "_ga",
  "count": 13,
  "results": [
    {
      "id": "256c18e8-d881-11e9-8a34-2a2ae2dbcce4",
      "platform": "Google Analytics",
      "category": "Analytics",
      "name": "_ga",
      "domain": "",
      "description": "ID used to identify users",
      "retention": "2 years",
      "dataController": "Google",
      "privacyLink": "https://business.safety.google/privacy/",
      "wildcardMatch": false
    }
  ]
}
```

---

## GET /stats

Get database statistics including totals, category breakdown, and top platforms.

### Example

```bash
curl "https://api.lwplugins.com/v1/cookie_db/stats"
```

```json
{
  "totalCookies": 2264,
  "totalPlatforms": 354,
  "categories": {
    "Analytics": 390,
    "Functional": 977,
    "Marketing": 861,
    "Necessary": 9,
    "Personalization": 3,
    "Security": 24
  },
  "topPlatforms": [
    { "name": "Nexx360", "count": 98 },
    { "name": "Salesforce", "count": 93 },
    { "name": "LinkedIn", "count": 79 }
  ]
}
```

---

## GET /platform

List all platforms with cookie counts, or get all cookies for a specific platform.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|:--------:|-------------|
| `name` | string | No | Platform name (omit to list all platforms) |

### Example: List All Platforms

```bash
curl "https://api.lwplugins.com/v1/cookie_db/platform"
```

```json
{
  "Google Analytics": 21,
  "Facebook": 26,
  "Shopify": 73,
  "WordPress": 29
}
```

### Example: Get Platform Cookies

```bash
curl "https://api.lwplugins.com/v1/cookie_db/platform?name=Google%20Analytics"
```

```json
{
  "query": "Google Analytics",
  "count": 21,
  "results": [
    {
      "id": "256c18e8-d881-11e9-8a34-2a2ae2dbcce4",
      "platform": "Google Analytics",
      "category": "Analytics",
      "name": "_ga",
      "domain": "",
      "description": "ID used to identify users",
      "retention": "2 years",
      "dataController": "Google",
      "privacyLink": "https://business.safety.google/privacy/",
      "wildcardMatch": false
    }
  ]
}
```

---

## GET /cookies

List all cookies in the database.

> **Note:** Response is limited to the first 100 cookies. Use `/search` or `/platform` for targeted queries.

### Example

```bash
curl "https://api.lwplugins.com/v1/cookie_db/cookies"
```

```json
{
  "query": "",
  "count": 2264,
  "results": [ /* First 100 cookies */ ]
}
```

---

## Data Models

### Cookie Object

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique identifier (UUID) |
| `platform` | string | Service/platform name |
| `category` | string | Cookie category |
| `name` | string | Cookie name |
| `domain` | string | Domain (empty = first-party) |
| `description` | string | Human-readable description |
| `retention` | string | Retention period |
| `dataController` | string | Data controller name |
| `privacyLink` | string | Privacy policy URL |
| `wildcardMatch` | boolean | If `true`, name is a prefix pattern |

### Categories

| Category | Description |
|----------|-------------|
| `Necessary` | Strictly necessary for basic functionality |
| `Functional` | Enhanced functionality and preferences |
| `Analytics` | Statistical and performance tracking |
| `Marketing` | Advertising and marketing purposes |
| `Personalization` | Content personalization |
| `Security` | Security-related cookies |
