# ADR-003 - Database

## Decision

The team decided to use **Supabase** as the database solution instead of Neon.

## Context

The project required selecting a PostgreSQL database solution. Two main options were evaluated:

1. **Supabase** - A backend-as-a-service platform that includes PostgreSQL database, authentication, storage, and other services
   - Pricing: Fixed monthly cost ($25/month for Pro plan)
   - Includes database, authentication, storage, and cronjobs in a single subscription
   - The team already has prior experience with Supabase

2. **Neon** - A serverless PostgreSQL database platform
   - Pricing: Variable costs based on usage (compute and storage)
   - Potentially cheaper for variable workloads
   - Requires additional implementation time
   - No prior team experience

Given that Supabase was already selected for authentication (see ADR-002), using Supabase for the database would leverage the same subscription and provide integrated services.

## Rationale

The decision was driven by the following considerations:

- **Integration with Existing Choice:** Since Supabase was already chosen for authentication (ADR-002), using Supabase for the database leverages the same $25/month subscription, providing both services without additional cost.

- **Team Experience:** The development team already has hands-on experience with Supabase from previous projects, reducing onboarding time and enabling faster implementation compared to learning a new platform like Neon.

- **Predictable Costs:** Supabase offers fixed monthly pricing, which facilitates budget planning and cost predictability. Neon's variable pricing model, while potentially cheaper, introduces uncertainty in cost estimation and could lead to unexpected expenses as usage scales.

- **Integrated Services:** With a single Supabase subscription, the project gains access to:
  - PostgreSQL database
  - Authentication (already selected)
  - File storage
  - Cronjobs
  - Edge functions

- **Faster Implementation:** Using Supabase eliminates the need for additional implementation time that would be required to set up and integrate Neon, especially given the team's lack of prior experience with the platform.

- **Unified Platform:** Having database, authentication, and storage in a single platform simplifies architecture, reduces operational complexity, and provides better integration between services.

## Consequences

### Positive

- Faster implementation due to team's prior experience with Supabase
- No additional cost for database since it's included in the existing Supabase subscription
- Predictable monthly costs facilitate budget planning
- Integrated services (auth, storage, cronjobs) available in the same platform
- Single vendor reduces operational overhead
- Better integration between authentication and database (e.g., Row Level Security policies)
- Well-documented APIs and SDKs
- Unified dashboard for managing all backend services

### Negative

- Vendor lock-in to Supabase platform
- Potential migration complexity if we need to switch providers in the future
- Fixed costs regardless of actual database usage (vs. Neon's pay-per-use model)
- May pay for database capacity we don't fully utilize
- Less flexibility compared to a dedicated database service

## Alternatives Considered

### Neon

- **Pros:**
  - Potentially lower costs for variable workloads due to pay-per-use pricing
  - Serverless architecture with automatic scaling
  - Separation of compute and storage
  - Focused specifically on PostgreSQL database
  - Can scale down to zero during inactivity (cost savings)
- **Cons:**
  - Variable costs make budget planning more difficult
  - Requires additional implementation time
  - No prior team experience, requiring learning curve
  - Does not include authentication, storage, or other services (would need separate solutions)
  - Additional integration work needed to connect with authentication (Supabase)
  - Cost uncertainty as usage scales

## Follow-up

- Monitor Supabase database performance and reliability in production
- Track actual database usage to ensure we're getting value from the fixed subscription
- Compare actual costs with projected Neon costs if usage patterns become clearer
- Consider revisiting this decision if:
  - Database usage patterns show significant cost savings potential with Neon
  - Project requirements change significantly
  - Supabase database limitations become a bottleneck
  - The project scales beyond Supabase's database capabilities

For now, Supabase provides the best balance of cost predictability, team experience, and integration with our existing authentication solution.

