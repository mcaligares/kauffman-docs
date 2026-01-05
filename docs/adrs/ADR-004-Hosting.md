# ADR-004 - Hosting

## Decision

The team decided to use **Vercel** for hosting the Next.js application instead of Netlify.

## Context

The project requires a hosting solution for a Next.js 15 application (see ADR-001). The hosting platform must support:

- Optimal deployment and performance for Next.js
- Production environment
- Staging environment in the same account without additional costs
- Predictable pricing

Two main options were evaluated:

1. **Vercel** - Platform created by the Next.js team
   - Pricing: Free plan available, Pro plan at $20/user/month
   - Next.js Integration: Native optimization and support
   - Staging: Preview deployments for branches (no additional cost)
   - Features: 1 TB bandwidth, 6,000 build minutes/month (Pro plan)

2. **Netlify** - General-purpose hosting platform
   - Pricing: Free plan available, Pro plan at $19/user/month
   - Next.js Integration: Supports Next.js but may require additional configuration
   - Staging: Deploy previews for branches (no additional cost)
   - Features: 400 GB bandwidth, 25,000 build minutes/month (Pro plan)

Given that Supabase was selected for backend services (ADRs 002, 003, 004) and does not include hosting, a separate hosting solution is required for the Next.js frontend application.

## Rationale

The decision was driven by the following considerations:

- **Native Next.js Integration:** Vercel is created and maintained by the Next.js team, ensuring native integration and optimization. It provides full support for Next.js features including Server-Side Rendering (SSR), Incremental Static Regeneration (ISR), Edge Functions, and Image Optimization. This results in better performance and easier deployment compared to Netlify.

- **Staging Environment Without Additional Cost:** Both platforms offer preview deployments for branches at no additional cost. Vercel automatically creates preview deployments for every pull request and branch, allowing the team to test changes in a staging-like environment before merging to production. This meets the requirement for a staging environment in the same account without increasing costs.

- **Better Performance for Next.js:** Vercel's infrastructure is specifically optimized for Next.js applications, providing faster build times, better caching strategies, and improved runtime performance. Features like Edge Middleware and Edge Functions work seamlessly with Next.js on Vercel.

- **Superior Developer Experience:** Vercel offers a more streamlined experience for Next.js projects with automatic optimizations, zero-configuration deployments, and better integration with Next.js-specific features. The deployment process is simpler and more intuitive for Next.js applications.

- **Cost Comparison:** While Netlify's Pro plan is slightly cheaper ($19/user/month vs $20/user/month), Vercel provides more bandwidth (1 TB vs 400 GB) which may be more valuable for the project. Both plans include unlimited preview deployments, meeting the staging requirement without additional costs.

- **Future-Proof:** As the creators of Next.js, Vercel ensures that new Next.js features and optimizations are immediately available on their platform, providing better long-term compatibility and support.

- **Team Experience:** The development team already has hands-on experience with Vercel from previous projects, reducing onboarding time and enabling faster implementation compared to learning a new platform like Netlify.

## Consequences

### Positive

- Faster implementation due to team's prior experience with Vercel
- Optimal performance for Next.js applications due to native integration
- Staging environment available through preview deployments at no additional cost
- Faster build times and deployments
- Better support for Next.js-specific features (ISR, Edge Functions, etc.)
- Automatic optimizations and zero-configuration deployments
- Seamless integration with Next.js development workflow
- Access to latest Next.js features and optimizations as they're released

### Negative

- Vendor lock-in to Vercel platform
- Slightly higher cost per user compared to Netlify ($20 vs $19/month)
- Less flexibility for non-Next.js projects if the tech stack changes
- Potential migration complexity if we need to switch providers in the future

## Alternatives Considered

### Netlify

- **Pros:**
  - Slightly cheaper Pro plan ($19/user/month vs $20/user/month)
  - More build minutes included (25,000 vs 6,000/month)
  - Good general-purpose hosting platform
  - Supports multiple frameworks beyond Next.js
  - Preview deployments available without additional cost
  - Additional features like Netlify Identity and Netlify Forms
- **Cons:**
  - Less optimized for Next.js compared to Vercel
  - May require additional configuration for Next.js-specific features
  - SSR support is less efficient than Vercel
  - Not created by Next.js team, so new features may have delayed support
  - Less seamless integration with Next.js development workflow
  - Potential performance differences for Next.js applications
  - No prior team experience with Netlify, requiring learning curve

## Staging Environment

Both platforms provide staging capabilities through preview deployments:

- **Vercel:** Automatically creates preview deployments for every branch and pull request. Each preview gets a unique URL, allowing the team to test changes before merging to production. These previews are included in the Pro plan at no additional cost.

- **Netlify:** Also provides deploy previews for branches and pull requests with unique URLs. These are included in the Pro plan without additional charges.

For the project's requirement of having a staging environment in the same account without increasing costs, both platforms meet this need. However, Vercel's tighter integration with Next.js makes it the preferred choice.

## Follow-up

- Monitor build times and bandwidth usage to ensure we stay within Pro plan limits
- Evaluate if additional Vercel features (Edge Functions, Edge Middleware) are being utilized effectively
- Consider revisiting this decision if:
  - Project requirements change significantly (e.g., need to support non-Next.js applications)
  - Netlify introduces significant Next.js optimizations that match Vercel
  - Cost structure changes unfavorably
  - The project scales beyond Vercel's capabilities

For now, Vercel provides the best balance of Next.js optimization, staging capabilities, and developer experience for our hosting needs.

