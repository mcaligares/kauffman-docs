# PitchBook API Endpoints Overview

This document provides a comprehensive overview of all available PitchBook API endpoints, organized by category.

---

## Entity General Endpoints

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| Entity People | 1 | Retrieves data about each team member of the specific entity | `GET /entities/{pbId}/people` | - |
| Entity Locations | 1 | Retrieves data about locations of HQ and alternate offices of the specific entity | `GET /entities/{pbId}/locations` | - |
| Entity Affiliates | 1 | Retrieves entities affiliated with the specific company | `GET /entities/affiliates` | - |
| Entity News | 1 | Retrieves news articles for the specific entity news for up to the last 30 days | `GET /companies/{pbId}/news` | - |

---

## Company Specific Endpoints

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| Company Bio | 1 | Retrieves key data points about specific company | `GET /companies/{pbId}/bio` | [company_bio.json](examples/company_bio.json) |
| Company Industry | 1 | Retrieves industries and verticals tagged to the specific company | `GET /companies/{pbId}/industries` | - |
| Company All Investors | 1 | Retrieves current and former investors of the specific company | `GET /companies/{pbId}/investors` | [company_investors.json](examples/company_investors.json) |
| Company Deals | 1 | Retrieves all deals of the specific company | `GET /companies/{pbId}/deals` | [company_deals.json](examples/company_deals.json) |
| Company Active Investors | 1 | Retrieves current investors of the specific company | `GET /companies/{pbId}/active-investors` | - |
| Company General Service Providers | 1 | Retrieves service providers hired for general services to the specific company | `GET /companies/{pbId}/general-service-providers` | - |
| Company Deal Service Providers | 1 | Retrieves service providers hired within the deals to the specific company | `GET /companies/{pbId}/deal-service-providers` | - |
| Company Most Recent Financing | 1 | Retrieves last financing details of the specific company | `GET /companies/{pbId}/most-recent-financing` | - |
| Company Most Recent Debt Financing | 1 | Retrieves debt and lender data within the specific deal | `GET /companies/{pbId}/most-recent-debt-financing` | - |
| Company VC Exit Predictions | 3 | For a given pbId returns exit predictions based on PitchBook machine learning. Company needs to have at least 2 rounds of funding in the past six years to qualify for this dataset | `GET /companies/{pbId}/vc-exit-predictions` | - |
| Most Recent Private Company Financials | 1 | Retrieves key financial data points of the specific company | `GET /companies/{pbId}/most-recent-financials` | - |
| Company Social Analytics | 1 | Retrieves social media and web signals data points of the specific company | `GET /companies/{pbId}/social-analytics` | - |
| Company Similar Companies | 1 | Retrieves top 10 similar companies/competitors | `GET /companies/{pbId}/similar-companies` | - |

---

## Deal Specific Endpoints

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| Deal Bio | 1 | Retrieves key data points about specific deal | `GET /deals/{pbId}/bio` | [deal_bio.json](examples/deal_bio.json) |
| Deal Detailed | 1 | Retrieves extended description of the specific deal | `GET /deals/{pbId}/detailed` | - |
| Deal Investors/Exiters | 1 | Retrieves investors, exiters and sellers within the specific deal | `GET /deals/{pbId}/investors/exiters` | [deal_investors_exiters.json](examples/deal_investors_exiters.json) |
| Deal Service Providers | 1 | Retrieves service providers hired within the specific deal | `GET /deals/{pbId}/service-providers` | - |
| Deal Valuation | 1 | Retrieves the company's valuation prior and after the specific deal | `GET /deals/{pbId}/valuation` | [deal_valuation.json](examples/deal_valuation.json) |
| Deal Stock Info | 1 | Retrieves key data about stock within the specific deal | `GET /deals/{pbId}/stock-info` | - |
| Deal Cap Table History | 10 | Retrieves stock prices values within the specific deal and prior to it | `GET /deals/{pbId}/cap-table-history` | - |
| Deal Debt/Lenders | 1 | Retrieves debt and lender data within the specific deal | `GET /deals/{pbId}/debt-lenders` | - |
| Deal Multiples | 1 | Retrieves key financial multiple values of the specific deal | `GET /deals/{pbId}/multiples` | - |
| Tranche Information | 1 | Retrieves key data about the tranches of the specific deal | `GET /deals/{pbId}/tranche-info` | - |

---

## People Specific Endpoints

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| People Bio | 1 | Retrieves key data points about specific person | `GET /people/{pbId}/bio` | [people_bio.json](examples/people_bio.json) |
| People Education/Work | 1 | Retrieves company, deal, fund and advisory roles of the specific person | `GET /people/{pbId}/education-work` | [people_education_work.json](examples/people_education_work.json) |
| People Contact | 1 | Retrieves contact information of the specific person | `GET /people/{pbId}/contact` | [people_contact.json](examples/people_contact.json) |

---

## Investor Specific Endpoints

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| Investor Bio | 1 | Retrieves key data points about specific investor | `GET /investors/{pbId}/bio` | [investor_bio.json](examples/investor_bio.json) |
| Active Investments | 1 | Retrieves active investments of the specific investor | `GET /investors/{pbId}/active-investments` | - |
| All Investments | 1 | Retrieves details of completed investments of the specific investor | `GET /investors/{pbId}/investments` | [all_investments.json](examples/all_investments.json) |
| Investment Preferences | 1 | Retrieves key investment preferences of the specific investor | `GET /investors/{pbId}/investment-preferences` | - |
| Investor Funds | 1 | Retrieves open and closed funds of the specific investor | `GET /investors/{pbId}/funds` | [investor_funds.json](examples/investor_funds.json) |
| Investor Last Closed Fund | 1 | Retrieves details of the last closed fund for the specific investor | `GET /investors/{pbId}/last-closed-fund` | - |
| Investor General Service Providers | 1 | Retrieves service providers hired for general services to the specific investor | `GET /investors/{pbId}/general-service-providers` | - |
| Investor Deal Service Providers | 1 | Retrieves service providers hired within the deals to the specific company | `GET /investors/{pbId}/deal-service-providers` | - |
| Investor Board Memberships | 1 | Retrieves board seats held by the specific investor | `GET /investors/{pbId}/board-seats` | - |

---

## Fund Specific Endpoints

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| Fund Bio | 1 | Retrieves key data points about specific fund | `GET /funds/{pbId}/bio` | [fund_bio.json](examples/fund_bio.json) |
| Most Recent Fund Performance | 1 | Retrieves most recent returns data of the specific fund | `GET /funds/{pbId}/performance` | - |
| Fund Commitments | 1 | Retrieves details of commitments related to the specific fund | `GET /funds/{pbId}/commitments` | - |
| Fund Cash Flows | 1 | Retrieves contributed and distributed amounts for the specific fund | `GET /funds/{pbId}/cashflows/{period}` | - |
| Fund People | 1 | Retrieves data about each team member of the specific fund | `GET /funds/{pbId}/team` | - |
| Fund Portfolio Holdings | 1 | Retrieves key portfolio holding information for the specified fund | `GET /funds/{pbId}/portfolio-holdings/{period}` | - |
| Fund All Investments | 1 | Retrieves current and former investments made through the specific fund | `GET /funds/{pbId}/investments` | [fund_investments.json](examples/fund_investments.json) |
| Fund Investment Preferences | 1 | Retrieves key investment preferences of the specific fund | `GET /funds/{pbId}/investment-preferences` | - |
| Fund Benchmark | 1 | Retrieves details of benchmark values of the specific fund | `GET /funds/{pbId}/benchmark` | - |
| Fund Active Investments | 1 | Retrieves current investments made through the specific fund | `GET /funds/{pbId}/active-investments` | - |
| Fund Service Providers | 1 | Retrieves service providers related to the specific fund | `GET /funds/{pbId}/service-providers` | - |

---

## Limited Partner Specific Endpoints

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| LP Bio | 1 | Retrieves key data points about specific limited partner | `GET /limited-partners/{pbId}/bio` | - |
| LP Commitment Prefs | 1 | Retrieves key investment preferences of the specific limited partner | `GET /limited-partners/{pbId}/commitment-prefs` | - |
| LP Commitments Detailed | 1 | Retrieves commitments information related to the specific limited partner | `GET /limited-partners/{pbId}/commitments-detailed` | - |
| LP Actual Allocations | 1 | Retrieves actual amounts of the portfolios by sectors for the specific limited partner | `GET /limited-partners/{pbId}/actual-allocations` | - |
| LP Target Allocations | 1 | Retrieves target amounts of the portfolios by sectors for the specific limited partner | `GET /limited-partners/{pbId}/target-allocations` | - |
| LP Service Providers | 1 | Retrieves service providers hired for services to the specific limited partner | `GET /limited-partners/{pbId}/service-providers` | - |
| LP Commitment Aggregates | 1 | Retrieves commitments counts to different fund types made by the specific limited partner | `GET /limited-partners/{pbId}/commitments-aggregates` | - |

---

## Service Providers Specific Endpoints

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| Service Provider Bio | 1 | Retrieves key data points about specific service provider | `GET /service-providers/{pbId}/bio` | - |
| Serviced Companies (General) | 1 | Retrieves companies that were serviced by the specific service provider | `GET /service-providers/{pbId}/serviced-companies` | - |
| Serviced Investors (General) | 1 | Retrieves investors that were serviced by the specific service provider | `GET /service-providers/{pbId}/serviced-investors` | - |
| Serviced Funds (General) | 1 | Retrieves funds that were serviced by the specific service provider | `GET /service-providers/{pbId}/serviced-funds` | - |
| Serviced LPs (General) | 1 | Retrieves limited partners that were serviced by the specific service provider | `GET /service-providers/{pbId}/serviced-lps` | - |
| Serviced Deals | 1 | Retrieves data about deals and corresponding companies that were serviced by the specific service provider | `GET /service-providers/{pbId}/serviced-deals` | - |

---

## Credit Analysis Endpoints (add-on only)

> ⚠️ **Note:** These endpoints require a separate package.

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| Credit News Search | Separate Package | Retrieves main info about the Credit news articles matching the specified criteria | `GET /credit-analysis/credit-news/search` | - |
| Most Recent Credit News | Separate Package | Retrieves up to 250 of the most recently published articles and their associated descriptions (excluding article content). Users can specify page number parameter to retrieve older articles | `GET /credit-analysis/credit-news/most-recent` | - |
| Credit News | Separate Package | Retrieves full article content for specific article IDs | `GET /credit-analysis/credit-news/{articleID}` | - |
| [Bulk] Credit News | Separate Package | Returns whole information about the specified Credit News articles. This bulk call allows you to send multiple articleIds within a single API call | `POST /credit-analysis/credit-news` | - |
| Credit News Attachment | Separate Package | Retrieves an exact attachment file for a specific article | `GET /credit-analysis/credit-news/{articleID}/attachment` | - |

---

## Patents Endpoints (add-on only)

> ⚠️ **Note:** These endpoints require a separate package.

| Endpoint Name | Credit Cost | Description | Example | Example Response |
|---------------|-------------|-------------|---------|------------------|
| Patent Search | Separate Package | Retrieves patents matching the specified criteria | `GET /companies/{pbId}/patents/search` | - |
| Patent Detailed | Separate Package | Retrieves extended description of the specific patent | `GET /companies/patents/{patentId}/detailed` | - |
| Patent File Download | Separate Package | Retrieves an exact file of a specific patent | `GET /companies/patents/{patentId}/download` | - |

---

## Available Example Files

The following example response files are available in the `examples/` folder:

| File | Related Endpoint |
|------|------------------|
| [company_bio.json](examples/company_bio.json) | Company Bio |
| [company_deals.json](examples/company_deals.json) | Company Deals |
| [company_investors.json](examples/company_investors.json) | Company All Investors |
| [deal_bio.json](examples/deal_bio.json) | Deal Bio |
| [deal_investors_exiters.json](examples/deal_investors_exiters.json) | Deal Investors/Exiters |
| [deal_valuation.json](examples/deal_valuation.json) | Deal Valuation |
| [people_bio.json](examples/people_bio.json) | People Bio |
| [people_contact.json](examples/people_contact.json) | People Contact |
| [people_education_work.json](examples/people_education_work.json) | People Education/Work |
| [investor_bio.json](examples/investor_bio.json) | Investor Bio |
| [investor_funds.json](examples/investor_funds.json) | Investor Funds |
| [all_investments.json](examples/all_investments.json) | All Investments |
| [fund_bio.json](examples/fund_bio.json) | Fund Bio |
| [fund_investments.json](examples/fund_investments.json) | Fund All Investments |
| [investor_search.json](examples/investor_search.json) | Investor Search* |
| [people_search.json](examples/people_search.json) | People Search* |
| [fund_search.json](examples/fund_search.json) | Fund Search* |

> *Search endpoints are part of the Search API (not listed in this overview)
