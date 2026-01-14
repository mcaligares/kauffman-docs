# API Data Dictionary

This document describes all entities and fields available in the API, generated from the complete CSV data dictionary.

---

## ACQUISITIONS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Unique acquisition identifier | Fundamentals - Core Firmographics |
| `acquired_organization_identifier` | Identifier of the acquired organization | Fundamentals - Core Firmographics |
| `acquiring_organization_identifier` | Identifier of the acquiring organization | Fundamentals - Core Firmographics |
| `announced_on` | Date the acquisition was announced | Fundamentals - Core Firmographics |
| `price` | Acquisition price | Fundamentals - Core Firmographics |
| `created_at` | Date record was created | Fundamentals - Core Firmographics |
| `updated_at` | Date record was last updated | Fundamentals - Core Firmographics |

---

## ADDRESSES

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | ID used to identify the Address | Fundamentals - Core Firmographics |
| `identifier` | ID used to identify the Address | Fundamentals - Core Firmographics |
| `country_code` | Country code of the address | Fundamentals - Core Firmographics |
| `created_at` | Date address was created | Fundamentals - Core Firmographics |
| `headquartered_organization_identifier` | HQ address of event | Fundamentals - Core Firmographics |
| `is_headquarters` | Indicates whether this address is headquarters | Fundamentals - Core Firmographics |
| `latitude` | Latitude of the address | Fundamentals - Core Firmographics |
| `longitude` | Longitude of the address | Fundamentals - Core Firmographics |
| `postal_code` | Postal code | Fundamentals - Core Firmographics |
| `region` | Region or state | Fundamentals - Core Firmographics |
| `street_1` | Street 1 of address | Fundamentals - Core Firmographics |
| `street_2` | Street 2 of address | Fundamentals - Core Firmographics |
| `type` | Address type | Fundamentals - Core Firmographics |
| `updated_at` | Date address was last updated | Fundamentals - Core Firmographics |

---

## CATEGORIES

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Category identifier | Fundamentals - Taxonomies |
| `name` | Category name | Fundamentals - Taxonomies |
| `category_group_identifier` | Parent category group | Fundamentals - Taxonomies |
| `created_at` | Creation date | Fundamentals - Taxonomies |
| `updated_at` | Last update date | Fundamentals - Taxonomies |

---

## CATEGORY_GROUPS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Category group identifier | Fundamentals - Taxonomies |
| `name` | Category group name | Fundamentals - Taxonomies |
| `created_at` | Creation date | Fundamentals - Taxonomies |
| `updated_at` | Last update date | Fundamentals - Taxonomies |

---

## DEGREES

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Degree identifier | Fundamentals - People |
| `name` | Degree name | Fundamentals - People |
| `created_at` | Creation date | Fundamentals - People |
| `updated_at` | Update date | Fundamentals - People |

---

## DIVERSITY_SPOTLIGHTS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Diversity spotlight identifier | Fundamentals - People |
| `person_identifier` | Related person | Fundamentals - People |
| `category` | Diversity category | Fundamentals - People |
| `created_at` | Creation date | Fundamentals - People |

---

## EVENTS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Event identifier | Fundamentals - Events |
| `name` | Event name | Fundamentals - Events |
| `type` | Event type | Fundamentals - Events |
| `starts_on` | Event start date | Fundamentals - Events |
| `ends_on` | Event end date | Fundamentals - Events |
| `location_identifier` | Event location | Fundamentals - Events |
| `created_at` | Creation date | Fundamentals - Events |
| `updated_at` | Update date | Fundamentals - Events |

---

## EVENT_APPEARANCES

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Event appearance identifier | Fundamentals - Events |
| `event_identifier` | Related event | Fundamentals - Events |
| `person_identifier` | Appearing person | Fundamentals - Events |
| `organization_identifier` | Related organization | Fundamentals - Events |
| `created_at` | Creation date | Fundamentals - Events |

---

## FIELD_VALUES_LABELS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Field value label identifier | Fundamentals - Metadata |
| `field_name` | Field name | Fundamentals - Metadata |
| `value` | Field value | Fundamentals - Metadata |
| `label` | Human-readable label | Fundamentals - Metadata |

---

## FUNDING_ROUNDS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Funding round identifier | Fundamentals - Funding |
| `organization_identifier` | Funded organization | Fundamentals - Funding |
| `announced_on` | Announcement date | Fundamentals - Funding |
| `investment_type` | Round type | Fundamentals - Funding |
| `money_raised` | Amount raised | Fundamentals - Funding |
| `created_at` | Creation date | Fundamentals - Funding |

---

## FUNDS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Fund identifier | Fundamentals - Funds |
| `name` | Fund name | Fundamentals - Funds |
| `organization_identifier` | Managing organization | Fundamentals - Funds |
| `founded_on` | Fund inception date | Fundamentals - Funds |
| `created_at` | Creation date | Fundamentals - Funds |

---

## INVESTMENTS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Investment identifier | Fundamentals - Funding |
| `funding_round_identifier` | Funding round | Fundamentals - Funding |
| `investor_identifier` | Investor entity | Fundamentals - Funding |
| `created_at` | Creation date | Fundamentals - Funding |

---

## IPOS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | IPO identifier | Fundamentals - Financials |
| `organization_identifier` | Public organization | Fundamentals - Financials |
| `stock_symbol` | Stock ticker | Fundamentals - Financials |
| `went_public_on` | IPO date | Fundamentals - Financials |

---

## JOBS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Job identifier | Fundamentals - Jobs |
| `title` | Job title | Fundamentals - Jobs |
| `organization_identifier` | Hiring organization | Fundamentals - Jobs |
| `posted_on` | Posting date | Fundamentals - Jobs |

---

## LOCATIONS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Location identifier | Fundamentals - Locations |
| `name` | Location name | Fundamentals - Locations |
| `country_code` | Country code | Fundamentals - Locations |

---

## ORGANIZATIONS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Organization identifier | Fundamentals - Core Firmographics |
| `name` | Organization name | Fundamentals - Core Firmographics |
| `primary_domain` | Primary domain | Fundamentals - Core Firmographics |
| `founded_year` | Founded year | Fundamentals - Core Firmographics |
| `estimated_num_employees` | Estimated employees | Fundamentals - Core Firmographics |

---

## OWNERSHIPS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Ownership identifier | Fundamentals - Ownership |
| `owner_identifier` | Owner entity | Fundamentals - Ownership |
| `owned_organization_identifier` | Owned organization | Fundamentals - Ownership |

---

## PEOPLE

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Person identifier | Fundamentals - People |
| `first_name` | First name | Fundamentals - People |
| `last_name` | Last name | Fundamentals - People |
| `full_name` | Full name | Fundamentals - People |
| `linkedin_url` | LinkedIn profile | Fundamentals - People |

---

## PRESS_REFERENCES

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Press reference identifier | Fundamentals - Media |
| `organization_identifier` | Related organization | Fundamentals - Media |
| `publisher` | Publisher name | Fundamentals - Media |
| `published_on` | Publish date | Fundamentals - Media |

---

## PRINCIPALS

| Field | Description | API Package |
|------|-------------|-------------|
| `UUID` | Principal identifier | Fundamentals - People |
| `organization_identifier` | Related organization | Fundamentals - People |
| `person_identifier` | Related person | Fundamentals - People |
| `role` | Principal role | Fundamentals - People |

---

*Generated from file sent by Crunchbase support*