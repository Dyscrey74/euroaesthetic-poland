# 🔧 API Configuration for EuroAesthetic Poland

## Required API Keys

### 1. ✈️ Flight Search API (Skyscanner via RapidAPI)
```
RAPIDAPI_KEY=your_rapidapi_key_here
RAPIDAPI_HOST=skyscanner-skyscanner-flight-search-v1.p.rapidapi.com
```
**Get your key**: https://rapidapi.com/skyscanner/api/skyscanner-flight-search
**Cost**: Free tier: 100 calls/month, Paid: $0.001 per call

### 2. 🏨 Hotel Booking API (Booking.com)
```
BOOKING_API_KEY=your_booking_api_key_here
BOOKING_BASE_URL=https://booking-com.p.rapidapi.com/v1
```
**Get your key**: https://rapidapi.com/booking/api/booking
**Cost**: Free tier: 1000 calls/month, Paid: $0.01 per call

### 3. 📅 Clinic Scheduling (Calendly)
```
CALENDLY_API_TOKEN=your_calendly_token_here
CALENDLY_BASE_URL=https://api.calendly.com
```
**Get your token**: https://calendly.com/integrations/api_webhooks
**Cost**: Free with Calendly account, Premium features require paid plan

### 4. 💳 Payment Processing (Stripe)
```
STRIPE_PUBLIC_KEY=pk_live_your_stripe_public_key
STRIPE_SECRET_KEY=sk_live_your_stripe_secret_key
```
**Get your keys**: https://dashboard.stripe.com/apikeys
**Cost**: 2.9% + 30¢ per transaction

### 5. 💱 Exchange Rates (Free API)
```
EXCHANGE_API_BASE_URL=https://api.exchangerate-api.com/v4/latest/EUR
```
**Cost**: Free (no key required)

## 🔧 Setup Instructions

### Method 1: Environment Variables (Production)
```bash
# Add to your VPS environment
export RAPIDAPI_KEY="your_key_here"
export BOOKING_API_KEY="your_key_here"
export CALENDLY_API_TOKEN="your_token_here"
export STRIPE_PUBLIC_KEY="pk_live_your_key"
export STRIPE_SECRET_KEY="sk_live_your_key"
```

### Method 2: .env File (Development)
Create `.env` file in project root:
```env
# Flight Search
RAPIDAPI_KEY=your_rapidapi_key_here
RAPIDAPI_HOST=skyscanner-skyscanner-flight-search-v1.p.rapidapi.com

# Hotel Booking
BOOKING_API_KEY=your_booking_api_key_here
BOOKING_BASE_URL=https://booking-com.p.rapidapi.com/v1

# Clinic Scheduling
CALENDLY_API_TOKEN=your_calendly_token_here
CALENDLY_BASE_URL=https://api.calendly.com

# Payment Processing
STRIPE_PUBLIC_KEY=pk_test_your_test_key_here
STRIPE_SECRET_KEY=sk_test_your_test_key_here

# Exchange Rates (Free)
EXCHANGE_API_BASE_URL=https://api.exchangerate-api.com/v4/latest/EUR
```

### Method 3: GitHub Secrets (CI/CD)
In your GitHub repository settings > Secrets and variables > Actions:
```
RAPIDAPI_KEY
BOOKING_API_KEY
CALENDLY_API_TOKEN
STRIPE_PUBLIC_KEY
STRIPE_SECRET_KEY
VPS_SSH_PRIVATE_KEY (for deployment)
```

## 📋 API Integration Status

### ✅ Ready to Implement
- **Skyscanner Flight Search**: Framework in place
- **Booking.com Hotels**: Structure ready
- **Calendly Scheduling**: Clinic booking ready
- **Stripe Payments**: Payment flow prepared
- **Exchange Rates**: Currency conversion ready

### 🔧 Implementation Code Examples

#### Flight Search Integration
```javascript
const searchFlights = async (from, to, date) => {
  const options = {
    method: 'GET',
    url: `https://skyscanner-skyscanner-flight-search-v1.p.rapidapi.com/apiservices/browsequotes/v1.0/PL/EUR/en-US/${from}/${to}/${date}`,
    headers: {
      'X-RapidAPI-Key': process.env.RAPIDAPI_KEY,
      'X-RapidAPI-Host': 'skyscanner-skyscanner-flight-search-v1.p.rapidapi.com'
    }
  };
  
  const response = await fetch(options.url, options);
  return await response.json();
};
```

#### Hotel Search Integration
```javascript
const searchHotels = async (destination, checkin, checkout, guests = 2) => {
  const response = await fetch(
    `https://booking-com.p.rapidapi.com/v1/hotels/search?query=${destination}&checkin_date=${checkin}&checkout_date=${checkout}&adults_number=${guests}`,
    {
      headers: {
        'X-RapidAPI-Key': process.env.BOOKING_API_KEY,
        'X-RapidAPI-Host': 'booking-com.p.rapidapi.com'
      }
    }
  );
  return await response.json();
};
```

#### Clinic Booking Integration
```javascript
const bookAppointment = async (clinicEventType, patientInfo, preferredTime) => {
  const response = await fetch('https://api.calendly.com/scheduled_events', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${process.env.CALENDLY_API_TOKEN}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      event_type_uuid: clinicEventType,
      start_time: preferredTime,
      end_time: new Date(new Date(preferredTime).getTime() + 30*60000).toISOString(),
      invitee: {
        email: patientInfo.email,
        name: patientInfo.name,
        text_reminder_number: patientInfo.phone
      }
    })
  });
  return await response.json();
};
```

#### Payment Processing
```javascript
const processPayment = async (amount, packageType, customerInfo) => {
  const stripe = Stripe(process.env.STRIPE_PUBLIC_KEY);
  
  const { clientSecret } = await fetch('/api/create-payment-intent', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      amount: amount * 100, // Stripe uses cents
      currency: 'eur',
      metadata: {
        package: packageType,
        customer_email: customerInfo.email
      }
    })
  }).then(r => r.json());
  
  return await stripe.confirmCardPayment(clientSecret);
};
```

## 💰 API Cost Estimation

### Monthly Costs (for 1000 bookings/month):
- **Skyscanner API**: €50-100 (flight searches)
- **Booking.com API**: €100-200 (hotel searches)
- **Calendly**: €0-50 (depending on plan)
- **Stripe**: €1,500-3,000 (2.9% of €50K revenue)
- **Total API Costs**: €1,650-3,350/month

### Revenue vs Costs:
- **Revenue** (1000 bookings × €599): €599,000/month
- **API Costs**: €3,350/month (0.56% of revenue)
- **Net API Efficiency**: 99.44% revenue retention

## 🔒 Security Best Practices

1. **Never expose API keys** in frontend code
2. **Use environment variables** for all sensitive data
3. **Implement rate limiting** to prevent API abuse
4. **Log API usage** for monitoring and debugging
5. **Use HTTPS** for all API communications
6. **Implement API key rotation** for security

## 📊 Monitoring & Analytics

### API Performance Monitoring
```javascript
const apiMetrics = {
  flights: { calls: 0, errors: 0, avgResponseTime: 0 },
  hotels: { calls: 0, errors: 0, avgResponseTime: 0 },
  bookings: { calls: 0, errors: 0, avgResponseTime: 0 },
  payments: { calls: 0, errors: 0, avgResponseTime: 0 }
};
```

### Success Metrics to Track
- API response times (<500ms target)
- Error rates (<1% target)  
- Conversion rates (API call to booking)
- Cost per successful transaction
- Customer satisfaction scores

---

## 🚀 Next Steps

1. **Sign up for each API service**
2. **Get your API keys and tokens** 
3. **Add keys to environment variables**
4. **Test each integration individually**
5. **Deploy with live API connections**
6. **Monitor performance and costs**

Your EuroAesthetic Poland platform will be fully operational with real-time pricing and booking capabilities! ✨