# PitchBook Coverage Analysis

Analysis of PitchBook API data to determine field coverage for Kauffman Fellows data enrichment.

## Source Files

| File | Description |
|------|-------------|
| `../data-field-reference.md` | Prioritized reference fields |
| `./examples/*.json` | PitchBook API response examples (17 endpoints) |

## API Endpoints Overview

PitchBook API organizes data by entity type with separate endpoints for each data category:

| Entity | Endpoints | Description |
|--------|-----------|-------------|
| **People** | Bio, Education/Work, Contact | Person profiles, work history, contact info |
| **Companies** | Bio, Deals, Investors | Company profiles and transaction history |
| **Deals** | Bio, Valuation, Investors/Exiters | Deal details, valuations, participants |
| **Investors** | Bio, Investments, Funds | Investor firm profiles and portfolios |
| **Funds** | Bio, Investments | Fund details and portfolio companies |

---

## Personal Data Coverage

Fields from prioritized Personal Data reference mapped to PitchBook:

| Airtable Field | PitchBook Endpoint | PitchBook Field | Coverage |
|----------------|-------------------|-----------------|----------|
| City | People Bio | `primaryOffice.city` | ✅ |
| State / Province | People Bio | `primaryOffice.stateProvince` | ✅ |
| Primary Region Based In | People Bio | `primaryOffice.globalRegion` | ✅ |
| Current Title Band | — | — | ❌ |
| Metropolitans | — | — | ❌ |
| Founding Member at Firm | — | — | ❌ |
| Currently Fundraising? | — | — | ❌ |
| All Roles | People Education/Work | `companyRoles[]` | ✅ |
| Current Board Seats | — | — | ❌ |
| Prior Board Seats | — | — | ❌ |
| Companies | People Education/Work | `companyRoles[].companyName` | ✅ |
| Groups In | — | — | ❌ |
| Current Firm | People Bio | `primaryEntityName` | ✅ |
| Events Attended / Attending | — | — | ❌ |
| Investments | People Education/Work | `dealRoles[].companyName` | ✅ |
| Current Firm Name | People Bio | `primaryEntityName` | ✅ |
| Firm Thesis | — | — | ❌ |

### Personal Data Summary

| Category | Count |
|----------|-------|
| **People Bio** | 5 fields |
| **People Education/Work** | 3 fields |
| **People Contact** | 0 fields |
| **Investor Bio** | 0 fields |
| **Not Available in PitchBook** | 9 fields |

**Total Coverage:** 8 of 17 fields (47%)

---

## Investment Data Coverage

Fields from prioritized Investment Data reference mapped to PitchBook:

| Airtable Field | PitchBook Endpoint | PitchBook Field | Coverage |
|----------------|-------------------|-----------------|----------|
| Name | People Bio | `primaryEntityName` | ✅ |
| Deal Attributable To (Fellow) | — | — | ❌ |
| Investment Date (Close Date on PB) | Deal Bio | `dealDate` | ✅ |
| HQ Country (from Link to Company) | Company Bio | `hqLocation.country` | ✅ |
| Industry Code (from Link to Company) | Company Bio | `sicCodes[].description` | ✅ |

### Investment Data Summary

| Category | Count |
|----------|-------|
| **People Bio** | 1 field |
| **Company Bio** | 2 fields |
| **Deal Bio** | 1 field |
| **Deal Valuation** | 0 fields |
| **Deal Investors/Exiters** | 0 fields |
| **Investor Bio** | 0 fields |
| **Not Available/Internal** | 1 field |

**Total Coverage:** 4 of 5 fields (80%)

---

## Fields Not Available in PitchBook

These fields must remain in Airtable (no external source):

### Personal Data

| Field | Reason |
|-------|--------|
| Current Title Band | Internal classification |
| Metropolitans | Internal aggregation |
| Founding Member at Firm | Internal KF data |
| Currently Fundraising? | Internal status |
| Current Board Seats | Internal tracking |
| Prior Board Seats | Internal tracking |
| Groups In | Internal KF data |
| Events Attended / Attending | Internal event tracking |
| Firm Thesis | Internal qualitative data |

### Investment Data

| Field | Reason |
|-------|--------|
| Deal Attributable To (Fellow) | Internal KF attribution |

---

## Summary

### Coverage Overview

| Data Type | Available fields | Not Available fields | Coverage |
|-----------|------------------|----------------------|----------|
| Personal Data | 8 fields | 9 fields | 47% |
| Investment Data | 4 fields | 1 field | 80% |

### Coverage by Endpoint

| Endpoint | Personal | Investment | Total |
|----------|----------|------------|-------|
| People Bio | 5 | 1 | 6 |
| People Education/Work | 3 | 0 | 3 |
| People Contact | 0 | 0 | 0 |
| Company Bio | 0 | 2 | 2 |
| Deal Bio | 0 | 1 | 1 |
| Deal Valuation | 0 | 0 | 0 |
| Deal Investors/Exiters | 0 | 0 | 0 |
| Investor Bio | 0 | 0 | 0 |

---

## Additional Fields Available

Fields not currently in Airtable that PitchBook can provide:

### Personal Data

| Field | Endpoint | PitchBook Field |
|-------|----------|-----------------|
| Gender | People Bio | `gender` |
| Phone | People Contact | `phone` |
| Work History | People Education/Work | `companyRoles[]` |
| Board Seats | People Education/Work | `boardSeats[]` |
| Fund Roles | People Education/Work | `fundRoles[]` |

### Investment / Company / Investor Data

| Field | Endpoint | PitchBook Field |
|-------|----------|-----------------|
| Company Financing Status | Company Bio | `financingStatus.description` |
| Investor Status | Investor Bio | `investorStatus.description` |
| Fund Status | Fund Bio | `fundStatus` |

---

*Analysis Date: January 2026*  
*Source: PitchBook API Examples (17 endpoints)*
