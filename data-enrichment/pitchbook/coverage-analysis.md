# PitchBook Coverage Analysis

Analysis of PitchBook API data to determine field coverage for Kauffman Fellows data enrichment.

## Source Files

| File | Description |
|------|-------------|
| `airtable/sample_normalized.csv` | Participant profiles (8 records, 31 fields) |
| `airtable/sample_investments.csv` | Investment data (15 records, 46 fields) |
| `pitchbook/examples/*.json` | PitchBook API response examples (17 endpoints) |

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

Fields from `sample_normalized.csv` mapped to PitchBook:

| Airtable Field | PitchBook Endpoint | PitchBook Field | Coverage |
|----------------|-------------------|-----------------|----------|
| Name | People Bio | `personName.full` | ✅ |
| KF Class # | - | - | ❌ Internal KF data |
| Headshot | - | - | ❌ Not available |
| Category | - | - | ❌ Internal KF data |
| Preferred First Name | People Bio | `personName.first` | ✅ |
| Last Name | People Bio | `personName.last` | ✅ |
| Email Address | People Contact | `email` | ✅ |
| State/Province | People Bio | `primaryOffice.stateProvince` | ✅ |
| Country | People Bio | `primaryOffice.country` | ✅ |
| Job Title | People Bio | `primaryPosition` | ✅ |
| Current Firm | People Bio | `primaryEntityName` | ✅ |
| Primary Fund Type (from Current Firm) | Investor Bio | `investorType[].type.description` | ✅ |
| Alternate Email | - | - | ❌ Not available |
| LinkedIn URL | People Bio | `linkedInProfileUrl` | ✅ |
| Primary Region Based In | People Bio | `primaryOffice.globalRegion` | ✅ |
| WhatsApp Country Code | - | - | ❌ Not available |
| WhatsApp Phone (exc. Country Code) | - | - | ❌ Not available |
| Date of Birth | - | - | ❌ Not available |
| % Direct VC | - | - | ❌ Internal metric |
| % Emerging Managers | - | - | ❌ Internal metric |
| % Established Managers | - | - | ❌ Internal metric |
| Investing Thesis | - | - | ❌ Internal knowledge |
| Professional Bio | People Bio | `biography` | ✅ |
| Education/Personal Bio | People Education/Work | `education[]` | ✅ |
| Biography URL | - | - | ❌ Internal KF link |
| WhatsApp # | - | - | ❌ Not available |
| Groups In | - | - | ❌ Internal KF data |
| Preferred Stages | - | - | ❌ Not available |
| Mentees Are: | - | - | ❌ Internal KF data |
| Mentors Class #: | - | - | ❌ Internal KF data |
| Update Biography | - | - | ❌ Internal link |

### Personal Data Summary

| Category | Count |
|----------|-------|
| **People Bio** | 11 fields |
| **People Education/Work** | 1 field |
| **People Contact** | 1 field |
| **Investor Bio** | 1 field |
| **Not Available in PitchBook** | 17 fields |

**Total Coverage:** 14 of 31 fields (45%)

---

## Investment Data Coverage

Fields from `sample_investments.csv` mapped to PitchBook:

| Airtable Field | PitchBook Endpoint | PitchBook Field | Coverage |
|----------------|-------------------|-----------------|----------|
| Name | Company Bio | `companyName.formalName` | ✅ |
| Company PBID | - | Direct lookup ID | ✅ Already in Airtable |
| KF NEW | - | - | ❌ Internal |
| Funds | - | - | ❌ Internal |
| Link to Company | - | - | ❌ Internal link |
| Founder(s) | - | - | ❌ Not available |
| Year Founded (from Link to Company) | Company Bio | `yearFounded` | ✅ |
| Last Known Valuation (from Link to Company) | - | - | ❌ Not directly available |
| Exit/Unicorn | - | - | ❌ Internal flag |
| Company Name (PB) | Company Bio | `companyName.formalName` | ✅ |
| Lead Partner PBID | - | Direct lookup ID | ✅ Already in Airtable |
| Lead Partner Name | Deal Investors/Exiters | `leadPartners[]` | ✅ |
| Investor PBID | - | Direct lookup ID | ✅ Already in Airtable |
| Deal Attributable To (Fellow): | - | - | ❌ Internal |
| Gender Identity (from Deal Attributable To) | People Bio | `gender` | ✅ |
| Deal Attributable To (Non-Fellow): | - | - | ❌ Internal |
| Investment Date (Close Date on PB) | Deal Bio | `dealDate` | ✅ |
| Deal Size | Deal Bio | `dealSize.amount` | ✅ |
| Post-Valuation (M) | Deal Valuation | `postValuation.amount` | ✅ |
| Previous Post-Valuation (M) | - | - | ❌ Not available |
| Deal Type | Deal Bio | `dealType1.description` | ✅ |
| Deal Stage [PB] | Deal Bio | `dealType2.description` | ✅ |
| Stage [Internal] | - | - | ❌ Internal |
| Exits (Did Firm Personally Exit)? | - | - | ❌ Internal |
| Notes | - | - | ❌ Internal |
| Future - ROUND INVESTED IN/ROUND ID? | - | - | ❌ Internal |
| Confidentiality | - | - | ❌ Internal |
| Count | - | - | ❌ Internal |
| Firms (from Funds) | - | - | ❌ Internal |
| Firms | - | - | ❌ Internal |
| Deals (Nested) | - | - | ❌ Internal |
| Added By (noloco) | - | - | ❌ Internal |
| Attribution at Firm? | - | - | ❌ Internal |
| Investor Name | Investor Bio | `investorName.formalName` | ✅ |
| Previous Round Date | - | - | ❌ Not available |
| Previous Deal Type | - | - | ❌ Not available |
| Exit Type (from Link to Company) | All Investments | `exitDealId` → Deal Bio | ⚠️ Requires lookup |
| Unicorn Status (from Link to Company) | - | - | ⚠️ Derived from valuation |
| HQ Country (from Link to Company) | Company Bio | `hqLocation.country` | ✅ |
| Industry Code (from Link to Company) | Company Bio | `sicCodes[].description` | ✅ |
| Industry Group (from Link to Company) | - | - | ⚠️ Derived from SIC |
| Industry Sector (from Link to Company) | - | - | ⚠️ Derived from SIC |
| Created | - | - | ❌ Internal |
| Investment Role | - | - | ❌ Not available |
| Fund Returner | - | - | ❌ Not available |
| Fund Multiple | - | - | ❌ Not available |

### Investment Data Summary

| Category | Count |
|----------|-------|
| **Company Bio** | 5 fields |
| **Deal Bio** | 4 fields |
| **Deal Valuation** | 1 field |
| **Deal Investors/Exiters** | 1 field |
| **Investor Bio** | 2 fields |
| **People Bio** | 1 field |
| **IDs (already in Airtable)** | 3 fields |
| **Not Available/Internal** | 29 fields |

**Total Coverage:** 17 of 46 fields (37%)

---

## Fields Not Available in PitchBook

These fields must remain in Airtable (no external source):

### Personal Data

| Field | Reason |
|-------|--------|
| Headshot | Not exposed via API |
| Date of Birth | Not tracked |
| Alternate Email | Only primary email available |
| WhatsApp Country Code | Not tracked |
| WhatsApp Phone | Not tracked |
| WhatsApp # | Not tracked |
| Preferred Stages | Not available |
| KF Class # | Internal KF data |
| Category | Internal KF data |
| Biography URL | Internal KF link |
| Groups In | Internal KF data |
| Mentees Are: | Internal KF data |
| Mentors Class #: | Internal KF data |
| Update Biography | Internal link |
| Investing Thesis | Internal knowledge |
| % Direct VC | Internal metric |
| % Emerging Managers | Internal metric |
| % Established Managers | Internal metric |

### Investment Data

| Field | Reason |
|-------|--------|
| Founder(s) | Not directly available |
| Last Known Valuation | Not directly available |
| Previous Post-Valuation (M) | Not available |
| Previous Round Date | Not available |
| Previous Deal Type | Not available |
| Investment Role | Not available |
| Fund Returner | Not available |
| Fund Multiple | Not available |
| All internal fields | KF-specific data |

---

## Summary

### Coverage Overview

| Data Type | Available | Not Available | Coverage |
|-----------|-----------|---------------|----------|
| Personal Data | 14 fields | 17 fields | 45% |
| Investment Data | 17 fields | 29 fields | 37% |
| **Total** | **31 fields** | **46 fields** | **40%** |

### Coverage by Endpoint

| Endpoint | Personal | Investment | Total |
|----------|----------|------------|-------|
| People Bio | 11 | 1 | 12 |
| People Education/Work | 1 | 0 | 1 |
| People Contact | 1 | 0 | 1 |
| Company Bio | 0 | 5 | 5 |
| Deal Bio | 0 | 4 | 4 |
| Deal Valuation | 0 | 1 | 1 |
| Deal Investors/Exiters | 0 | 1 | 1 |
| Investor Bio | 1 | 2 | 3 |

### Key Limitations

1. **No headshot** - Profile images not available via API
2. **No date of birth** - Not tracked by PitchBook
3. **No Preferred Stages** - Investor stage preferences not available
4. **Previous round data** - Historical valuation data not available
5. **Fund performance** - Fund Returner and Fund Multiple not available

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

### Investment Data

| Field | Endpoint | PitchBook Field |
|-------|----------|-----------------|
| Deal Status | Deal Bio | `dealStatus.description` |
| Deal Class | Deal Bio | `dealClass.description` |
| Pre-Valuation | Deal Valuation | `preValuation.amount` |
| Company Employees | Company Bio | `employees` |
| Company Financing Status | Company Bio | `financingStatus.description` |
| Company Total Money Raised | Company Bio | `totalMoneyRaised.amount` |

### Investor/Fund Data

| Field | Endpoint | PitchBook Field |
|-------|----------|-----------------|
| AUM | Investor Bio | `assetsUnderManagement.amount` |
| Dry Powder | Investor Bio | `dryPowder.amount` |
| Investor Status | Investor Bio | `investorStatus.description` |
| Fund Size | Fund Bio | `fundSize.amount` |
| Fund Vintage | Fund Bio | `vintage` |
| Fund Status | Fund Bio | `fundStatus` |

---

*Analysis Date: January 2026*  
*Source: PitchBook API Examples (17 endpoints)*
