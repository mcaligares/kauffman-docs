# Airtable Sync: Hybrid Solution Analysis

## Overview

This document analyzes a hybrid synchronization approach that combines Airbyte for inbound sync with a custom outbound solution and a local override table for immediate data availability.

## How It Works

### 1. Inbound Sync (Airbyte)

Airbyte handles the complex task of pulling data from Airtable:

- **Frequency:** Hourly (Standard plan) or 15 minutes (Pro plan)
- **Mode:** Full Refresh (downloads entire dataset)
- **Destination:** `users_main` table with `synced_at` timestamp

### 2. Outbound Sync (Custom)

When a user modifies their profile in the application:

1. **Write to Airtable:** Direct API call updates Airtable immediately
2. **Write to Override:** Same data saved to `users_override` with timestamp
3. **Client sees changes:** Airtable is updated, admin can see changes
4. **App sees changes:** Override table provides immediate local access

### 3. Read Strategy (Merge Logic)

When the application reads user data:

1. Query both `users_main` and `users_override`
2. Compare timestamps: `modified_at` vs `synced_at`
3. If override is newer → use override data
4. If main is newer → use main data (Airtable has been updated)

## Development Effort Estimate

| Component | Complexity |
|-----------|------------|
| Airbyte setup + configuration | Low |
| Override table + schema | Low |
| Merged view | Low |
| Outbound sync logic | Low |
| Query layer updates | Low |
| Testing | Medium |

## Cost Analysis

Based on project data volume:

| Metric | Value |
|--------|-------|
| Records per sync | 40,000 |
| Price per million records | $15 |
| **Cost per sync** | **$0.60** |

### Airbyte Cloud by Sync Frequency

| Frequency | Syncs/Month | Monthly Cost |
|-----------|-------------|--------------|
| Every 1 hour | 720 | **$432** |
| Every 15 min | 2,880 | **$1,728** |

| Component | Cost |
|-----------|------|
| Airbyte (hourly sync) | $432/month |
| Supabase (existing) | $0 additional |
| **Total monthly (hourly)** | **$432** |
| **Total monthly (15-min)** | **$1,728** |

## Advantages

- **Reduced development effort** - Airbyte handles complex inbound sync
- **Immediate outbound** - Changes visible in Airtable instantly
- **Immediate local reads** - Override table provides instant access
- **Managed infrastructure** - Airbyte handles inbound complexity
- **Simpler outbound** - Only user profiles (limited scope)
- **Built-in monitoring** - Airbyte dashboard for inbound

## Disadvantages

- **1-hour inbound latency** - Airbyte Standard minimum (or 15 min with Pro)
- **Query complexity** - Every read requires merge logic
- **Two data sources** - Potential for confusion
- **Airbyte cost** - Additional $50-70/month
- **Full Refresh inefficiency** - Downloads all data every sync
- **Still needs custom outbound** - Not fully managed

## Conclusion

The hybrid solution offers a **balanced trade-off** between development effort and functionality. It reduces inbound complexity by using Airbyte while maintaining immediate outbound sync for user changes. The main trade-off is accepting 1-hour inbound latency and adding query complexity through the override table mechanism.

## References

- [Airbyte Pricing](https://airbyte.com/pricing)
- [Airbyte Jobs API](https://reference.airbyte.com/reference/createjob)
- [Airbyte Airtable Source](https://docs.airbyte.com/integrations/sources/airtable)
- [Supabase Views](https://supabase.com/docs/guides/database/tables#views)
