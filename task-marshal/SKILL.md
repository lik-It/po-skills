# Skill: Task Marshal

## Purpose
To ingest unstructured communications (Emails, Meetings) and convert them into a structured, linked file system of Action Items.

## Directory Structure
- `/sources/emails/` & `/sources/meetings/`
- `/tasks/active/`
- `/tasks/archive/`
- `action_items.md` (The Master List)

## Workflow

### Step 1: Ingest & Categorize
1.  Determine the **Category** (e.g., "Deployment", "Compliance", "Team Sync").
2.  Create a Source File: `/sources/[category]/[YYYY-MM-DD]_[Source_Title].md`.
3.  Paste raw text into this file.

### Step 2: Extract & Contextualize
For every distinct action item found in the source:
1.  Create a **Task File**: `/tasks/active/[YYYY-MM-DD]_[Task_Name].md`.
2.  **Task File Template:**
    ```markdown
    # Task: [Task Name]
    **Status:** [Open/In Progress]
    **Assignee:** [Name]
    **Due Date:** [YYYY-MM-DD] or [TBD]
    **Priority:** [High/Medium/Low]
    
    ## Context & Analysis
    [Detailed description derived from the source. Explain WHY this is a task.]
    
    ## Source Links
    - Derived from: [[../sources/emails/original_email.md]]
    ```

### Step 3: Update Master List
Update `action_items.md`.

**Master List Format:**
| Status | Priority | Task (Click for Context) | Assignee | Source |
| :--- | :--- | :--- | :--- | :--- |
| 🔴 Open | High | [Fix ServiceNow Timeouts](../tasks/active/fix_timeouts.md) | @DevTeam | [Email](../sources/emails/alert_email.md) |

## "Stop & Ask" Triggers
- If the assignee is implied but not named (e.g., "We need to fix this"), ask: *"Who should own the 'Fix This' task?"*
- If the category is unclear, ask: *"Should I file this under 'Operations' or 'Development'?"*
