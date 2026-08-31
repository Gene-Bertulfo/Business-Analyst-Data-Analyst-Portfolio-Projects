# Solving Reporting Blind Spots: A Ground-Up Call Center KPI Dashboard

**Workflow:** [Miro Board] https://miro.com/app/board/uXjVHsw84PM=/?share_link_id=231602619900

## Goal
Replace a manual, narrative-driven reporting process with a live, metrics-first dashboard, one that tracks agent-level performance daily instead of only in hindsight, surfaces trends and branch comparisons automatically, and removes the need to manually re-derive the same insights from scratch every week.

## Description

*This project is a replica of my actual work, except I used Power BI for my personal project, in my actual work, the dashboard was built in Excel.*

Every day, I sent EOD reports covering disposition totals and decline reasons, and every Friday I consolidated the week into a summary report. I later discovered that summary wasn't just for internal review, it was being forwarded directly to the client, rewritten as narrative prose. The numbers were mine, but restated as a long-form article instead of being structured around the metrics that actually explain performance. On top of that, agent-level KPIs weren't tracked daily, so performance issues went unnoticed until they'd already compounded, and there was no trend visibility or branch comparison, every insight had to be manually re-derived each week from data that had already been compiled once.

To fix this, I built a full data pipeline: pulling seven source sheets across two Excel workbooks (call logs, appointments, callback responses, hours worked, store info, employee info, and disposition data) into Power Query, layering a Rolling Calendar for time-based analysis, and modeling relationships in Power Pivot. Since I didn't have access to softphone-level call data, I engineered an Outbound Weight proxy metric by combining Leads Worked and Callback counts. The final model feeds a Power BI dashboard with separate views for General Managers, Team Leads, and Agents, each structured around the KPIs that role actually needs to act on.

## Results
Idle Rate has held steady at ~47-48% for months, a structural pattern, not a one-off  
Given CDJR's structurally capped lead supply (~781/dealership/month) and the brief, single-purpose nature of each call, current volume may not be sufficient to keep the full team continuously active, worth investigating as a demand or staffing question rather than an agent productivity issue  
GM Dashboard revealed Westboro leads in Connect volume, yet Millbury converts at a higher rate with fewer connects  
TL Dashboard traced this back to Westboro also carrying the highest Disconnect Rate (37% of leads worked), explaining why their high connect volume wasn't converting  

## Teck Stack
Excel Power Query, Power BI, DAX

## Skills Demonstrated
ETL & data modeling  
Proxy-metric engineering  
DAX measure development  
Multi-tier dashboard design  
Root-cause diagnostic analysis  
Market-sizing/capacity reasoning  
Reporting integrity awareness

## In-depth details of the project
