# ADR-006 - Airtable Synchronization System

## Decision

The team decided to implement a **Custom Solution** for Airtable synchronization instead of using Airbyte or a Hybrid approach.

## Context

The project required implementing a synchronization layer between Airtable (client's primary data management tool) and the application database (Supabase PostgreSQL). Direct Airtable API consumption was ruled out due to rate limits (5 req/sec) making the application unusable with concurrent users. Three synchronization approaches were evaluated:

1. **Airbyte Only** - Managed ETL platform with pre-built Airtable connector
   - Sync frequency: 1 hour (Standard) or 15 minutes (Pro)
   - Sync mode: Full Refresh only (no incremental)
   - Direction: **Unidirectional only** (cannot write to Airtable)
   - Pricing: $15 per million records synced

2. **Custom Solution** - Purpose-built bidirectional sync using Airtable webhooks and queues
   - Sync frequency: ~1 second (near real-time)
   - Sync mode: Incremental (event-driven)
   - Direction: **Bidirectional**
   - Pricing: Uses existing Supabase infrastructure

3. **Hybrid Solution** - Airbyte for inbound + custom outbound + override table
   - Sync frequency: 1 hour inbound, immediate outbound
   - Sync mode: Full Refresh inbound, event-driven outbound
   - Direction: **Bidirectional** (with query complexity)
   - Pricing: Airbyte costs + development

Detailed analysis of each solution is available in the [Airtable Sync System documentation](../airtable-sync-system/).

## Rationale

The decision was driven by the following considerations:

- **Bidirectional Sync Requirement:** Airbyte is fundamentally unidirectional - there is no Airtable destination connector, making outbound sync impossible. The application requires writing user changes back to Airtable, which only the Custom and Hybrid solutions support.

- **Cost Efficiency:** With 40,000 records per sync at $15/million records, Airbyte costs $0.60 per sync. At hourly frequency this equals $432/month; at 15-minute frequency, $1,728/month. The Custom Solution has $0 monthly operational cost, saving $5,184-20,736 annually.

- **Sync Latency:** The Custom Solution provides near real-time sync (~1 second) using webhooks, while Airbyte-based solutions have minimum 1-hour latency (Standard plan) or 15 minutes (Pro plan).

- **Sync Efficiency:** Airbyte's Airtable connector only supports Full Refresh mode, downloading all 40,000 records on every sync. The Custom Solution uses webhooks to sync only changed records, significantly reducing data transfer and API usage.

- **Query Simplicity:** The Custom Solution maintains a single source of truth (database), while the Hybrid Solution requires merge logic in every query to compare timestamps between main and override tables.

- **Infrastructure Reuse:** The Custom Solution leverages existing Supabase infrastructure (cronjobs, database) already included in the $25/month plan, avoiding additional vendor dependencies.

## Consequences

### Positive

- Near real-time bidirectional sync (~1 second latency)
- No additional monthly costs ($0 vs $432-1,728)
- Event-driven efficiency (only syncs changed records)
- Single source of truth simplifies queries and debugging
- Full control over sync behavior and conflict resolution
- No vendor lock-in beyond Airtable API
- Annual savings of $5,184-20,736 compared to Airbyte solutions

### Negative

- Higher upfront development investment (2-3 weeks vs 1-2 weeks for Hybrid)
- Team must maintain sync infrastructure
- Must implement custom monitoring and alerting
- Webhook reliability depends on Airtable delivery
- Complex edge cases require thorough testing

## Alternatives Considered

### Airbyte Only

- **Pros:**
  - Pre-built, tested connector (minimal development)
  - Managed infrastructure with built-in monitoring
  - Automatic rate limit handling
  - Large community and documentation
- **Cons:**
  - **No bidirectional sync** - Cannot write to Airtable (disqualifying)
  - Full Refresh only (downloads all data every sync)
  - High monthly costs ($432-1,728/month)
  - Minimum 1-hour sync frequency on Standard plan

See [detailed analysis](../airtable-sync-system/01-airbyte-only.md).

### Hybrid Solution

- **Pros:**
  - Faster development (1-2 weeks vs 2-3 weeks)
  - Managed inbound sync via Airbyte
  - Bidirectional sync capability
  - Built-in monitoring for inbound via Airbyte dashboard
- **Cons:**
  - High monthly costs ($432-1,728/month for Airbyte)
  - Query complexity (merge logic between main and override tables)
  - 1-hour inbound latency
  - Two data sources can cause confusion
  - Full Refresh inefficiency remains

See [detailed analysis](../airtable-sync-system/03-hybrid-solution.md).

## Cost Comparison

For the project's data volume:
- **Records per sync:** 40,000
- **Airbyte pricing:** $15 per million records

### Custom Solution
- Supabase (existing): $0 additional
- Development: 2-3 weeks one-time
- **Total: $0/month**

### Airbyte (1-hour sync)
- Syncs per month: 720
- Cost per sync: $0.60
- **Total: $432/month**

### Airbyte (15-minute sync)
- Syncs per month: 2,880
- Cost per sync: $0.60
- **Total: $1,728/month**

### Annual Cost Comparison

| Solution | Monthly | Annual |
|----------|---------|--------|
| Custom | $0 | $0 |
| Airbyte (hourly) | $432 | $5,184 |
| Airbyte (15-min) | $1,728 | $20,736 |

## Follow-up

- Document sync system architecture and operations runbook
- Implement monitoring dashboard for sync health (queue depth, latency, errors)
- Create alerts for sync failures and data drift
- Establish fallback procedures (switch to Hybrid if blockers emerge)
- Consider revisiting this decision if:
  - Airbyte adds Airtable destination connector (bidirectional)
  - Airbyte adds incremental sync for Airtable
  - Development constraints make Custom Solution unfeasible

For now, the Custom Solution provides the best balance of functionality, cost efficiency, and performance for our synchronization needs.
