---
name: sentry
description: To filter, categorize, and consolidate real-time system alerts and notifications. To protect the Product Owner from alert fatigue while ensuring critical signals are never missed.
---

## Directory Structure
- `/sources/alerts/` (Raw alert dumps/feeds)
- `/alerts/active/` (Ongoing issues requiring attention)
- `/alerts/history/` (Resolved or acknowledged alerts)
- `alert_dashboard.md` (The Live Status View)

## Workflow

### Step 1: Ingest & Deduplicate
When the user provides a stream of alerts (e.g., email subject lines, Splunk/ServiceNow notification feeds):
1.  **Normalization:** Strip timestamps to compare the core message.
    - *Input:* "Error 500 at 12:01", "Error 500 at 12:02", "Error 500 at 12:03"
    - *Core:* "Error 500"
2.  **Counting:** Instead of creating 3 entries, create **1 entry** with a count (e.g., "Count: 3").

### Step 2: Classify Severity (The Filter)
Compare the Core Message against the severity rules:
- **🚨 CRITICAL (Wake Up):**
    - Production Outages.
    - Security Breaches.
    - Failed Deployments.
- **⚠️ WARNING (Review Daily):**
    - API Latency spikes.
    - Disk space warnings (>80%).
    - Approaching SLA breaches.
- **ℹ️ INFO (Log & Ignore):**
    - Scheduled maintenance notifications.
    - Successful backups.

### Step 3: Architect the Alert File
If the alert is **CRITICAL** or **WARNING**, create/update a Context File:
- File: `/alerts/active/[Type]_[Severity].md`
- **Content Template:**
    ```markdown
    # Alert: [Alert Name]
    **First Seen:** [Timestamp]
    **Last Seen:** [Timestamp]
    **Occurrence Count:** [X]
    **Severity:** [Level]
    
    ## Impact Analysis
    [Brief summary of what this alert stops the business from doing.]
    
    ## Source Data
    - [[../sources/alerts/raw_feed_batch_1.md]]
    ```

### Step 4: Update Dashboard
Update `alert_dashboard.md`. 
*Note: This file should be cleared/archived daily or weekly.*

**Dashboard Format:**
| Status | Count | Alert Message | First Detected | Action |
| :--- | :--- | :--- | :--- | :--- |
| 🚨 CRIT | 15 | [Payment Gateway 502](../alerts/active/payment_502.md) | 10:00 AM | **Assign to On-Call** |
| ⚠️ WARN | 1 | [License Expiring in 30 Days](../alerts/active/license.md) | 09:15 AM | Add to Backlog |

## Integration with Other Skills
- **If an Alert becomes an Incident:** Link the Alert File to the Ops Analyst Incident File.
- **If an Alert requires human work:** Link the Alert File to a Task Marshal To-Do File.

## "Stop & Ask" Triggers
- **New Error Types:** If an alert text has never been seen before and contains ambiguous technical jargon, ask: *"Is 'Error Code: X99' considered Critical or Info?"*
- **Flood Control:** If alerts exceed 100/minute, ask: *"I detect an alert storm. Should I suppress all notifications for the next hour and summarize later?"*
