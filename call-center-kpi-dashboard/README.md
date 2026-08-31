# Solving Reporting Blind Spots: A Ground-Up Call Center KPI Dashboard

**Workflow:** [Miro Board] https://miro.com/app/board/uXjVHsw84PM=/?share_link_id=231602619900

## Goal
Replace a manual, narrative-driven reporting process with a live, metrics-first dashboard, one that tracks agent-level performance daily instead of only in hindsight, surfaces trends and branch comparisons automatically, and removes the need to manually re-derive the same insights from scratch every week.

## Description



Every day, I sent EOD reports covering disposition totals and decline reasons, and every Friday I consolidated the week into a summary report. I later discovered that summary wasn't just for internal review, it was being forwarded directly to the client, rewritten as narrative prose. The numbers were mine, but restated as a long-form article instead of being structured around the metrics that actually explain performance. On top of that, agent-level KPIs weren't tracked daily, so performance issues went unnoticed until they'd already compounded, and there was no trend visibility or branch comparison, every insight had to be manually re-derived each week from data that had already been compiled once.

To fix this, I built a full data pipeline: pulling seven source sheets across two Excel workbooks (call logs, appointments, callback responses, hours worked, store info, employee info, and disposition data) into Power Query, layering a Rolling Calendar for time-based analysis, and modeling relationships in Power Pivot. Since I didn't have access to softphone-level call data, I engineered an Outbound Weight proxy metric by combining Leads Worked and Callback counts. The final model feeds a Power BI dashboard with separate views for General Managers, Team Leads, and Agents, each structured around the KPIs that role actually needs to act on.
