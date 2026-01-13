# Skill: Ops Analyst

## Purpose
To ingest structured operational data (ServiceNow Incidents, Problems) and Organization Documents, analyzing them to identify stability trends, SLA risks, and hygiene improvements.

## Directory Structure
- `/sources/incidents/` (Raw Incident Dumps)
- `/sources/org_docs/` (SLA Policies, Architecture Docs)
- `/insights/trends/` (Analysis of specific problem areas)
- `platform_health.md` (The Health Dashboard)

## Workflow

### Step 1: Ingest & Contextualize
1.  **Classify:** "Reference Document" or "Operational Data".
2.  **Save Source:** `/sources/incidents/[YYYY-MM-DD]_[Type].md` or `/sources/org_docs/`.

### Step 2: Pattern Recognition (The Analysis)
1.  **Group by Failure:** Identify recurring keywords (e.g., "Timeout", "API Latency").
2.  **Cross-Reference:** Compare against `/sources/org_docs/` (e.g., Check SLA breaches).
3.  **Generate Insight:** Create a detailed analysis file (`/insights/trends/[YYYY-MM-DD]_[Topic].md`) containing Root Cause hypothesis and Hygiene Score.

### Step 3: Advice & Suggestions
In the Insight file, provide a **"Hygiene Score"**:
- **Stable:** No major incidents.
- **Degrading:** Recurring low-priority incidents (noise).
- **Critical:** SLA breaches or P1/P2 incidents.

### Step 4: Update Health Dashboard
Update `platform_health.md`.

**Dashboard Format:**
| Risk Level | Trend Topic (Click for Insight) | Incident Count | Source Data |
| :--- | :--- | :--- | :--- |
| 🔴 Critical | [API Gateway Timeouts](../insights/trends/api_timeouts.md) | 12 | [Dump](../sources/incidents/week4_dump.md) |

## "Stop & Ask" Triggers
- **Unknown Acronyms:** Ask: *"What does 'ABC' refer to in this ticket?"*
- **SLA Ambiguity:** Ask: *"What is the standard SLA for P3 incidents?"*
