# PitchBook API Endpoints Analysis

Analysis of PitchBook API endpoints to determine the optimal flow for enriching Kauffman Fellows data.

**Objective:** Evaluate API usage to determine commercial viability, estimate costs, and assess coverage.

**API Documentation:** https://documenter.getpostman.com/view/5190535/TzCV1iRc

---

## Available Data in Airtable

### Person Data (sample_normalized.csv)

| Field | Example | Usable for Lookup |
|-------|---------|-------------------|
| Name | "Allen Taylor" | ✅ (search fallback) |
| Preferred First Name | "Allen" | ✅ (search fallback) |
| Last Name | "Taylor" | ✅ (search fallback) |
| Current Firm | "Endeavor Catalyst" | ✅ (search filter) |

### Investment Data (sample_investments.csv)

| Field | Example | Usable for Lookup |
|-------|---------|-------------------|
| Company PBID | "155890-63" | ✅ (direct lookup) |
| Investor PBID | "185886-37" | ✅ (direct lookup) |
| Lead Partner PBID | "185886-37" | ✅ (direct lookup) |
| Company Name (PB) | "Rappi" | ✅ (search fallback) |

---

## API Endpoints by Category

### People Endpoints

| Endpoint | Use Case |
|----------|----------|
| `GET /people/{pbId}/bio` | Get person profile and current position |
| `GET /people/{pbId}/education-work` | Get education and work history |
| `GET /people/{pbId}/contact` | Get email and phone |

### Company Endpoints

| Endpoint | Use Case |
|----------|----------|
| `GET /companies/{pbId}/bio` | Get company profile |
| `GET /companies/{pbId}/deals` | List all deals for company |
| `GET /companies/{pbId}/investors` | Get investor history |

### Deal Endpoints

| Endpoint | Use Case |
|----------|----------|
| `GET /deals/{pbId}/bio` | Get deal summary (size, type, date) |
| `GET /deals/{pbId}/valuation` | Get pre/post valuations |
| `GET /deals/{pbId}/investors-exiters` | Get deal participants and lead partners |

### Investor Endpoints

| Endpoint | Use Case |
|----------|----------|
| `GET /investors/{pbId}/bio` | Get investor firm profile |
| `GET /investors/{pbId}/investments` | Get all investments by firm |
| `GET /investors/{pbId}/funds` | Get fund list |

---

## API Call Flows

### Flow A: Basic Data

Get Fellow profile and investment list in a single flow.

**Step 1 → People Bio**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET /people/{pbId}/bio` |
| Credit Cost | 1 |
| Input | Lead Partner PBID from Airtable |
| Output | `personName`, `biography`, `linkedInProfileUrl`, `primaryPosition`, `primaryEntityName`, `primaryOffice` |

**Step 2 → People Education/Work**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET /people/{pbId}/education-work` |
| Credit Cost | 1 |
| Input | PBID from Step 1 |
| Output | `education[]`, `companyRoles[]`, `dealRoles[]` (investment list with dealId, companyName, date) |

**Credits per user:** 2

---

### Flow B: With Deal Details

Get Fellow data plus deal sizes and types (no valuations).

**Step 1 → People Bio**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET /people/{pbId}/bio` |
| Credit Cost | 1 |
| Input | Lead Partner PBID from Airtable |
| Output | `personName`, `biography`, `linkedInProfileUrl`, `primaryPosition`, `primaryEntityName`, `primaryOffice` |

**Step 2 → People Education/Work**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET /people/{pbId}/education-work` |
| Credit Cost | 1 |
| Input | PBID from Step 1 |
| Output | `education[]`, `dealRoles[]` with `dealId` for each investment |

**Step 3 → Deal Bio (per deal)**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET /deals/{dealId}/bio` |
| Credit Cost | 1 per deal |
| Input | Deal IDs from Step 2 |
| Output | `dealDate`, `dealSize`, `dealType1`, `dealStatus` |

**Credits per user:** 2 + (1 × number of deals)

---

### Flow C: Full Data with Valuations

Get complete data including deal sizes and valuations.

**Step 1 → People Bio**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET /people/{pbId}/bio` |
| Credit Cost | 1 |
| Input | Lead Partner PBID from Airtable |
| Output | `personName`, `biography`, `linkedInProfileUrl`, `primaryPosition`, `primaryEntityName`, `primaryOffice` |

**Step 2 → People Education/Work**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET /people/{pbId}/education-work` |
| Credit Cost | 1 |
| Input | PBID from Step 1 |
| Output | `education[]`, `dealRoles[]` with `dealId` for each investment |

**Step 3 → Deal Bio (per deal)**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET /deals/{dealId}/bio` |
| Credit Cost | 1 per deal |
| Input | Deal IDs from Step 2 |
| Output | `dealDate`, `dealSize`, `dealType1`, `dealStatus` |

**Step 4 → Deal Valuation (per deal)**

| Attribute | Value |
|-----------|-------|
| Endpoint | `GET /deals/{dealId}/valuation` |
| Credit Cost | 1 per deal |
| Input | Deal IDs from Step 2 |
| Output | `preValuation`, `postValuation` |

**Credits per user:** 2 + (2 × number of deals)

---

### Flow D: Search-Based Lookup ⚠️ FALLBACK ONLY

> **Status:** Use only when PBID is not available in Airtable.

**Reasons to avoid:**
- Search may return multiple results for common names
- Requires manual validation to confirm correct match
- Most records in Airtable already have PBIDs

**Step 1 → People Search**

| Attribute | Value |
|-----------|-------|
| Endpoint | `POST /people/search` |
| Credit Cost | 1 |
| Filter | Name + Current Firm |
| Input | "Allen Taylor", "Endeavor Catalyst" |
| Output | Person PBID (if found) |

**Step 2 → Continue with Flow A, B or C**

| Attribute | Value |
|-----------|-------|
| Input | PBID from Step 1 |

**Credits per user:** 1 + Flow A, B or C

---

## Is Deal Valuation Endpoint Necessary?

Analysis based on `sample_investments.csv` fields:

| Airtable Field | Source: Flow A | Source: Flow B | Source: Flow C |
|----------------|----------------|----------------|----------------|
| Company Name (PB) | ✅ dealRoles | ✅ dealRoles | ✅ dealRoles |
| Investment Date (Close Date on PB) | ✅ dealRoles | ✅ dealRoles | ✅ dealRoles |
| Deal Type | ❌ Not available | ✅ Deal Bio | ✅ Deal Bio |
| Deal Stage [PB] | ❌ Not available | ✅ Deal Bio | ✅ Deal Bio |
| Deal Size | ❌ Not available | ✅ Deal Bio | ✅ Deal Bio |
| Post-Valuation (M) | ❌ Not available | ❌ Not available | ✅ Deal Valuation |

**Conclusion:** Deal Valuation endpoint is only needed if Post-Valuation data is required. Based on sample data, only 6 of 15 investments (40%) have valuation data in Airtable.

**Recommendation:** Start with Flow B (with Deal Bio). Add Flow C only if valuation tracking is a priority.

---

## Coverage Analysis

### Flow A: Basic Coverage

| Airtable Field | Covered | Source |
|----------------|---------|--------|
| Company Name (PB) | ✅ | `dealRoles[].companyName` |
| Investment Date (Close Date on PB) | ✅ | `dealRoles[].dealDate` |
| Deal Type | ❌ | Requires Deal Bio |
| Deal Stage [PB] | ❌ | Requires Deal Bio |
| Deal Size | ❌ | Requires Deal Bio |
| Post-Valuation (M) | ❌ | Requires Deal Valuation |
| Lead Partner Name | ❌ | Requires Deal Investors/Exiters |
| HQ Country (from Link to Company) | ❌ | Requires Company Bio |
| Industry Code (from Link to Company) | ❌ | Requires Company Bio |

**Coverage: 2/9 fields (22%)**

### Flow B: With Deal Details Coverage

| Airtable Field | Covered | Source |
|----------------|---------|--------|
| Company Name (PB) | ✅ | `dealRoles[].companyName` |
| Investment Date (Close Date on PB) | ✅ | `dealRoles[].dealDate` |
| Deal Type | ✅ | `dealType1.description` |
| Deal Stage [PB] | ✅ | `dealType2.description` |
| Deal Size | ✅ | `dealSize.amount` |
| Post-Valuation (M) | ❌ | Requires Deal Valuation |
| Lead Partner Name | ❌ | Requires Deal Investors/Exiters |
| HQ Country (from Link to Company) | ❌ | Requires Company Bio |
| Industry Code (from Link to Company) | ❌ | Requires Company Bio |

**Coverage: 5/9 fields (56%)**

### Flow C: Full Data Coverage

| Airtable Field | Covered | Source |
|----------------|---------|--------|
| Company Name (PB) | ✅ | `dealRoles[].companyName` |
| Investment Date (Close Date on PB) | ✅ | `dealRoles[].dealDate` |
| Deal Type | ✅ | `dealType1.description` |
| Deal Stage [PB] | ✅ | `dealType2.description` |
| Deal Size | ✅ | `dealSize.amount` |
| Post-Valuation (M) | ✅ | `postValuation.amount` |
| Lead Partner Name | ❌ | Requires Deal Investors/Exiters |
| HQ Country (from Link to Company) | ❌ | Requires Company Bio |
| Industry Code (from Link to Company) | ❌ | Requires Company Bio |

**Coverage: 6/9 fields (67%)**

### Person Data Coverage (People Bio + Education/Work)

| Airtable Field | Covered | PitchBook Field |
|----------------|---------|-----------------|
| Name | ✅ | `personName.full` |
| Preferred First Name | ✅ | `personName.first` |
| Last Name | ✅ | `personName.last` |
| LinkedIn URL | ✅ | `linkedInProfileUrl` |
| Job Title | ✅ | `primaryPosition` |
| Current Firm | ✅ | `primaryEntityName` |
| State/Province | ✅ | `primaryOffice.stateProvince` |
| Country | ✅ | `primaryOffice.country` |
| Professional Bio | ✅ | `biography` |
| Education/Personal Bio | ✅ | `education[]` |

**Coverage: 10/10 fields (100%)**

---

## Request Estimates

### Flow A: Basic

| Requests | Per User |
|----------|----------|
| People Bio | 1 |
| People Education/Work | 1 |
| **Total** | **2** |

### Flow B: With Deal Details

| Requests | Per User |
|----------|----------|
| People Bio | 1 |
| People Education/Work | 1 |
| Deal Bio (per deal) | n |
| **Total** | **2 + n** |

### Flow C: Full Data

| Requests | Per User |
|----------|----------|
| People Bio | 1 |
| People Education/Work | 1 |
| Deal Bio (per deal) | n |
| Deal Valuation (per deal) | n |
| **Total** | **2 + 2n** |

---

## Cost Estimation

### Assumptions

| Parameter | Value |
|-----------|-------|
| Current users (initial load) | 1,200 |
| New users per year | 60 |
| Average deals per user | 10 |
| Refresh frequency | Every 6 months (optional) |

### Flow A: Basic

| Scenario | Credits |
|----------|---------|
| Initial load (1,200 users × 2) | 2,400 |
| New users Year 1 (60 × 2) | 120 |
| **Year 1 Total** | **2,520** |

| Scenario | Credits |
|----------|---------|
| Year 1 Total | 2,520 |
| 6-month refresh (1,260 users × 2) | 2,520 |
| **Year 1 Total (with refresh)** | **5,040** |

### Flow B: With Deal Details

| Scenario | Credits |
|----------|---------|
| Initial load (1,200 users × 12) | 14,400 |
| New users Year 1 (60 × 12) | 720 |
| **Year 1 Total** | **15,120** |

| Scenario | Credits |
|----------|---------|
| Year 1 Total | 15,120 |
| 6-month refresh (1,260 users × 12) | 15,120 |
| **Year 1 Total (with refresh)** | **30,240** |

### Flow C: Full Data

| Scenario | Credits |
|----------|---------|
| Initial load (1,200 users × 22) | 26,400 |
| New users Year 1 (60 × 22) | 1,320 |
| **Year 1 Total** | **27,720** |

| Scenario | Credits |
|----------|---------|
| Year 1 Total | 27,720 |
| 6-month refresh (1,260 users × 22) | 27,720 |
| **Year 1 Total (with refresh)** | **55,440** |

### Summary Table

| Scenario | Flow A | Flow B | Flow C |
|----------|--------|--------|--------|
| 1 user (10 deals) | 2 credits | 12 credits | 22 credits |
| 60 users (new/year) | 120 credits | 720 credits | 1,320 credits |
| 1,200 users (initial) | 2,400 credits | 14,400 credits | 26,400 credits |
| Year 1 (no refresh) | 2,520 credits | 15,120 credits | 27,720 credits |
| Year 1 (with refresh) | 5,040 credits | 30,240 credits | 55,440 credits |

---

## Pricing

> ⏳ **TODO:** Complete this section with PitchBook API pricing details.

### Credit Cost

| Parameter | Value |
|-----------|-------|
| Cost per credit | $ TBD |
| Minimum purchase | TBD |
| Volume discounts | TBD |

### Estimated Annual Cost

| Scenario | Credits | Estimated Cost |
|----------|---------|----------------|
| Flow A - Year 1 (no refresh) | 2,520 | $ TBD |
| Flow A - Year 1 (with refresh) | 5,040 | $ TBD |
| Flow B - Year 1 (no refresh) | 15,120 | $ TBD |
| Flow B - Year 1 (with refresh) | 30,240 | $ TBD |
| Flow C - Year 1 (no refresh) | 27,720 | $ TBD |
| Flow C - Year 1 (with refresh) | 55,440 | $ TBD |

---

## Warnings

### Missing PBID

⚠️ **Users Without PBID**

When a user does not have a PBID in Airtable:

- Data will not be loaded if the People Search does not find an exact match
- Search must return a single unique result to be usable
- Users with common names may not be enriched automatically

### Search Limitations

⚠️ **People Search**

PitchBook People Search has limitations:

- No LinkedIn URL as search parameter
- Name matching may return multiple results for common names
- A required field (such as Current Firm) should be set to increase match probability

---

## Summary

> **Note:** Estimates assume an average of 10 deals per user.

### Recommended Flow

| Use Case | Flow | Credits/User | Coverage |
|----------|------|--------------|----------|
| Basic enrichment | A | 2 | 22% |
| With deal details | B | 12 | 56% |
| Full investment data | C | 22 | 67% |

### Flow Comparison

| Flow | Person Data | Investment Data | Valuations | Credits/User |
|------|-------------|-----------------|------------|--------------|
| Flow A | ✅ 100% | ⚠️ 22% | ❌ No | 2 |
| Flow B | ✅ 100% | ⚠️ 56% | ❌ No | 12 |
| Flow C | ✅ 100% | ✅ 67% | ✅ Yes | 22 |

### Decision Matrix

| Priority | Recommended | Flow | Why |
|----------|-------------|------|-----|
| Minimize cost | Flow A | Basic | 2 credits, person data only |
| Balance cost/data | Flow B | With Deal Details | 12 credits, deal sizes included |
| Maximize data | Flow C | Full Data | 22 credits, valuations included |

---

## Response Examples

Example JSON responses are available in the `examples/` folder:

| File | Endpoint | Description |
|------|----------|-------------|
| `people_bio.json` | People Bio | Person profile with position |
| `people_education_work.json` | People Education/Work | Education, work history, dealRoles, fundRoles |
| `people_contact.json` | People Contact | Email and phone |
| `deal_bio.json` | Deal Bio | Deal size, type, date, status |
| `deal_valuation.json` | Deal Valuation | Pre and post valuations |
| `deal_investors_exiters.json` | Deal Investors/Exiters | Deal participants and lead partners |
| `company_bio.json` | Company Bio | Company profile |
| `company_deals.json` | Company Deals | List of all company deals |
| `company_investors.json` | Company All Investors | Investor history |
| `investor_bio.json` | Investor Bio | Investor firm profile |
| `investor_funds.json` | Investor Funds | Fund list |
| `all_investments.json` | All Investments | Full investment portfolio |
| `fund_bio.json` | Fund Bio | Fund details |
| `fund_investments.json` | Fund Investments | Fund portfolio |

---

## API References

- API Documentation: https://documenter.getpostman.com/view/5190535/TzCV1iRc
- Endpoints Overview: [endpoints_overview.md](endpoints_overview.md)

---

*Analysis Date: January 2026*
