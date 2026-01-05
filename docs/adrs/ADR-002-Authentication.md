# ADR-002 - Authentication via Magic Link

## Decision

The team decided to use **Supabase** for authentication via magic link instead of Clerk or NextAuth.js.

## Context

The project required implementing user authentication using magic link (email-based authentication). Three main options were evaluated:

1. **Clerk** - A dedicated authentication service ([clerk.com/user-authentication](https://clerk.com/user-authentication))
   - Pricing: [clerk.com/pricing](https://clerk.com/pricing)
   - Pro plan: $25/month + $0.02 per MAU after 10,000 free users

2. **Supabase** - A backend-as-a-service platform with built-in authentication ([supabase.com/docs/guides/auth](https://supabase.com/docs/guides/auth))
   - Pricing: [supabase.com/pricing](https://supabase.com/pricing)
   - Pro plan: $25/month, includes 100,000 MAUs, PostgreSQL database, storage, and cronjobs

3. **NextAuth.js** - Open-source authentication library ([next-auth.js.org/providers/email](https://next-auth.js.org/providers/email))
   - Pricing: Free (open source)
   - Requires custom implementation and SMTP server configuration

The team needed to balance cost, implementation time, and additional features that could benefit the project.

## Rationale

The decision was driven by the following considerations:

- **Cost Efficiency:** While NextAuth.js is free, the time investment required for implementation offsets the cost savings. Clerk and Supabase offer similar monthly pricing ($25/month), but Supabase provides significantly more value.

- **Additional Services:** Supabase's Pro plan includes not only authentication and user management but also:
  - PostgreSQL database
  - File storage
  - Cronjobs
  - 100,000 monthly active users (vs Clerk's 10,000 free tier)

- **Faster Time to Market:** Supabase provides a complete, ready-to-use authentication solution with magic link support, reducing development time compared to building a custom solution with NextAuth.js.

- **Better Value Proposition:** For the same $25/month as Clerk, Supabase offers a more comprehensive backend infrastructure that aligns with the project's current and future needs.

- **Unified Platform:** Having authentication, database, and storage in a single platform simplifies architecture, reduces vendor management overhead, and provides better integration between services.

- **Team Experience:** The development team already has hands-on experience with Supabase from previous projects, reducing onboarding time and enabling faster implementation.

## Consequences

### Positive

- Faster implementation of authentication functionality due to team's prior experience
- Access to PostgreSQL database without additional cost
- File storage capabilities included
- Cronjob functionality for scheduled tasks
- Higher user limit (100,000 MAUs) compared to Clerk's free tier
- Single platform reduces operational complexity
- Well-documented authentication APIs and SDKs
- Built-in user management features

### Negative

- Vendor lock-in to Supabase platform
- Potential migration complexity if we need to switch providers in the future
- Less specialized authentication features compared to Clerk's dedicated focus
- May require learning Supabase-specific patterns and APIs

## Alternatives Considered

### Clerk

- **Pros:**
  - Dedicated authentication service with extensive features
  - Excellent developer experience and pre-built UI components
  - Strong focus on authentication and user management
  - Good documentation and support
- **Cons:**
  - Same monthly cost as Supabase but without additional services
  - Lower free tier (10,000 MAUs vs Supabase's 100,000)
  - Only provides authentication, requiring separate solutions for database and storage
  - Additional costs for extra features (add-ons at $100/month each)
  - No prior team experience with Clerk, requiring learning curve

### NextAuth.js

- **Pros:**
  - Completely free and open source
  - Full control over implementation
  - No vendor lock-in
  - Extensive customization options
- **Cons:**
  - Significant development time required
  - Need to set up and manage SMTP server for email delivery
  - Must implement user management, session handling, and security features
  - Higher maintenance burden
  - What we save in cost, we lose in implementation time

## Follow-up

- Monitor Supabase's performance and reliability in production
- Evaluate if additional Supabase features (database, storage, cronjobs) are being utilized effectively
- Consider revisiting this decision if:
  - Project requirements change significantly
  - Supabase pricing structure changes unfavorably
  - We need features exclusive to Clerk or other authentication providers
  - The project scales beyond Supabase's capabilities

For now, Supabase provides the best balance of cost, features, and implementation speed for our authentication needs.

