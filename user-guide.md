---
layout: default
title: User Guide
---

# User Guide

Complete guide to using manaqr for QR code management and generation.

## Table of Contents

- [Dashboard Overview](#dashboard-overview)
- [Creating QR Codes](#creating-qr-codes)
- [Managing QR Codes](#managing-qr-codes)
- [Customization](#customization)
- [Analytics and Tracking](#analytics-and-tracking)
- [Batch Operations](#batch-operations)
- [Export and Integration](#export-and-integration)
- [Advanced Features](#advanced-features)

## Dashboard Overview

### Navigation

The manaqr dashboard provides a central hub for all your QR code activities:

- **Home** - Overview of recent QR codes and statistics
- **QR Codes** - Browse and manage all your QR codes
- **Analytics** - View detailed scan analytics
- **Settings** - Configure your account and API keys
- **Templates** - Save and reuse QR code designs

### Quick Actions

- **Create New** - Generate a new QR code
- **Import** - Bulk import QR codes from CSV
- **Folders** - Organize QR codes into folders
- **Search** - Find QR codes by name, type, or tag

## Creating QR Codes

### QR Code Types

#### 1. URL QR Codes

Link to any website:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  name: 'Company Website'
});
```

**Use Cases**: Marketing materials, product packaging, business cards

#### 2. Text QR Codes

Store plain text:

```javascript
const qr = await client.create({
  type: 'text',
  data: 'Welcome to our event! Your access code is: ABC123',
  name: 'Event Access Code'
});
```

**Use Cases**: Instructions, access codes, messages

#### 3. Email QR Codes

Pre-fill email information:

```javascript
const qr = await client.create({
  type: 'email',
  data: {
    email: 'contact@example.com',
    subject: 'Product Inquiry',
    body: 'I would like to know more about...'
  },
  name: 'Contact Email'
});
```

**Use Cases**: Customer support, feedback forms, contact cards

#### 4. Phone QR Codes

Dial phone numbers directly:

```javascript
const qr = await client.create({
  type: 'phone',
  data: '+1-555-123-4567',
  name: 'Customer Service'
});
```

**Use Cases**: Support hotlines, sales contacts, emergency numbers

#### 5. SMS QR Codes

Send pre-filled text messages:

```javascript
const qr = await client.create({
  type: 'sms',
  data: {
    phone: '+1-555-123-4567',
    message: 'I would like more information'
  },
  name: 'Info Request'
});
```

**Use Cases**: Promotions, feedback collection, quick responses

#### 6. WiFi QR Codes

Share WiFi credentials:

```javascript
const qr = await client.create({
  type: 'wifi',
  data: {
    ssid: 'MyNetwork',
    password: 'securepassword',
    encryption: 'WPA', // WPA, WEP, or nopass
    hidden: false
  },
  name: 'Office WiFi'
});
```

**Use Cases**: Guest networks, events, cafes, hotels

#### 7. vCard QR Codes

Share contact information:

```javascript
const qr = await client.create({
  type: 'vcard',
  data: {
    firstName: 'John',
    lastName: 'Doe',
    organization: 'Example Corp',
    title: 'CEO',
    email: 'john@example.com',
    phone: '+1-555-123-4567',
    website: 'https://example.com',
    address: {
      street: '123 Main St',
      city: 'New York',
      state: 'NY',
      zip: '10001',
      country: 'USA'
    }
  },
  name: 'John Doe Contact'
});
```

**Use Cases**: Business cards, networking events, email signatures

#### 8. Event QR Codes

Add calendar events:

```javascript
const qr = await client.create({
  type: 'event',
  data: {
    title: 'Annual Conference',
    location: 'Convention Center',
    startDate: '2024-09-15T09:00:00',
    endDate: '2024-09-15T17:00:00',
    description: 'Join us for our annual conference'
  },
  name: 'Conference Event'
});
```

**Use Cases**: Events, meetings, conferences, webinars

## Managing QR Codes

### Viewing QR Codes

```javascript
// Get all QR codes
const qrCodes = await client.list({
  limit: 50,
  offset: 0,
  sort: 'created_at',
  order: 'desc'
});

// Get specific QR code
const qr = await client.get('qr-code-id');
```

### Updating QR Codes

```javascript
// Update QR code
const updated = await client.update('qr-code-id', {
  name: 'New Name',
  tags: ['updated', 'active']
});
```

### Deleting QR Codes

```javascript
// Delete single QR code
await client.delete('qr-code-id');

// Bulk delete
await client.bulkDelete(['id1', 'id2', 'id3']);
```

### Organizing with Folders

```javascript
// Create folder
const folder = await client.createFolder({
  name: 'Marketing 2024',
  description: 'Q2 Marketing Campaign'
});

// Move QR code to folder
await client.moveToFolder('qr-code-id', folder.id);
```

### Tagging System

```javascript
// Add tags
await client.addTags('qr-code-id', ['campaign', 'print', 'active']);

// Remove tags
await client.removeTags('qr-code-id', ['active']);

// Search by tags
const qrCodes = await client.search({
  tags: ['campaign', 'print']
});
```

## Customization

### Colors

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  customization: {
    foregroundColor: '#1e40af', // Blue
    backgroundColor: '#ffffff', // White
    
    // Gradient support
    gradientType: 'linear',
    gradientStartColor: '#1e40af',
    gradientEndColor: '#7c3aed'
  }
});
```

### Logos

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  customization: {
    logo: 'https://example.com/logo.png',
    logoSize: 0.2, // 20% of QR size
    logoMargin: 2, // Pixels around logo
    logoCornerRadius: 8 // Rounded corners
  }
});
```

### Patterns and Styles

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  customization: {
    dotStyle: 'rounded', // square, rounded, dots, classy
    cornerSquareStyle: 'extra-rounded', // dot, square, extra-rounded
    cornerDotStyle: 'dot' // dot, square
  }
});
```

### Frames and Backgrounds

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  customization: {
    frame: {
      style: 'banner', // none, banner, box, circle
      text: 'Scan Me!',
      color: '#1e40af',
      textColor: '#ffffff'
    },
    backgroundImage: 'https://example.com/bg.png',
    backgroundOpacity: 0.1
  }
});
```

## Analytics and Tracking

### Enabling Tracking

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  tracking: {
    enabled: true,
    campaign: 'summer-2024',
    source: 'instagram',
    medium: 'social'
  }
});
```

### Viewing Analytics

```javascript
// Get scan statistics
const stats = await client.getStats('qr-code-id', {
  startDate: '2024-01-01',
  endDate: '2024-12-31',
  groupBy: 'day' // hour, day, week, month
});

console.log(stats);
// {
//   totalScans: 1234,
//   uniqueScans: 567,
//   scansByDay: [...],
//   topCountries: [...],
//   topDevices: [...],
//   topBrowsers: [...]
// }
```

### Real-time Analytics

Subscribe to real-time scan events:

```javascript
client.subscribe('qr-code-id', (event) => {
  console.log('New scan:', event);
  // {
  //   timestamp: '2024-06-15T10:30:00Z',
  //   country: 'US',
  //   city: 'New York',
  //   device: 'iPhone',
  //   browser: 'Safari'
  // }
});
```

### Analytics Dashboard

Access comprehensive analytics in the web dashboard:

- **Overview** - Total scans, unique visitors, conversion rate
- **Time Series** - Scan trends over time
- **Geographic** - Map view of scan locations
- **Devices** - Device and OS breakdown
- **Referrers** - Traffic sources
- **Conversion** - Goal tracking and funnel analysis

## Batch Operations

### Bulk Create

```javascript
const qrCodes = await client.bulkCreate([
  { type: 'url', data: 'https://example.com/page1', name: 'Page 1' },
  { type: 'url', data: 'https://example.com/page2', name: 'Page 2' },
  { type: 'url', data: 'https://example.com/page3', name: 'Page 3' }
]);
```

### CSV Import

```javascript
const fs = require('fs');

const csvData = fs.readFileSync('qrcodes.csv', 'utf8');
const result = await client.importCSV(csvData);

console.log(`Imported ${result.success} QR codes`);
```

CSV format:
```csv
type,data,name,tags
url,https://example.com/1,Product 1,"product,active"
url,https://example.com/2,Product 2,"product,active"
```

### Bulk Update

```javascript
await client.bulkUpdate(['id1', 'id2', 'id3'], {
  tags: ['updated'],
  folder: 'archive-2024'
});
```

## Export and Integration

### Export Formats

```javascript
// Download QR code
const image = await client.download('qr-code-id', {
  format: 'png', // png, jpg, svg, pdf, eps
  size: 1000, // pixels (for raster formats)
  dpi: 300 // for print formats
});

// Save to file
fs.writeFileSync('qrcode.png', image);
```

### Webhooks

Configure webhooks for real-time notifications:

```javascript
await client.createWebhook({
  url: 'https://your-app.com/webhook',
  events: ['qr.created', 'qr.scanned', 'qr.updated'],
  secret: 'webhook-secret'
});
```

### API Integration

Integrate with other platforms:

```javascript
// Zapier integration
const zapier = await client.enableIntegration('zapier');

// Shopify integration
const shopify = await client.enableIntegration('shopify', {
  apiKey: 'shopify-api-key',
  shopDomain: 'your-shop.myshopify.com'
});
```

## Advanced Features

### Dynamic QR Codes

Create QR codes that can be updated without reprinting:

```javascript
const qr = await client.create({
  type: 'dynamic',
  target: 'https://example.com',
  name: 'Dynamic QR'
});

// Update target later
await client.updateTarget('qr-code-id', 'https://newurl.com');
```

### A/B Testing

Test different destinations:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  abTest: {
    enabled: true,
    variants: [
      { url: 'https://example.com/variant-a', weight: 50 },
      { url: 'https://example.com/variant-b', weight: 50 }
    ]
  }
});
```

### Geofencing

Route users based on location:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  geofencing: [
    { country: 'US', target: 'https://example.com/us' },
    { country: 'UK', target: 'https://example.com/uk' },
    { default: 'https://example.com' }
  ]
});
```

### Password Protection

Protect QR code content:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com/private',
  security: {
    password: 'secret123',
    maxScans: 100, // Limit total scans
    expiresAt: '2024-12-31T23:59:59Z'
  }
});
```

### Templates

Save and reuse designs:

```javascript
// Save as template
await client.saveTemplate('qr-code-id', {
  name: 'Brand Template',
  description: 'Standard brand colors and logo'
});

// Use template
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  template: 'brand-template-id'
});
```

## Best Practices

### 1. Design Guidelines

- **Contrast**: Ensure 3:1 minimum contrast ratio
- **Size**: Minimum 2cm x 2cm for print
- **Quiet Zone**: Maintain 4-module margin
- **Logo**: Keep under 30% of QR size
- **Error Correction**: Use H level with logos

### 2. Testing

Always test QR codes:
- Test with multiple devices and scanners
- Verify in different lighting conditions
- Test at actual print size
- Check URL redirects work correctly

### 3. Analytics Setup

- Use consistent naming conventions
- Tag QR codes for easy filtering
- Set up conversion tracking
- Monitor performance regularly

### 4. Security

- Use HTTPS URLs only
- Enable password protection for sensitive content
- Set expiration dates when appropriate
- Monitor for unusual scan patterns
- Rotate API keys regularly

## Troubleshooting

### Common Issues

**QR Code Not Scanning**
- Increase contrast
- Use higher error correction
- Remove or reduce logo size
- Increase quiet zone margin

**Low Scan Rates**
- Test in actual environment
- Verify placement and visibility
- Check call-to-action clarity
- Ensure target URL is mobile-friendly

**Analytics Not Working**
- Verify tracking is enabled
- Check webhook configuration
- Ensure proper URL parameters
- Review firewall/privacy settings

## Need Help?

- [API Reference](api-reference.md) - Detailed API documentation
- [FAQ](faq.md) - Frequently asked questions
- [Community Forum](https://github.com/manaqr/docs/discussions)
- Email: support@manaqr.io

---

**Previous**: [← Getting Started](getting-started.md) | **Next**: [API Reference →](api-reference.md)
