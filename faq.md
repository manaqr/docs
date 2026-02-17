---
layout: default
title: FAQ
---

# Frequently Asked Questions

Find answers to common questions about manaqr.

## General Questions

### What is manaqr?

manaqr is a comprehensive QR code management and generation system that allows you to create, customize, track, and manage QR codes at scale. It provides a web dashboard, REST API, and SDKs for multiple programming languages.

### Is manaqr free?

manaqr offers a free tier with limited features:
- 100 QR codes per month
- 1,000 scans per month
- Basic customization
- Standard support

Paid plans offer more QR codes, scans, advanced features, and priority support.

### How do I get started?

1. Sign up at [manaqr.io](https://manaqr.io)
2. Get your API key from Settings → API Keys
3. Install the SDK: `npm install manaqr` or `pip install manaqr`
4. Follow the [Getting Started Guide](getting-started.md)

### What programming languages are supported?

manaqr provides official SDKs for:
- JavaScript/Node.js
- Python
- PHP (coming soon)
- Ruby (coming soon)
- Go (coming soon)

You can also use the REST API directly from any language.

## QR Code Creation

### What types of QR codes can I create?

manaqr supports multiple QR code types:
- **URL** - Website links
- **Text** - Plain text
- **Email** - Email addresses with subject/body
- **Phone** - Phone numbers
- **SMS** - Text messages
- **WiFi** - WiFi credentials
- **vCard** - Contact information
- **Event** - Calendar events

### Can I customize the appearance of QR codes?

Yes! You can customize:
- Colors (foreground and background)
- Logos and images
- Patterns and styles
- Frames and borders
- Size and resolution
- Error correction levels

### What is the recommended size for QR codes?

For print: Minimum 2cm x 2cm (0.8" x 0.8")  
For digital: 300x300 pixels minimum  
For high-quality print: Use at least 1000x1000 pixels at 300 DPI

### Can I add my logo to QR codes?

Yes! Upload your logo and manaqr will:
- Automatically resize and position it
- Maintain QR code scannability
- Apply appropriate error correction
- Support transparency

Keep logos under 30% of the QR code size for best results.

### What error correction level should I use?

- **L (Low, ~7%)** - Simple QR codes without logos
- **M (Medium, ~15%)** - Default, good for most cases
- **Q (Quartile, ~25%)** - Use with small logos
- **H (High, ~30%)** - Best for logos or damaged surfaces

Higher levels create denser QR codes but allow more damage/obstruction.

## Analytics and Tracking

### How does QR code tracking work?

When tracking is enabled, manaqr creates a short URL that redirects to your target. This allows us to:
- Count scans
- Track location data
- Identify devices and browsers
- Measure engagement
- Provide real-time analytics

### What analytics data is collected?

We collect:
- Timestamp of scan
- Country, city, and region
- Device type and OS
- Browser information
- IP address (anonymized)
- Referrer source

We respect user privacy and comply with GDPR, CCPA, and other privacy regulations.

### Can I track scans in real-time?

Yes! Use webhooks to receive real-time notifications when QR codes are scanned:

```javascript
await client.createWebhook({
  url: 'https://your-app.com/webhook',
  events: ['qr.scanned']
});
```

### How long is analytics data retained?

- **Free tier**: 30 days
- **Pro tier**: 1 year
- **Business tier**: 2 years
- **Enterprise tier**: Unlimited

You can export data anytime in CSV or JSON format.

### Can I disable tracking for specific QR codes?

Yes! Set `tracking.enabled` to `false` when creating the QR code:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  tracking: { enabled: false }
});
```

This creates a static QR code that directly encodes the URL.

## Dynamic QR Codes

### What are dynamic QR codes?

Dynamic QR codes use a short URL that redirects to your target. You can change the destination without reprinting the QR code.

### How do I create a dynamic QR code?

All QR codes with tracking enabled are dynamic. Create one like this:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  tracking: { enabled: true }
});

// Update target later
await client.updateTarget(qr.id, 'https://newurl.com');
```

### What's the difference between static and dynamic QR codes?

| Feature | Static | Dynamic |
|---------|--------|---------|
| Edit destination | ❌ No | ✅ Yes |
| Track scans | ❌ No | ✅ Yes |
| Analytics | ❌ No | ✅ Yes |
| URL length | Direct | Short URL |
| Price | Lower | Higher |

### Can I convert a static QR code to dynamic?

No, this requires generating a new QR code. Plan ahead if you need to update destinations later.

## Integration and API

### What is an API key?

An API key authenticates your requests to the manaqr API. Keep it secure and never share it publicly.

Get your API key from Settings → API Keys in the dashboard.

### How do I use the API?

Include your API key in the Authorization header:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://api.manaqr.io/v1/qrcodes
```

See the [API Reference](api-reference.md) for complete documentation.

### What are the rate limits?

Rate limits vary by tier:

| Tier | Requests/Second | Requests/Hour |
|------|----------------|---------------|
| Free | 5 | 1,000 |
| Pro | 20 | 10,000 |
| Business | 50 | 50,000 |
| Enterprise | Custom | Custom |

Exceeding limits returns a 429 status code with `Retry-After` header.

### Can I use manaqr with Zapier?

Yes! manaqr integrates with Zapier, allowing you to:
- Create QR codes from other apps
- Trigger actions when QR codes are scanned
- Send scan data to spreadsheets, CRMs, etc.

Search for "manaqr" in Zapier to get started.

### Does manaqr support webhooks?

Yes! Set up webhooks to receive real-time notifications for:
- QR code creation
- QR code scans
- QR code updates
- QR code deletion

See [Webhooks](api-reference.md#webhooks) in the API Reference.

## Security and Privacy

### Is my data secure?

Yes! manaqr uses:
- TLS/SSL encryption for all connections
- AES-256 encryption for data at rest
- Regular security audits
- SOC 2 Type II compliance (Enterprise tier)

### Do you comply with privacy regulations?

Yes, we comply with:
- GDPR (General Data Protection Regulation)
- CCPA (California Consumer Privacy Act)
- Other regional privacy laws

Users can request data export or deletion anytime.

### Can I password-protect QR codes?

Yes! Add password protection when creating QR codes:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com/private',
  security: {
    password: 'secret123'
  }
});
```

Users must enter the password before accessing the content.

### Can QR codes expire?

Yes! Set an expiration date:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  security: {
    expiresAt: '2024-12-31T23:59:59Z'
  }
});
```

After expiration, scans will show an error message.

### Can I limit the number of scans?

Yes! Set a maximum scan limit:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  security: {
    maxScans: 100
  }
});
```

After reaching the limit, new scans will be blocked.

## Billing and Pricing

### What payment methods do you accept?

We accept:
- Credit cards (Visa, MasterCard, Amex)
- Debit cards
- PayPal
- Bank transfers (Enterprise plans)

### Can I upgrade or downgrade my plan?

Yes! Upgrade or downgrade anytime from Settings → Billing. Changes take effect immediately.

- **Upgrade**: Prorated charge for remaining period
- **Downgrade**: Credit applied to next billing cycle

### What happens if I exceed my plan limits?

- **QR codes**: Creation blocked until next month or plan upgrade
- **Scans**: Tracking continues but may be throttled
- **API requests**: 429 status code returned

We'll send email warnings before you reach limits.

### Is there a money-back guarantee?

Yes! We offer a 30-day money-back guarantee on all paid plans. No questions asked.

### Do you offer discounts?

Yes! We offer discounts for:
- Annual billing (20% off)
- Non-profits (50% off)
- Educational institutions (50% off)
- Startups (inquire for custom pricing)

Contact sales@manaqr.io for discount codes.

## Technical Issues

### My QR code won't scan. What's wrong?

Common issues:
1. **Low contrast** - Ensure sufficient contrast between foreground and background
2. **Too small** - QR code must be at least 2cm x 2cm for print
3. **Logo too large** - Keep logos under 30% of QR size
4. **Poor print quality** - Use high-resolution images (300+ DPI)
5. **Damaged or dirty** - Clean the surface or use higher error correction

### The QR code image is blurry. How do I fix it?

Generate at higher resolution:

```javascript
const image = await client.download('qr-id', {
  format: 'png',
  size: 2000, // Higher resolution
  dpi: 300 // For print
});
```

For print, use vector formats (SVG, EPS, PDF) instead of raster (PNG, JPG).

### Can I use QR codes offline?

Static QR codes (with tracking disabled) work offline. They encode data directly and don't require internet.

Dynamic QR codes (with tracking) require internet to redirect to the target URL.

### How do I export all my QR codes?

Use the bulk export feature:

```javascript
// Export all QR codes as CSV
const csv = await client.export({
  format: 'csv',
  fields: ['id', 'name', 'type', 'data', 'scans']
});
```

Or use the web dashboard: QR Codes → Export → CSV/JSON

### Can I import existing QR codes?

You can import QR code data (URLs, names, etc.) from CSV:

```javascript
const csv = `type,data,name
url,https://example.com/1,Product 1
url,https://example.com/2,Product 2`;

const result = await client.importCSV(csv);
```

Note: This creates new QR codes; it doesn't import existing QR code images.

## Advanced Features

### What is A/B testing?

A/B testing randomly directs scans to different URLs to test which performs better:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  abTest: {
    enabled: true,
    variants: [
      { url: 'https://example.com/a', weight: 50 },
      { url: 'https://example.com/b', weight: 50 }
    ]
  }
});
```

Track performance in analytics to see which variant converts better.

### What is geofencing?

Geofencing routes users to different URLs based on their location:

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

Perfect for international campaigns or location-specific content.

### Can I schedule QR codes to activate later?

Yes! Set activation and expiration dates:

```javascript
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  schedule: {
    activeAt: '2024-07-01T00:00:00Z',
    expiresAt: '2024-07-31T23:59:59Z'
  }
});
```

### How do I use templates?

Save a QR code design as a template:

```javascript
// Save template
await client.saveTemplate('qr-id', {
  name: 'Brand Template'
});

// Use template
const qr = await client.create({
  type: 'url',
  data: 'https://example.com',
  template: 'brand-template-id'
});
```

Templates preserve customization, colors, logos, and styles.

## Still Have Questions?

- 📖 [Getting Started](getting-started.md)
- 📘 [User Guide](user-guide.md)
- 🔧 [API Reference](api-reference.md)
- 💬 [Community Forum](https://github.com/manaqr/docs/discussions)
- 📧 Email: support@manaqr.io

---

**Previous**: [← API Reference](api-reference.md) | **Next**: [Contributing →](contributing.md)
