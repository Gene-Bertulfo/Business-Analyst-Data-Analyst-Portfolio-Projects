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

### Safe column mapping
Every incoming campaign list has different column names and different column counts. This function matches incoming column names (like "cust id" or "mobile number") to a standard internal set of field names but only when it's certain.

```python
def map_columns(incoming_columns):
    alias_lookup = build_alias_lookup()
    mapping = {}
    unmapped = []
    suggestions = {}

    for col in incoming_columns:
        norm = _normalize(col)
        if norm in alias_lookup:
            mapping[col] = alias_lookup[norm]
            continue
        unmapped.append(col)
        close = get_close_matches(norm, alias_lookup.keys(), n=1, cutoff=0.6)
        if close:
            suggestions[col] = alias_lookup[close[0]]

    return mapping, unmapped, suggestions
```

### Composite-key matching (VIN + Customer ID)
A VIN alone isn't a reliable match, vehicles change ownership. A Customer ID alone isn't reliable either, one customer can own multiple vehicles.
So the system builds two lookup indexes and only counts a "Full Match" when a record appears in **both**: 

```python

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
```

### Full Match
Once a lead is confirmed as a Full Match (same VIN + same Customer ID already in the Masterfile), the system only updates one thing, the last service appointment date and never touches the customer's disposition.

```python

if full_match is not None:
            row_num = full_match["_row"]
            master_date_ts = pd.to_datetime(full_match.get("LAST SERVICE APPT"), errors="coerce")
            lead_date = lead["LAST SERVICE APPT"]

            if pd.notna(lead_date) and (pd.isna(master_date_ts) or lead_date > master_date_ts):
                ws.cell(row=row_num, column=col_index["LAST SERVICE APPT"]).value = lead_date.to_pydatetime()
                action = "Updated LAST SERVICE APPT to newer date from campaign"
            else:
                action = "Kept existing Masterfile date (already most recent, or campaign had none)"

            log.append({"VIN": vin, "CUSTOMER ID": cust_id, "MATCH TYPE": "Full Match", "ACTION": action})
```

### VIN Conflict
Handles the case where the VIN already exists in the Masterfile, but it's tied to a different Customer ID than the incoming lead, meaning either the vehicle changed ownership, or there's a data error somewhere.

```python
elif vin_matches:
    existing_cust = vin_matches[0]["_CUSTID"]
    log.append({
        "VIN": vin, "CUSTOMER ID": cust_id,
        "MATCH TYPE": "CONFLICT - VIN linked to a different Customer ID",
        "ACTION": f"Not touched. Masterfile has this VIN under Customer ID {existing_cust}. Review manually.",
    })
```

### Possible 2nd Vehicle
Handles the case where the Customer ID already exists, but with a different VIN likely an existing customer who bought or leased another vehicle.

```python
elif cust_matches:
    # NOT added to the Masterfile — reported only, per your call.
    log.append({
        "VIN": vin, "CUSTOMER ID": cust_id,
        "MATCH TYPE": "New Vehicle for Existing Customer",
        "ACTION": "NOT added to Masterfile. Reported only — review and add manually if valid.",
    })
```

### New Lead
The only case where a new row actually gets added, this only runs when neither the VIN nor the Customer ID matched anything already in the Masterfile.

```python
else:
    values = {col: lead[col] for col in STANDARD_COLUMNS if col in col_index}
    values["STATUS"] = "New Lead"
    current_last_row = append_row_to_table(ws, table, col_index, current_last_row, values)

    new_rec = dict(values)
    new_rec["_row"] = current_last_row
    new_rec["_VIN"] = vin
    new_rec["_CUSTID"] = cust_id
    vin_index.setdefault(vin, []).append(new_rec)
    cust_index.setdefault(cust_id, []).append(new_rec)

    log.append({"VIN": vin, "CUSTOMER ID": cust_id, "MATCH TYPE": "New Lead", "ACTION": "Added as new row"})
```

### Openpyxl formatting-preservation
Adds a new lead as a properly formatted row inside the client's existing Excel Table, instead of blowing away all their colors, filters, and banding the way a plain pandas export would.

```python
def append_row_to_table(ws, table, col_index, last_row, values):
    new_row = last_row + 1
    for header, col in col_index.items():
        template_cell = ws.cell(row=last_row, column=col)
        new_cell = ws.cell(row=new_row, column=col)
        new_cell.value = _to_excel_value(values.get(header))
        new_cell._style = copy_style(template_cell._style)

    min_col, min_row, max_col, _ = range_boundaries(table.ref)
    new_ref = f"{get_column_letter(min_col)}{min_row}:{get_column_letter(max_col)}{new_row}"
    table.ref = new_ref
    if table.autoFilter is not None:
        table.autoFilter.ref = new_ref

    return new_row
```

### Dynamic Next Reach-Out Date
Once a lead's disposition (STATUS) is set, this formula automatically calculates when that lead should be contacted again, no agent ever manually enters a follow-up date.

```excel
=IF(OR([@STATUS]="Ghost call", [@STATUS]="Left voicemail", [@STATUS]="Left voicemail & sent sms", [@STATUS]="Left voicemail, sent sms, sent email", [@STATUS]="Mailbox is full", [@STATUS]="VM not setup", [@STATUS]="Customer disconnected"), WORKDAY([@[LAST REACH OUT]],15),
    IF([@STATUS]="For follow-up", WORKDAY([@[LAST REACH OUT]],5),
    IF([@STATUS]="Done",EDATE([@[LAST SERVICE APPT]],7),
    IF([@STATUS]="", WORKDAY([@[LAST REACH OUT]],1),
    IF(OR([@STATUS]="New Lead",[@STATUS]="Missed"), WORKDAY([@[LAST REACH OUT]],1), "")))))
```

### Today() Filter
This Power Query step filters ToReachOut.xlsx down to only leads whose NEXT REACH OUT date is today, so agents only ever see what's due right now.

### EOD Disposition Update
### VBA Macro
### Appointment
### Callback
