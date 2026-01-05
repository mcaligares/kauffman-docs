# ADR-001 - NextJS

## Decision

The team decided to use **Next.js 15** instead of **Next.js 16** for this project.

## Context

A new web project needed to be initialized using Next.js. At the time of the decision, [**Next.js 16 had been recently released**](https://github.com/vercel/next.js/releases/tag/v16.0.0), with limited real-world adoption inside the company.

The team discussed whether to adopt the latest major version (Next.js 16) or to rely on **Next.js 15**, which was already in active use across existing projects.

Internal feedback from the team highlighted the following points:

- No existing production projects were using Next.js 16
- Next.js 15 was already proven to be stable in real projects
- Next.js 16 was perceived as relatively new and potentially unstable
- The current project did **not require any features exclusive to Next.js 16**

Given the project scope — primarily data listing and integrations with external services — there was no strong technical requirement to adopt the newest version.

## Rationale

The decision was driven by the following considerations:

- **Stability:** Next.js 15 is widely adopted and battle-tested in production environments.
- **Team Experience:** The development team already has hands-on experience with Next.js 15.
- **Lower Risk:** Avoids potential bugs, breaking changes, or undocumented behaviors present in a recently released major version.
- **Project Simplicity:** The project does not require cutting-edge features introduced in Next.js 16.
- **Faster Delivery:** Reduces onboarding time and unexpected troubleshooting, enabling faster development.

Choosing Next.js 15 aligns with a pragmatic approach focused on reliability rather than early adoption.

## Consequences

### Positive

- Predictable development experience
- Reduced technical risk
- Easier onboarding for developers
- Availability of community support, documentation, and known solutions
- The project does not immediately benefit from potential improvements or new features in Next.js 16

### Negative

- A future upgrade may be required once Next.js 16 matures and becomes the company standard

## Alternatives Considered

### Next.js 16

- **Pros:**
    - Access to the latest features and improvements
    - Longer future support window
- **Cons:**
    - Limited internal adoption
    - Short time in the ecosystem
    - Higher risk of instability
    - Potential breaking changes and tooling gaps

## Follow-up

- Re-evaluate upgrading to Next.js 16 once:
    - It is widely adopted within the company
    - It proves stability in production environments
    - Clear benefits for the project are identified

Until then, Next.js 15 remains the preferred and supported choice for this project.