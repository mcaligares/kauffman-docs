# Tech Stack

**Essential Base Components**

> Basic: NextJS 15 + Supabase + Vercel + Mapbox
Total cost: $45 fixed + $3.2 per user
> 
1. Web Framework: Next.js 15
    - Full-stack capabilities
    - Development efficiency
2. Infrastructure: Supabase
    - $25 per month
    - Most of what we need is included in Supabase, to simplify and reduce costs
3. Hosting: Vercel
    - $20/month
    - Framework optimization
    - Deployment reliability
4. Map: Mapbox
    - Free up to 50k loads (e.g. 500 users doing 100 map loads per month)
5. Payments: Stripe
    - $3.2 Cost per user ($100 pricing)
        - If user pays annually: $2.93
    - 2.9% + 30**¢** per successful transaction for domestic cards
</aside>

# Infrastructure

### [Supabase](https://supabase.com/)-Centered Stack (Recommended) ✅

Supabase provides all the services in a very simple way to implement. That will save us time of infrastructure setup, and speed up the development roadmap.
We have experience implementing Supabase in many different projects, and is the best option at this stage.

---

### Components

- Authentication
- Storage
- Database
- Queue system
- Cron jobs

### **Advantages**

- Unified platform
- Simple API integration
- Framework compatibility
- Infrastructure simplification
- **$25/month base pricing**

### **Limitations**

- Queue system complexity (we can use Upstash instead $1 usd more)
- Service scalability considerations

# Web Hosting

### [Vercel](https://vercel.com/)

Any other platform besides Vercel has problems with NextJS or missing features. Using Vercel will help us make sure the webapp is going to work perfectly.

We have a lot of experience with this tool, so development process will be faster compared with other options.

---

**Advantages**

- Next.js optimization
- **Pro plan: $20/month/user**
- Automatic preview deployments

**Limitations**

- Potential bandwidth cost scaling. Estimation: $200 with a big buffer (e.g. know of webapps with 250k users and paying ~$70)
- $20/user organization cost

# Payments

### [Stripe](https://stripe.com/)

---

**Pricing model**

- https://stripe.com/pricing
- 2.9% + 30**¢** per successful transaction for domestic cards

**Advantages**

- Fast to integrate, easy to scale, secure and reliable, loved by devs and trusted by the market
- Scales from MVPs to high-volume businesses
- Enable features like subscriptions, one-time payments, invoicing, etc., without building them from scratch
- Dozens of payment methods (cards, wallets like Apple Pay/Google Pay, bank debits, etc.)
- Fraud protection via Stripe Radar (uses ML to detect suspicious activity)
- Start with hosted checkout (like Stripe Checkout) — Later move to custom UI with their APIs
- API integration
- Well known and trustworthy for users

# Authentication

### Supabase (Recommended) ✅

---

**Advantages**

- Simple implementation
- Flexible auth flows
- 50k MAU free tier
- Storage integration

**Notes**

- Custom role management needed
- Database structure for permissions required

# Database

### Supabase (PostgreSQL) ✅

---

**Advantages**

- Free tier (500MB)
- Pro plan: $25/month (8GB)
- Automatic scaling
- Integrated features:
    - Automated backups
    - Management dashboard
    - Cron jobs
    - Queue system
    - Row-level security

**Limitations**

- Free tier pauses after 1 week inactivity
- Manual dashboard reactivation required

# Storage

### Supabase Storage (Recommended) ✅
(*in case we needed*)

---

**Advantages**

- Integrated CDN
- S3 backend
- Image transformation API
- Free tier: 1GB
- Pro tier: 100GB ($25/month)

## 💲Cost Analysis Examples

<aside>
👉

### Small Application Scenario

- Storage: 5GB
- Transfer: 100GB/month
- Requests: 100k/month

---

### **Supabase Pro**

- $25/month (all inclusive)
</aside>

<aside>
👉

### Large Application Scenario

- Storage: 500GB
- Transfer: 5TB/month
- Requests: 1M/month

---

### **Supabase Pro**

- Base: $25/month
- Additional storage: (400GB × $0.021) ≈ $8.4/month
- Total: ~$33.4/month
</aside>

# Schedule Tasks (Crons)

### Option 1
Supabase (Recommended) ✅

---

**Advantages**

- Database integration
- HTTP request capability
- Edge function support
- Integrated monitoring

# Map

### Mapbox (Recommended) ✅

---

**Advantages**

- High-performance rendering of points and polygons using Mapbox GL
- Vector tiles support for scalable and customizable map layers
- Multiple map styles including satellite, topographic, and custom styles
- Full interactivity support: click events, tooltips, layer toggles, and more
- **50k monthly loads free.** From 50,001 to 100,000, the cost is $5 USD per 1,000 map loads.

# Analytics

| **Feature / Tool** | **Microsoft Clarity** | **Google Analytics (GA4)** | **Mixpanel** |
| --- | --- | --- | --- |
| Ease of Setup | Very easy, just a JS snippet | Moderate, requires config | Easy, may require event planning |
| Session Recording | ✅ Native and automatic | ❌ Not available | 🔄 Requires integration |
| Heatmaps | ✅ Built-in | ❌ Not built-in | ❌ Not built-in |
| Event Tracking | ❌ Limited (auto only) | ✅ Configurable | ✅ Granular & advanced |
| Dashboard | ✅ Very clear & visual | Moderate, requires learning | ✅ Clear with guided UI |
| User Segmentation | Basic | Moderate | ✅ Very strong |
| Real-Time Data | Minimal | ✅ Yes | ✅ Yes |
| Free Tier | ✅ 100% free | ✅ Free up to limits | ✅ Free up to 20M events/month |
| Best For | Quick insights & UX tracking | Broad website traffic analysis | Product analytics & user behavior |