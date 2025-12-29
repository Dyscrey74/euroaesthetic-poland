# EuroAesthetic Poland - API Setup Guide

## 📡 Live API Endpoints

Base URL: `https://n8n.srv803546.hstgr.cloud/webhook`

---

## 1. Hotel Search API

**Endpoint**: `POST /ea-hotel-search`

### Request Body
```json
{
  "destination": "Warsaw",
  "arrivalDate": "2025-02-15",
  "departureDate": "2025-02-18",
  "tier": "premium",
  "adults": 1,
  "currency": "EUR"
}
```

### Parameters
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| destination | string | Yes | Polish city (Warsaw, Krakow, Wroclaw, Gdansk, Poznan) |
| arrivalDate | string | Yes | Check-in date (YYYY-MM-DD) |
| departureDate | string | Yes | Check-out date (YYYY-MM-DD) |
| tier | string | Yes | Package tier: entry, premium, vip |
| adults | number | No | Number of guests (default: 1) |
| currency | string | No | Currency code (default: EUR) |

### Tier Filtering Logic
- **entry**: 3★ hotels (reviewScore >= 7)
- **premium**: 4★ hotels (reviewScore >= 8)
- **vip**: 5★ luxury hotels (reviewScore >= 9)

---

## 2. Flight Search API

**Endpoint**: `POST /ea-flight-search`

### Request Body
```json
{
  "fromCity": "Paris",
  "toCity": "Warsaw",
  "departDate": "2025-02-15",
  "returnDate": "2025-02-18",
  "tier": "premium",
  "adults": 1,
  "currency": "EUR"
}
```

### Supported Cities
**Departure**: Paris, London, Amsterdam, Brussels, Dublin, Frankfurt, Munich, Vienna, Zurich, Prague, Rome, Milan, Barcelona, Madrid, Lisbon, Copenhagen, Stockholm, Oslo, Helsinki

**Arrival**: Warsaw, Krakow, Wroclaw, Gdansk, Poznan

### Tier Cabin Classes
- **entry/premium**: ECONOMY
- **vip**: BUSINESS

---

## 3. Payment API

**Endpoint**: `POST /ea-payment`

### Request Body
```json
{
  "tier": "premium",
  "name": "John Smith",
  "email": "john@example.com",
  "successUrl": "https://yoursite.com/success",
  "cancelUrl": "https://yoursite.com/cancel"
}
```

### Response
```json
{
  "success": true,
  "checkoutUrl": "https://checkout.stripe.com/...",
  "sessionId": "cs_..."
}
```

### Package Prices
- **entry**: €299
- **premium**: €599
- **vip**: €1,299

---

## 4. Inquiry Form API

**Endpoint**: `POST /ea-inquiry`

### Request Body
```json
{
  "name": "John Smith",
  "email": "john@example.com",
  "phone": "+33123456789",
  "message": "I'm interested in the Premium package",
  "tier": "premium"
}
```

---

## 🔐 Authentication

Currently, all endpoints are open for demo purposes. Production deployment should implement API key authentication.

---

## 💳 Payment Testing

Stripe test mode is enabled. Use these test cards:
- **Success**: 4242 4242 4242 4242
- **Decline**: 4000 0000 0000 0002

---

## 📧 Email Notifications

Automated emails sent via SendGrid:
- Booking confirmations
- Inquiry acknowledgments
- Payment receipts

---

## 🔗 Related Resources

- [Live Demo](https://dyscrey74.github.io/euroaesthetic-poland)
- [n8n Workflows](https://n8n.srv803546.hstgr.cloud)
- [RapidAPI Hub](https://rapidapi.com/tipsters/api/booking-com15)