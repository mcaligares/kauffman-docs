# ADR-007 - Authentication Strategy

## Decision

The team will implement a **Temporary Dual Authentication** approach:

- **Memberstack**: Continues handling authentication for restricted content in the existing Webflow site
- **Supabase**: Handles authentication for the new platform

This is a transitional strategy. As content migrates from Webflow to the new platform, Memberstack will be progressively deprecated along with its Zapier integration.

## Context

The client has approximately 1,800 users and uses two separate authentication systems:

1. **Webflow + Memberstack**: Main site with restricted content using passwordless authentication
2. **Next.js + Supabase**: New platform with its own magic link authentication

**Current problem**: Users must log in twice to access all content - once in Webflow/Memberstack and again in Next.js/Supabase.

### Updated Context (Post-Client Meeting)

During a meeting with the client, the following key points were clarified:

1. **Memberstack is not strategic**: The client adopted Memberstack as an initial solution for restricted content but has no interest in continuing with it long-term.

2. **Supabase as the authentication authority**: The client accepted managing users directly from Supabase, consolidating authentication in a single system.

3. **Content migration planned**: The restricted content currently in Webflow will eventually migrate to the new platform, eliminating the need for Memberstack.

4. **Current Zapier integration**: The client uses Zapier to create/modify users in Memberstack by listening to changes in Airtable. This integration would also be deprecated.

### Why Not Implement SSO?

The original proposal (Memberstack as centralized IdP with OIDC + JWT Exchange) was evaluated but **discarded** for the following reasons:

- **High technical effort for a temporary solution**: Implementing OIDC flow, JWT exchange, and session synchronization requires 8-13 days of development for a system that will eventually be deprecated.

- **Unnecessary complexity**: Adding custom SSO code creates technical debt that will need to be removed during deprecation.

- **Cost without long-term benefit**: Maintaining a Memberstack Business subscription for SSO features makes no sense if the plan is to phase out Memberstack.

## Rationale

The decision to maintain dual authentication temporarily was driven by:

- **Minimal development effort**: No additional authentication code required. Both systems continue working as-is.

- **Technology consolidation path**: Clear roadmap to reduce systems. Zapier and Memberstack will be deprecated.

- **Cost reduction**: Eventual elimination of Memberstack subscription and Zapier integration costs.

- **Reduced maintenance burden**: Fewer moving parts means less operational complexity.

- **Client alignment**: This approach matches the client's stated goals and preferences.

## Consequences

### Positive

- **Zero development for authentication**: No OIDC implementation, no JWT exchange, no session sync
- **Clear deprecation path**: Memberstack phases out as content migrates
- **Cost savings**: Eventually eliminates Memberstack (~$25-49/month) and Zapier costs
- **Simplified architecture**: Supabase becomes the single authentication authority
- **Lower technical debt**: No temporary SSO code to maintain and then remove

### Negative

- **Temporary dual login**: Users will continue logging in twice until content migration completes
- **User friction in transition**: Some users may be confused by having two login experiences
- **Dependent on content migration**: Benefits only fully realized after Webflow content moves to new platform

## Alternatives Considered

### Option 1: Memberstack as IdP + OIDC/JWT Exchange

- **Status**: DISCARDED
- **Reason**: High technical effort (8-13 days) for a system that will be deprecated
- **Additional cost**: Memberstack Business upgrade ($49/month)
- **Technical debt**: Custom SSO code would need to be removed during deprecation

### Option 2: Supabase as Identity Provider for Both

- **Status**: NOT VIABLE (short-term)
- **Reason**: Memberstack does not accept external identity providers. Would require replacing Memberstack in Webflow entirely.

### Option 3: Third-Party Auth Provider (Auth0, WorkOS)

- **Status**: DISCARDED
- **Reason**: Adds cost and complexity for a temporary problem. Memberstack still cannot consume external IdPs.

## Architecture

### Current State

- Airtable (users) > Zapier (sync) > Memberstack (auth) > Webflow (restricted content)
- Airtable (users) > Supabase (auth + data) > New Platform (Next.js)

### Target State

- Airtable (users) > Supabase (auth + data) > Unified Platform (all content)

## Cost Comparison

| Concept | Current | Proposed (Transition) | Target State |
|---------|---------|----------------------|--------------|
| Memberstack | ~$25/month | ~$25/month | $0 |
| Zapier | ~$20/month* | ~$20/month* | $0 |
| Supabase Pro | $25/month | $25/month | $25/month |
| Development | - | $0 | - |
| **Total** | ~$70/month | ~$70/month | **$25/month** |

*Estimated Zapier cost for user sync automation

## Follow-up

1. **Track content migration progress**: Monitor which restricted content has been migrated to plan Memberstack deprecation timeline.

2. **User communication**: Prepare messaging for users about the eventual transition to single sign-on via Supabase.

3. **Document Zapier workflows**: Before deprecation, document existing Zapier automations to ensure any necessary functionality is replicated or is no longer needed.

4. **Supabase user management**: Ensure Supabase admin capabilities meet client needs for user administration (bulk import, role management, etc.).

This approach prioritizes **pragmatism over technical elegance**, avoiding investment in a complex SSO solution for a system that will be phased out.
