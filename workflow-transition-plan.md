---
layout: default
title: Workflow Transition Plan
---

# Workflow Transition Plan

**Document ID:** WTP-001  
**Version:** 1.0  
**Effective Date:** 7 April 2026  
**Parent Document:** WVP-001 (Workflow Validation Plan)  
**Author:** David Blair

---

## 1. Purpose

This document defines a phased approach to transition from current working practices to the system described in the Workflow Validation Plan (WVP-001). The transition is designed to be incremental, building one layer at a time, so that each phase is stable before the next begins.

The underlying principle: **change the structure first, then change the behaviour, then refine.**

Attempting to adopt the full system in a single step will fail. The daily rhythm depends on the ADO structure. The ADO structure depends on a clean inventory of work. The inventory depends on an honest audit of where things currently live.

So the sequence matters.

---

## 2. Current State Assessment

Before any changes, you need a clear picture of where things stand today. This is not a planning exercise. It is a fact-finding one.

### 2.1 Work Location Audit

Answer these questions honestly:

| Question | Where to Look |
|---|---|
| Where do your tasks currently live? | ADO? Outlook? Sticky notes? Your head? Teams messages? |
| How many open items do you have right now? | Count them. All of them. |
| How many of those are blocked by someone else? | Estimate. |
| How many have no clear next step? | Estimate. |
| What recurring work do you do that is never tracked? | Compliance checks, release prep, team support, etc. |

### 2.2 Honest Starting Point

Most people in this situation find:

- 30-50% of active work exists only in their head
- Waiting/blocked items have no tracking at all
- Priority is determined by whoever asked most recently
- There is no consistent distinction between "I can act on this" and "I am waiting"

If that matches your reality, the transition plan is designed for exactly this starting point.

---

## 3. Transition Phases

The transition has four phases over approximately four weeks. Each phase has a clear goal, specific actions, and a checkpoint before moving on.

```
Week 1: Foundation (ADO structure + work capture)
Week 2: Visibility (queries, dashboard, initial rhythm)
Week 3: Rhythm (full daily and weekly operating cadence)
Week 4: Stabilisation (refine, fix friction, confirm habits)
```

---

## Phase 1: Foundation (Week 1)

**Goal:** Get the ADO structure right and capture all existing work into it.

Nothing else matters until the system of record is clean.

### Day 1-2: ADO Configuration

These are one-time setup tasks.

**1. Add custom fields to your work item types:**

| Field | Type | Notes |
|---|---|---|
| Priority | Picklist (P1/P2/P3) | If a Priority field already exists, decide whether to use it or create a custom one matching your definitions. |
| Next Action | Plain Text | Add to the work item form in a prominent position. |
| Waiting On | String or Identity | Add to the form, ideally near State. |
| Follow-up Date | Date | Optional for now. Add the field but don't enforce it yet. |

**2. Configure workflow states:**

Add or rename states to match:

- Scheduled
- Active
- Waiting
- Done

If your process template makes this difficult (e.g. shared across teams), use a workaround: create a custom "Workflow State" picklist field and use that instead of modifying the built-in state. The queries can filter on either.

**3. Customise the work item form layout:**

Ensure the following are visible without scrolling:

- Title
- State (or Workflow State)
- Priority
- Next Action
- Waiting On

### Day 3-5: The Big Capture

This is the most important step in the entire transition. You are going to get everything out of your head and into ADO.

**Procedure:**

1. **Block 2-3 hours.** This is not a quick task.

2. **Brain dump.** Write down every piece of work you are currently responsible for, aware of, or worried about. Do not filter. Do not prioritise. Just list.
   - Development tasks
   - Product decisions pending
   - Things your team needs from you
   - Release items
   - Compliance and validation work
   - Follow-ups you owe people
   - Things people owe you
   - Anything that nags at you

3. **Check other sources.** Go through:
   - Existing ADO items assigned to you
   - Outlook inbox (flagged items, unresolved threads)
   - Teams/Slack messages with open commitments
   - Meeting notes from the last 2 weeks
   - Any personal notes, lists, or sticky notes

4. **Create ADO items for everything.** Every single thing from steps 2 and 3 that doesn't already have an ADO item gets one now.

5. **For each item, fill in:**
   - Title (action-oriented, specific)
   - State: Active, Waiting, or Scheduled
   - Priority: P1, P2, or P3
   - Next Action: one concrete step
   - Waiting On: if applicable

**Do not try to be perfect.** Your priority assignments will be rough. Your next actions might be vague. That is fine. The goal is completeness, not precision. Precision comes in Phase 2.

### Phase 1 Checkpoint

Before moving on, confirm:

- [ ] Custom fields exist and are visible on work item forms
- [ ] States are configured (or workaround field is in place)
- [ ] Every known piece of work has an ADO item
- [ ] Each item has at minimum: State, Priority, and a rough Next Action
- [ ] Nothing significant is still living only in your head, inbox, or notes

---

## Phase 2: Visibility (Week 2)

**Goal:** Build the queries and dashboard so you can actually see your work clearly. Begin a simplified daily routine.

### Day 1-2: Create Queries

Build the six queries defined in WVP-001 Section 5:

1. **My Active Work** (Assigned to me, State = Active, sorted by Priority then Due Date)
2. **My Active P1** (Assigned to me, State = Active, Priority = P1)
3. **Waiting On (All)** (Assigned to me, State = Waiting)
4. **Waiting On (Stale)** (Assigned to me, State = Waiting, Changed Date < @Today - 2)
5. **Due This Week** (Assigned to me, Due Date <= @Today + 7, State != Done)
6. **Team Blockers** (Team members, State = Waiting/Blocked, Priority = P1/P2)

**Configure columns** on each query to show only: Title, Priority, State, Due Date, Waiting On, Changed Date.

Save them in a personal folder. Favourite them.

### Day 3: Build the Dashboard

Create a new ADO dashboard using the layout from WVP-001 Section 6:

- Top row: Query tiles (My Active P1 count, Due This Week count, Waiting On count)
- Middle row: Query result lists (My Active Work, Waiting On Stale)
- Bottom row: Team Blockers

This should take 30-60 minutes.

### Day 4-5: Begin Simplified Morning Routine

You are not yet following the full daily rhythm. Instead, do a stripped-down version:

**Each morning, spend 15-20 minutes:**

1. Open the dashboard
2. Look at Waiting On (Stale). Chase one or two items.
3. Look at My Active P1. Confirm these are still your top priorities.
4. Pick 2-3 things to focus on today.

That's it. No afternoon reset yet. No full control block. Just get used to starting the day by looking at ADO instead of reacting to whatever arrives first.

### Phase 2 Checkpoint

Before moving on, confirm:

- [ ] All 6 queries are built and returning sensible results
- [ ] Dashboard is created and usable
- [ ] You have done the simplified morning routine for at least 3 consecutive days
- [ ] You have chased at least one stale Waiting item using the query
- [ ] The queries are revealing things you would have otherwise forgotten

---

## Phase 3: Rhythm (Week 3)

**Goal:** Adopt the full daily and weekly operating rhythm from WVP-001.

### Introduce the Morning Control Block

Expand the simplified morning routine into the full 40-minute block (08:45 to 09:25):

| Step | Duration | Action |
|---|---|---|
| Clear Waiting On | 10-15 min | Chase stale items, send messages |
| Review Deadlines | 10 min | Check Due This Week, pull forward at-risk items |
| Decide the Day | 15 min | Select 2-3 P1s and 1-2 P2s, update Next Actions |

**Block this in Outlook as a recurring event.** It is not optional.

### Introduce the Afternoon Reset

Add the 30-minute reset (16:30 to 17:00):

| Step | Duration | Action |
|---|---|---|
| Update touched items | 10 min | State, Next Action, Waiting On |
| Move blocked work | 5 min | Anything blocked goes to Waiting |
| Reconcile plan vs reality | 10 min | What didn't get done? Why? |
| Prepare tomorrow | 5 min | Identify likely P1s |

**Block this in Outlook as a recurring event.**

### Introduce the Interruption Protocol

When new work arrives during the day:

1. Create/update ADO item immediately
2. Assign priority
3. P1: interrupt current work. P2/P3: leave in system, continue.

This will feel slow at first. You will be tempted to just "quickly handle" things without logging them. Resist that. The logging is the system.

### Introduce the Weekly Review

Schedule a 60-minute recurring block. Same day, same time, every week.

Follow the procedure from WVP-001 Section 9:

1. Review all open work (20 min)
2. Clean Waiting On (10 min)
3. Reassign priorities (15 min)
4. Plan the week (15 min)

### Adopt the Day Structure

Begin shaping your days around the intended structure:

```
08:45  Control Block
09:30  Standup
10:00  Deep Work
12:00  Lunch
13:00  Operational Window
15:00  Flexible Block
16:30  Reset
```

Do not force this rigidly in week 3. Aim for the shape, not perfection. Some days will not fit. That is expected.

### Phase 3 Checkpoint

Before moving on, confirm:

- [ ] Morning Control Block completed on at least 4 out of 5 days
- [ ] Afternoon Reset completed on at least 4 out of 5 days
- [ ] At least one Weekly Review completed
- [ ] New incoming work is being captured in ADO before being acted on (most of the time)
- [ ] You are picking daily P1s deliberately, not by default
- [ ] Blocked items are moving to Waiting state on the same day they become blocked

---

## Phase 4: Stabilisation (Week 4)

**Goal:** Identify friction, fix it, and confirm the system is sustainable.

### What to Watch For

By week 4, you will have enough experience to notice where the system rubs:

| Symptom | Likely Cause | Fix |
|---|---|---|
| Morning Control Block feels rushed | Too many items in Active | Enforce the 5-7 Active item limit more aggressively. Push things to Scheduled. |
| Queries return too many results | Priority inflation (too many P1s) | Be more honest about what is truly P1. Most things are P2. |
| Items go stale in Waiting without you noticing | Not checking Query 4 daily | Make it the first thing in the Control Block, not the last. |
| Afternoon Reset gets skipped | End-of-day fatigue or overrun | Move it 30 minutes earlier if needed. Or do a minimal version (just update states). |
| New work still not being captured | Habit not formed yet | Put a physical reminder near your monitor: "Is it in ADO?" |
| Next Action field is often empty | Feels like overhead | Reframe it: this is a message to your future self. Even "Review and decide next step" is better than blank. |
| Weekly Review feels too long | Reviewing items that haven't changed | Consider marking items as "reviewed" during the weekly pass so you can skip unchanged ones. |

### Refine

Make small adjustments:

- Tweak query filters if they are too broad or narrow
- Adjust dashboard widgets if some are not useful
- Add or remove a column from query results
- Change the time of the Control Block or Reset if needed

Do not redesign the system. Adjust within it.

### Phase 4 Checkpoint (Go/No-Go for Steady State)

The system is considered operational when all of the following are true for at least one full working week:

- [ ] All work is in ADO. No significant items living elsewhere.
- [ ] Morning Control Block and Afternoon Reset happen daily.
- [ ] Weekly Review happens on schedule.
- [ ] Waiting items are chased within 2 days consistently.
- [ ] Daily P1 selection is a deliberate choice, not a reaction.
- [ ] You can open the dashboard and know within 60 seconds what needs your attention.
- [ ] The system survives a disruptive day (heavy interruptions, unplanned work) without collapsing.

If these are met, you are operating under WVP-001.

---

## 4. Common Failure Modes

Things that will threaten the transition. Name them now so you recognise them when they appear.

### "I'll start properly on Monday"
The capture step (Phase 1, Day 3-5) is the hardest part. It is tempting to defer it. Do not. Until everything is in ADO, you are running two systems (your head and ADO), which is worse than running one.

### Perfectionism in the capture phase
You will want every item to have a perfect title, correct priority, and a clear next action before you create it. This kills momentum. Create the item. Fill in what you can. Fix it during Phase 2 or the first Weekly Review.

### Skipping the Afternoon Reset
This is the first ritual that gets dropped under pressure. When this happens consistently, items stop being updated, states drift, and the morning Control Block becomes unreliable because the data is stale. Even a 10-minute shortened version is better than skipping it.

### Making everything P1
If you have more than 3-4 P1 items on any given day, your priority system is failing. P1 means "this must move today." Not "this is important." Not "someone senior asked for it." Most work is P2. Accept that.

### Not capturing interruptions
The hardest habit to build. When something urgent lands, the instinct is to just do it. That works in the moment but breaks the system, because untracked work creates invisible load and makes your queries incomplete. Capture first, then act. Even if capture means a 30-second ADO item with just a title.

---

## 5. What Success Looks Like

After four weeks, the practical difference should be:

**Before:**
- Start the day unsure what matters most
- Blocked work drifts silently
- Interruptions feel chaotic and derailing
- End the day uncertain about what was accomplished or what was missed
- Carry a background anxiety about forgotten commitments

**After:**
- Start the day knowing exactly what matters
- Blocked work is visible, tracked, and chased on a schedule
- Interruptions are absorbed into a system that expects them
- End the day with a clean, updated record of all open work
- Carry nothing in your head that is not also in ADO

---

## 6. Document Control

| Version | Date | Change Description |
|---|---|---|
| 1.0 | 7 April 2026 | Initial version. |
