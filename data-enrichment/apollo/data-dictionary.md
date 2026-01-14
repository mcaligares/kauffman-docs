# People, Account & Organization – Data Dictionary

This document defines the structure and meaning of the enriched person, account, and organization fields returned by the API.

---

## 1. Person

Represents an individual enriched with personal, professional, and contact information.

### Core Fields

| Field | Type | Description |
|------|------|-------------|
| `id` | String | Unique person identifier |
| `first_name` | String | Person’s first name |
| `last_name` | String | Person’s last name |
| `name` | String | Full name |
| `headline` | String | Professional headline |
| `title` | String | Current job title |
| `seniority` | String | Seniority level |
| `functions` | Array<String> | Functional roles |
| `departments` | Array<String> | Associated departments |
| `subdepartments` | Array<String> | Sub-departments |
| `intent_strength` | String | Detected intent level |
| `show_intent` | Boolean | Indicates intent availability |
| `enrichedAt` | DateTime | Enrichment timestamp |

---

### Contact Information

| Field | Type | Description |
|------|------|-------------|
| `email` | String | Primary email address |
| `email_status` | String | Email validation status |
| `email_domain_catchall` | Boolean | Catch-all domain flag |
| `personal_emails` | Array<String> | Additional emails |
| `extrapolated_email_confidence` | Number | Confidence score |
| `phone_numbers` | Array<PhoneNumber> | Associated phone numbers |
| `time_zone` | String | Time zone |

#### PhoneNumber

| Field | Type | Description |
|------|------|-------------|
| `raw_number` | String | Original phone number |
| `sanitized_number` | String | Normalized phone number |
| `type` | String | Phone type |
| `status` | String | Validation status |
| `position` | Number | Priority order |
| `dnc_status` | Boolean | Do-not-call flag |
| `dnc_other_info` | String | Additional DNC info |

---

### Location

| Field | Type | Description |
|------|------|-------------|
| `city` | String | City |
| `state` | String | State or region |
| `country` | String | Country |
| `postal_code` | String | Postal code |
| `street_address` | String | Street address |
| `formatted_address` | String | Full formatted address |

---

### Social Profiles

| Field | Type | Description |
|------|------|-------------|
| `linkedin_url` | String | LinkedIn profile |
| `facebook_url` | String | Facebook profile |
| `twitter_url` | String | Twitter/X profile |
| `github_url` | String | GitHub profile |
| `photo_url` | String | Profile photo |

---

### Employment History

| Field | Type | Description |
|------|------|-------------|
| `employment_history` | Array<EmploymentHistory> | Employment and education records |

#### EmploymentHistory

| Field | Type | Description |
|------|------|-------------|
| `id` | String | Record ID |
| `_id` | String | Internal identifier |
| `key` | String | Deduplication key |
| `kind` | String | Record type |
| `current` | Boolean | Current position |
| `title` | String | Role or degree |
| `organization_id` | String | Organization ID |
| `organization_name` | String | Organization name |
| `start_date` | Date | Start date |
| `end_date` | Date | End date |
| `description` | String | Description |
| `degree` | String | Degree obtained |
| `major` | String | Field of study |
| `grade_level` | String | Academic level |
| `emails` | Array<String> | Associated emails |
| `raw_address` | String | Raw address |
| `org_matched_by_name` | Boolean | Name-based match |
| `created_at` | DateTime | Created timestamp |
| `updated_at` | DateTime | Updated timestamp |

---

## 2. Account

Represents the CRM-level account associated with a person.

### Core Fields

| Field | Type | Description |
|------|------|-------------|
| `account.id` | String | Account ID |
| `account.name` | String | Account name |
| `account.domain` | String | Primary domain |
| `account.primary_domain` | String | Canonical domain |
| `account.website_url` | String | Website URL |
| `account.logo_url` | String | Logo URL |
| `account.existence_level` | String | Company maturity |
| `account.modality` | String | Business modality |
| `account.founded_year` | Number | Founded year |
| `account.created_at` | DateTime | Creation date |
| `account.organization_id` | String | Linked organization |

---

### CRM & Ownership

| Field | Type | Description |
|------|------|-------------|
| `account.owner_id` | String | Internal owner |
| `account.crm_owner_id` | String | CRM owner |
| `account.creator_id` | String | Creator ID |
| `account.team_id` | String | Team ID |
| `account.salesforce_id` | String | Salesforce ID |
| `account.hubspot_id` | String | HubSpot ID |
| `account.crm_record_url` | String | CRM record URL |
| `account.account_stage_id` | String | Pipeline stage |
| `account.account_playbook_statuses` | Array<String> | Playbook statuses |

---

### Account Contact & Location

| Field | Type | Description |
|------|------|-------------|
| `account.phone` | String | Main phone |
| `account.primary_phone` | Object | Primary phone object |
| `account.sanitized_phone` | String | Normalized phone |
| `account.phone_status` | String | Phone status |
| `account.city` | String | City |
| `account.state` | String | State |
| `account.country` | String | Country |
| `account.postal_code` | String | Postal code |
| `account.street_address` | String | Street address |
| `account.raw_address` | String | Raw address |

---

### Online Presence

| Field | Type | Description |
|------|------|-------------|
| `account.linkedin_url` | String | LinkedIn page |
| `account.facebook_url` | String | Facebook page |
| `account.twitter_url` | String | Twitter/X |
| `account.blog_url` | String | Blog URL |
| `account.crunchbase_url` | String | Crunchbase |
| `account.angellist_url` | String | AngelList |
| `account.alexa_ranking` | Number | Alexa rank |

---

## 3. Organization

Canonical organization entity shared across accounts.

### Core Fields

| Field | Type | Description |
|------|------|-------------|
| `organization.id` | String | Organization ID |
| `organization.name` | String | Organization name |
| `organization.primary_domain` | String | Primary domain |
| `organization.website_url` | String | Website |
| `organization.logo_url` | String | Logo |
| `organization.founded_year` | Number | Founded year |
| `organization.estimated_num_employees` | Number | Estimated headcount |
| `organization.num_suborganizations` | Number | Subsidiaries count |

---

### Industry & Classification

| Field | Type | Description |
|------|------|-------------|
| `organization.industry` | String | Primary industry |
| `organization.industries` | Array<String> | Industries |
| `organization.secondary_industries` | Array<String> | Secondary industries |
| `organization.industry_tag_id` | String | Industry tag ID |
| `organization.industry_tag_hash` | Object | Industry tag map |
| `organization.naics_codes` | Array<String> | NAICS codes |
| `organization.sic_codes` | Array<String> | SIC codes |
| `organization.keywords` | Array<String> | Keywords |

---

### Growth & Metrics

| Field | Type | Description |
|------|------|-------------|
| `organization.employee_metrics` | Object | Employee metrics |
| `organization.organization_headcount_six_month_growth` | Number | 6-month growth |
| `organization.organization_headcount_twelve_month_growth` | Number | 12-month growth |
| `organization.organization_headcount_twenty_four_month_growth` | Number | 24-month growth |

---

### Funding & Revenue

| Field | Type | Description |
|------|------|-------------|
| `organization.annual_revenue` | Number | Annual revenue |
| `organization.annual_revenue_printed` | String | Revenue (formatted) |
| `organization.organization_revenue` | Number | Internal estimate |
| `organization.organization_revenue_printed` | String | Revenue (formatted) |
| `organization.total_funding` | Number | Total funding |
| `organization.total_funding_printed` | String | Funding (formatted) |
| `organization.latest_funding_stage` | String | Latest stage |
| `organization.latest_funding_round_date` | Date | Latest funding date |

---

### Funding Events

| Field | Type | Description |
|------|------|-------------|
| `organization.funding_events` | Array<FundingEvent> | Funding history |

#### FundingEvent

| Field | Type | Description |
|------|------|-------------|
| `id` | String | Event ID |
| `type` | String | Round type |
| `amount` | Number | Amount raised |
| `currency` | String | Currency |
| `date` | Date | Funding date |
| `investors` | Array<String> | Investors |
| `news_url` | String | Related news |

---

*Generated from examples in `apollo/examples/`*
