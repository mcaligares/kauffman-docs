# Data Enrichment Providers

## Detailed Provider Analysis

Each provider included in this comparison has a dedicated technical analysis covering:
- Available endpoints and data coverage
- Enrichment flows and fallback strategies
- Request and credit consumption
- Cost estimation assumptions
- Implementation complexity and risks

For full technical details, refer to the following documents:

- **[PitchBook API Endpoints Analysis](./pitchbook/api-endpoints-analysis.md)**

- **[Crunchbase API Endpoints Analysis](./crunchbase/api-endpoints-analysis.md)**

- **[Apollo Enrichment API Analysis](./apollo/api-endpoints-analysis.md)**

- **[Signal Investor Profiles Analysis](./signal/api-endpoints-analysis.md)**

These detailed analyses support the recommendations and estimates presented in this comparative document.

## Evaluation Criteria

The following criteria are used to compare data providers, prioritized by the value they generate for the project and the client at this stage:

1. **Data Coverage**
   Ability to cover required personal and investment-related fields defined in Airtable.

2. **Match Probability**
   Likelihood of successfully finding a user profile based on available identifiers and platform adoption.

3. **Cost & Licensing Model**
   Annual cost of the recommended plan and how usage scales within credit/request limits.

4. **Annual Usage Volume**
   Number of requests or credits consumed per year, with and without periodic refreshes.

5. **Implementation Complexity**
   Engineering effort required to implement, maintain, and operate the integration.

---

## Cross-Service Comparison

> This comparison is based on the **recommended flow per service**, as defined in each individual analysis.

### Service Overview

| Dimension | PitchBook | Crunchbase | Signal | Apollo |
|---------|-----------|------------|--------|--------|
| Recommended flow | Flow B – With Deal Details | Flow B – Advanced Investment Data | Flow A – Public Profile Access | Flow A – People Enrichment |
| Primary data focus | Investments + people | Investments + people | Investor profiles + investments | Personal & contact data |
| Personal data coverage | **100%** (10/10) | **100%** (11/11) | **83%** (10/12) | **~40%** (4/10) |
| Investment data coverage | Medium (56%) | High (88%) | High (89%) | ❌ Not available |
| Estimated match probability | **~90%** | **~70%** | **~50%** | **~70%** |
| Implementation complexity | Medium | High | **Low** | Low |
| Authentication required | ✅ | ✅ | ❌ | ❌ |
| Recommended plan | TBD | TBD | **Freelancer** | **Basic** |
| Plan pricing | TBD | TBD | **$49 / month ($588 / year)** | **$49 / month (~$588 / year)** |

---

### Annual Usage & Cost Consumption

> Estimated annual usage based on the recommended flow for each service, shown **with and without refresh**.

| Service | Year 1 (no refresh) | Year 1 (with refresh) |
|--------|----------------------|------------------------|
| PitchBook | 15,120 requests | 30,240 requests |
| Crunchbase | 2,520 requests | 5,040 requests |
| Signal | 31,500 credits (1,260 requests) | 63,000 credits (2,520 requests) |
| Apollo | 1,260 credits | 2,520 credits |

---

## Notes & Assumptions

- **Match probability values are estimates**, based on:
  - Identifier availability (PBID, LinkedIn URL, email)
  - How profiles are resolved (ID-based vs name/slug-based)
  - General adoption and likelihood of users having profiles on each platform
- **PitchBook shows the highest match probability (~90%)** because most Airtable users already have a PBID, indicating it is the current underlying data source.
- **Signal relies on public profile slugs only** in the recommended flow and has no fallback, resulting in a lower and uncertain match rate (~50%).
- **Apollo focuses exclusively on personal/contact enrichment** and does not provide investment data.
- **PitchBook and Crunchbase pricing remain TBD**, but annual request volumes are included to support commercial discussions.
- Signal credit usage comfortably fits within the **Freelancer plan (250,000 credits/month)** even with refreshes.
