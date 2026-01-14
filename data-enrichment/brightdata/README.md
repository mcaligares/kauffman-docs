# BrightData — Service Evaluation (Discarded)

## Overview

**BrightData** is a data acquisition platform offering two primary approaches:

1. **Pre-built datasets** (one-time or subscription-based)
2. **Custom scraping infrastructure** (pay-as-you-go)

The service was evaluated as a potential alternative data source for **user-level enrichment (investors / people profiles)**.  
**After analysis, BrightData is not recommended for adoption** due to data orientation mismatch, high and inflexible pricing, and significant implementation and operational complexity.

---

## Access Methods Evaluated

### 1. Dataset Provider

BrightData offers pre-collected datasets sourced from multiple platforms.

**Available datasets evaluated:**
- LinkedIn profiles
- LinkedIn company profiles
- Crunchbase companies information
- Companies enriched dataset
- PitchBook companies information
- ZoomInfo companies information

#### Dataset Pricing

| Plan | Volume | Price |
|-----|--------|-------|
| One-time purchase | 100k records | **$250 USD** |
| Monthly subscription | 100k records / month | **$600 USD** |

#### Dataset Fit Assessment

| Dataset | Fit for User Enrichment |
|-------|--------------------------|
| LinkedIn profiles | Partial |
| LinkedIn company profiles | ❌ |
| Crunchbase companies information | ❌ |
| Companies enriched dataset | ❌ |
| PitchBook companies information | ❌ |
| ZoomInfo companies information | ❌ |

**Critical Limitation:**
- **PitchBook and Crunchbase datasets are company-centric**, not person-centric.
- The current data model and enrichment flows are focused on **users / people (investors)**.
- As a result, the most valuable datasets **do not align with the required schema** and cannot be used as a primary data source.

#### Additional Observations

- Dataset pricing is **fixed per volume**, regardless of actual usable records.
- No support for incremental enrichment or selective lookup.
- High data waste when only a subset of records is relevant.
- Poor cost–benefit ratio compared to API-based enrichment services.

---

### 2. Scraper (Pay-as-you-go)

BrightData also provides scraping infrastructure for custom data extraction.

#### Scraper Pricing

- **$1.50 USD per 1,000 records** (pay-as-you-go)
- Cost is variable and depends on:
  - Page complexity
  - Retries
  - Anti-bot measures
  - Scraping duration

#### Functional Constraints

- **LinkedIn URL is mandatory**
  - No native search capability
  - Profiles cannot be resolved by name, firm, or other identifiers
- Coverage is limited to users who already have a LinkedIn URL.
- No fallback or resolution strategy when URLs are missing.

---

## Scraping Implementation Complexity

BrightData scraping can be executed in **synchronous** or **asynchronous** mode, both introducing significant operational complexity.

### Synchronous Scraping

- The client must **keep the request open** until scraping completes.
- Scraping duration is **non-deterministic**:
  - Can take seconds or several minutes per profile.
- Requires backend infrastructure capable of:
  - Long-lived requests
  - High timeout thresholds
  - Connection management at scale
- Poor fit for standard API-based architectures.

### Asynchronous Scraping

- Each scrape is treated as an **independent background process**.
- The client must:
  - Trigger the scrape
  - Track job IDs
  - Poll or query for completion
- Completion time is unknown at request time.
- Requires implementation of:
  - Process orchestration
  - Job state management
  - Retry and failure handling
- Adds significant engineering overhead compared to deterministic API calls.

---

## Coverage & Fit Assessment

| Dimension | Assessment |
|---------|------------|
| User (person) data coverage | Medium (URL-dependent) |
| Company / investment data | High (dataset-based, company-level) |
| Profile resolution | **Low** (no search, URL required) |
| Cost predictability | **Low** |
| Incremental enrichment | ❌ |
| Operational complexity | **Very High** |
| Maintenance effort | **High** |
| Legal / ToS risk | Medium–High |

---

## Summary & Decision

### Decision

**BrightData is not recommended and is excluded from the proposed data architecture.**

### Rationale

- The most relevant datasets (PitchBook, Crunchbase) are **company-focused**, while the project requires **user-level enrichment**.
- Dataset pricing is high and inflexible relative to actual usable coverage.
- The scraping approach:
  - Requires LinkedIn URLs
  - Has no search or fallback mechanism
  - Introduces **non-deterministic execution time**
  - Requires complex sync or async infrastructure to operate reliably
- Overall implementation, operational, and maintenance cost is **significantly higher** than API-based alternatives (Apollo, Crunchbase API, PitchBook API, Signal).

Given these constraints, BrightData does not provide a viable or scalable solution for the current enrichment requirements and is therefore **discarded from further consideration**.

---

## Status

**❌ Discarded — Not included in recommended data sources**
