# Crunchbase API Endpoints Analysis

Analysis of Crunchbase API endpoints to determine the optimal flow for enriching Kauffman Fellows data.

**Objective:** Evaluate API usage to determine commercial viability, estimate costs, and assess coverage.

**API Documentation:** https://data.crunchbase.com/reference

---

## Available Data in Airtable

### Person Data (sample_normalized.csv)

| Field | Example | Usable as Filter |
|-------|---------|------------------|
| Name | "Allen Taylor" | ✅ |
| Preferred First Name | "Allen" | ✅ |
| Last Name | "Taylor" | ✅ |
| LinkedIn URL | "linkedin.com/in/aktaylor/" | ✅ (best option) |
| Current Firm | "Endeavor Catalyst" | ✅ |
| Country | "United States" | ✅ |

### Investment Data (sample_investments.csv)

| Field | Example | Available in Crunchbase |
|-------|---------|-------------------------|
| Company Name (PB) | "Rappi" | ✅ |
| Investment Date (Close Date on PB) | "06/17/18" | ✅ |
| Deal Size | "$40.00" | ✅ (Advanced only) |
| Post-Valuation (M) | "$663.32" | ✅ (Advanced only) |
| Deal Stage [PB] | "Series A" | ✅ |
| HQ Country (from Link to Company) | "Mexico" | ✅ |
| Industry Code (from Link to Company) | "Internet Retail" | ✅ |
| Exit Type (from Link to Company) | "Not Exited" | ✅ |

---

## API Endpoints by Package

### Firmographic Package

| Endpoint | Use Case |
|----------|----------|
| `POST /searches/people` | Find person by LinkedIn or name |
| `POST /searches/organizations` | Find company by name |
| `GET /entities/people/{uuid}` | Get person details |

### Core Financials Package

| Endpoint | Use Case |
|----------|----------|
| `POST /searches/investments` | Get investments (basic: date, type, stage) |
| `GET /entities/investments/{uuid}` | Get investment details |

### Advanced Financials Package

| Endpoint | Use Case |
|----------|----------|
| `POST /searches/investments` | Get investments (full: amount, lead status) |
| `POST /searches/funding_rounds` | Get round details (valuations) |
| `POST /searches/acquisitions` | Get M&A exit details |
| `POST /searches/funds` | Get fund information |

---

## API Call Flows

### Flow A: Basic Investment Data

Get person profile and investment list without valuation details.

**Step 1 → Search Person**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /searches/people` |
| Package | Firmographic |
| Filter | LinkedIn URL or Name |
| Input | `linkedin.com/in/aktaylor/` |
| Output | Person UUID, profile data |

**Step 2 → Search Investments**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /searches/investments` |
| Package | Core Financials |
| Filter | `investor_identifier` = Person UUID |
| Input | UUID from Step 1 |
| Output | Investment list (date, type, stage, company) |

**Requests per user:** 2

---

### Flow B: Full Investment Data with Valuations

Get complete investment data including deal sizes and valuations.

**Step 1 → Search Person**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /searches/people` |
| Package | Firmographic |
| Filter | LinkedIn URL or Name |
| Input | `linkedin.com/in/aktaylor/` |
| Output | Person UUID, profile data |

**Step 2 → Search Investments (Advanced)**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /searches/investments` |
| Package | Advanced Financials |
| Filter | `investor_identifier` = Person UUID |
| Input | UUID from Step 1 |
| Output | Investment list with `money_invested`, `is_lead_investor`, `funding_round_identifier[]` |

**Step 3 → Search Funding Rounds (Batch)**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /searches/funding_rounds` |
| Package | Advanced Financials |
| Filter | `uuid` IN [array of funding_round_identifiers from Step 2] |
| Input | Array of Round UUIDs |
| Output | All valuations in single request: `post_money_valuation`, `pre_money_valuation`, `money_raised` |

**Requests per user:** 3

---

### Flow C: Organization-Based Search ⚠️ DISCARDED

> **Status:** This flow is discarded and not recommended for production use.

**Reasons for discarding:**
- User data may not be up-to-date (job changes not reflected)
- Some users may not be associated with an organization in Crunchbase
- Would lose track of users who are independent investors or between jobs
- Requires additional validation to match users to organizations

**Step 1 → Search Organization**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /searches/organizations` |
| Package | Firmographic |
| Filter | Organization name |
| Input | "Endeavor Catalyst" |
| Output | Organization UUID |

**Step 2 → Search Investments**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /searches/investments` |
| Package | Core or Advanced Financials |
| Filter | `investor_identifier` = Org UUID |
| Output | All investments by the firm |

**Requests per user:** 2 (but unreliable data)

---

## Is Funding Rounds Endpoint Necessary?

Analysis based on `sample_investments.csv` fields:

| Airtable Field | Source Without FundingRound | Source With FundingRound |
|----------------|----------------------------|--------------------------|
| Company Name (PB) | ✅ Investments | ✅ Investments |
| Investment Date (Close Date on PB) | ✅ Investments | ✅ Investments |
| Deal Stage [PB] | ✅ Investments | ✅ Investments |
| Deal Size | ✅ Investments (Advanced) | ✅ Investments (Advanced) |
| Post-Valuation (M) | ❌ Not available | ✅ FundingRound |

**Conclusion:** FundingRound endpoint is only needed if Post-Valuation data is required. Based on sample data, only 6 of 15 investments (40%) have valuation data in Airtable. 

**Recommendation:** Start with Flow A (without FundingRound). Add Flow B only if valuation tracking is a priority.

---

## Coverage Analysis

### Flow A: Core Financials Coverage

| Airtable Field | Covered | Source |
|----------------|---------|--------|
| Company Name (PB) | ✅ | `organization_identifier.value` |
| Investment Date (Close Date on PB) | ✅ | `announced_on` |
| Deal Type | ✅ | `funding_round_investment_type` |
| Deal Stage [PB] | ✅ | `investor_stage` |
| Deal Size | ❌ | Requires Advanced |
| Post-Valuation (M) | ❌ | Requires FundingRound |
| Lead Partner Name | ❌ | Requires Advanced |
| HQ Country (from Link to Company) | ✅ | Via Organization lookup |
| Industry Code (from Link to Company) | ✅ | Via Organization lookup |

**Coverage: 6/9 fields (67%)**

### Flow B: Advanced Financials Coverage

| Airtable Field | Covered | Source |
|----------------|---------|--------|
| Company Name (PB) | ✅ | `organization_identifier.value` |
| Investment Date (Close Date on PB) | ✅ | `announced_on` |
| Deal Type | ✅ | `funding_round_investment_type` |
| Deal Stage [PB] | ✅ | `investor_stage` |
| Deal Size | ✅ | `money_invested` |
| Post-Valuation (M) | ✅ | `post_money_valuation` (FundingRound) |
| Lead Partner Name | ✅ | `partner_identifiers` |
| HQ Country (from Link to Company) | ✅ | Via Organization lookup |
| Industry Code (from Link to Company) | ✅ | Via Organization lookup |

**Coverage: 9/9 fields (100%)**

### Person Data Coverage (Search Person response)

| Airtable Field | Covered | Crunchbase Field |
|----------------|---------|------------------|
| Name | ✅ | `name` |
| Preferred First Name | ✅ | `first_name` |
| Last Name | ✅ | `last_name` |
| LinkedIn URL | ✅ | `linkedin.value` |
| Job Title | ✅ | `primary_job_title` |
| Current Firm | ✅ | `primary_organization.value` |
| State/Province | ✅ | `location_identifiers` |
| Country | ✅ | `location_identifiers` |
| Headshot | ✅ | `image_url` |
| Professional Bio | ✅ | `description` |
| Preferred Stages | ✅ | `investor_stage` |

**Coverage: 11/11 fields (100%)**

---

## Request Estimates

### Flow A: Basic (Core Financials)

| Requests | Per User |
|----------|----------|
| Search Person | 1 |
| Search Investments | 1 |
| **Total** | **2** |

### Flow B: Full Data (Advanced Financials)

| Requests | Per User |
|----------|----------|
| Search Person | 1 |
| Search Investments | 1 |
| Search FundingRounds (batch) | 1 |
| **Total** | **3** |

---

## Cost Estimation

### Assumptions

| Parameter | Value |
|-----------|-------|
| Current users (initial load) | 1,200 |
| New users per year | 60 |
| Refresh frequency | Every 6 months (optional) |

### Flow A: Core Financials

| Scenario | Requests |
|----------|----------|
| Initial load (1,200 users × 2) | 2,400 |
| New users Year 1 (60 × 2) | 120 |
| **Year 1 Total** | **2,520** |

| Scenario | Requests |
|----------|----------|
| Year 1 Total | 2,520 |
| 6-month refresh (1,260 users × 2) | 2,520 |
| **Year 1 Total (with refresh)** | **5,040** |

### Flow B: Advanced Financials

| Scenario | Requests |
|----------|----------|
| Initial load (1,200 users × 3) | 3,600 |
| New users Year 1 (60 × 3) | 180 |
| **Year 1 Total** | **3,780** |

| Scenario | Requests |
|----------|----------|
| Year 1 Total | 3,780 |
| 6-month refresh (1,260 users × 3) | 3,780 |
| **Year 1 Total (with refresh)** | **7,560** |

### Summary Table

| Scenario | Flow A (Core) | Flow B (Advanced) |
|----------|---------------|-------------------|
| 1 user | 2 requests | 3 requests |
| 60 users (new/year) | 120 requests | 180 requests |
| 1,200 users (initial) | 2,400 requests | 3,600 requests |
| Year 1 (no refresh) | 2,520 requests | 3,780 requests |
| Year 1 (with refresh) | 5,040 requests | 7,560 requests |

---

## Pricing

> ⏳ **TODO:** Complete this section with Crunchbase API pricing details.

### API Plans

| Plan | Price | Requests Included | Cost per Extra Request |
|------|-------|-------------------|------------------------|
| Basic | $ TBD | TBD | $ TBD |
| Pro | $ TBD | TBD | $ TBD |
| Enterprise | $ TBD | TBD | $ TBD |

### Estimated Annual Cost

| Scenario | Requests | Plan | Estimated Cost |
|----------|----------|------|----------------|
| Flow A - Year 1 (no refresh) | 2,520 | TBD | $ TBD |
| Flow A - Year 1 (with refresh) | 5,040 | TBD | $ TBD |
| Flow B - Year 1 (no refresh) | 3,780 | TBD | $ TBD |
| Flow B - Year 1 (with refresh) | 7,560 | TBD | $ TBD |

---

## Warnings

### Data Usage Restrictions

⚠️ **AI and Machine Learning Usage**

Crunchbase may have restrictions on how their data can be used. Before implementing any AI features:

- **Disclosure requirement:** Crunchbase wants to know how their data will be used
- **Training restrictions:** There may be issues if we use the data to train machine learning models
- **Terms of Service:** Review the API Terms of Service for specific clauses about data processing and AI

---

## Summary

### Recommended Flow

| Use Case | Flow | Package | Requests/User | Coverage |
|----------|------|---------|---------------|----------|
| Basic enrichment | A | Core Financials | 2 | 67% |
| Full investment data | B | Advanced Financials | 3 | 100% |

### Package Comparison

| Package | Person Data | Investment Data | Valuations | Requests/User |
|---------|-------------|-----------------|------------|---------------|
| Core Financials | ✅ 100% | ⚠️ 67% | ❌ No | 2 |
| Advanced Financials | ✅ 100% | ✅ 100% | ✅ Yes | 3 |

### Decision Matrix

| Priority | Recommended | Package | Why |
|----------|-------------|---------|-----|
| Minimize cost | Flow A | Core Financials | 60% coverage, fewer requests |
| Maximize data | Flow B | Advanced Financials | 90% coverage, valuations included |
| Balance | Flow A + selective B | Core + Advanced | Use Advanced only for high-value profiles |

---

## Response Examples

Example JSON responses are available in the `examples/` folder:

| File | Endpoint | Description |
|------|----------|-------------|
| `examples/people.json` | Search Person | Person profile with investor metrics |
| `examples/investments.json` | Search Investments | Investment records with deal details |
| `examples/funding_rounds.json` | Search FundingRounds | Round details with valuations |
| `examples/organizations.json` | Search Organizations | Company profile with funding data |

---

## API References

- Search People: https://data.crunchbase.com/reference/searchpeople-1
- Search Investments (Core): https://data.crunchbase.com/reference/searchinvestments
- Search Investments (Advanced): https://data.crunchbase.com/reference/searchinvestments-1
- Search FundingRounds: https://data.crunchbase.com/reference/searchfundingrounds

---

*Analysis Date: January 2026*
