# ADR-005 - Supabase vs Firebase Platform Comparison

## Decision

The team decided to use **Supabase** as the primary backend platform instead of Firebase.

## Context

The project required selecting a comprehensive backend platform that could provide multiple services including authentication, user management, database, storage, and potentially hosting. Two main options were evaluated:

1. **Supabase** - A backend-as-a-service platform
   - Services: Authentication, user management, PostgreSQL database, file storage, and cronjobs
   - Hosting: Not included (requires separate hosting solution)
   - Pricing: Fixed monthly costs ($25/month for Pro plan)
   - Team Experience: The team has prior implementation experience with Supabase

2. **Firebase** - Google's backend-as-a-service platform
   - Services: Authentication, database (Firestore/Realtime Database), file storage (Cloud Storage), and hosting (Firebase Hosting)
   - User Management: Limited built-in user management features
   - Pricing: Variable costs based on usage (pay-per-use model)
   - Team Experience: No prior team experience with Firebase

Given that Supabase was already selected for authentication (ADR-002) and database (ADR-003), this decision evaluates whether to continue with Supabase or migrate to Firebase for a more comprehensive solution.

## Rationale

The decision was driven by the following considerations:

- **Cost Analysis:** For the project's expected usage (1,000-2,000 users, 2-4 GB database, 100-150 GB bandwidth), Supabase's fixed $25/month Pro plan provides better cost predictability. While Firebase's pay-per-use model might appear cheaper initially, variable costs can become unpredictable as usage scales, making budget planning difficult.

- **Team Experience:** The development team already has hands-on experience with Supabase from previous projects and from implementing authentication and database solutions in this project (ADRs 002 and 003). Switching to Firebase would require learning a new platform, increasing implementation time and risk.

- **Cost Predictability:** Supabase's fixed monthly pricing ($25/month) includes:
  - 100,000 monthly active users
  - 8 GB database storage
  - 250 GB bandwidth
  - File storage
  - All authentication features
  
  For the project's expected scale (1k-2k users, 2-4 GB database, 100-150 GB bandwidth), all usage falls within the included limits, resulting in a predictable $25/month cost. Firebase's variable pricing introduces uncertainty and potential cost overruns.

- **Integrated Services:** Supabase provides a unified platform with integrated authentication, database, storage, and cronjobs. While Firebase also offers these services, Supabase's PostgreSQL database provides better SQL capabilities and is more familiar to the team.

- **User Management:** Supabase includes comprehensive user management features, while Firebase's user management is more limited and requires additional implementation.

- **Hosting Consideration:** While Firebase includes hosting and Supabase does not, the project is using Next.js (ADR-001), which can be hosted on Vercel or other platforms. The lack of hosting in Supabase is not a blocker, and Next.js hosting solutions are well-established and cost-effective.

- **Consistency:** Continuing with Supabase maintains consistency with previous architectural decisions (ADRs 002 and 003), avoiding the complexity and risk of introducing a new platform mid-project.

## Consequences

### Positive

- Predictable monthly costs ($25/month) for expected usage levels
- Faster implementation due to team's existing Supabase experience
- Consistent platform across all backend services (auth, database, storage)
- No additional learning curve or migration effort
- Comprehensive user management features included
- PostgreSQL database provides SQL capabilities and better data modeling
- All expected usage (1k-2k users, 2-4 GB DB, 100-150 GB bandwidth) fits within included limits
- Unified dashboard for managing all services

### Negative

- No built-in hosting solution (requires separate hosting for Next.js application)
- Vendor lock-in to Supabase platform
- Fixed costs regardless of actual usage (may pay for unused capacity)
- Less flexibility compared to Firebase's modular service selection
- Potential migration complexity if we need to switch providers in the future

## Alternatives Considered

### Firebase

- **Pros:**
  - Includes hosting solution (Firebase Hosting)
  - Pay-per-use pricing can be cheaper for very low usage
  - Comprehensive suite of services (auth, database, storage, hosting, functions)
  - Strong integration with other Google Cloud services
  - Good documentation and community support
  - Real-time database capabilities
- **Cons:**
  - Variable costs make budget planning difficult
  - No prior team experience, requiring significant learning curve
  - Limited user management features compared to Supabase
  - Firestore (NoSQL) may not be ideal for relational data needs
  - Would require migration from existing Supabase implementation (ADRs 002 and 003)
  - Cost uncertainty as usage scales
  - Additional implementation time to learn and integrate Firebase

## Cost Comparison

For the project's expected usage:
- **Users:** 1,000-2,000 monthly active users
- **Database:** 2-4 GB storage
- **Bandwidth:** 100-150 GB/month

### Supabase (Pro Plan - $25/month)
- Users: 1k-2k users (within 100k included) = $0 additional
- Database: 2-4 GB (within 8 GB included) = $0 additional
- Bandwidth: 100-150 GB (within 250 GB included) = $0 additional
- **Total: $25/month (fixed)**

### Firebase (Estimated)
- Authentication: ~$1-2/month for 1k-2k users
- Firestore: Variable based on reads/writes (estimated $5-15/month)
- Storage: Variable based on usage (estimated $2-5/month)
- Bandwidth: Variable (estimated $5-10/month)
- Hosting: Minimal cost for static hosting
- **Total: ~$13-32/month (variable, unpredictable)**

While Firebase might appear slightly cheaper in some scenarios, the variable nature of costs and lack of team experience make Supabase the better choice for this project.

## Follow-up

- Monitor actual usage patterns to validate cost assumptions
- Evaluate if hosting requirements change significantly
- Consider revisiting this decision if:
  - Usage scales significantly beyond Supabase's included limits
  - Firebase pricing becomes more competitive with better predictability
  - Project requirements change to heavily favor Firebase-specific features
  - Hosting becomes a critical requirement that cannot be met separately

For now, Supabase provides the best balance of cost predictability, team experience, and feature completeness for our project needs.

