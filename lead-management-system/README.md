# Solving Duplicate Lead Calls: A Ground-Up Lead Management System

**Workflow:** [Miro Board] https://miro.com/app/board/uXjVHsqUTgE=/?share_link_id=366313810124

## Goal
Build a single, self-maintaining system to replace a manual lead process with no structure,
one that eliminates duplicate outreach, standardizes messy incoming data automatically, and
gives every agent a live, accurate list of who to call and when, without manual clean up.

## Description

*This project is a replica of my actual work, except I used Power BI for my personal project, in my actual work, the dashboard was built in Excel.*  

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
5,000+ leads instantly integrated with self-assigning contact dates, no manual sorting  
100% of today's leads surfaced in one refresh  
Live: agent outreach synced to file automatically  

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

## In-depth details of the project

### Composite-key matching (VIN + Customer ID)
A VIN alone isn't a reliable match — vehicles change ownership. A Customer
ID alone isn't reliable either — one customer can own multiple vehicles.
So the system builds two lookup indexes and only counts a "Full Match"
when a record appears in **both**: 

'''python

def process_leads_against_table(ws, table, incoming_df):
    records, col_index, header_row, last_row = read_table_records(ws, table)
    for rec in records:
        rec["_VIN"] = str(rec.get("VIN") or "").strip().upper()
        rec["_CUSTID"] = str(rec.get("CUSTOMER ID") or "").strip()

    vin_index = {}
    cust_index = {}
    for rec in records:
        vin_index.setdefault(rec["_VIN"], []).append(rec)
        cust_index.setdefault(rec["_CUSTID"], []).append(rec)

    for _, lead in incoming_df.iterrows():
        vin = lead["VIN"]
        cust_id = lead["CUSTOMER ID"] if pd.notna(lead["CUSTOMER ID"]) else ""
        vin_matches = vin_index.get(vin, [])
        cust_matches = cust_index.get(cust_id, []) if cust_id else []

        full_match = None
        if vin_matches and cust_matches:
            common = [r for r in vin_matches if r in cust_matches]
            if common:
                full_match = common[0]
'''
