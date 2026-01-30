# Airbyte Solution Analysis

## Overview

This document analyzes using Airbyte as the sole synchronization solution between Airtable and the application database (Supabase PostgreSQL).

## Airbyte Platform Details

### Pricing Structure

| Plan | Monthly Cost | Sync Frequency | Key Features |
|------|--------------|----------------|--------------|
| Open Source | Free (self-hosted) | 5 min minimum | Requires infrastructure |
| Standard | $10 + $2.50/credit | **1 hour minimum** | 4 credits included |
| Plus | Capacity-based (annual) | 15 minutes | Predictable costs |
| Pro | Capacity-based | 15 minutes | RBAC, encryption |
| Enterprise | $5,000+/month | Sub-5 minutes | Premium support |

### Credit System

- Credits are Airbyte's billing unit for data movement
- Initial historical sync consumes significant credits (full dataset)
- Subsequent syncs also use Full Refresh (no incremental for Airtable)
- Minimum purchase: 20 credits
- Credits expire after 1 year of inactivity

### Airtable Source Connector

| Feature | Support | Notes |
|---------|---------|-------|
| Full Refresh - Overwrite | Yes | Replaces all data |
| Full Refresh - Append | Yes | Adds without removing |
| **Incremental Sync** | **No** | Not supported |
| Rate Limit Handling | Automatic | Respects 5 req/sec |
| Table Rename Detection | No | Breaks sync if renamed |
| Schema Changes | Manual | Requires recreating source |

**Critical Limitation:** Every sync downloads the entire dataset, not just changes.

### PostgreSQL Destination Connector

| Feature | Support |
|---------|---------|
| Full Refresh Sync | Yes |
| Incremental - Append | Yes |
| Incremental - Append + Deduped | Yes |
| **Scale Recommendation** | < 10 GB only |

**Metadata columns added:**
- `_airbyte_raw_id`
- `_airbyte_extracted_at`
- `_airbyte_meta`
- `_airbyte_generation_id`

### Sync Frequency Constraints

| Deployment | Minimum Interval | Notes |
|------------|------------------|-------|
| Airbyte Cloud (Standard) | 60 minutes | Cannot be reduced |
| Airbyte Cloud (Pro) | 15 minutes | Requires annual plan |
| Self-hosted | 5 minutes | Requires infrastructure |

**Jitter Warning:** Airbyte Cloud may delay syncs by up to 30 minutes after scheduled time.

## Manual Sync via API

Airbyte provides an API to trigger syncs on-demand:

```bash
POST https://api.airbyte.com/v1/jobs
{
  "connectionId": "your-connection-uuid",
  "jobType": "sync"
}
```

### API Limitations

| Aspect | Constraint |
|--------|------------|
| Concurrent syncs | 1 per connection |
| Sync duration | Minutes (Full Refresh) |
| Credit consumption | Per sync execution |

## Project Requirements vs Airbyte Capabilities

| Requirement | Project Need | Airbyte Capability | Status |
|-------------|--------------|-------------------|--------|
| Data volume | < 10,000 records | < 10 GB recommended | ✅ Met |
| Inbound sync | Required | Supported | ✅ Met |
| Sync latency | 5-15 minutes | 1 hour (Standard) / 15 min (Pro) | ⚠️ Partial |
| **Outbound sync** | **Required** | **Not supported** | ❌ **Blocker** |
| Bidirectional | Essential | Unidirectional only | ❌ **Blocker** |

## Critical Limitation: No Outbound Sync

**Airbyte is fundamentally a unidirectional ETL tool.**

- Airbyte moves data from **sources** to **destinations**
- Airtable exists only as a **source** connector
- There is **no Airtable destination connector**
- Application changes **cannot be written back** to Airtable

This is a **disqualifying limitation** for projects requiring bidirectional sync.

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

### Airbyte Self-Hosted

| Component | Cost |
|-----------|------|
| Airbyte license | $0 (open source) |
| Infrastructure (DigitalOcean) | $20-40/month |
| **Total** | **$20-40/month + maintenance** |

**Note:** Self-hosted still requires Full Refresh syncs but avoids per-record pricing.

## Advantages

- Pre-built, tested connectors
- Managed infrastructure (Cloud version)
- Built-in monitoring dashboard
- Automatic rate limit handling
- No custom sync code to maintain
- Large community and documentation

## Disadvantages

- **No bidirectional sync** (deal-breaker)
- Minimum 1-hour sync frequency (Standard plan)
- Full Refresh only for Airtable (inefficient)
- Credit-based pricing can be unpredictable
- Adds another platform to manage
- Cannot write application changes to Airtable

## Conclusion

**Airbyte alone is NOT viable for this project.**

The absence of outbound sync (writing to Airtable) is a fundamental blocker. Even with API-triggered syncs and the Pro plan's 15-minute frequency, Airbyte cannot fulfill the bidirectional synchronization requirement.

## References

- [Airbyte Pricing](https://airbyte.com/pricing)
- [Airbyte Airtable Source](https://docs.airbyte.com/integrations/sources/airtable)
- [Airbyte PostgreSQL Destination](https://docs.airbyte.com/integrations/destinations/postgres)
- [Airbyte Jobs API](https://reference.airbyte.com/reference/createjob)
- [Airbyte Sync Schedules](https://docs.airbyte.com/platform/using-airbyte/core-concepts/sync-schedules)
