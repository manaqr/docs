---
layout: default
title: API Reference
---

# API Reference

Complete API documentation for manaqr.

## Table of Contents

- [Authentication](#authentication)
- [QR Code Operations](#qr-code-operations)
- [Analytics](#analytics)
- [Folders and Organization](#folders-and-organization)
- [Webhooks](#webhooks)
- [Error Handling](#error-handling)
- [Rate Limits](#rate-limits)

## Base URL

```
https://api.manaqr.io/v1
```

## Authentication

All API requests require authentication via API key in the header:

```bash
Authorization: Bearer YOUR_API_KEY
```

### Example Request

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://api.manaqr.io/v1/qrcodes
```

## QR Code Operations

### Create QR Code

Create a new QR code.

**Endpoint**: `POST /qrcodes`

**Request Body**:

```json
{
  "type": "url",
  "data": "https://example.com",
  "name": "Example QR Code",
  "customization": {
    "foregroundColor": "#000000",
    "backgroundColor": "#FFFFFF",
    "size": 500,
    "errorCorrection": "M",
    "logo": "https://example.com/logo.png",
    "logoSize": 0.2
  },
  "tracking": {
    "enabled": true,
    "campaign": "summer-2024"
  },
  "tags": ["marketing", "campaign"]
}
```

**Response**: `201 Created`

```json
{
  "id": "qr_abc123",
  "type": "url",
  "data": "https://example.com",
  "name": "Example QR Code",
  "imageUrl": "https://cdn.manaqr.io/qr/abc123.png",
  "shortUrl": "https://mnq.to/abc123",
  "customization": {...},
  "tracking": {...},
  "tags": ["marketing", "campaign"],
  "createdAt": "2024-06-15T10:30:00Z",
  "updatedAt": "2024-06-15T10:30:00Z"
}
```

### Get QR Code

Retrieve a specific QR code.

**Endpoint**: `GET /qrcodes/{id}`

**Response**: `200 OK`

```json
{
  "id": "qr_abc123",
  "type": "url",
  "data": "https://example.com",
  "name": "Example QR Code",
  "imageUrl": "https://cdn.manaqr.io/qr/abc123.png",
  "shortUrl": "https://mnq.to/abc123",
  "stats": {
    "totalScans": 150,
    "uniqueScans": 87,
    "lastScanAt": "2024-06-15T14:20:00Z"
  },
  "createdAt": "2024-06-15T10:30:00Z",
  "updatedAt": "2024-06-15T10:30:00Z"
}
```

### List QR Codes

List all QR codes with pagination and filtering.

**Endpoint**: `GET /qrcodes`

**Query Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `limit` | integer | Number of results (default: 50, max: 100) |
| `offset` | integer | Pagination offset (default: 0) |
| `sort` | string | Sort field (created_at, name, scans) |
| `order` | string | Sort order (asc, desc) |
| `tags` | string | Filter by tags (comma-separated) |
| `folder` | string | Filter by folder ID |
| `search` | string | Search by name or data |

**Example Request**:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     "https://api.manaqr.io/v1/qrcodes?limit=10&sort=scans&order=desc&tags=marketing"
```

**Response**: `200 OK`

```json
{
  "data": [
    {
      "id": "qr_abc123",
      "type": "url",
      "name": "Example QR Code",
      "stats": {
        "totalScans": 150
      },
      "createdAt": "2024-06-15T10:30:00Z"
    }
  ],
  "pagination": {
    "total": 245,
    "limit": 10,
    "offset": 0,
    "hasMore": true
  }
}
```

### Update QR Code

Update an existing QR code.

**Endpoint**: `PATCH /qrcodes/{id}`

**Request Body**:

```json
{
  "name": "Updated Name",
  "tags": ["updated", "active"],
  "customization": {
    "foregroundColor": "#1e40af"
  }
}
```

**Response**: `200 OK`

```json
{
  "id": "qr_abc123",
  "name": "Updated Name",
  "tags": ["updated", "active"],
  "updatedAt": "2024-06-15T15:00:00Z"
}
```

### Delete QR Code

Delete a QR code.

**Endpoint**: `DELETE /qrcodes/{id}`

**Response**: `204 No Content`

### Bulk Create

Create multiple QR codes in one request.

**Endpoint**: `POST /qrcodes/bulk`

**Request Body**:

```json
{
  "qrcodes": [
    {
      "type": "url",
      "data": "https://example.com/1",
      "name": "QR 1"
    },
    {
      "type": "url",
      "data": "https://example.com/2",
      "name": "QR 2"
    }
  ]
}
```

**Response**: `201 Created`

```json
{
  "success": 2,
  "failed": 0,
  "qrcodes": [
    {
      "id": "qr_abc123",
      "name": "QR 1"
    },
    {
      "id": "qr_abc124",
      "name": "QR 2"
    }
  ]
}
```

### Download QR Code

Download QR code in various formats.

**Endpoint**: `GET /qrcodes/{id}/download`

**Query Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `format` | string | File format (png, jpg, svg, pdf, eps) |
| `size` | integer | Size in pixels (for raster formats) |
| `dpi` | integer | DPI for print formats (default: 300) |

**Example Request**:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     "https://api.manaqr.io/v1/qrcodes/qr_abc123/download?format=png&size=1000" \
     -o qrcode.png
```

**Response**: `200 OK` (binary data)

## Analytics

### Get QR Code Statistics

Retrieve scan statistics for a QR code.

**Endpoint**: `GET /qrcodes/{id}/stats`

**Query Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `startDate` | string | Start date (ISO 8601) |
| `endDate` | string | End date (ISO 8601) |
| `groupBy` | string | Grouping (hour, day, week, month) |

**Example Request**:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     "https://api.manaqr.io/v1/qrcodes/qr_abc123/stats?startDate=2024-01-01&endDate=2024-12-31&groupBy=day"
```

**Response**: `200 OK`

```json
{
  "id": "qr_abc123",
  "period": {
    "start": "2024-01-01T00:00:00Z",
    "end": "2024-12-31T23:59:59Z"
  },
  "summary": {
    "totalScans": 1234,
    "uniqueScans": 567,
    "avgScansPerDay": 3.4,
    "conversionRate": 0.45
  },
  "timeSeries": [
    {
      "date": "2024-06-01",
      "scans": 15,
      "uniqueScans": 12
    },
    {
      "date": "2024-06-02",
      "scans": 23,
      "uniqueScans": 18
    }
  ],
  "geographic": {
    "countries": [
      {
        "code": "US",
        "name": "United States",
        "scans": 500
      },
      {
        "code": "UK",
        "name": "United Kingdom",
        "scans": 300
      }
    ],
    "cities": [
      {
        "name": "New York",
        "country": "US",
        "scans": 200
      }
    ]
  },
  "devices": {
    "types": [
      {
        "type": "mobile",
        "scans": 800
      },
      {
        "type": "desktop",
        "scans": 300
      }
    ],
    "os": [
      {
        "name": "iOS",
        "scans": 600
      },
      {
        "name": "Android",
        "scans": 500
      }
    ],
    "browsers": [
      {
        "name": "Safari",
        "scans": 550
      },
      {
        "name": "Chrome",
        "scans": 500
      }
    ]
  },
  "referrers": [
    {
      "source": "direct",
      "scans": 700
    },
    {
      "source": "instagram",
      "scans": 300
    }
  ]
}
```

### Get Scan Events

Retrieve individual scan events.

**Endpoint**: `GET /qrcodes/{id}/scans`

**Query Parameters**:

| Parameter | Type | Description |
|-----------|------|-------------|
| `limit` | integer | Number of results (default: 50, max: 100) |
| `offset` | integer | Pagination offset |
| `startDate` | string | Start date (ISO 8601) |
| `endDate` | string | End date (ISO 8601) |

**Response**: `200 OK`

```json
{
  "data": [
    {
      "id": "scan_xyz789",
      "timestamp": "2024-06-15T14:20:00Z",
      "country": "US",
      "city": "New York",
      "region": "NY",
      "device": "iPhone 13",
      "os": "iOS 16.5",
      "browser": "Safari",
      "ipAddress": "192.168.1.1",
      "userAgent": "Mozilla/5.0..."
    }
  ],
  "pagination": {
    "total": 1234,
    "limit": 50,
    "offset": 0,
    "hasMore": true
  }
}
```

## Folders and Organization

### Create Folder

**Endpoint**: `POST /folders`

**Request Body**:

```json
{
  "name": "Marketing 2024",
  "description": "Q2 Marketing Campaign",
  "parent": null
}
```

**Response**: `201 Created`

```json
{
  "id": "folder_def456",
  "name": "Marketing 2024",
  "description": "Q2 Marketing Campaign",
  "parent": null,
  "qrCodeCount": 0,
  "createdAt": "2024-06-15T10:30:00Z"
}
```

### List Folders

**Endpoint**: `GET /folders`

**Response**: `200 OK`

```json
{
  "data": [
    {
      "id": "folder_def456",
      "name": "Marketing 2024",
      "qrCodeCount": 15,
      "createdAt": "2024-06-15T10:30:00Z"
    }
  ]
}
```

### Move QR Code to Folder

**Endpoint**: `POST /qrcodes/{id}/move`

**Request Body**:

```json
{
  "folderId": "folder_def456"
}
```

**Response**: `200 OK`

## Webhooks

### Create Webhook

**Endpoint**: `POST /webhooks`

**Request Body**:

```json
{
  "url": "https://your-app.com/webhook",
  "events": [
    "qr.created",
    "qr.scanned",
    "qr.updated",
    "qr.deleted"
  ],
  "secret": "webhook-secret-key"
}
```

**Response**: `201 Created`

```json
{
  "id": "webhook_ghi789",
  "url": "https://your-app.com/webhook",
  "events": ["qr.created", "qr.scanned", "qr.updated", "qr.deleted"],
  "active": true,
  "createdAt": "2024-06-15T10:30:00Z"
}
```

### Webhook Payload

Example webhook payload for `qr.scanned` event:

```json
{
  "event": "qr.scanned",
  "timestamp": "2024-06-15T14:20:00Z",
  "data": {
    "qrCodeId": "qr_abc123",
    "scanId": "scan_xyz789",
    "country": "US",
    "city": "New York",
    "device": "iPhone 13",
    "os": "iOS 16.5"
  }
}
```

### Verifying Webhooks

Verify webhook signatures using HMAC-SHA256:

```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const hash = crypto
    .createHmac('sha256', secret)
    .update(JSON.stringify(payload))
    .digest('hex');
  
  return signature === hash;
}
```

### List Webhooks

**Endpoint**: `GET /webhooks`

**Response**: `200 OK`

```json
{
  "data": [
    {
      "id": "webhook_ghi789",
      "url": "https://your-app.com/webhook",
      "events": ["qr.created", "qr.scanned"],
      "active": true,
      "lastDeliveryAt": "2024-06-15T14:20:00Z",
      "lastStatus": 200
    }
  ]
}
```

### Delete Webhook

**Endpoint**: `DELETE /webhooks/{id}`

**Response**: `204 No Content`

## Error Handling

### Error Response Format

```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "The request body is invalid",
    "details": [
      {
        "field": "data",
        "message": "This field is required"
      }
    ]
  }
}
```

### Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `INVALID_REQUEST` | 400 | Invalid request format or parameters |
| `UNAUTHORIZED` | 401 | Invalid or missing API key |
| `FORBIDDEN` | 403 | Insufficient permissions |
| `NOT_FOUND` | 404 | Resource not found |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests |
| `INTERNAL_ERROR` | 500 | Internal server error |

### Handling Errors

```javascript
const Manaqr = require('manaqr');
const client = new Manaqr({ apiKey: 'your-api-key' });

try {
  const qr = await client.create({
    type: 'url',
    data: 'https://example.com'
  });
} catch (error) {
  if (error.code === 'RATE_LIMIT_EXCEEDED') {
    console.error('Rate limit exceeded. Retry after:', error.retryAfter);
  } else if (error.code === 'INVALID_REQUEST') {
    console.error('Invalid request:', error.details);
  } else {
    console.error('Error:', error.message);
  }
}
```

## Rate Limits

### Limits by Tier

| Tier | Requests/Second | Requests/Hour | QR Codes/Month |
|------|----------------|---------------|----------------|
| Free | 5 | 1,000 | 100 |
| Pro | 20 | 10,000 | 1,000 |
| Business | 50 | 50,000 | 10,000 |
| Enterprise | Custom | Custom | Unlimited |

### Rate Limit Headers

```
X-RateLimit-Limit: 20
X-RateLimit-Remaining: 15
X-RateLimit-Reset: 1686830400
```

### Handling Rate Limits

```javascript
async function createWithRetry(data, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await client.create(data);
    } catch (error) {
      if (error.code === 'RATE_LIMIT_EXCEEDED' && i < maxRetries - 1) {
        const delay = error.retryAfter || 1000;
        await new Promise(resolve => setTimeout(resolve, delay));
      } else {
        throw error;
      }
    }
  }
}
```

## SDK Reference

### Node.js

```javascript
const Manaqr = require('manaqr');

// Initialize
const client = new Manaqr({
  apiKey: 'your-api-key',
  timeout: 30000, // 30 seconds
  retries: 3
});

// Create QR code
const qr = await client.create({...});

// Get QR code
const qr = await client.get('qr-id');

// List QR codes
const qrCodes = await client.list({...});

// Update QR code
const updated = await client.update('qr-id', {...});

// Delete QR code
await client.delete('qr-id');

// Get statistics
const stats = await client.getStats('qr-id', {...});

// Download QR code
const image = await client.download('qr-id', {...});
```

### Python

```python
from manaqr import Client

# Initialize
client = Client(
    api_key='your-api-key',
    timeout=30,
    retries=3
)

# Create QR code
qr = client.create(...)

# Get QR code
qr = client.get('qr-id')

# List QR codes
qr_codes = client.list(...)

# Update QR code
updated = client.update('qr-id', ...)

# Delete QR code
client.delete('qr-id')

# Get statistics
stats = client.get_stats('qr-id', ...)

# Download QR code
image = client.download('qr-id', ...)
```

## Examples

### Complete Integration Example

```javascript
const Manaqr = require('manaqr');
const express = require('express');

const app = express();
const client = new Manaqr({ apiKey: process.env.MANAQR_API_KEY });

// Create QR code endpoint
app.post('/qrcodes', async (req, res) => {
  try {
    const qr = await client.create({
      type: 'url',
      data: req.body.url,
      name: req.body.name,
      customization: {
        foregroundColor: '#1e40af',
        logo: 'https://example.com/logo.png'
      },
      tracking: {
        enabled: true,
        campaign: req.body.campaign
      }
    });
    
    res.json(qr);
  } catch (error) {
    res.status(error.statusCode || 500).json({
      error: error.message
    });
  }
});

// Webhook endpoint
app.post('/webhook', async (req, res) => {
  const signature = req.headers['x-manaqr-signature'];
  const payload = req.body;
  
  if (!verifyWebhook(payload, signature, process.env.WEBHOOK_SECRET)) {
    return res.status(401).send('Invalid signature');
  }
  
  if (payload.event === 'qr.scanned') {
    console.log('QR scanned:', payload.data);
    // Handle scan event
  }
  
  res.sendStatus(200);
});

app.listen(3000);
```

## Need Help?

- [User Guide](user-guide.md) - Comprehensive usage guide
- [FAQ](faq.md) - Frequently asked questions
- [GitHub Discussions](https://github.com/manaqr/docs/discussions)
- Email: api@manaqr.io

---

**Previous**: [← User Guide](user-guide.md) | **Next**: [FAQ →](faq.md)
