# AI Adoption & Workforce Impact Analysis

---

## The Question Nobody Could Agree On

 # AI reshaping the global workforce gradually, or did something break, all at once, the moment generative AI tools hit the mainstream?

That was the brief handed down from the director: investigate how AI adoption is reshaping employment across countries, industries, and skill categories between 2021 and 2024, with particular attention to what changed once generative AI arrived in late 2022. A policy coalition needed the answer before deciding where to send reskilling funding, and "AI is disrupting jobs" isn't specific enough to write a check against. They needed to know *which* jobs, *where*, and *how fast*.

So I pulled 300 quarterly records spanning **30 countries, 25 industries, and 8 skill categories**, built a full Power BI data model from scratch, and went looking for the answer.

The answer, it turns out, is the second one. AI adoption didn't creep upward. It nearly doubled in a single quarter.

---

## Approach

**1. Data validation first, visuals second.**
Before touching a single chart, I checked for data quality issues. Two columns — `avg_wage_change_pct` and `ai_tool_usage_hours_per_week`, had nulls. The obvious shortcut is to fill nulls with zero, but zero is already a *real, meaningful* value for both fields (0% wage change, 0 hours of AI tool use). Filling nulls with zero would have silently mixed "we don't know" with "the answer is zero", quietly biasing every average downstream. Instead, I imputed using the **segment median** (grouped by skill category + generative AI era) and flagged every imputed row with a boolean column, so nothing pretends to be more certain than it is.

**2. A proper star schema.**
One fact table (`fact_workforce_ai_index`) connected to four dimension tables, country, industry, skill category, and date, each validated for referential integrity: zero orphan foreign keys, zero duplicate grain, every dimension key unique. `dim_date` was marked as a date table, and because the data is genuinely quarterly (not daily transactions rolled up), I used index-based DAX (`date_id - 1`, `date_id - 4`) for QoQ/YoY logic rather than forcing a daily calendar table onto data that doesn't have daily granularity.

**3. 26+ DAX measures, each tied to a specific question in the brief.**
Not measures for measures' sake, every one traces back to a guiding question: *Which industries combine high automation exposure with low AI investment? Which skill categories have the biggest gap between replaceability and reskilling time? Which country-industry combination has the largest shortfall between risk and reskilling spend?*

**4. A 3-page report that argues, not just displays.**
Executive Summary → Detailed Analysis → Actionable Insights, mirroring the brief's own deliverable structure, so each page has a distinct job: show the trend, diagnose where the problem is, and recommend what to do about it.

---

## Insights

**AI adoption nearly doubled the moment generative AI went mainstream.**
Average adoption sat in a narrow 16–22% band through 2021 and most of 2022, then jumped to 40.2% in Q4 2022 and has held in the mid-30s to mid-40s ever since. Not a trend line. A step-change.

**Job displacement has outpaced job creation in every single quarter.**
586,785 jobs displaced against 296,355 created, a net shortfall of 290,430 jobs, with no quarter bucking the pattern.

**The riskiest industry isn't the only one worth worrying about.**
Information Technology tops the displacement risk index (5.18/10), unsurprising. But Food & Beverage sits right behind it at 5.11/10, a sector that barely registers in most AI policy conversations despite carrying nearly as much exposure.

**Infrastructure is a weaker shield than everyone assumes.**
Digital infrastructure quality barely correlates with displacement risk (r ≈ -0.19). STEM graduate density does more work (r ≈ -0.32) — skills pipelines appear to matter more than fiber-optic cables.

**The developed-vs-emerging adoption gap is smaller than the headlines suggest.**
30.98% vs. 30.22%. AI adoption is not simply tracking traditional economic development lines.

**"Advanced" policy maturity is quietly outperforming "Established."**
Countries with Advanced AI policy maturity post a 0.60 job creation-to-displacement ratio, better than the supposedly more mature "Established" tier's 0.44.

**The clearest red flag in the whole dataset: money isn't following risk.**
Italy's Food & Beverage sector carries the **maximum possible displacement risk score** in the entire dataset, a perfect 10 out of 10, yet receives just **$5.72** in reskilling investment per displaced worker. The thinnest safety net attached to the highest exposure anywhere in the study.

---

## Recommendation

Funding should follow the risk, not the headlines.

**Immediate priority:** Italy's Food & Beverage sector, Nigeria's Retail & E-commerce workforce, and Singapore's Healthcare sector, not because they're the most visible, but because they're the most exposed and the least protected.

**Study, don't just spend:** rather than pouring further money into infrastructure, coalitions would do better studying what "Advanced" policy-maturity countries are doing right, their approach to reskilling is already outperforming more "Established" peers, and that playbook is more replicable than raw infrastructure spend.

**Watch the mid-tier risk, not just the top.** Food & Beverage's position at #2 in displacement risk, while receiving almost no dedicated policy attention, is exactly the kind of blind spot that turns into next year's emergency.

---

## Tools Used

**Power BI** — star-schema data modeling, DAX measure development, 3-page interactive report design.

## Acknowledgements

Built for the **Onyx Data DataDNA Challenge**. Thanks to Onyx Data for a brief that was genuinely worth digging into.

*Note: this analysis is based on a synthetic dataset (300 records, 2021–2024) prepared for the purposes of this challenge exercise. Findings should be treated as illustrative of analytical method and directional insight.*
