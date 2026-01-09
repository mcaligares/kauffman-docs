# Kauffman Fellows — Project Documentation

Welcome to the **Kauffman Fellows** project documentation.  
This site centralizes all technical, architectural, and discovery-related documentation used to design, build, and evolve the platform.

The documentation is organized into **Technical Documentation** and **Discovery & Data Research**, covering both implementation decisions and external data providers analysis.

---

## 📚 Documentation Structure

### 1. Technical Documentation

This section describes the system architecture, technology stack, and key technical decisions made throughout the project lifecycle.

#### Tech Stack
- [Tech Stack v1](./docs/tech-stack-v1.md)
- [Tech Stack v2](./docs/tech-stack-v2.md)

#### Architecture Decision Records (ADRs)
Architectural Decision Records document important design choices, including context, rationale, and consequences.

- [ADR-001 — Next.js](./docs/adrs/ADR-001-NextJS.md)
- [ADR-002 — Authentication](./docs/adrs/ADR-002-Authentication.md)
- [ADR-003 — Database](./docs/adrs/ADR-003-Database.md)
- [ADR-004 — Hosting](./docs/adrs/ADR-004-Hosting.md)
- [ADR-005 — Supabase vs Firebase](./docs/adrs/ADR-005-Supabase-vs-Firebase.md)

---

### 2. Discovery & Data Provider Research

This section contains discovery documents and technical analysis of third-party data providers used to enrich the Kauffman Fellows dataset.

Each provider follows a **consistent structure**:
- `api-endpoints-analysis.md` — API capabilities, endpoints, limits, and usage considerations
- `data-dictionary.md` — Entity definitions and field-level descriptions
- `/examples` — Sample API responses and payloads

#### Analyzed Providers

##### PitchBook
- [API Endpoints Analysis](./pitchbook/api-endpoints-analysis.md)
- [Data Dictionary](./pitchbook/data-dictionary.md)
- Examples: `/pitchbook/examples`

##### Crunchbase
- [API Endpoints Analysis](./crunchbase/api-endpoints-analysis.md)
- [Data Dictionary](./crunchbase/data-dictionary.md)
- Examples: `/crunchbase/examples`

##### Apollo
- [API Endpoints Analysis](./apollo/api-endpoints-analysis.md)
- [Data Dictionary](./apollo/data-dictionary.md)
- Examples: `/apollo/examples`

##### Signal
- [API Endpoints Analysis](./signal/api-endpoints-analysis.md)
- [Data Dictionary](./signal/data-dictionary.md)
- Examples: `/signal/examples`

#### Providers Pending Analysis

The following providers are planned but still under analysis:

- **Bright Data**
  - API Endpoints Analysis (pending)
  - Data Dictionary (pending)

- **ZoomInfo**
  - API Endpoints Analysis (pending)
  - Data Dictionary (pending)

- **DealRoom**
  - API Endpoints Analysis (pending)
  - Data Dictionary (pending)

---

## 🧭 How to Use This Documentation

- Use **Technical Documentation** to understand system architecture, infrastructure, and design decisions.
- Use **Discovery & Data Provider Research** to evaluate data sources, compare coverage, and understand entity models.
- ADRs should be referenced when introducing changes that may impact architecture or core assumptions.

---

## 📌 Notes

- This documentation is intended to evolve alongside the project.
- New ADRs, provider analyses, and updates should follow the existing structure to ensure consistency.

---

**Kauffman Fellows — Documentation Index**
