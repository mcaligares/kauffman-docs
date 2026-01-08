# Signal API Endpoints Analysis

Analysis of Signal NFX GraphQL endpoints to determine the optimal flow for enriching Kauffman Fellows data.

**Objective:** Evaluate scraper-based data extraction to determine commercial viability, estimate costs, and assess coverage.

**Platform URL:** https://signal.nfx.com/

---

## Important Considerations

> **Note:** Signal does not provide a public API. Data extraction requires a web scraper service and authentication. Review Terms of Service before implementing.

### Access Method

| Aspect | Details |
|--------|---------|
| Access Type | Web scraper service |
| Authentication | Login required for API access |
| API Type | GraphQL (unofficial, undocumented) |
| Endpoint | `POST https://signal-api.nfx.com/graphql` |

### Cost Components

Scraper services typically include login/session management, page requests, and proxy/anti-bot measures within their subscription or credit-based pricing.

---

## Available Data in Airtable

### Person Data (sample_normalized.csv)

| Field | Example | Usable for Lookup |
|-------|---------|-------------------|
| Name | "Allen Taylor" | ✅ (search + slug generation) |
| Preferred First Name | "Allen" | ✅ (slug generation) |
| Last Name | "Taylor" | ✅ (slug generation) |
| LinkedIn URL | "linkedin.com/in/aktaylor/" | ⚠️ (not a search parameter) |
| Current Firm | "Endeavor Catalyst" | ✅ (search filter) |

### Investment Data (sample_investments.csv)

| Field | Example | Match with Signal |
|-------|---------|-------------------|
| Company Name (PB) | "Rappi" | ✅ (compare with investments_on_record) |
| Investment Date (Close Date on PB) | "06/17/18" | ✅ (compare with funding_round.date) |
| Deal Stage [PB] | "Series A" | ✅ (compare with funding_round.stage) |
| Lead Partner Name | "Allen Taylor" | ✅ (lookup key) |

---

## GraphQL Queries Available

### Autocomplete Query

| Attribute | Value |
|-----------|-------|
| Query Name | `InvestorsAutocompleteQuery` |
| Purpose | Search investors/firms by name |
| Input | `name_or_firm: String`, `first: Int` |
| Output | Investor IDs, Person slugs, Firm info |

### Investor Profile Query

| Attribute | Value |
|-----------|-------|
| Query Name | `InvestorProfileLoad` |
| Purpose | Get complete investor profile |
| Input | `personId: ID!` |
| Output | Full profile with investments, education, positions, network |

---

## API Call Flows

### Flow A: Direct Profile Access

Get investor profile using generated slug.

**Step 0 → Generate Slug**

Transform the user name to kebab-case slug (e.g., "Allen Taylor" → `"allen-taylor"`). This is a local transformation with no request cost.

**Step 1 → Login Session**

Authenticate with Signal to obtain a Bearer token. Cost: 1 request.

**Step 2 → Investor Profile**

| Attribute | Value |
|-----------|-------|
| Query | `InvestorProfileLoad` |
| Input | Person slug or ID derived from name |
| Output | Complete profile including: person data, firm, investments, education, positions, network |
| Cost | 1 request |

**Requests per user:** 2 (1 login + 1 profile)

---

### Flow B: Search + Profile Lookup

Use when slug-based lookup fails.

**Step 0 → Login Session**

Authenticate with Signal to obtain a Bearer token. Cost: 1 request.

**Step 1 → Autocomplete Search**

| Attribute | Value |
|-----------|-------|
| Query | `InvestorsAutocompleteQuery` |
| Input | `name_or_firm: "Allen Taylor"` |
| Output | List of matching investors with IDs |
| Cost | 1 request |

**Step 2 → Investor Profile**

| Attribute | Value |
|-----------|-------|
| Query | `InvestorProfileLoad` |
| Input | Person ID from Step 1 |
| Output | Complete profile |
| Cost | 1 request |

**Requests per user:** 3 (1 login + 1 search + 1 profile)

---

### Flow C: Batch Extraction

Bulk extraction for multiple users.

**Step 0 → Login Session (Once)**

A single authentication is performed at the start. The Bearer token is reused for all users in the batch. Cost: 1 request total.

**Step 1 → Profile Extraction (Per User)**

| Attribute | Value |
|-----------|-------|
| Query | `InvestorProfileLoad` |
| Input | Generated slug per user |
| Cost | 1 request per user |

**Step 2 → Fallback Search (Only if Direct Fails)**

| Attribute | Value |
|-----------|-------|
| Query | `InvestorsAutocompleteQuery` → `InvestorProfileLoad` |
| Trigger | When slug-based lookup returns no results |
| Cost | +1 request per failed lookup |

**Requests total:** 1 login + (1.5 × N users)

Assuming 50% require fallback: 50% get 1 request (direct), 50% get 2 requests (search + profile) = 1.5 average per user.

---

## Coverage Analysis

### Person Data Coverage

| Airtable Field | Covered | Signal Field |
|----------------|---------|--------------|
| Name | ✅ | `person.name` |
| Preferred First Name | ✅ | `person.first_name` |
| Last Name | ✅ | `person.last_name` |
| LinkedIn URL | ✅ | `person.linkedin_url` |
| Job Title | ✅ | `headline`, `position` |
| Current Firm | ✅ | `firm.name` |
| Country | ✅ | `location.display_name` |
| State/Province | ✅ | `location.display_name` |
| Headshot | ✅ | `image_urls[]` |
| Twitter URL | ✅ | `person.twitter_url` |
| Crunchbase URL | ✅ | `person.crunchbase_url` |
| Biography | ⚠️ | Not available (only headline) |

**Coverage: 10/12 fields (83%)**

### Investment Data Coverage

| Airtable Field | Covered | Signal Field |
|----------------|---------|--------------|
| Company Name (PB) | ✅ | `investments_on_record[].company_display_name` |
| Investment Date (Close Date on PB) | ✅ | `investor_profile_funding_rounds[].funding_round.date` |
| Deal Stage [PB] | ✅ | `investor_profile_funding_rounds[].funding_round.stage` |
| Deal Size | ✅ | `investor_profile_funding_rounds[].funding_round.amount` |
| Post-Valuation (M) | ❌ | Not available in Signal |
| Lead Investor | ✅ | `investor_profile_funding_rounds[].is_lead` |
| Board Role | ✅ | `investor_profile_funding_rounds[].board_role` |
| Co-investors | ✅ | `investments_on_record[].coinvestor_names[]` |
| Total Raised | ✅ | `investments_on_record[].total_raised` |

**Coverage: 8/9 fields (89%)**

---

## Request Estimates

### Flow A: Direct Profile Access

| Requests | Per User |
|----------|----------|
| Login Session | 1 |
| Investor Profile | 1 |
| **Total per user** | **2** |

### Flow B: Search + Profile

| Requests | Per User |
|----------|----------|
| Login Session | 1 |
| Autocomplete Search | 1 |
| Investor Profile | 1 |
| **Total per user** | **3** |

### Flow C: Batch Extraction

| Requests | Total |
|----------|-------|
| Login Session | 1 (shared) |
| Direct Profile (50% of users) | 0.5 × N |
| Search + Profile (50% of users) | 1.0 × N |
| **Total** | **1 + (1.5 × N)** |

---

## Cost Estimation

### Assumptions

| Parameter | Value |
|-----------|-------|
| Current users (initial load) | 1,200 |
| New users per year | 60 |
| Refresh frequency | Every 6 months (optional) |
| Users found via direct slug (Flow C) | 50% |
| Users requiring search fallback (Flow C) | 50% |

### Request Volume Estimates

| Scenario | Flow A | Flow B | Flow C |
|----------|--------|--------|--------|
| 1 user | 2 | 3 | 2.5 |
| 60 users (new/year) | 120 | 180 | 91 |
| 1,200 users (initial) | 2,400 | 3,600 | 1,801 |

> **Flow C calculation:** 1 login + (1.5 × N users)

### Year 1 Estimates

| Scenario | Flow A | Flow B | Flow C |
|----------|--------|--------|--------|
| Initial load (1,200 users) | 2,400 | 3,600 | 1,801 |
| New users (60) | 120 | 180 | 91 |
| **Year 1 Total (no refresh)** | **2,520** | **3,780** | **1,892** |

| Scenario | Flow A | Flow B | Flow C |
|----------|--------|--------|--------|
| Year 1 (no refresh) | 2,520 | 3,780 | 1,892 |
| 6-month refresh (1,260 users) | 2,520 | 3,780 | 1,891 |
| **Year 1 Total (with refresh)** | **5,040** | **7,560** | **3,783** |

---

## Pricing

> ⏳ **TODO:** Complete this section with scraper service pricing.

### Cost per Request

| Parameter | Value |
|-----------|-------|
| Cost per request | $ TBD |

### Flow A: Estimated Annual Cost

| Scenario | Requests | Estimated Cost |
|----------|----------|----------------|
| Year 1 (no refresh) | 2,520 | $ TBD |
| Year 1 (with refresh) | 2,520 | $ TBD |

### Flow B: Estimated Annual Cost

| Scenario | Requests | Estimated Cost |
|----------|----------|----------------|
| Year 1 (no refresh) | 3,780 | $ TBD |
| Year 1 (with refresh) | 2,520 | $ TBD |

### Flow C: Estimated Annual Cost

| Scenario | Requests | Estimated Cost |
|----------|----------|----------------|
| Year 1 (no refresh) | 1,892 | $ TBD |
| Year 1 (with refresh) | 3,783 | $ TBD |

---

## Warnings

### Data Usage & Legal Restrictions

⚠️ Signal / NFX Data Usage

Before extracting or using data from Signal (NFX), consider the following:

- **Terms of Service:** Scraping or automated data extraction may violate Signal/NFX ToS, even if technically feasible.

- **Authorization:** Signal is a private platform; access requires valid credentials. That means consider the session expiration, security measures and IP restrictions.

### Technical & Platform Risks

⚠️ Platform Stability and Access

- **Undocumented APIs:** Signal is based on an undocumented GraphQL schema that may change without notice.

- **Breaking changes:** Authentication or request structure may change at any time.

- **Rate limiting:** Endpoints may be throttled or blocked.

### Data Quality Limitations

⚠️ Incomplete or inconsistent data: Not all Kauffman Fellows have Signal profiles, and even when profiles exist, investment and activity data may be partial or outdated.

---
## Summary and Recommendation

### Primary Recommendation

**Flow C (Batch Extraction)** is the recommended default approach for this project.

**Why:**
- Lowest average cost per user (~1.5 requests/user)
- Shared authentication session reduces overhead
- Scales efficiently for initial load and periodic refreshes

This flow should be used for the initial data load and any scheduled synchronization jobs.

---

### Alternative Flows (Specific Use Cases)

**Flow A – Direct Profile Access**  
Use only for:
- One-off or manual lookups
- Internal tooling or debugging
- Low-volume scenarios where simplicity is preferred over optimization

**Flow B – Search + Profile Lookup**  
Use only when:
- Name validation is required
- High accuracy is critical
- False positives must be explicitly avoided

This flow trades higher request cost for increased reliability.

---

## Response Examples

Example JSON responses are available in the `examples/` folder:

| File | Query | Description |
|------|-------|-------------|
| `autocomplete-query.json` | Autocomplete | Query structure for search |
| `autocomplete-response.json` | Autocomplete | Sample search results |
| `investor-profile-query.json` | Investor Profile | Full query structure |
| `investor-profile-response.json` | Investor Profile | Complete profile with investments |

---

## API References

- Signal Platform: https://signal.nfx.com/
- GraphQL Endpoint: `https://signal-api.nfx.com/graphql`
- Response Dictionary: [response-dictionary.md](response-dictionary.md)
- Unofficial Library: https://github.com/ChrisMichaelPerezSantiago/signal.nfx

---

*Analysis Date: January 2026*
