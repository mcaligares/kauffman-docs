# Signal API - Data Dictionary

This document describes the response structure of Signal's GraphQL API endpoints.

---

## 1. Autocomplete

**Endpoint:** `POST https://signal-api.nfx.com/graphql`

**Description:** Searches for investors and firms by name. Useful for implementing autocomplete searches and obtaining investor IDs for subsequent queries.

### Response Structure

```json
{
  "data": {
    "investors": { ... },
    "firms": { ... }
  }
}
```

### Fields

| Field | Type | Description |
|-------|------|-------------|
| `data` | Object | Main response container |
| `data.investors` | InvestorProfileConnection | Connection with matching investors |
| `data.firms` | AuthorizedFirmConnection | Connection with matching firms |

### InvestorProfileConnection

| Field | Type | Description |
|-------|------|-------------|
| `edges` | Array\<InvestorProfileEdge\> | List of investor results |
| `__typename` | String | `"InvestorProfileConnection"` |

### InvestorProfileEdge

| Field | Type | Description |
|-------|------|-------------|
| `node` | InvestorProfile | Investor data |
| `__typename` | String | `"InvestorProfileEdge"` |

### InvestorProfile (autocomplete)

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique investor profile ID |
| `person` | Person | Investor's personal data |
| `firm` | Firm | Investor's current firm |
| `image_urls` | Array\<String\> | Profile image URLs |
| `__typename` | String | `"InvestorProfile"` |

### Person (autocomplete)

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique person ID |
| `slug` | String | URL slug (e.g., `"allen-taylor"`) |
| `name` | String | Full name |
| `first_name` | String | First name |
| `last_name` | String | Last name |
| `__typename` | String | `"Person"` |

### Firm (autocomplete)

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique firm ID |
| `name` | String | Firm name |
| `__typename` | String | `"Firm"` |

### AuthorizedFirmConnection

| Field | Type | Description |
|-------|------|-------------|
| `edges` | Array\<FirmEdge\> | List of matching firms |
| `__typename` | String | `"AuthorizedFirmConnection"` |

---

## 2. Investor Profile

**Endpoint:** `POST https://signal-api.nfx.com/graphql`

**Description:** Retrieves the complete investor profile, including personal information, investment history, education, work experience, and network connections.

### Response Structure

```json
{
  "data": {
    "investor_profile": { ... }
  }
}
```

### InvestorProfile (full)

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique profile ID |
| `claimed` | Boolean | Indicates if the profile has been claimed by the investor |
| `can_edit` | Boolean | Indicates if the current user can edit the profile |
| `include_in_list` | Boolean | Indicates if included in listings |
| `in_founder_investor_list` | Boolean | Is in founder-investor list |
| `in_diverse_investor_list` | Boolean | Is in diverse investor list |
| `in_female_investor_list` | Boolean | Is in female investor list |
| `in_invests_in_diverse_founders_investor_list` | Boolean | Invests in diverse founders |
| `in_invests_in_female_founders_investor_list` | Boolean | Invests in female founders |
| `leads_rounds` | Boolean \| null | Indicates if leads investment rounds |
| `person` | Person | Full personal data |
| `stages` | Array\<Tag\> | Preferred investment stages |
| `position` | String | Position at firm (e.g., `"managing_partner"`) |
| `min_investment` | String | Minimum investment in USD |
| `max_investment` | String | Maximum investment in USD |
| `target_investment` | String | Target investment in USD |
| `areas_of_interest_freeform` | String | Areas of interest in free text |
| `no_current_interest_freeform` | String | Areas with no current interest |
| `vote_count` | Integer | Number of votes/endorsements |
| `headline` | String | Professional headline/title |
| `previous_position` | String \| null | Previous position |
| `previous_firm` | String \| null | Previous firm |
| `location` | Tag | Investor's location |
| `firm` | Firm | Current firm |
| `degrees` | Array\<InvestorProfileDegree\> | Education history |
| `positions` | Array\<InvestorProfilePosition\> | Work history |
| `investments_on_record` | InvestorProfileInvestmentsOnRecordConnection | Investments made |
| `investor_profile_funding_rounds` | InvestorProfileFundingRoundConnection | Funding rounds participated in |
| `network_list_investor_profiles` | InvestorProfileTypeConnection | Co-investors |
| `investing_connections` | PeopleRelationshipConnection | Investment connections |
| `__typename` | String | `"InvestorProfile"` |

### Person (full)

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique person ID |
| `slug` | String | URL slug |
| `first_name` | String | First name |
| `last_name` | String | Last name |
| `name` | String | Full name |
| `linkedin_url` | String \| null | LinkedIn URL |
| `facebook_url` | String \| null | Facebook URL |
| `twitter_url` | String \| null | Twitter URL |
| `crunchbase_url` | String \| null | Crunchbase URL |
| `angellist_url` | String \| null | AngelList URL |
| `roles` | Array\<String\> | Roles (e.g., `["VC", "Investor"]`) |
| `url` | String \| null | Personal or company URL |
| `is_me` | Boolean | Indicates if authenticated user |
| `first_degree_count` | Integer | Number of first-degree connections |
| `is_on_target_list` | Boolean | Is on user's target list |
| `relationship_strength` | String \| null | Relationship strength |
| `email_from_my_contacts_list` | String \| null | Email from contacts list |
| `__typename` | String | `"Person"` |

### Firm (full)

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique firm ID |
| `name` | String | Firm name |
| `slug` | String | URL slug |
| `current_fund_size` | String | Current fund size (e.g., `"$292M"`) |
| `__typename` | String | `"Firm"` |

### Tag

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique tag ID |
| `kind` | String | Tag type (e.g., `"stage"`) |
| `display_name` | String | Display name (e.g., `"Series A"`, `"Seed"`) |
| `__typename` | String | `"Tag"` |

### InvestorProfileDegree

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique degree ID |
| `name` | String | Degree name |
| `field_of_study` | String | Field of study |
| `school` | Tag | Educational institution |
| `__typename` | String | `"InvestorProfileDegree"` |

### InvestorProfilePosition

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique position ID |
| `title` | String | Job title |
| `company` | Tag | Company |
| `start_date` | MonthYear | Start date |
| `end_date` | MonthYear \| null | End date (null if current) |
| `__typename` | String | `"InvestorProfilePosition"` |

### MonthYear

| Field | Type | Description |
|-------|------|-------------|
| `month` | String | Month (1-12) |
| `year` | String | Year |
| `__typename` | String | `"MonthYear"` |

### InvestorProfileInvestmentsOnRecordConnection

| Field | Type | Description |
|-------|------|-------------|
| `pageInfo` | PageInfo | Pagination info |
| `record_count` | Integer | Total investments |
| `edges` | Array\<InvestorProfileInvestmentsOnRecordEdge\> | List of investments |
| `__typename` | String | `"InvestorProfileInvestmentsOnRecordConnection"` |

### InvestorProfileInvestmentsOnRecord

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique investment ID |
| `company_display_name` | String | Company name |
| `total_raised` | Array\<String\> | Total raised by the company |
| `coinvestor_names` | Array\<String\> | Co-investor names |
| `investor_profile_funding_rounds` | Array\<InvestorProfileFundingRound\> | Rounds participated in |
| `__typename` | String | `"InvestorProfileInvestmentsOnRecord"` |

### InvestorProfileFundingRoundConnection

| Field | Type | Description |
|-------|------|-------------|
| `pageInfo` | PageInfo | Pagination info |
| `record_count` | Integer | Total rounds |
| `edges` | Array\<InvestorProfileFundingRoundEdge\> | List of rounds |
| `__typename` | String | `"InvestorProfileFundingRoundConnection"` |

### InvestorProfileFundingRound

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique ID |
| `is_lead` | Boolean \| null | Indicates if was lead investor |
| `board_role` | String \| null | Board role |
| `funding_round` | FundingRound | Round data |
| `__typename` | String | `"InvestorProfileFundingRound"` |

### FundingRound

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique round ID |
| `stage` | String | Stage (e.g., `"Seed"`, `"Series A"`, `"Series B"`) |
| `date` | String | Date in ISO 8601 format |
| `amount` | String | Round amount (e.g., `"$100M"`) |
| `company` | Tag | Company (only in some responses) |
| `__typename` | String | `"FundingRound"` |

### InvestorProfileTypeConnection (Co-investors)

| Field | Type | Description |
|-------|------|-------------|
| `list_type` | String | List type (e.g., `"COINVESTORS"`) |
| `edges` | Array\<InvestorProfileTypeEdge\> | List of co-investors |
| `__typename` | String | `"InvestorProfileTypeConnection"` |

### InvestorProfileTypeEdge

| Field | Type | Description |
|-------|------|-------------|
| `node` | InvestorProfile | Co-investor profile |
| `__typename` | String | `"InvestorProfileTypeEdge"` |

### PeopleRelationshipConnection

| Field | Type | Description |
|-------|------|-------------|
| `record_count` | Integer | Total connections |
| `edges` | Array\<PeopleRelationshipEdge\> | List of connections |
| `__typename` | String | `"PeopleRelationshipConnection"` |

### PeopleRelationship

| Field | Type | Description |
|-------|------|-------------|
| `id` | String | Unique relationship ID |
| `target_person` | Person | Connected person |
| `__typename` | String | `"PeopleRelationship"` |

### PageInfo

| Field | Type | Description |
|-------|------|-------------|
| `hasNextPage` | Boolean | Indicates if there are more pages |
| `__typename` | String | `"PageInfo"` |

---

*Generated from examples in `signal/examples/`*
