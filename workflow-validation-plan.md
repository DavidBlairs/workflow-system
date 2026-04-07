---
layout: default
title: Workflow Validation Plan
---

# Workflow Validation Plan

**Document ID:** WVP-001  
**Version:** 1.0  
**Effective Date:** 6 April 2026  
**Author:** David Blair  
**Role:** Product Owner / Senior Developer  
**Environment:** SAFe, GxP Regulated (Pharma)

---

## 1. Purpose

This document defines the strategy, structure, and operating procedures for a personal workflow management system designed to bring structured control to a combined Product Owner / Senior Developer role within a SAFe organisation operating under GxP compliance requirements.

The system is modelled on legal case management principles, specifically the practice of maintaining continuous visibility, explicit state tracking, and disciplined triage across concurrent workstreams. It replaces ad hoc prioritisation with a repeatable, measurable operating rhythm.

This plan serves as the authoritative reference for how work is captured, classified, prioritised, executed, tracked, and reviewed.

---

## 2. Scope

### 2.1 Domains Covered

This system governs all work falling within the following domains:

| Domain | Examples |
|---|---|
| **Software Development** | Feature development, bug fixes, technical debt, code reviews, pull requests |
| **Product Ownership** | Backlog refinement, stakeholder alignment, requirements definition, PI Planning |
| **Team Management** | Task allocation, blocker resolution, capacity management (2-person team) |
| **Release Management** | Release packaging, deployment coordination, release notes, go/no-go decisions |
| **GxP Compliance** | Validation documentation, audit preparation, change control, deviation handling |

### 2.2 Boundaries

- This plan covers **individual and small-team workflow management** only.
- It does not replace or override organisational SAFe ceremonies, GxP SOPs, or ADO project-level governance.
- It operates **within** existing tooling (Azure DevOps, Microsoft Outlook) without requiring additional software.

---

## 3. Tooling

### 3.1 Primary System: Azure DevOps (ADO)

ADO serves as the **single source of truth** for all actionable work. If a piece of work does not exist in ADO, it does not exist in this system.

**Role:** Work item storage, state tracking, prioritisation, querying, dashboarding.

### 3.2 Supporting System: Microsoft Outlook

Outlook serves two narrow purposes only:

1. **Calendar:** Fixed commitments (meetings, ceremonies) and defensive deep work blocks.
2. **Communication:** Follow-ups, chasing dependencies, stakeholder correspondence.

Outlook is **not** used for task management, prioritisation, or work tracking.

### 3.3 Principle of Separation

| Function | Tool |
|---|---|
| What work exists | ADO |
| What state work is in | ADO |
| What to do next | ADO (via queries/dashboard) |
| When am I unavailable | Outlook Calendar |
| Protected focus time | Outlook Calendar |
| Chasing / follow-ups | Outlook Email |

---

## 4. ADO Configuration

### 4.1 Custom Fields

The following fields must be present on all relevant work item types (User Story, Task, Bug, or any custom types in use):

| Field Name | Type | Required | Purpose |
|---|---|---|---|
| **Priority** | Picklist | Yes | Explicit ranking: P1 / P2 / P3 |
| **Next Action** | Plain Text | Yes (by discipline) | The single, concrete next step to advance this item |
| **Waiting On** | String or Identity | Conditional (when State = Waiting) | The person or team currently blocking progress |
| **Follow-up Date** | Date | Optional | The date by which the item should be revisited if still blocked |

### 4.2 Field Definitions

**Priority:**

| Value | Definition | Implication |
|---|---|---|
| **P1** | Must move now. Deadline-critical, blocking others, or high organisational impact. | Interrupts current work. Worked on first each day. |
| **P2** | Important but not immediately urgent. Meaningful progress expected this week. | Worked on when no P1 is actionable. |
| **P3** | Background. Relevant but no immediate pressure. | Only addressed when P1/P2 are clear or blocked. |

**Next Action:**
- Must be a concrete, executable step, not a description of the work item itself.
- Bad: "Release task"
- Good: "Validate release package for audit sign-off"
- Updated every time the item is touched.

**Waiting On:**
- Populated whenever State transitions to Waiting.
- Must identify a specific person or team, not a vague dependency.
- Bad: "Waiting on someone"
- Good: "Waiting on QA, Sarah to complete test execution"

### 4.3 Workflow States

All tracked work items must use the following state model:

| State | Definition | Entry Criteria |
|---|---|---|
| **Scheduled** | Work is defined and planned but not yet started. Not currently actionable. | Item has been created and prioritised. |
| **Active** | Work is in progress or immediately actionable. You can take the next step right now. | Next Action is defined. No external dependency blocking progress. |
| **Waiting** | Blocked by an external dependency. Cannot progress until someone else acts. | Waiting On field is populated. Follow-up Date is set (recommended). |
| **Done** | Work is complete. No further action required. | Acceptance criteria met. All sub-tasks resolved. |

**State Transition Rules:**

- An item **cannot** be in Active if it has an unresolved external dependency.
- An item **cannot** be in Waiting without a populated Waiting On field.
- When an item moves from Waiting → Active, the Waiting On field should be cleared.
- No more than **5–7 items** should be in Active state at any time. Everything else is Scheduled or Waiting.

### 4.4 Work Item Titling Convention

Titles must be **action-oriented and specific**. They should read as a clear instruction, not a label.

| Domain | Bad Title | Good Title |
|---|---|---|
| Development | "API update" | "Implement retry logic for external API timeout handling" |
| Compliance | "Validation" | "Draft OQ protocol for reporting module v2.3" |
| Release | "Release task" | "Prepare release notes and obtain QA sign-off for R4.1" |
| Product | "Stakeholder request" | "Clarify acceptance criteria for dashboard filtering with Sarah" |

---

## 5. ADO Queries

### 5.1 Core Queries

Five purpose-built queries form the backbone of daily operations. Each query answers **one question only**.

#### Query 1: My Active Work
**Question answered:** What can I act on right now?

| Clause | Operator | Value |
|---|---|---|
| Assigned To | = | @Me |
| State | = | Active |
| **Order By** | | Priority (descending), Due Date (ascending) |

#### Query 2: My Active P1
**Question answered:** What is critical and actionable right now?

| Clause | Operator | Value |
|---|---|---|
| Assigned To | = | @Me |
| State | = | Active |
| Priority | = | P1 |

#### Query 3: Waiting On (All)
**Question answered:** What am I blocked on?

| Clause | Operator | Value |
|---|---|---|
| Assigned To | = | @Me |
| State | = | Waiting |

#### Query 4: Waiting On (Stale)
**Question answered:** What has been blocked too long and needs chasing?

| Clause | Operator | Value |
|---|---|---|
| Assigned To | = | @Me |
| State | = | Waiting |
| Changed Date | < | @Today - 2 |

#### Query 5: Due This Week
**Question answered:** What is approaching a deadline?

| Clause | Operator | Value |
|---|---|---|
| Assigned To | = | @Me |
| Due Date | <= | @Today + 7 |
| State | <> | Done |

### 5.2 Supplementary Query

#### Query 6: Team Blockers
**Question answered:** Is my team stuck on anything?

| Clause | Operator | Value |
|---|---|---|
| Assigned To | In | [Team members] |
| State | In | Waiting, Blocked |
| Priority | In | P1, P2 |

### 5.3 Query Column Configuration

All query results should display **only** the following columns to reduce noise:

- Title
- Priority
- State
- Due Date
- Waiting On
- Changed Date (Last Updated)

---

## 6. ADO Dashboard

### 6.1 Purpose

The dashboard is the **single screen** used to orient each morning. It must answer three questions instantly:

1. What must I act on today?
2. What is at risk?
3. What am I waiting on?

### 6.2 Layout

#### Top Row: Immediate Awareness (Tiles)

| Widget | Type | Source Query | Purpose |
|---|---|---|---|
| My Active P1 | Query Tile (count) + Query List | Query 2 | Execution focus |
| Due This Week | Query Tile (count) | Query 5 | Risk indicator |
| Waiting On | Query Tile (count) | Query 3 | Dependency load |

#### Middle Row: Action Views (Lists)

| Widget | Type | Source Query | Purpose |
|---|---|---|---|
| My Active Work | Query Results | Query 1 | Primary working list |
| Waiting On (Stale) | Query Results | Query 4 | Follow-up engine |

#### Bottom Row: Situational Awareness

| Widget | Type | Source Query | Purpose |
|---|---|---|---|
| Team Blockers | Query Results | Query 6 | Product Owner oversight |

---

## 7. Outlook Calendar Configuration

### 7.1 Calendar Purpose

The calendar answers one question only: **When am I unavailable?**

It is not used for task prioritisation or work planning.

### 7.2 Permitted Calendar Entries

| Entry Type | Examples | Characteristic |
|---|---|---|
| **Fixed Commitments** | Meetings, SAFe ceremonies (standup, PI Planning, retrospectives) | Immovable, set by others |
| **Deep Work Blocks** | "Dev work", "Compliance documentation" | Defensive, self-imposed, max 2 per day |
| **Control/Reset Blocks** | Morning Control Block, Afternoon Reset | Structural, recurring, non-negotiable |

### 7.3 Rules

- Maximum **2 deep work blocks** per day.
- Deep work blocks are **movable** if a genuine P1 interruption arises, but they must be **rescheduled**, not deleted.
- No attempt is made to schedule the full day. Unblocked time remains flexible for operational work and interruptions.

---

## 8. Daily Operating Rhythm

### 8.1 Overview

The day is divided into five phases. Two are fixed rituals (Control Block, Reset). Three are flexible execution windows.

```
08:45 ─── Control Block (40 min) ──── orientation, not execution
09:25 ─── transition
09:30 ─── Standup (15 min) ─────────── alignment with team
09:45 ─── transition
10:00 ─── Deep Work (2 hrs) ────────── highest-value P1 execution
12:00 ─── Lunch
13:00 ─── Operational Window (2 hrs) ─ interrupts, tickets, team support
15:00 ─── Flexible Block (1.5 hrs) ── second deep work or operational
16:30 ─── Reset Block (30 min) ─────── closure, update, prepare tomorrow
17:00 ─── End of day
```

### 8.2 Morning Control Block (08:45 – 09:25)

**Purpose:** Establish control before the day's demands begin. This is the most important block of the day.

**Rules:**
- No coding.
- No responding to messages.
- No reactive work of any kind.

**Procedure:**

| Step | Duration | Action | Tool |
|---|---|---|---|
| 1. Clear Waiting On | 10–15 min | Open Query 4 (Stale). Chase anything >2 days old. Send messages/emails while people are coming online. | ADO → Outlook |
| 2. Review Deadlines | 10 min | Open Query 5 (Due This Week). Identify anything at risk. Pull forward items that need earlier attention. | ADO |
| 3. Decide the Day | 15 min | Open Query 1 (My Active Work). Select **2–3 P1 items** and **1–2 P2 items**. Update their Next Action field if needed. | ADO |

**Exit Criteria:**
You can clearly state: *"If I only do these 2–3 things today, the day is a success."*

### 8.3 Standup (09:30 – 09:45)

Because the Control Block has already been completed:

- You know what you are working on.
- You know what is blocked.
- You can raise issues with specificity.
- You steer the conversation rather than reacting to it.

Standup functions as an **alignment tool**, not a planning session.

### 8.4 Deep Work Block (10:00 – 12:00)

**Purpose:** Execute the highest-priority P1 item requiring sustained focus.

**Rules:**
- Work on the **single highest-priority Active P1** that is not blocked.
- Minimise context switching.
- If interrupted by a genuinely urgent P1 event, handle it, then return.

### 8.5 Operational Window (13:00 – 15:00)

**Purpose:** Absorb the inherently fragmented parts of the role.

**Activities:**
- Incoming tickets and triage
- Team support and unblocking
- Slack / email / ad hoc requests
- Quick decisions and stakeholder responses

**This window is where interruptions are expected and budgeted for.** They are not failures of planning.

### 8.6 Flexible Block (15:00 – 16:30)

**Purpose:** Second execution window or continued operational work, depending on the day.

**Options:**
- Second deep work session (if P1 work remains)
- Compliance / release management tasks
- Backlog refinement or PI Planning preparation
- Continued operational work if the day has been heavy

### 8.7 Afternoon Reset Block (16:30 – 17:00)

**Purpose:** Close the day cleanly. Prevent drift. Hand a clean system to tomorrow.

**Procedure:**

| Step | Duration | Action |
|---|---|---|
| 1. Update All Touched Items | 10 min | Every item worked on today: update State, Next Action, Waiting On as needed. |
| 2. Move Blocked Work | 5 min | Anything now blocked → State = Waiting, Waiting On populated. |
| 3. Reconcile Plan vs Reality | 10 min | What didn't get done? Why? (Interruptions? Underestimated? Dependency?) This builds accuracy over time. |
| 4. Prepare Tomorrow | 5 min | Identify likely P1s for tomorrow. No detailed plan, just awareness. |

**Exit Criteria:**
Every open item has a current State, a defined Next Action, and no ambiguity about what happens next.

---

## 9. Weekly Operating Rhythm

### 9.1 Weekly Review (60 minutes, same time each week)

**Purpose:** Reset the entire system. Prevent silent drift across all domains.

This is the session that maintains long-term control. Daily rituals keep the day manageable; the weekly review keeps the week and beyond on track.

**Procedure:**

| Step | Duration | Action |
|---|---|---|
| 1. Review All Open Work | 20 min | Examine every item across all domains (Dev, Release, Compliance, Product). For each: Is it still relevant? Is the State correct? Does it have a clear Next Action? |
| 2. Clean Waiting On | 10 min | Chase anything stale. Escalate if necessary. Remove items no longer blocked. |
| 3. Reassign Priorities | 15 min | Re-evaluate all P1/P2/P3 assignments. Be decisive: what is truly P1 this week? What can be downgraded? |
| 4. Plan the Week | 15 min | Identify the major items for the week. Optionally block 3–5 deep work slots in the calendar. Set or adjust internal deadlines. |

### 9.2 Internal Deadline Management

External deadlines (court dates in legal terms; release dates, audit dates, PI boundaries in yours) are immovable.

For every external deadline, an **internal deadline** must be established that is pulled forward:

| External Deadline | Internal Deadline | Buffer |
|---|---|---|
| Friday delivery | Wednesday: first draft complete | Thursday: review + contingency |
| End of PI | 1 week prior: all items in final validation | Remaining week: buffer for issues |
| Audit date | 2 weeks prior: all documentation complete | Remaining time: review cycles |

This creates space for the unexpected, which always occurs.

---

## 10. Interruption Handling Protocol

### 10.1 Triage Decision Framework

When a new piece of work arrives (ticket, request, message, escalation), the following decision process applies immediately:

```
New work arrives
       │
       ▼
  Capture in ADO immediately
  (create/update work item)
       │
       ▼
  Assign Priority
       │
       ├── P1 (urgent + important)
       │     → Interrupts current work
       │     → Begin immediately
       │
       ├── P2 (important, not urgent)
       │     → Add to Active list
       │     → Continue current work
       │
       └── P3 (background)
             → Add to Scheduled
             → Continue current work
```

### 10.2 Context Switching Protocol

When switching tasks (whether by choice or interruption):

1. **Pause cleanly:** Update the current item's Next Action to reflect exactly where you stopped.
2. **Switch:** Begin the new item.
3. **Return:** When returning to the original item, the Next Action tells you exactly where to resume.

This eliminates the cognitive cost of re-orientation after interruption.

### 10.3 Capacity Allocation

The day is not planned at 100% capacity. Approximate allocation:

| Allocation | Percentage | Purpose |
|---|---|---|
| Planned work | 60–70% | P1 and P2 items selected during Control Block |
| Shock absorber | 30–40% | Unexpected interruptions, urgent requests, ad hoc support |

This buffer is what allows the system to absorb disruption without collapsing.

---

## 11. Dependency Management ("Hot Potato" Protocol)

### 11.1 Principle

If something is with someone else, **it is still owned by you until done.** Delegation of a step does not transfer accountability for the outcome.

### 11.2 Required Tracking

Every item in Waiting state must have:

| Field | Content |
|---|---|
| **Waiting On** | Specific person or team |
| **Next Action** | What that person needs to do (for your reference) |
| **Follow-up Date** | When you will chase if no movement |

### 11.3 Follow-up Cadence

| Elapsed Time Without Update | Action |
|---|---|
| 2 days | First chase (message or email) |
| 4 days | Second chase + verbal follow-up |
| 7 days | Escalation or re-prioritisation |

Stale waiting items are surfaced automatically by **Query 4 (Waiting On, Stale)** during the Morning Control Block.

---

## 12. Governing Rules

These rules are non-negotiable. They are the constraints that make the system function.

1. **Never hold tasks in your head.** Every actionable item is in ADO.
2. **Never leave a task without a Next Action.** If you touch it, update it.
3. **Never leave blocked work without a Waiting On.** If you can't act, name who can.
4. **Only work from the Active list.** The dashboard and queries decide priority, not memory or instinct.
5. **Limit daily focus to 2–3 P1 priorities.** More than that is dilution, not productivity.
6. **Don't start the day with execution. Start with orientation.** The Morning Control Block comes before all other work.
7. **Close the day with closure, not abandonment.** The Afternoon Reset ensures nothing drifts overnight.
8. **Treat internal deadlines as real deadlines.** External deadlines are too late to be useful targets.
9. **Interruptions are expected, not exceptional.** The system is designed to absorb them, not prevent them.
10. **Update state continuously, not in batches.** Real-time accuracy enables real-time decisions.

---

## 13. Acceptance Criteria

The system is considered operational and effective when the following conditions are consistently met:

| # | Criterion | Measurement |
|---|---|---|
| 1 | All actionable work exists in ADO with correct State, Priority, and Next Action. | Spot check during Weekly Review. Zero orphaned items. |
| 2 | No item remains in Waiting state for >2 days without a follow-up action. | Query 4 returns zero stale items at end of each day. |
| 3 | The Morning Control Block is completed before Standup on every working day. | Self-assessed daily compliance. |
| 4 | The Afternoon Reset is completed before end of day on every working day. | Every item touched that day has current State and Next Action at 17:00. |
| 5 | The Weekly Review is completed every week without exception. | Calendar commitment honoured; all open items reviewed. |
| 6 | No more than 5–7 items in Active state at any time. | Dashboard tile count for Active Work. |
| 7 | Daily P1 selection is limited to 2–3 items. | Morning Control Block output. |
| 8 | The day's structure (Control → Standup → Deep → Operational → Flexible → Reset) is followed. | Self-assessed daily compliance. |
| 9 | Priority decisions are made explicitly using P1/P2/P3, not implicitly by recency or requester seniority. | Observable in ADO field consistency. |
| 10 | Blocked items are transitioned to Waiting immediately upon encountering a dependency, not deferred. | No Active items with unresolved external dependencies. |

---

## 14. Future Enhancements (Out of Scope for v1.0)

The following are identified opportunities for automation and enhancement once the base system is stable and consistently followed:

| Enhancement | Description | Prerequisite |
|---|---|---|
| **Power Automate: Stale Alerts** | Automated notification when a Waiting item exceeds 2 days without update. | Consistent use of Waiting state + Changed Date field. |
| **Power Automate: Deadline Warnings** | Alert when a Due Date is within 3 days and State is not Done. | Consistent use of Due Date field. |
| **Dashboard: Blocked Work Trends** | Power BI report showing average time in Waiting state by person/team. | Sufficient historical data (4+ weeks). |
| **Dashboard: Throughput Analysis** | Items completed per week by domain, to inform capacity planning. | Consistent use of Done state + domain tagging. |
| **AI Augmentation** | Automated Next Action suggestions, state summarisation. | Mature, clean data in ADO. |

---

## 15. Document Control

| Version | Date | Change Description |
|---|---|---|
| 1.0 | 6 April 2026 | Initial version. |

---

*This plan is a living document. It should be reviewed and updated as part of the Weekly Review whenever a structural change to the system is identified.*
