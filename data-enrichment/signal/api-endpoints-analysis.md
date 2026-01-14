# Signal API Endpoints Analysis

Analysis of Signal NFX GraphQL endpoints to determine the optimal flow for enriching Kauffman Fellows data.

**Objective:** Evaluate scraper-based data extraction to determine commercial viability, estimate costs, and assess coverage.

**Platform URL:** https://signal.nfx.com/

---

## Important Considerations

> **Note:** Signal does not provide a public API. Data extraction requires a web scraper service and authentication. Review Terms of Service before implementing.

### Access Method


| Aspect | GraphQL (Private API) | Public Profile |
|--------|----------------------|----------------|
| Access Type | Web scraper service | Public web scraping |
| Authentication | Login required | Not required |
| API Type | GraphQL (unofficial, undocumented) | HTML (public website) |
| Endpoint | `POST https://signal-api.nfx.com/graphql` | `GET https://signal.nfx.com/investors/{slug}` |


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

## Public Profile Data Available

### Investor Public Profile Page

| Attribute | Value |
|-----------|-------|
| URL Pattern | `https://signal.nfx.com/investors/{slug}` |
| Access Method | HTTP GET (HTML scraping) |
| Authentication | Not required |
| Identifier | Investor slug (e.g. `allen-taylor`) |
| Response Type | HTML page |

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

### Flow A: Public Profile Access

Get public investor profile using generated slug.

**Step 0 → Generate Slug**

Transform the user name to kebab-case slug (e.g., "Allen Taylor" → `"allen-taylor"`). This is a local transformation with no request cost.

**Step 1 → Public Investor Profile**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET https://signal.nfx.com/investors/{slug}` |
| Access Method | Public profile page |
| Input | Investor slug (e.g. `allen-taylor`) |
| Output | Public investor profile: person data, firm, investments, roles |
| Cost | 1 request |

**Requests per user:** 1 (1 public profile)

---

### Flow B: Direct Profile Access

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

### Flow C: Search + Profile Lookup

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

### Flow A: Public Profile Access

| Requests | Per User |
|----------|----------|
| Investor Profile | 1 |
| **Total per user** | **1** |


### Flow B: Direct Profile Access

| Requests | Per User |
|----------|----------|
| Login Session | 1 |
| Investor Profile | 1 |
| **Total per user** | **2** |

### Flow C: Search + Profile

| Requests | Per User |
|----------|----------|
| Login Session | 1 |
| Autocomplete Search | 1 |
| Investor Profile | 1 |
| **Total per user** | **3** |

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
| 1 user | 1 | 2 | 3 |
| 60 users (new/year) | 60 | 120 | 180 |
| 1,200 users (initial) | 1,200 | 2,400 | 3,600 |

### Year 1 Estimates

| Scenario | Flow A | Flow B | Flow C |
|----------|--------|--------|--------|
| Initial load (1,200 users) | 1,200 | 2,400 | 3,600 |
| New users (60) | 60 | 120 | 180 |
| **Year 1 Total (no refresh)** | **1,260** | **2,520** | **3,780** |

| Scenario | Flow A | Flow B | Flow C |
|----------|--------|--------|--------|
| Year 1 (no refresh) | 1,260 | 2,520 | 3,780 |
| 6-month refresh (1,260 users) | 1,260 | 2,520 | 3,780 |
| **Year 1 Total (with refresh)** | **2,520** | **5,040** | **7,560** |

---

## Pricing

### ScrapingBee Service Plans

| Plan | Monthly Price | Credits Included (Monthly) |
|------|---------------|----------------------------|
| Freelance | $49 | 250,000 |
| Startup | $99 | 1,000,000 |
| Business | $249 | 3,000,000 |
| Business+ | $599 | 8,000,000 |

### Cost per Request

| Parameter | Value |
|-----------|-------|
| Cost per request | 25 credits |

### Flow A: Public Profile Access – Estimated Annual Cost

| Scenario | Requests | Credits |
|----------|----------|---------|
| Year 1 (no refresh) | 1,260 | 31,500 |
| Year 1 (with refresh) | 2,520 | 63,000 |

### Flow B: Direct Profile Access – Estimated Annual Cost

| Scenario | Requests | Credits |
|----------|----------|---------|
| Year 1 (no refresh) | 2,520 | 63,000 |
| Year 1 (with refresh) | 5,040 | 126,000 |

### Flow C: Search + Profile Lookup – Estimated Annual Cost

| Scenario | Requests | Credits |
|----------|----------|---------|
| Year 1 (no refresh) | 3,780 | 94,500 |
| Year 1 (with refresh) | 7,560 | 189,000 |

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

## Summary & Recommendation

### Summary Comparison

| Dimension | Flow A: Public Profile | Flow B: Direct Profile | Flow C: Search + Profile |
|---------|------------------------|------------------------|--------------------------|
| Primary use case | Lightweight enrichment via public pages | Full private profile enrichment | Maximum coverage with fallback resolution |
| Data coverage | Medium (public-only fields) | High (full private profile) | High (full private profile) |
| Investment data | Partial | Complete | Complete |
| Network & extended data | ❌ | ✅ | ✅ |
| Authentication required | ❌ | ✅ | ✅ |
| Requests per user | 1 | 2 | 3 |
| Year 1 requests (no refresh) | 1,260 | 2,520 | 3,780 |
| Year 1 requests (with refresh) | 2,520 | 5,040 | 7,560 |
| Scraper credit cost | Very Low | Low | Medium |
| Implementation complexity | **Low** | **Very High** | **Extremely High** |
| Incremental value vs previous flow | — | High | Limited |

---

### Recommendation

**Recommended Primary Flow:** **Flow A – Public Profile Access**

**Decision:**  
Use **Flow A as the default and recommended enrichment strategy**.

**Rationale:**
- **Flow A offers the best cost–benefit ratio**, requiring only **1 request per user**, no authentication flow, and minimal scraper logic.
- **Implementation complexity is low**, making it faster to ship, easier to maintain, and more resilient to platform changes.
- While **Flow B provides richer data**, it introduces **significant technical complexity** (authenticated sessions, GraphQL stability, token handling) that materially increases implementation and maintenance cost.
- **Flow C further increases complexity and request volume**, while providing only marginal incremental value over Flow B.
- Given the undocumented nature of Signal’s private GraphQL API, **Flows B and C carry higher operational and legal risk** and should not be part of the initial architecture.

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
- ScrapingBee Pricing: https://www.scrapingbee.com/pricing/
- ScrapingBee Documentation: https://www.scrapingbee.com/documentation/

---

*Analysis Date: January 2026*
