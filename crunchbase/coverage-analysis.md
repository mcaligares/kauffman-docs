# Crunchbase Coverage Analysis

Analysis of Crunchbase data dictionary to determine field coverage for Kauffman Fellows data enrichment.

## Source Files

| File | Description |
|------|-------------|
| `airtable/sample_normalized.csv` | Participant profiles (8 records, 31 fields) |
| `airtable/sample_investments.csv` | Investment data (15 records, 46 fields) |
| `crunchbase/data_dictionary.csv` | Crunchbase API field reference (458 entries) |

## API Packages Overview

Crunchbase API fields are organized into 4 packages with different pricing tiers:

| Package | Tier | Description |
|---------|------|-------------|
| **Fundamentals - Core Firmographics** | Base | Basic company and people data |
| **Fundamentals - Advanced Firmographics** | Premium | Extended profiles, events, diversity data |
| **Fundamentals - Core Financials** | Base | Basic funding and investment data |
| **Fundamentals - Advanced Financials** | Premium | Detailed deal data, valuations, acquisitions |

---

## Personal Data Coverage

Fields from `sample_normalized.csv` mapped to Crunchbase:

| Airtable Field | Crunchbase Entity | Crunchbase Field | Package | Coverage |
|----------------|-------------------|------------------|---------|----------|
| Name | PEOPLE | `name` | Core Firmographics | ✅ |
| KF Class # | - | - | - | ❌ Internal KF data |
| Headshot | PEOPLE | `image_url` | Core Firmographics | ✅ |
| Category | - | - | - | ❌ Internal KF data |
| Preferred First Name | PEOPLE | `first_name` | Core Firmographics | ✅ |
| Last Name | PEOPLE | `last_name` | Core Firmographics | ✅ |
| Email Address | - | - | - | ❌ Not available |
| State/Province | LOCATIONS | `region_code` | Core Firmographics | ✅ |
| Country | LOCATIONS | `country_code` | Core Firmographics | ✅ |
| Job Title | PEOPLE | `primary_job_title` | Core Firmographics | ✅ |
| Current Firm | PEOPLE | `primary_organization` | Core Firmographics | ✅ |
| Primary Fund Type (from Current Firm) | ORGANIZATIONS | `company_type` | Core Firmographics | ✅ |
| Alternate Email | - | - | - | ❌ Not available |
| LinkedIn URL | PEOPLE | `linkedin` | Core Firmographics | ✅ |
| Primary Region Based In | LOCATIONS | `groups` | Core Firmographics | ✅ |
| WhatsApp Country Code | - | - | - | ❌ Not available |
| WhatsApp Phone (exc. Country Code) | - | - | - | ❌ Not available |
| Date of Birth | PEOPLE | `born_on` | Core Firmographics | ✅ |
| % Direct VC | - | - | - | ❌ Internal metric |
| % Emerging Managers | - | - | - | ❌ Internal metric |
| % Established Managers | - | - | - | ❌ Internal metric |
| Investing Thesis | - | - | - | ❌ Internal knowledge |
| Professional Bio | PEOPLE | `description` | Core Firmographics | ✅ |
| Education/Personal Bio | DEGREES | `subject`, `type_name`, `school_identifier` | Advanced Firmographics | ✅ |
| Biography URL | - | - | - | ❌ Internal KF link |
| WhatsApp # | - | - | - | ❌ Not available |
| Groups In | - | - | - | ❌ Internal KF data |
| Preferred Stages | PEOPLE | `investor_stage` | Core Financials | ✅ |
| Mentees Are: | - | - | - | ❌ Internal KF data |
| Mentors Class #: | - | - | - | ❌ Internal KF data |
| Update Biography | - | - | - | ❌ Internal link |

### Personal Data Summary

| Category | Count |
|----------|-------|
| **Core Firmographics** | 13 fields |
| **Advanced Firmographics** | 1 field |
| **Core Financials** | 1 field |
| **Not Available in Crunchbase** | 16 fields |

**Total Coverage:** 15 of 31 fields (48%)

---

## Investment Data Coverage

Fields from `sample_investments.csv` mapped to Crunchbase:

| Airtable Field | Crunchbase Entity | Crunchbase Field | Package | Coverage |
|----------------|-------------------|------------------|---------|----------|
| Name | ORGANIZATIONS | `name` | Core Firmographics | ✅ |
| Company PBID | - | - | - | ❌ PitchBook ID |
| KF NEW | - | - | - | ❌ Internal |
| Funds | - | - | - | ❌ Internal |
| Link to Company | - | - | - | ❌ Internal link |
| Founder(s) | ORGANIZATIONS | `founder_identifiers` | Core Firmographics | ✅ |
| Year Founded (from Link to Company) | ORGANIZATIONS | `founded_on` | Core Firmographics | ✅ |
| Last Known Valuation (from Link to Company) | ORGANIZATIONS | `valuation` | Advanced Financials | ✅ |
| Exit/Unicorn | - | - | - | ❌ Internal flag |
| Company Name (PB) | ORGANIZATIONS | `name` | Core Firmographics | ✅ |
| Lead Partner PBID | - | - | - | ❌ PitchBook ID |
| Lead Partner Name | INVESTMENTS | `partner_identifiers` | Advanced Financials | ✅ |
| Investor PBID | - | - | - | ❌ PitchBook ID |
| Deal Attributable To (Fellow): | - | - | - | ❌ Internal |
| Gender Identity (from Deal Attributable To) | - | - | - | ❌ Internal |
| Deal Attributable To (Non-Fellow): | - | - | - | ❌ Internal |
| Investment Date (Close Date on PB) | INVESTMENTS | `announced_on` | Core Financials | ✅ |
| Deal Size | INVESTMENTS | `money_invested` | Advanced Financials | ✅ |
| Post-Valuation (M) | FUNDING_ROUNDS | `post_money_valuation` | Advanced Financials | ✅ |
| Previous Post-Valuation (M) | - | - | - | ❌ Not available |
| Deal Type | INVESTMENTS | `funding_round_investment_type` | Core Financials | ✅ |
| Deal Stage [PB] | INVESTMENTS | `investor_stage` | Core Financials | ✅ |
| Stage [Internal] | - | - | - | ❌ Internal |
| Exits (Did Firm Personally Exit)? | - | - | - | ❌ Internal |
| Notes | - | - | - | ❌ Internal |
| Future - ROUND INVESTED IN/ROUND ID? | - | - | - | ❌ Internal |
| Confidentiality | - | - | - | ❌ Internal |
| Count | - | - | - | ❌ Internal |
| Firms (from Funds) | - | - | - | ❌ Internal |
| Firms | - | - | - | ❌ Internal |
| Deals (Nested) | - | - | - | ❌ Internal |
| Added By (noloco) | - | - | - | ❌ Internal |
| Attribution at Firm? | - | - | - | ❌ Internal |
| Investor Name | INVESTMENTS | `investor_identifier` | Advanced Financials | ✅ |
| Previous Round Date | - | - | - | ❌ Not available |
| Previous Deal Type | - | - | - | ❌ Not available |
| Exit Type (from Link to Company) | ACQUISITIONS | `acquisition_type` | Advanced Financials | ✅ |
| Unicorn Status (from Link to Company) | ORGANIZATIONS | `valuation` (>$1B) | Advanced Financials | ⚠️ Derived |
| HQ Country (from Link to Company) | ORGANIZATIONS | `location_identifiers` | Core Firmographics | ✅ |
| Industry Code (from Link to Company) | ORGANIZATIONS | `categories` | Core Firmographics | ✅ |
| Industry Group (from Link to Company) | ORGANIZATIONS | `category_groups` | Core Firmographics | ✅ |
| Industry Sector (from Link to Company) | CATEGORIES | `category_groups` | Core Firmographics | ✅ |
| Created | - | - | - | ❌ Internal |
| Investment Role | - | - | - | ❌ Internal |
| Fund Returner | - | - | - | ❌ Not available |
| Fund Multiple | - | - | - | ❌ Not available |

### Investment Data Summary

| Category | Count |
|----------|-------|
| **Core Firmographics** | 8 fields |
| **Core Financials** | 3 fields |
| **Advanced Financials** | 7 fields |
| **Not Available/Internal** | 28 fields |

**Total Coverage:** 18 of 46 fields (39%)

---

## Fields Not Available in Crunchbase

These fields must remain in Airtable (no external source):

### Personal Data

| Field | Reason |
|-------|--------|
| Email Address | Privacy - not exposed via API |
| Alternate Email | Not available |
| WhatsApp Country Code | Not tracked |
| WhatsApp Phone | Not tracked |
| WhatsApp # | Not tracked |
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
| Company PBID | PitchBook ID |
| Lead Partner PBID | PitchBook ID |
| Investor PBID | PitchBook ID |
| Previous Post-Valuation (M) | Not available |
| Previous Round Date | Not available |
| Previous Deal Type | Not available |
| Fund Returner | Not available |
| Fund Multiple | Not available |
| All internal fields | KF-specific data |

---

## Summary

### Coverage Overview

| Data Type | Available | Not Available | Coverage |
|-----------|-----------|---------------|----------|
| Personal Data | 15 fields | 16 fields | 48% |
| Investment Data | 18 fields | 28 fields | 39% |
| **Total** | **33 fields** | **44 fields** | **43%** |

### Coverage by Package

| Package | Personal | Investment | Total |
|---------|----------|------------|-------|
| Core Firmographics | 13 | 8 | 21 |
| Advanced Firmographics | 1 | 0 | 1 |
| Core Financials | 1 | 3 | 4 |
| Advanced Financials | 0 | 7 | 7 |

### Key Limitations

1. **No email data** - Crunchbase does not expose email addresses via API
2. **No WhatsApp data** - Not tracked by Crunchbase
3. **PitchBook IDs** - Cannot be mapped to Crunchbase identifiers
4. **Previous round data** - Historical valuation data not available
5. **Fund performance** - Fund Returner and Fund Multiple not available

---

## Additional Fields Available

Fields not currently in Airtable that Crunchbase can provide:

### Personal Data

| Field | Entity | Crunchbase Field | Package |
|-------|--------|------------------|---------|
| Gender | PEOPLE | `gender` | Core Firmographics |
| Short Description | PEOPLE | `short_description` | Core Firmographics |
| Num Founded Organizations | PEOPLE | `num_founded_organizations` | Core Firmographics |
| Num Portfolio Organizations | PEOPLE | `num_portfolio_organizations` | Core Firmographics |
| Num Investments | PEOPLE | `num_investments` | Core Financials |
| Num Exits | PEOPLE | `num_exits` | Core Financials |

### Investment Data

| Field | Entity | Crunchbase Field | Package |
|-------|--------|------------------|---------|
| Pre-Valuation | FUNDING_ROUNDS | `pre_money_valuation` | Advanced Financials |
| Money Raised | FUNDING_ROUNDS | `money_raised` | Advanced Financials |
| Num Investors | FUNDING_ROUNDS | `num_investors` | Advanced Financials |
| Company Employees | ORGANIZATIONS | `num_employees_enum` | Core Firmographics |
| Company Revenue | ORGANIZATIONS | `revenue_range` | Advanced Financials |
| Total Funding | ORGANIZATIONS | `funding_total` | Advanced Financials |
| Num Funding Rounds | ORGANIZATIONS | `num_funding_rounds` | Core Financials |
| Last Funding Date | ORGANIZATIONS | `last_funding_at` | Core Financials |
| IPO Status | ORGANIZATIONS | `ipo_status` | Core Financials |
| Acquisition Price | ACQUISITIONS | `price` | Advanced Financials |

---

*Analysis Date: January 2026*  
*Source: Crunchbase Data Dictionary (458 fields)*
