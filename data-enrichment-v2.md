# Data Enrichment Tools Evaluation

## Context

**Kauffman Fellows** is a global learning and leadership community for venture capital investors. The organization maintains data about its community participants in Airtable.

### Objective

Evaluate third-party tools to keep participant data up-to-date automatically. This analysis aims to determine how much of the required data each tool can provide.

### Data Categories

Participant data can be classified into three main categories:

| Category | Description | Data Source | Status |
|----------|-------------|-------------|--------|
| **Personal Data** | Contact information, professional profile, employment history | Apollo (API) | ✅ 8 samples available |
| **Investment Data** | Investment thesis, portfolio companies, fund details | Crunchbase (API) | ⏳ Pending samples |
| **Community Data** | KF cohort, roles, groups, mentorship relationships | Airtable + Kauffman Staff | ✅ Manual maintenance |

> **Note**: Community Data is internal to Kauffman Fellows and cannot be obtained from external tools. This data is maintained manually by the Kauffman Staff through Airtable.

### Source Data

- **File**: `airtable/sample.csv` (normalized: `airtable/sample_normalized.csv`)
- **Records**: 8 participant profiles
- **Fields**: 31 columns

### Samples Analyzed

**Apollo** (Personal Data):
- `apollo/allen.json`
- `apollo/andy.json`
- `apollo/dina.json`
- `apollo/eric.json`
- `apollo/jason.json`
- `apollo/jessica.json`
- `apollo/liat.json`
- `apollo/rodrigo.json`

**Crunchbase** (Investment Data):
- ⏳ No samples available yet

---

## Field Classification

Before analyzing tool coverage, fields are classified by category:

| Field | Category | Priority | Notes |
|-------|----------|----------|-------|
| Name | Personal | Critical | User identification |
| Preferred First Name | Personal | Critical | Personalization |
| Last Name | Personal | Critical | User identification |
| Email Address | Personal | Critical | Primary contact |
| Alternate Email | Personal | Medium | Backup contact |
| LinkedIn URL | Personal | Critical | Professional network |
| Job Title | Personal | High | Current role |
| Current Firm | Personal | High | Organization context |
| Country | Personal | High | Location |
| State/Province | Personal | High | Location |
| Primary Region Based In | Personal | High | Geographic classification |
| Professional Bio | Personal | High | Background summary |
| Education/Personal Bio | Personal | Medium | Education details |
| Biography URL | Personal | Medium | External profile |
| Headshot | Personal | Low | Profile photo |
| WhatsApp Country Code | Personal | Low | Alternative contact |
| WhatsApp Phone (exc. Country Code) | Personal | Low | Alternative contact |
| WhatsApp # | Personal | Low | Alternative contact |
| Date of Birth | Personal | Low | Personal info |
| Primary Fund Type (from Current Firm) | Investment | High | Fund classification |
| Investing Thesis | Investment | High | Investment focus |
| Preferred Stages | Investment | High | Investment stages |
| % Direct VC | Investment | Medium | Allocation metrics |
| % Emerging Managers | Investment | Medium | Allocation metrics |
| % Established Managers | Investment | Medium | Allocation metrics |
| KF Class # | Community | Medium | Kauffman Fellows cohort |
| Category | Community | Medium | Fellow, Mentor, Board Member |
| Groups In | Community | Medium | Community groups |
| Mentees Are: | Community | Medium | Mentorship relationships |
| Mentors Class #: | Community | Medium | Mentor relationships |
| Update Biography | Metadata | Low | Internal workflow |

---

## Apollo Coverage Analysis (Personal Data)

### Simple Comparison

| Field | Apollo | Comments |
|-------|--------|----------|
| Name | ✅ | Available as `name`, `first_name`, `last_name` |
| Preferred First Name | ✅ | Available as `first_name` |
| Last Name | ✅ | Available as `last_name` |
| Email Address | ⚠️ | Available but often locked - only 3/8 records have real emails (37.5%) |
| Alternate Email | ⚠️ | Apollo provides single email, doesn't distinguish alternate |
| LinkedIn URL | ✅ | Available as `linkedin_url` |
| Job Title | ✅ | Available as `title` or `headline` |
| Current Firm | ✅ | Available as `organization.name` (7/8 = 87.5%) |
| Country | ✅ | Available as `country` |
| State/Province | ✅ | Available as `state` or `organization.state` |
| Primary Region Based In | ✅ | Can be derived from `country` and `state` |
| Professional Bio | ⚠️ | Only `headline` available, not full bio |
| Education/Personal Bio | ❌ | Not available |
| Biography URL | ❌ | Not available |
| Headshot | ✅ | Available as `photo_url` |
| WhatsApp Country Code | ⚠️ | Available in `phone_numbers` but only 1/8 records (12.5%) |
| WhatsApp Phone | ⚠️ | Available in `phone_numbers` but only 1/8 records (12.5%) |
| WhatsApp # | ⚠️ | Available in `phone_numbers` but only 1/8 records (12.5%) |
| Date of Birth | ❌ | Not available (privacy restrictions) |
| Primary Fund Type | ✅ | Available as `organization.industry` (7/8 = 87.5%) |
| Investing Thesis | ❌ | Not available - investment data |
| Preferred Stages | ❌ | Not available - investment data |
| % Direct VC | ❌ | Not available - investment data |
| % Emerging Managers | ❌ | Not available - investment data |
| % Established Managers | ❌ | Not available - investment data |
| KF Class # | ⚠️ | Requires parsing `employment_history` for "Kauffman Fellows" |
| Category | ❌ | Not available - community data |
| Groups In | ❌ | Not available - community data |
| Mentees Are: | ❌ | Not available - community data |
| Mentors Class #: | ❌ | Not available - community data |
| Update Biography | ❌ | Not available - metadata |

**Legend:**
- ✅ = Available (>75% coverage)
- ⚠️ = Partially available or requires processing
- ❌ = Not available

### Detailed Coverage

| Field | Apollo Path | Coverage | Quality | Requires Processing |
|-------|-------------|----------|---------|---------------------|
| Name | `name`, `first_name`, `last_name` | 8/8 (100%) | High | No |
| Preferred First Name | `first_name` | 8/8 (100%) | High | No |
| Last Name | `last_name` | 8/8 (100%) | High | No |
| Email Address | `email` | 3/8 (37.5%) | Medium | No (but often locked) |
| Alternate Email | `email` | 3/8 (37.5%) | Low | No |
| LinkedIn URL | `linkedin_url` | 8/8 (100%) | High | No |
| Job Title | `title`, `headline` | 8/8 (100%) | High | No |
| Current Firm | `organization.name` | 7/8 (87.5%) | High | No |
| Country | `country` | 8/8 (100%) | High | No |
| State/Province | `state` | 8/8 (100%) | High | No |
| Primary Region Based In | `country`, `state` | 8/8 (100%) | Medium | Yes - derived |
| Professional Bio | `headline` | 8/8 (100%) | Low | No (only headline) |
| Headshot | `photo_url` | 8/8 (100%) | High | No |
| WhatsApp fields | `phone_numbers[]` | 1/8 (12.5%) | Low | Yes - parsing required |
| Primary Fund Type | `organization.industry` | 7/8 (87.5%) | High | No |
| KF Class # | `employment_history[]` | 8/8 (100%) | Medium | Yes - requires parsing |

## Additional Apollo Data (Not in Source)

Apollo provides additional valuable fields not present in the Airtable source:

| Apollo Field | Description | Value for Platform |
|-------------|-------------|-------------------|
| `employment_history` | Complete work history with dates, titles, organizations | High |
| `organization.total_funding` | Total funding raised by organization | High |
| `organization.founded_year` | Year organization was founded | Medium |
| `organization.estimated_num_employees` | Company size | Medium |
| `seniority` | Seniority level (c_suite, founder, etc.) | High |
| `email_status` | Email verification status | High |
| `twitter_url` | Twitter profile | Medium |
| `github_url` | GitHub profile | Low |
| `departments` | Department classification | Medium |
| `functions` | Functional area | Medium |
| `time_zone` | Time zone | Low |

## Apollo Coverage Summary

| Metric | Value |
|--------|-------|
| **Personal Data Fields** | 19 |
| **Fully Available (✅)** | 11 (58%) |
| **Partially Available (⚠️)** | 5 (26%) |
| **Not Available (❌)** | 3 (16%) |
| **Overall Personal Data Coverage** | ~84% |

---

## Crunchbase Coverage Analysis (Investment Data)

> ⏳ **Pending**: No Crunchbase samples available yet.

### Expected Coverage

Based on Crunchbase's known capabilities, expected coverage for investment-specific fields:

| Field | Expected Coverage | Notes |
|-------|-------------------|-------|
| Investing Thesis | ⚠️ | May be partial - Crunchbase focuses on company/fund data |
| Preferred Stages | ✅ | Typically available in investor profiles |
| % Direct VC | ❌ | Internal metrics - unlikely to be available |
| % Emerging Managers | ❌ | Internal metrics - unlikely to be available |
| % Established Managers | ❌ | Internal metrics - unlikely to be available |
| Portfolio Companies | ✅ | Core Crunchbase data |
| Fund Details | ✅ | Core Crunchbase data |
| Recent Investments | ✅ | Core Crunchbase data |

### Additional Crunchbase Data (Expected)

| Field | Description | Value for Platform |
|-------|-------------|-------------------|
| Portfolio companies | List of investments made | High |
| Investment rounds | Participation in funding rounds | High |
| Fund size | AUM and fund details | High |
| Co-investors | Frequent co-investment partners | Medium |
| Exit history | Portfolio exits and returns | High |
| Investment focus | Sectors and geographies | High |

> **Action Required**: Obtain Crunchbase sample data to complete this analysis.

---

## Community Data (Airtable + Kauffman Staff)

Community Data is **internal to Kauffman Fellows** and represents information that only the organization knows about its participants. This data cannot be obtained from any external enrichment tool.

### Ownership & Maintenance

| Aspect | Details |
|--------|---------|
| **Owner** | Kauffman Fellows Staff |
| **Storage** | Airtable |
| **Update Frequency** | As needed (event-driven) |
| **Automation** | Manual entry by staff |

### Community Data Fields

| Field | Description | Update Trigger |
|-------|-------------|----------------|
| **KF Class #** | Kauffman Fellows cohort (e.g., "Class 16", "Class 22") | On fellowship enrollment |
| **Category** | Role in community: Fellow, Mentor, Board Member, Alumni | On role assignment/change |
| **Groups In** | Community groups and SIGs (Special Interest Groups) | On group join/leave |
| **Mentees Are:** | List of assigned mentees | On mentorship pairing |
| **Mentors Class #:** | Mentor's cohort number | On mentorship pairing |

### Why Community Data Cannot Be Automated

1. **Internal Knowledge**: KF Class assignments are internal decisions made during the fellowship selection process
2. **Relationship Data**: Mentor-mentee pairings are curated by Kauffman Fellows staff
3. **Group Memberships**: SIG and community group participation is tracked internally
4. **Role Evolution**: Category changes (Fellow → Mentor → Board Member) are organizational decisions
5. **Privacy**: Some community relationships are confidential

### Staff Responsibilities

The Kauffman Staff is responsible for:

- ✅ Assigning new fellows to their cohort (KF Class #)
- ✅ Updating participant categories as roles evolve
- ✅ Managing group memberships
- ✅ Pairing mentors with mentees
- ✅ Maintaining accurate community relationship data

### Integration Notes

While KF Class # can sometimes be found in Apollo's `employment_history` (participants often list "Kauffman Fellows" as an organization), this data:
- May be outdated
- May not include the exact class number
- Should be verified against the authoritative Airtable source

**Recommendation**: Always use Airtable as the source of truth for Community Data.

---

## Combined Coverage Matrix

| Field | Category | Apollo | Crunchbase | Airtable + Staff | Primary Source |
|-------|----------|--------|------------|------------------|----------------|
| Name | Personal | ✅ | - | - | Apollo |
| Preferred First Name | Personal | ✅ | - | - | Apollo |
| Last Name | Personal | ✅ | - | - | Apollo |
| Email Address | Personal | ⚠️ | - | ✅ | Apollo + Airtable (backup) |
| LinkedIn URL | Personal | ✅ | - | - | Apollo |
| Job Title | Personal | ✅ | - | - | Apollo |
| Current Firm | Personal | ✅ | - | - | Apollo |
| Country | Personal | ✅ | - | - | Apollo |
| State/Province | Personal | ✅ | - | - | Apollo |
| Primary Region Based In | Personal | ✅ | - | - | Apollo |
| Professional Bio | Personal | ⚠️ | - | ✅ | Airtable (full bio) |
| Education/Personal Bio | Personal | ❌ | - | ✅ | Airtable |
| Headshot | Personal | ✅ | - | ✅ | Apollo (or Airtable) |
| Date of Birth | Personal | ❌ | - | ✅ | Airtable |
| WhatsApp fields | Personal | ⚠️ | - | ✅ | Airtable |
| Primary Fund Type | Investment | ✅ | ⏳ | - | Apollo / Crunchbase |
| Investing Thesis | Investment | ❌ | ⏳ | ✅ | Airtable |
| Preferred Stages | Investment | ❌ | ⏳ | ✅ | Crunchbase / Airtable |
| % Direct VC | Investment | ❌ | ❌ | ✅ | Airtable (internal) |
| % Emerging Managers | Investment | ❌ | ❌ | ✅ | Airtable (internal) |
| % Established Managers | Investment | ❌ | ❌ | ✅ | Airtable (internal) |
| KF Class # | Community | ⚠️ | - | ✅ | **Airtable (Staff)** |
| Category | Community | ❌ | - | ✅ | **Airtable (Staff)** |
| Groups In | Community | ❌ | - | ✅ | **Airtable (Staff)** |
| Mentees Are: | Community | ❌ | - | ✅ | **Airtable (Staff)** |
| Mentors Class #: | Community | ❌ | - | ✅ | **Airtable (Staff)** |

**Legend:**
- ✅ = Data source provides this field
- ⚠️ = Partial coverage or requires processing
- ❌ = Not available from this source
- ⏳ = Pending sample analysis
- `-` = Not applicable for this source
- **Primary Source**: Recommended data source for this field

---

## Recommendations

### 1. Hybrid Data Strategy

Use a combination of data sources based on category:

```
┌──────────────────────────────────────────────────────────────────────────┐
│                           DATA SOURCES                                   │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────────────┐  │
│  │     APOLLO      │  │   CRUNCHBASE    │  │  AIRTABLE + KF STAFF    │  │
│  │   (API - Auto)  │  │   (API - Auto)  │  │      (Manual)           │  │
│  ├─────────────────┤  ├─────────────────┤  ├─────────────────────────┤  │
│  │  Personal Data  │  │ Investment Data │  │    Community Data       │  │
│  │                 │  │                 │  │                         │  │
│  │  • Name         │  │  • Portfolio    │  │  • KF Class #           │  │
│  │  • Email        │  │  • Funding      │  │  • Category (Role)      │  │
│  │  • Title        │  │  • Stages       │  │  • Groups / SIGs        │  │
│  │  • Company      │  │  • Exits        │  │  • Mentees              │  │
│  │  • Location     │  │  • Co-investors │  │  • Mentors              │  │
│  │  • Photo        │  │                 │  │                         │  │
│  │  • LinkedIn     │  │                 │  │  + Personal (backup):   │  │
│  │  • Employment   │  │                 │  │  • Full Bios            │  │
│  │    History      │  │                 │  │  • DOB, WhatsApp        │  │
│  └─────────────────┘  └─────────────────┘  └─────────────────────────┘  │
│         ▲                    ▲                        ▲                 │
│         │                    │                        │                 │
│    Automated             Automated                 Manual               │
│    Enrichment            Enrichment              Maintenance            │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

### 2. Apollo Integration (Personal Data)

**Use Apollo for:**
- ✅ Identity verification (name, LinkedIn)
- ✅ Professional information (title, company, location)
- ✅ Profile photos
- ⚠️ Email (with fallback to Airtable for locked emails)
- ⚠️ Employment history enrichment

**Keep in Airtable:**
- Full professional and education bios
- Date of birth
- WhatsApp contact details (low Apollo coverage)

### 3. Crunchbase Integration (Investment Data - Pending)

**Expected use cases:**
- Portfolio company tracking
- Investment round participation
- Fund performance metrics
- Co-investor network mapping

**Action Required:** Obtain Crunchbase API samples to validate coverage.

### 4. Airtable + Kauffman Staff (Community Data)

**Staff-maintained fields (cannot be automated):**

| Field | Maintained By | Update Trigger |
|-------|---------------|----------------|
| KF Class # | Staff | Fellowship enrollment |
| Category | Staff | Role assignment/change |
| Groups In | Staff | Group membership changes |
| Mentees Are: | Staff | Mentorship pairing |
| Mentors Class #: | Staff | Mentorship pairing |

**Additional Airtable-maintained fields:**

| Category | Fields | Reason |
|----------|--------|--------|
| Community | All community fields | Internal organizational data |
| Investment (Internal) | % Direct VC, % Emerging/Established Managers | Confidential metrics |
| Personal (Backup) | Full bios, DOB, WhatsApp | Low API coverage or privacy |
| Metadata | Update Biography | Internal workflow |

### 5. Data Source Priority

When multiple sources have the same field, use this priority:

| Data Type | Priority Order |
|-----------|----------------|
| Personal Data | Apollo → Airtable (fallback) |
| Investment Data | Crunchbase → Airtable (fallback) |
| Community Data | Airtable only (source of truth) |

---

## Next Steps

1. **Crunchbase Samples**: Obtain and analyze Crunchbase API response samples for the same 8 participants
2. **API Cost Analysis**: Compare pricing for Apollo and Crunchbase APIs
3. **Update Frequency**: Determine optimal refresh rates for each data category
4. **Data Merge Strategy**: Define conflict resolution rules when enriched data differs from source

---

## Appendix

### Sample Files Reference

| Tool | File | Size | Description |
|------|------|------|-------------|
| Apollo | allen.json | 41KB | Full enrichment data |
| Apollo | andy.json | 20KB | Full enrichment data |
| Apollo | dina.json | 9.2KB | Full enrichment data |
| Apollo | eric.json | 5.6KB | Full enrichment data |
| Apollo | jason.json | 6.5KB | Full enrichment data |
| Apollo | jessica.json | 5.8KB | Full enrichment data |
| Apollo | liat.json | 6.8KB | Full enrichment data |
| Apollo | rodrigo.json | 6.4KB | Full enrichment data |
| Crunchbase | ⏳ | - | Pending |

### Analysis Methodology

1. Source fields extracted from `airtable/sample.csv` header (31 fields)
2. Apollo response structure analyzed from 8 JSON samples
3. Field mapping performed manually based on semantic meaning
4. Coverage calculated as: (records with data) / (total records)
5. Priority assigned based on importance for VC investment platform

---

*Analysis Date: December 2024*  
*Source: Airtable sample (8 records)*  
*Apollo Samples: 8 JSON files*  
*Crunchbase Samples: Pending*
