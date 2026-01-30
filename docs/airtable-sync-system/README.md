# Airtable Sync System Documentation

This folder contains detailed analysis of different synchronization approaches between Airtable and the application database (Supabase PostgreSQL).

## Documents

| Document | Description |
|----------|-------------|
| [airbyte-solution.md](./airbyte-solution.md) | Analysis of using Airbyte as the sole sync solution |
| [custom-solution.md](./custom-solution.md) | Analysis of a fully custom bidirectional sync system |
| [hybrid-solution.md](./hybrid-solution.md) | Analysis of combining Airbyte (inbound) with custom outbound |

## Related ADR

- [ADR-006 - Airtable Synchronization System](../adrs/ADR-006-Airtable-Sync.md) - Decision and comparison of sync approaches

## Quick Comparison

| Aspect | Airbyte Only | Custom | Hybrid |
|--------|--------------|--------|--------|
| Inbound latency | 1 hour | ~1 second | 1 hour |
| Outbound sync | ❌ No | ✅ Yes | ✅ Yes |
| Bidirectional | ❌ No | ✅ Yes | ✅ Yes |
| Monthly cost | $432-1,728 | $0 | $432-1,728 |
| Development effort | Low | High | Medium |
| Viable for project | ❌ No | ✅ Yes | ✅ Fallback |

## Context

The project requires synchronization between Airtable (client's data management tool) and the application database. Key requirements:

- **Bidirectional sync** - Application must write changes back to Airtable
- **Acceptable latency** - 5-15 minutes for most data
- **User modifications** - Users can edit their own profiles
- **Client workflow** - Admin must see user changes in Airtable
