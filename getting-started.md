---
layout: default
title: Getting Started
---

# Getting Started with manaqr

This guide will help you get started with manaqr quickly. Whether you're using the web interface, API, or SDKs, you'll be creating QR codes in minutes.

## Table of Contents

- [Installation](#installation)
- [Authentication](#authentication)
- [Your First QR Code](#your-first-qr-code)
- [Basic Configuration](#basic-configuration)
- [Next Steps](#next-steps)

## Installation

### Web Interface

No installation required! Simply visit [manaqr.io](https://manaqr.io) and create an account to start using the web dashboard.

### Node.js

```bash
npm install manaqr
# or
yarn add manaqr
```

### Python

```bash
pip install manaqr
```

### CLI Tool

```bash
npm install -g manaqr-cli
# or
pip install manaqr-cli
```

## Authentication

### Getting Your API Key

1. Log in to your manaqr account
2. Navigate to **Settings** → **API Keys**
3. Click **Generate New API Key**
4. Copy and securely store your API key

### Using Your API Key

#### Node.js

```javascript
const Manaqr = require('manaqr');

const client = new Manaqr({
  apiKey: 'your-api-key-here'
});
```

#### Python

```python
from manaqr import Client

client = Client(api_key='your-api-key-here')
```

#### CLI

```bash
manaqr auth login --api-key your-api-key-here
```

## Your First QR Code

### Using the Web Interface

1. Click **Create New QR Code** from your dashboard
2. Select a QR code type (URL, Text, vCard, etc.)
3. Enter your data
4. Customize appearance (optional)
5. Click **Generate**
6. Download or share your QR code

### Using Node.js

```javascript
const Manaqr = require('manaqr');

const client = new Manaqr({ apiKey: 'your-api-key' });

async function createQR() {
  try {
    const qr = await client.create({
      type: 'url',
      data: 'https://example.com',
      name: 'My First QR Code'
    });
    
    console.log('QR Code created!');
    console.log('Image URL:', qr.imageUrl);
    console.log('Short URL:', qr.shortUrl);
    console.log('QR ID:', qr.id);
  } catch (error) {
    console.error('Error:', error.message);
  }
}

createQR();
```

### Using Python

```python
from manaqr import Client

client = Client(api_key='your-api-key')

# Create a QR code
qr = client.create(
    type='url',
    data='https://example.com',
    name='My First QR Code'
)

print(f'QR Code created!')
print(f'Image URL: {qr.image_url}')
print(f'Short URL: {qr.short_url}')
print(f'QR ID: {qr.id}')
```

### Using the CLI

```bash
manaqr create url https://example.com \
  --name "My First QR Code" \
  --output qrcode.png
```

## Basic Configuration

### QR Code Types

manaqr supports multiple QR code types:

- **URL** - Website links
- **Text** - Plain text messages
- **Email** - Email addresses with optional subject/body
- **Phone** - Phone numbers
- **SMS** - SMS messages with pre-filled text
- **WiFi** - WiFi network credentials
- **vCard** - Contact information
- **Event** - Calendar events

### Customization Options

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  
  // Customization
  customization: {
    // Colors
    foregroundColor: '#1a1a1a',
    backgroundColor: '#ffffff',
    
    // Logo
    logo: 'https://example.com/logo.png',
    logoSize: 0.2, // 20% of QR code size
    
    // Size
    size: 500, // pixels
    
    // Error correction level
    errorCorrection: 'H', // L, M, Q, or H
    
    // Pattern
    dotStyle: 'rounded', // square, rounded, dots
    
    // Margin
    margin: 4 // quiet zone size
  },
  
  // Tracking
  tracking: {
    enabled: true,
    campaign: 'summer-2024',
    source: 'website'
  }
});
```

## Quick Tips

### 1. Error Correction Levels

Choose the right error correction level:
- **L (Low)** - ~7% correction - Use for large, simple QR codes
- **M (Medium)** - ~15% correction - Default, good balance
- **Q (Quartile)** - ~25% correction - Good with logos
- **H (High)** - ~30% correction - Best with logos or when damage is likely

### 2. Testing QR Codes

Always test your QR codes:
```bash
manaqr test qr-code-id
```

Or use the web interface's built-in scanner.

### 3. Organizing QR Codes

Use tags and folders to organize your QR codes:
```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  tags: ['marketing', 'campaign-2024'],
  folder: 'Marketing Campaigns'
});
```

## Environment Variables

For better security, use environment variables:

```bash
# .env file
MANAQR_API_KEY=your-api-key-here
MANAQR_ENVIRONMENT=production
```

```javascript
require('dotenv').config();

const client = new Manaqr({
  apiKey: process.env.MANAQR_API_KEY
});
```

## Rate Limits

Free tier limits:
- 100 QR codes per month
- 1,000 scans per month
- 5 requests per second

Pro tier and above have higher limits. See [Pricing](https://manaqr.io/pricing) for details.

## Next Steps

Now that you've created your first QR code, explore more features:

- [User Guide](user-guide.md) - Learn about all features
- [API Reference](api-reference.md) - Complete API documentation
- [FAQ](faq.md) - Common questions and answers

## Troubleshooting

### QR Code Not Scanning

1. Ensure sufficient contrast between foreground and background
2. Check error correction level (use H with logos)
3. Verify minimum size (at least 2cm x 2cm when printed)
4. Test with multiple QR code scanners

### API Errors

```javascript
try {
  const qr = await client.create({...});
} catch (error) {
  if (error.code === 'INVALID_API_KEY') {
    console.error('Check your API key');
  } else if (error.code === 'RATE_LIMIT_EXCEEDED') {
    console.error('Rate limit exceeded. Wait before retrying.');
  } else {
    console.error('Error:', error.message);
  }
}
```

## Getting Help

- 📖 [User Guide](user-guide.md)
- 🔧 [API Reference](api-reference.md)
- 💬 [Community Forum](https://github.com/manaqr/docs/discussions)
- 📧 Email: support@manaqr.io

---

**Next**: [User Guide →](user-guide.md)
