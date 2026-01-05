# Apollo vs Airtable Source Data Comparison

## Overview
This document compares the fields available in the Airtable source CSV with data that can be obtained from Apollo API for investment platform users.

---

## 1. Simple Comparison Table

| Field | Apollo | Comments |
|-------|--------|----------|
| Name | ✅ | Available as `name`, `first_name`, `last_name` |
| Preferred First Name | ✅ | Available as `first_name` |
| Last Name | ✅ | Available as `last_name` |
| Email Address | ⚠️ | Available as `email` but often locked (`email_not_unlocked@domain.com`) - only 3/8 records have real emails |
| Alternate Email | ⚠️ | Same as Email Address - Apollo doesn't distinguish alternate emails |
| LinkedIn URL | ✅ | Available as `linkedin_url` |
| Job Title | ✅ | Available as `title` or `headline` |
| Current Firm | ✅ | Available as `organization.name` (7/8 records) |
| Primary Fund Type (from Current Firm) | ✅ | Available as `organization.industry` (7/8 records) |
| Country | ✅ | Available as `country` |
| State/Province | ✅ | Available as `state` or `organization.state` |
| Primary Region Based In | ✅ | Can be derived from `country` and `state` |
| Professional Bio | ⚠️ | Partial match - only `headline` available, not full bio |
| Education/Personal Bio | ❌ | Not available in Apollo |
| Biography URL | ❌ | Not available in Apollo |
| Headshot | ✅ | Available as `photo_url` |
| WhatsApp Country Code | ⚠️ | Available in `phone_numbers` but only 1/8 records have phone data |
| WhatsApp Phone (exc. Country Code) | ⚠️ | Available in `phone_numbers` but only 1/8 records have phone data |
| WhatsApp # | ⚠️ | Available in `phone_numbers` but only 1/8 records have phone data |
| Date of Birth | ❌ | Not available in Apollo |
| % Direct VC | ❌ | Not available in Apollo |
| % Emerging Managers | ❌ | Not available in Apollo |
| % Established Managers | ❌ | Not available in Apollo |
| Investing Thesis | ❌ | Not available in Apollo |
| Preferred Stages | ❌ | Not available in Apollo |
| Groups In | ❌ | Not available in Apollo |
| Mentees Are: | ❌ | Not available in Apollo |
| Mentors Class #: | ❌ | Not available in Apollo |
| KF Class # | ⚠️ | Can be found in `employment_history` but requires parsing |
| Category | ❌ | Not available in Apollo |
| Update Biography | ❌ | Not available in Apollo |

**Legend:**
- ✅ = Available in Apollo
- ⚠️ = Partially available or requires additional processing
- ❌ = Not available in Apollo

---

## 2. Detailed Comparison Table

| Field | Source Type | Apollo Available | Apollo Field Path | Coverage | Data Quality | Calculated/Derived | Notes |
|-------|-------------|------------------|-------------------|----------|--------------|-------------------|-------|
| **Identity & Contact** |
| Name | Direct | ✅ | `name`, `first_name`, `last_name` | 8/8 (100%) | High | No | Full name available |
| Preferred First Name | Direct | ✅ | `first_name` | 8/8 (100%) | High | No | Direct match |
| Last Name | Direct | ✅ | `last_name` | 8/8 (100%) | High | No | Direct match |
| Email Address | Direct | ⚠️ | `email` | 3/8 (37.5%) | Medium | No | Many emails are locked/unavailable |
| Alternate Email | Direct | ⚠️ | `email` | 3/8 (37.5%) | Low | No | Apollo doesn't distinguish alternate emails |
| LinkedIn URL | Direct | ✅ | `linkedin_url` | 8/8 (100%) | High | No | Direct match |
| **Professional Information** |
| Job Title | Direct | ✅ | `title`, `headline` | 8/8 (100%) | High | No | Available in title field |
| Current Firm | Direct | ✅ | `organization.name`, `account.name` | 7/8 (87.5%) | High | No | Most records have organization data |
| Primary Fund Type | Direct | ✅ | `organization.industry` | 7/8 (87.5%) | High | No | Industry classification available |
| Professional Bio | Direct | ⚠️ | `headline` | 8/8 (100%) | Low | No | Only headline available, not full bio |
| Education/Personal Bio | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| Biography URL | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| **Location** |
| Country | Direct | ✅ | `country`, `organization.country` | 8/8 (100%) | High | No | Direct match |
| State/Province | Direct | ✅ | `state`, `organization.state` | 8/8 (100%) | High | No | Direct match |
| Primary Region Based In | Direct | ✅ | `country`, `state` | 8/8 (100%) | Medium | Yes | Can be derived from country/state |
| **Investment-Specific** |
| Investing Thesis | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| Preferred Stages | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| % Direct VC | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| % Emerging Managers | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| % Established Managers | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| **Community & Relationships** |
| KF Class # | Direct | ⚠️ | `employment_history[].organization_name` | 8/8 (100%) | Medium | Yes | Requires parsing employment history for "Kauffman Fellows" |
| Category | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| Groups In | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| Mentees Are: | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| Mentors Class #: | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| **Contact Methods** |
| WhatsApp Country Code | Direct | ⚠️ | `phone_numbers[].raw_number` | 1/8 (12.5%) | Low | Yes | Requires parsing phone number format |
| WhatsApp Phone | Direct | ⚠️ | `phone_numbers[].raw_number` | 1/8 (12.5%) | Low | Yes | Requires parsing phone number format |
| WhatsApp # | Direct | ⚠️ | `phone_numbers[].raw_number` | 1/8 (12.5%) | Low | Yes | Requires parsing phone number format |
| **Media & Assets** |
| Headshot | Direct | ✅ | `photo_url` | 8/8 (100%) | High | No | Direct match |
| **Metadata** |
| Date of Birth | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |
| Update Biography | Direct | ❌ | N/A | 0/8 (0%) | N/A | No | Not available in Apollo |

### Additional Apollo Fields Not in Source

Apollo provides several fields that are not in the Airtable source:

| Apollo Field | Description | Potential Value |
|-------------|-------------|-----------------|
| `employment_history` | Complete work history with dates, titles, organizations | High - Can extract career progression |
| `organization.founded_year` | Year organization was founded | Medium - Useful for context |
| `organization.total_funding` | Total funding raised by organization | High - Investment context |
| `organization.estimated_num_employees` | Company size | Medium - Scale indicator |
| `organization.technologies` | Tech stack used by organization | Low - Technical context |
| `twitter_url` | Twitter profile | Medium - Additional contact/social |
| `github_url` | GitHub profile | Low - Technical indicator |
| `facebook_url` | Facebook profile | Low - Social presence |
| `formatted_address` | Complete formatted address | Medium - Location data |
| `time_zone` | Time zone | Low - Contact timing |
| `email_status` | Email verification status | High - Data quality indicator |
| `departments` | Department classification | Medium - Role categorization |
| `functions` | Functional area | Medium - Role categorization |
| `seniority` | Seniority level (c_suite, founder, etc.) | High - Investment relevance |
| `intent_strength` | Buying intent signal | Medium - Engagement indicator |

---

## 3. Field Prioritization for Investment Platform

Fields are prioritized based on their importance for an investment platform where users are investors, VCs, and fund managers.

### Critical Priority
**Essential for user identification, authentication, and basic contact**

1. **Name** (✅ Apollo)
   - Required for user identification
   - Apollo provides: `name`, `first_name`, `last_name`

2. **Preferred First Name** (✅ Apollo)
   - Personalization and communication
   - Apollo provides: `first_name`

3. **Last Name** (✅ Apollo)
   - User identification and formal communication
   - Apollo provides: `last_name`

4. **Email Address** (⚠️ Apollo - Partial)
   - Critical for authentication and communication
   - **Issue**: Only 37.5% coverage, many emails are locked
   - **Recommendation**: Apollo email enrichment may require additional credits/unlocking

5. **LinkedIn URL** (✅ Apollo)
   - Professional verification and networking
   - Apollo provides: `linkedin_url`

### High Priority
**Important for investment platform functionality and user matching**

6. **Job Title** (✅ Apollo)
   - Role identification and filtering
   - Apollo provides: `title`, `headline`

7. **Current Firm** (✅ Apollo)
   - Organization context and credibility
   - Apollo provides: `organization.name` (87.5% coverage)

8. **Primary Fund Type** (✅ Apollo)
   - Investment focus classification
   - Apollo provides: `organization.industry` (87.5% coverage)

9. **Country** (✅ Apollo)
   - Geographic filtering and regional matching
   - Apollo provides: `country`

10. **State/Province** (✅ Apollo)
    - Regional filtering within countries
    - Apollo provides: `state`

11. **Primary Region Based In** (✅ Apollo - Derived)
    - Regional classification for investment focus
    - Can be derived from `country` and `state`

12. **Professional Bio** (⚠️ Apollo - Partial)
    - User credibility and background
    - **Issue**: Only `headline` available, not full bio
    - **Recommendation**: Use headline as summary, full bio not available

13. **Investing Thesis** (❌ Apollo)
    - Investment focus and strategy
    - **Not available in Apollo** - Must be collected separately

14. **Preferred Stages** (❌ Apollo)
    - Investment stage preferences
    - **Not available in Apollo** - Must be collected separately

### Medium Priority
**Useful for enhanced functionality and user experience**

15. **KF Class #** (⚠️ Apollo - Requires Parsing)
    - Community affiliation and networking
    - Available in `employment_history` but requires parsing

16. **Category** (❌ Apollo)
    - User classification (Fellow, Mentor, Board Member)
    - **Not available in Apollo** - Must be collected separately

17. **Alternate Email** (⚠️ Apollo - Same as Primary)
    - Backup contact method
    - Apollo doesn't distinguish alternate emails

18. **Biography URL** (❌ Apollo)
    - External profile link
    - **Not available in Apollo** - Must be collected separately

19. **Groups In** (❌ Apollo)
    - Community groups and affiliations
    - **Not available in Apollo** - Must be collected separately

20. **Mentees Are:** (❌ Apollo)
    - Mentorship relationships
    - **Not available in Apollo** - Must be collected separately

21. **Mentors Class #:** (❌ Apollo)
    - Mentor relationships
    - **Not available in Apollo** - Must be collected separately

22. **Education/Personal Bio** (❌ Apollo)
    - Personal background and education
    - **Not available in Apollo** - Must be collected separately

### Low Priority
**Nice to have but not essential**

23. **Headshot** (✅ Apollo)
    - Profile photo for visual identification
    - Apollo provides: `photo_url`

24. **WhatsApp Country Code** (⚠️ Apollo - Low Coverage)
    - Alternative contact method
    - Only 12.5% coverage in Apollo

25. **WhatsApp Phone** (⚠️ Apollo - Low Coverage)
    - Alternative contact method
    - Only 12.5% coverage in Apollo

26. **WhatsApp #** (⚠️ Apollo - Low Coverage)
    - Alternative contact method
    - Only 12.5% coverage in Apollo

27. **Date of Birth** (❌ Apollo)
    - Personal information
    - **Not available in Apollo** - Privacy/legal restrictions

28. **% Direct VC** (❌ Apollo)
    - Investment allocation metrics
    - **Not available in Apollo** - Internal metrics

29. **% Emerging Managers** (❌ Apollo)
    - Investment allocation metrics
    - **Not available in Apollo** - Internal metrics

30. **% Established Managers** (❌ Apollo)
    - Investment allocation metrics
    - **Not available in Apollo** - Internal metrics

31. **Update Biography** (❌ Apollo)
    - Metadata field
    - **Not available in Apollo** - Internal workflow field

---

## 4. Summary & Recommendations

### Data Coverage Summary

- **✅ Fully Available (11 fields)**: Name, Preferred First Name, Last Name, LinkedIn URL, Job Title, Current Firm, Primary Fund Type, Country, State/Province, Primary Region Based In, Headshot
- **⚠️ Partially Available (6 fields)**: Email Address, Alternate Email, Professional Bio, WhatsApp fields, KF Class #
- **❌ Not Available (14 fields)**: Investment-specific fields, community fields, personal bio, metadata fields

### Key Findings

1. **Core Identity Data**: Apollo excels at providing basic identity and professional information (name, title, company, location)

2. **Email Limitation**: Only 37.5% of records have accessible emails. This is a critical limitation for an investment platform that requires email communication.

3. **Investment-Specific Data Gap**: Apollo does not provide investment-specific information such as:
   - Investing thesis
   - Preferred investment stages
   - Investment allocation percentages
   - These are critical for an investment platform and must be collected separately

4. **Community Data Gap**: Apollo does not provide community-specific information:
   - Kauffman Fellows class numbers (though can be parsed from employment history)
   - Category (Fellow, Mentor, Board Member)
   - Groups and affiliations
   - Mentorship relationships

5. **Rich Employment History**: Apollo provides detailed employment history that could be valuable for understanding career progression and background.

### Recommendations

1. **Hybrid Approach**: Use Apollo for basic identity and professional data, but maintain Airtable source for:
   - Investment-specific fields (thesis, stages, allocations)
   - Community-specific fields (KF Class, Category, Groups, Mentorship)
   - Full professional and education bios

2. **Email Strategy**: 
   - Apollo email enrichment may require additional API credits
   - Consider using Apollo for email discovery but verify with source data
   - Implement fallback to source email addresses

3. **Data Enrichment**: Use Apollo's additional fields (employment_history, organization details, seniority) to enhance user profiles beyond what's in the source.

4. **KF Class Parsing**: Implement logic to extract KF Class numbers from Apollo's `employment_history` by searching for "Kauffman Fellows" entries.

5. **Phone Number Parsing**: If WhatsApp contact is important, implement parsing logic for Apollo's `phone_numbers` field, though coverage is low (12.5%).

---

## 5. Apollo API Integration Considerations

### Fields Requiring Additional Processing

1. **KF Class #**: Parse `employment_history` array for entries containing "Kauffman Fellows"
2. **Primary Region Based In**: Derive from `country` and `state` fields
3. **Phone Numbers**: Parse `phone_numbers` array to extract country code and number
4. **Email Unlocking**: May require additional API calls or credits to unlock email addresses

### Recommended Integration Strategy

1. **Primary Data Source**: Use Airtable CSV as primary source for investment and community-specific data
2. **Apollo Enrichment**: Use Apollo to:
   - Verify and update basic identity information
   - Enrich with employment history
   - Add organization details (funding, size, technologies)
   - Provide additional contact methods (Twitter, GitHub)
   - Add seniority and department classifications
3. **Data Merge**: Combine both sources, prioritizing Airtable for investment-specific fields and Apollo for professional enrichment

---

*Analysis Date: 2025-01-27*  
*Source: Airtable sample.csv (8 records)*  
*Apollo Samples: 8 JSON files (allen, andy, dina, eric, jason, jessica, liat, rodrigo)*

