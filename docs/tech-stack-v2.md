# Tech Stack

**Essential Base Components**

> Basic: NextJS 15 + Supabase + Vercel + Airtable + Resend
Total cost: $45 fixed + variable costs
> 
1. Web Framework: Next.js 15
    - Full-stack capabilities
    - Mobile-first web application
    - Server Components and App Router
2. Infrastructure: Supabase
    - $25 per month
    - Authentication, Database, Storage, Cron Jobs
3. Hosting: Vercel
    - $20/month
    - Framework optimization
    - Deployment reliability
4. Content Management: Airtable
    - Cost not included in calculation
    - Primary data source and backoffice (CMS)
5. Email: Resend
    - Variable pricing
    - Magic link email delivery
6. Observability & Logging: New Relic
    - Centralized logging and observability
    - Native integration via Vercel
    - Free tier (30-day data retention, 100GB ingestion limit)

# Architecture

![kauffman-v2.png](imgs/kauffman-v2.png)

## Architecture Summary

- **Frontend/UI:** Next.js 15 + Tailwind CSS + shadcn/ui
- **Content Management:** Airtable (direct integration)
- **Authentication:** Supabase Auth (Magic Links)
- **Email:** Resend
- **Database:** PostgreSQL (Supabase)
- **Data Enrichment:** Apollo + Crunchbase
- **Background Jobs:** Supabase Cron Jobs
- **Deployment:** Vercel
- **Logging**: New Relic

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
- Production and staging environments

**Limitations**

- Potential bandwidth cost scaling
- $20/user organization cost

# Framework & UI

### **Next.js 15**

React-based framework used to build a **mobile-first web application**.
Leverages modern App Router features and Server Components.
Deployed on **Vercel**, ensuring scalability, performance, and SEO benefits.

---

**Advantages**

- Full-stack capabilities
- Server Components and Server Functions
- API Routes for internal endpoints
- SEO benefits
- Development efficiency

### **Tailwind CSS**

Utility-first CSS framework for a clean, responsive, and consistent UI.
Enables rapid UI development with minimal custom CSS.

---

**Advantages**

- Rapid UI development
- Responsive design
- Consistent styling
- Minimal custom CSS needed

### **shadcn/ui**

Reusable, accessible UI components built on top of Tailwind.
Ensures design consistency and faster development.

---

**Advantages**

- Pre-built accessible components
- Design consistency
- Faster development
- Built on Tailwind CSS

# Content Management

### [Airtable](https://airtable.com/)

Acts as the **primary data source** and **backoffice (CMS)** for the client.
Used to manage: Users, Events, Groups, Modules, etc.
Any new fields added by the client in Airtable are automatically reflected in the web application.
The web app is mostly **read-only**, focused on listing and displaying data.

---

**Advantages**

- Direct integration via Airtable JavaScript SDK
- No custom backend or intermediate API layer required
- Client-managed content
- Automatic field reflection in web app
- User-friendly backoffice interface

**Limitations**

- Not suitable for sensitive or enriched user information
- Read-only focus (no write operations from web app)

**Notes**

- Cost not included in total calculation

### **Airtable JavaScript SDK**

Official Airtable library used to fetch data directly from Airtable.
No custom backend or intermediate API layer is introduced for this data.

---

**Advantages**

- Official library
- Direct data fetching
- No intermediate layer needed
- Simple integration

# Authentication

### Supabase Auth (Magic Links) (Recommended) ✅

Secure, passwordless authentication using magic links.
Supabase manages: Users, Sessions, Tokens.

---

**Advantages**

- Simple implementation
- Passwordless authentication (magic links)
- Flexible auth flows
- 50k MAU free tier
- Storage integration
- Secure session management

**Notes**

- Custom role management needed
- Database structure for permissions required

# Email

### [Resend](https://resend.com/)

Email service used by Supabase to deliver magic link emails.
Provides reliable email delivery and good developer experience.

---

**Advantages**

- Reliable email delivery
- Good developer experience
- Integration with Supabase
- Magic link support

**Limitations**

- Variable pricing based on volume

# Database

### Supabase (PostgreSQL) ✅

Stores **private and enriched user data**, separate from Airtable.
Used to persist:
- User configuration
- Personal data fetched from external APIs
- Investment-related data

*Airtable is intentionally not used for sensitive or enriched user information.*

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

### Supabase (Recommended) ✅

Scheduled jobs trigger HTTP requests to a dedicated Next.js API endpoint.
The endpoint:
1. Iterates over users
2. Calls Apollo and Crunchbase APIs
3. Updates user data in Supabase PostgreSQL

This ensures automated and periodic data updates.

---

**Advantages**

- Database integration
- HTTP request capability
- Edge function support
- Integrated monitoring
- Automated periodic data updates

# Data Enrichment & External APIs

### **Apollo**

Used to retrieve personal and professional user data from LinkedIn.
Results are normalized and stored in Supabase PostgreSQL.

---

**Advantages**

- LinkedIn data enrichment
- Professional profile information
- API integration

**Limitations**

- Variable pricing based on API usage
- Requires API key management

### **Crunchbase**

Used to retrieve investment-related information for users.
Results are normalized and stored in Supabase PostgreSQL.

---

**Advantages**

- Investment data enrichment
- Company and funding information
- API integration

**Limitations**

- Variable pricing based on API usage
- Requires API key management

# Application Logic

- **Server Components / Server Functions**
    - Used to fetch and render Airtable data.
- **API Routes**
    - Used for internal endpoints, including cron-triggered updates.
- The application contains minimal business logic and focuses mainly on data presentation.

---

**Advantages**

- Server-side rendering for performance
- Minimal client-side logic
- Focus on data presentation
- Efficient data fetching

# External Links & Integrations

- **Event registration:** Redirects to external platforms via links.
- **WhatsApp groups:** Users join via external invitation links.

*No internal handling of registrations or group memberships is required.*

---

**Advantages**

- Simple integration approach
- No complex registration system needed
- External platform flexibility

# Observability & Logging

### [New Relic](https://newrelic.com/)

New Relic is used as the **centralized logging and observability layer** for the entire application.

It provides visibility into application behavior, errors, and performance across all components.

The integration is handled directly through **Vercel**, minimizing setup complexity and operational overhead.

---

**Coverage**

All system components are monitored, including:

- Next.js application (Server Components and API Routes)
- Supabase services (database interactions, cron jobs)
- Background jobs
- External API integrations

**Captured data includes:**

- Application logs
- Errors and exceptions
- Request/response traces
- Latency and performance metrics
- Custom events

---

**Advantages**

- Centralized observability across the full stack
- Error and performance visibility in production
- Native integration with Vercel
- Free tier suitable for current application scale

---

**Limitations**

- Free tier limited to **100GB of data ingestion**
- **30-day data retention**
- Upgrade to a paid plan required if ingestion limits are reached

*This is not expected to be a constraint in the near term, since in production only errors and warnings will be logged, and data retention is capped at 30 days, preventing long-term accumulation.*

# Version Control & Deployments (CI/CD)

- **GitHub** is used as the single source of truth.
- Automatic deployments:
    - `production` and `staging` are deployed on **Vercel**.
    - `develop` are deployed on **Coolify**

---

**Advantages**

- Version control with GitHub
- Automatic deployments
- Multiple environment support
- CI/CD integration

