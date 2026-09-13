# Creator Support Static Page

This frontend is now a **support-only static page**.

## Removed

- `?asset=...` URL parameter handling
- Project/asset badge
- Download API calls
- Free/paid download flow
- `I've Supported / Continue to Download` button
- AWS API endpoint configuration

## Current page

The page only provides:

- UPI QR payment
- UPI deep link
- UPI ID copy button
- Patreon support link

## Files

```text
frontend_support_static/
├── index.html
├── config.js
├── vercel.json
└── README.md
```

## Configure payment details

Edit `config.js`:

```javascript
window.APP_CONFIG = {
  UPI_ID: "your-upi-id@ybl",
  PAYEE_NAME: "Your Name",
  PATREON_URL: "https://www.patreon.com/your-page"
};
```

## Vercel

Deploy this directory as a plain static site. No query parameter is required.

Open only the normal site URL, for example:

```text
https://your-project.vercel.app/
```
