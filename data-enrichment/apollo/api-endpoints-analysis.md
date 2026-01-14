# Apollo API Endpoints Analysis

Analysis of Apollo.io API endpoints to determine the optimal flow for enriching **personal data categories** for Kauffman Fellows.

**Objective:** Evaluate API usage to determine commercial viability, estimate costs, and assess coverage.

**Primary Focus:** Personal data coverage (identity, professional, location, contact).

**Documentation Base:** [https://docs.apollo.io/](https://docs.apollo.io/)

---

## Available Data in Airtable

Based on `sample_normalized.csv`, the following fields are currently available and can be used as lookup keys:

| Field                | Example                     | Usable as Filter |
|----------------------|-----------------------------|------------------|
| Name                 | "Allen Taylor"              | ✅ |
| Preferred First Name | "Allen"                     | ✅ |
| Last Name            | "Taylor"                    | ✅ |
| Email Address        | "allen@firm.com"            | ✅ |
| LinkedIn URL         | "linkedin.com/in/aktaylor/" | ✅ |
| Current Firm         | "Endeavor Catalyst"         | ✅ |
| Job Title            | "Partner"                   | ✅ |
| Country              | "United States"             | ✅ |

**Best identifiers for Apollo:**

1. LinkedIn URL
2. Email Address
3. Full Name + Company (fallback)

---

## Apollo API Overview (People)

Apollo provides three relevant People endpoints:

| Name                   | Purpose                                    | Endpoint                      |
| ---------------------- | ------------------------------------------ | ----------------------------- |
| People Search          | Locate a person and return partial profile | `POST /v1/people/search`      |
| People Enrichment      | Enrich a single person using identifiers   | `POST /v1/people/enrich`      |
| Bulk People Enrichment | Enrich multiple people in one request      | `POST /v1/people/bulk_enrich` |

---

## API Call Flows

### Flow A: Basic Data

Get Fellow personal and professional profile using a single enrichment call.

**Step 1 → People Enrichment**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /v1/people/enrich` |
| Credit Cost | 1 |
| Input | `linkedin_url` OR `email` |
| Output | `first_name`, `last_name`, `name`, `title`, `organization_name`, `linkedin_url`, `email/emails`, `location`, `photo_url`, `bio`, `education[]` |

**Request per user:** 1

---

### Flow B: Bulk Data

Get Fellow personal and professional profiles using bulk enrichment.  
Each request supports up to **10 people**, allowing enrichment of multiple users in a single API call.

**Step 1 → Bulk People Enrichment**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /v1/people/bulk_enrich` |
| Credit Cost | 1 per person |
| Input | Up to 10 people per request using `linkedin_url` OR `email` OR `first_name + last_name + company` |
| Output | Array of enriched profiles with the same schema as single enrichment |

**Request per 10 users:** 10
**Credits per request:** 1

---

### Flow C: Search + Enrichment

Resolve profiles when no direct identifier (LinkedIn URL or email) is available.

**Step 1 → People Search**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /v1/people/search` |
| Credit Cost | 0 |
| Input | `first_name`, `last_name`, `organization_name` |
| Output | Candidate list with `person_id`, `name`, `title`, `organization`, `linkedin_url` |

**Credits per request:** 0

**Step 2 → People Enrichment**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /v1/people/enrich` |
| Credit Cost | 1 |
| Input | Selected `linkedin_url` or `person_id` |
| Output | Full enriched personal profile |

**Credits per user:** 1  
**Requests per user:** 2

---

## Coverage Analysis

#### Flow A: Basic Data

| Airtable Field | Covered | Apollo Field |
|----------------|---------|--------------|
| Name | ⚠️ | `people[].first_name`, `people[].last_name_obfuscated` |
| Preferred First Name | ✅ | `people[].first_name` |
| Last Name | ⚠️ | `people[].last_name_obfuscated` |
| LinkedIn URL | ❌ | Not supported by endpoint |
| Job Title | ✅ | `people[].title` |
| Current Firm | ✅ | `people[].organization.name` |
| State/Province | ❌ | Not supported by endpoint |
| Country | ❌ | Not supported by endpoint |
| Professional Bio | ❌ | Not supported by endpoint |
| Education/Personal Bio | ❌ | Not supported by endpoint |

**Coverage: 4/10 fields (40%)**

#### Flow B: Bulk Data

| Airtable Field | Covered | Apollo Field |
|----------------|---------|--------------|
| Name | ✅ | `person.name` |
| Preferred First Name | ✅ | `person.first_name` |
| Last Name | ✅ | `person.last_name` |
| LinkedIn URL | ✅ | `person.linkedin_url` |
| Job Title | ✅ | `person.title` |
| Current Firm | ✅ | `person.organization.name` |
| State/Province | ✅ | `person.state` |
| Country | ✅ | `person.country` |
| Professional Bio | ⚠️ | `person.headline` |
| Education/Personal Bio | ❌ | Not supported by endpoint |

**Coverage: 8/10 fields (80%)**

#### Flow C: Search + Enrichment

| Airtable Field | Covered | Apollo Field |
|----------------|---------|--------------|
| Name | ✅ | `matches[].name` |
| Preferred First Name | ✅ | `matches[].first_name` |
| Last Name | ✅ | `matches[].last_name` |
| LinkedIn URL | ✅ | `matches[].linkedin_url` |
| Job Title | ✅ | `matches[].title` |
| Current Firm | ✅ | `matches[].organization.name` |
| State/Province | ✅ | `matches[].state` |
| Country | ✅ | `matches[].country` |
| Professional Bio | ⚠️ | `matches[].headline` |
| Education/Personal Bio | ❌ | Not supported by endpoint |

**Coverage: 8/10 fields (80%)**

> Email addresses and phone numbers are not returned by default. These fields require `reveal_personal_emails` and/or `reveal_phone_number` parameters and consume additional credits.

---
## Request Estimates

### Flow A: Basic Data

| Requests | Per User |
|----------|----------|
| People Enrichment | 1 |
| **Total** | **1** |

### Flow B: Bulk Data

| Requests | Per User |
|----------|----------|
| Bulk People Enrichment (per 10 users) | 1 |
| **Total** | **1 per 10 users** |

### Flow C: Search + Enrichment

| Requests | Per User |
|----------|----------|
| People Search | 1 |
| People Enrichment | 1 |
| **Total** | **2** |

> Email and phone numbers can only be requested via `/v1/people/enrich` and `/v1/people/bulk_enrich` using the parameters `reveal_personal_emails` (+1 credit) and `reveal_phone_number` (+4 credits). These parameters do not increase the number of requests. Email and phone are not supported by `/v1/people/search`.*

## Cost Estimation

### Assumptions

| Parameter | Value |
|-----------|-------|
| Current users (initial load) | 1,200 |
| New users per year | 60 |
| Refresh frequency | Every 6 months (optional) |

### Flow A: Basic Data

| Scenario | Credits | With Email + Phone |
|----------|---------|-------------------|
| Initial load (1,200 users × 1) | 1,200 | 7,200 |
| New users Year 1 (60 × 1) | 60 | 360 |
| **Year 1 Total** | **1,260** | **7,560** |

| Scenario | Credits | With Email + Phone |
|----------|---------|-------------------|
| Initial load (1,200 users × 1) | 1,200 | 7,200 |
| New users Year 1 (60 × 1) | 60 | 360 |
| 6-month refresh (1,260 users × 1) | 1,260 | 7,560 |
| **Year 1 Total (with refresh)** | **2,520** | **15,120** |

### Flow B: Bulk Data

| Scenario | Credits | With Email + Phone |
|----------|---------|-------------------|
| Initial load (1,200 users × 1) | 1,200 | 7,200 |
| New users Year 1 (60 × 1) | 60 | 360 |
| **Year 1 Total** | **1,260** | **7,560** |

| Scenario | Credits | With Email + Phone |
|----------|---------|-------------------|
| Initial load (1,200 users × 1) | 1,200 | 7,200 |
| New users Year 1 (60 × 1) | 60 | 360 |
| 6-month refresh (1,260 users × 1) | 1,260 | 7,560 |
| **Year 1 Total (with refresh)** | **2,520** | **15,120** |

### Flow C: Search + Enrichment

| Scenario | Credits | With Email + Phone |
|----------|---------|-------------------|
| Initial load (1,200 users × 1) | 1,200 | 7,200 |
| New users Year 1 (60 × 1) | 60 | 360 |
| **Year 1 Total** | **1,260** | **7,560** |

| Scenario | Credits | With Email + Phone |
|----------|---------|-------------------|
| Initial load (1,200 users × 1) | 1,200 | 7,200 |
| New users Year 1 (60 × 1) | 60 | 360 |
| 6-month refresh (1,260 users × 1) | 1,260 | 7,560 |
| **Year 1 Total (with refresh)** | **2,520** | **15,120** |

### Summary Table

| Scenario | Flow A: Basic Data | Flow B: Bulk Data | Flow C: Search + Enrichment |
|----------|-------------------|-------------------|-----------------------------|
| 1 user | 1 credit | 1 credit | 1 credit |
| 1 user (with email + phone) | 6 credits | 6 credits | 6 credits |
| 60 users (new/year) | 60 credits | 60 credits | 60 credits |
| 60 users (new/year, with email + phone) | 360 credits | 360 credits | 360 credits |
| 1,200 users (initial) | 1,200 credits | 1,200 credits | 1,200 credits |
| 1,200 users (initial, with email + phone) | 7,200 credits | 7,200 credits | 7,200 credits |
| Year 1 (no refresh) | 1,260 credits | 1,260 credits | 1,260 credits |
| Year 1 (no refresh, with email + phone) | 7,560 credits | 7,560 credits | 7,560 credits |
| Year 1 (with refresh) | 2,520 credits | 2,520 credits | 2,520 credits |
| Year 1 (with refresh, with email + phone) | 15,120 credits | 15,120 credits | 15,120 credits |

> Note: Flow C includes an additional `/v1/people/search` request compared to other flows. This request does not consume credits, but increases the total number of requests per user.

---

## Pricing

### API Plans

| Plan | Monthly Price (per user) | Credits Included |
|------|--------------------------|------------------|
| Free | $0 | 75 |
| Basic | $59 | 2,500 |
| Professional | $99 | 4,000 |
| Organization | $149 | 6,000 |


| Plan | Annual Price (per user / month) | Credits Included |
|------|----------------------------------|-----------------|
| Free | $0 | 900 |
| Basic | $49 | 30,000 |
| Professional | $79 | 48,000 |
| Organization | $119 | 72,000 |

> Flows assume batched execution to comply with Apollo API rate limits (100 req/min on paid plans).

### Estimated Annual Cost

| Scenario | Annual Credits Required | Plan | Cost (Monthly Billing) | Cost (Annual Billing) |
|----------|-------------------------|------|------------------------|-----------------------|
| Flow A – Year 1 (no refresh) | 1,260 | Basic | $708 | $588 |
| Flow A – Year 1 (with refresh) | 2,520 | Basic | $708 | $588 |
| Flow B – Year 1 (no refresh) | 1,260 | Basic | $708 | $588 |
| Flow B – Year 1 (with refresh) | 2,520 | Basic | $708 | $588 |
| Flow C – Year 1 (no refresh) | 1,260 | Basic | $708 | $588 |
| Flow C – Year 1 (with refresh) | 2,520 | Basic | $708 | $588 |

> **Note:** Email and phone reveal parameters are not included in the cost estimates above.  
> These fields are assumed to be already available for the majority of users.  
> Including paid reveal parameters would significantly increase initial load credits and introduce monthly credit limit constraints, particularly under the Apollo Basic plan.


---

## Plan Capacity Threshold (Basic Plan)

### Assumptions

| Parameter | Value |
|-----------|-------|
| Plan | Apollo Basic |
| Annual credit limit | 30,000 credits |
| Initial users | 1,200 |
| New users per year | 60 |
| Credits per user per year (Flow A) | 1 |

### User-Based Capacity

| Metric | Value |
|------|------|
| Max users supported per year | 30,000 users |
| Current users | 1,200 |
| Remaining capacity | 28,800 users |

### Time-Based Capacity (User Growth)

| Metric | Value |
|------|------|
| Annual user growth | 60 users |
| Users needed to exceed plan | 28,800 users |
| Estimated years to exceed limit | ~480 years |

---

## Warnings

### Identifier Dependency

⚠️ **Users Without LinkedIn URL or Email**

When a user does not have a LinkedIn URL or email available:

- Flow A cannot be executed and enrichment will fail
- Identity resolution depends on Flow C (People Search + Enrichment)
- Users with common names may return multiple search matches and require additional validation
- Incorrect or outdated identifiers may result in failed or incorrect enrichment

---

### Paid Email & Phone Reveal

⚠️ **Optional Contact Data Requires Additional Credits**

Email addresses and phone numbers are not returned by default:

- Retrieval requires `reveal_personal_emails` (+1 credit) and/or `reveal_phone_number` (+4 credits)
- These parameters do not increase the number of requests, but significantly increase credit consumption
- Including email and phone for large initial loads may exceed monthly credit limits on the Basic plan
- Contact data should be requested selectively and treated as an on-demand enrichment

---

### Rate Limits & Execution Strategy

⚠️ **API Rate Limits**

Apollo enforces request rate limits across all plans:

- Paid plans are limited to 100 requests per minute
- Large batch operations must be executed incrementally
- Bulk enrichment reduces request count but does not remove rate constraints
- Proper batching and scheduling are required to avoid throttling

---

### Data Freshness

⚠️ **Data May Become Stale Over Time**

Apollo data reflects third-party sources and may not update in real time:

- Job title, company, and location changes may lag behind actual user updates
- Periodic refreshes are required to maintain accuracy
- Refresh frequency should be aligned with business needs rather than assumed real-time correctness

---

## Summary & Recommendation

### Summary Comparison

| Dimension | Flow A: Enrichment Only | Flow C: Search + Enrichment |
|---------|-------------------------|-----------------------------|
| Primary use case | Users with LinkedIn URL or email | Users without direct identifiers |
| Estimated coverage | ~80% of users | ~100% of users |
| Personal data coverage | High | High |
| Email & phone availability | Optional (paid reveal) | Optional (paid reveal) |
| Requests per user | 1 | 2 (1 search + 1 enrichment) |
| Credit cost | Low | Low (search has no credit cost) |
| Implementation complexity | Low | Medium |
| Risk of profile resolution failure | Medium (identifier-dependent) | Low (search-based resolution) |

---

### Recommendation

**Recommended Plan:** Apollo **Basic**  
**Recommended Primary Flow:** **Flow A – People Enrichment**  
**Recommended Fallback Flow:** **Flow C – Search + Enrichment**

**Decision:**  
Adopting **Flow A as the default** with **Flow C as a fallback** delivers the best balance between coverage, cost control, operational simplicity, and long-term scalability under the Apollo Basic plan.

**Rationale:**
- Approximately **80% of users** are expected to have a LinkedIn URL or email, making **Flow A** the most efficient and lowest-complexity default.
- **Flow A** provides strong personal data coverage with minimal requests and comfortably fits within the Apollo Basic annual credit limits.
- For the remaining **~20% of users** without direct identifiers, **Flow C** acts as a **controlled fallback**, reducing profile resolution failure risk by resolving identities via search before enrichment.
- Although **Flow C** introduces an additional request, the search step does **not consume credits**, keeping overall cost impact minimal.
- This approach allows for an **incremental implementation**:
  - Start with Flow A for simplicity and speed.
  - Add Flow C selectively only when identifiers are missing, avoiding unnecessary complexity for the majority of users.

---

## Response Examples

Example JSON responses are available in the `examples/` folder:

| File | Endpoint | Description |
|------|----------|-------------|
| `bulk_people_enrichment.json` | Bulk People Enrichment | Enrich data for up to 10 people with a single API call |
| `people_enrichment.json` | People Enrichment | Enrich data for 1 person |
| `people_search.json` | People Search | Find net new people in the Apollo database |

---

## API References

- API Reference: https://docs.apollo.io/reference
- API Overview: https://docs.apollo.io/docs/api-overview

---
*Analysis Date: January 2026*
