# Solving Duplicate Lead Calls: A Ground-Up Lead Management System

**Workflow:** [Miro Board] https://miro.com/app/board/uXjVHsqUTgE=/?share_link_id=366313810124

## Goal
Build a single, self-maintaining system to replace a manual lead process with no structure,
one that eliminates duplicate outreach, standardizes messy incoming data automatically, and
gives every agent a live, accurate list of who to call and when, without manual clean up.

## Description
When I joined a brand-new call center operation, there was no lead management process in 
place, this was the client's first time outsourcing outreach, so nothing existed to prevent 
the same customer from being called multiple times. That gap became obvious fast: on our 
very first campaign, leads were called twice in one day and up to 4-5 times each due to
undetected duplicates, eventually leading to a direct customer complaint. Lead management 
wasn't part of my role, but since no one else was solving it, I built the fix myself.

The system is centered on a masterfile that cross-checks every new lead list against existing 
records using a composite VIN + Customer ID key, automatically classifying each lead as new, 
a possible second vehicle, a full match, or a VIN conflict. From there, leads are 
automatically queued into a daily call list (filtered to only what's due today), synced live 
to agents via OneDrive. As calls are logged, dispositions flow back into the masterfile, 
next-reach-out dates update automatically based on the outcome, and leads needing appointments 
or callbacks are routed into their own tracking views, including reopening missed callbacks 
for any available agent to pick up.

## Result
~2 hours saved on every list, scaling with volume
| 5,000+ leads instantly integrated with self-assigning contact dates, no manual sorting
| 100% of today's leads surfaced in one refresh
| Live: agent outreach synced to file automatically

## Teck Stack
Python, Excel Power Query, Excel VBA Macro, OneDrive live sync, Miro

## Skills Demonstrated
Process Mapping
Iterative debugging against real data
Requirements gathering & scooping under real ambiguity
Deterministic matching logic with composite keys
Translating business rules into working code
Root-cause debugging & controlled rollback
Data governance & audit-trail design

## Technical Details
