
---
title: 'Mapping BlackRock's Capital Influence Network'
description: 'A network analysis revealing how passive index funds shape industry landscapes through strategic holdings.'
pubDate: 'Jul 18 2025'
heroImage: '/network1.png'
---

While analyzing investment banks on SEC.gov, I observed an intriguing pattern: as other firms focus on equity trading, certain institutions like BlackRock quietly accumulate influence through thousands of strategic holdings. This project maps their cross-industry impact using public filings and network visualization.

Explore the interactive <a href="https://example.com/blackrock-network" target="_blank">Network Graph</a>.

## Methodology

### Data Sources
- **SEC Form HF13 filings** (13F Holdings Reports)
- **Public company ownership data** (market cap, sector classification)
- **Policy influence scores** from industry regulatory databases

## Step Breakdown

#### Catalogue
- Data Collection
- SQL Processing
- Network Construction
- Sector Impact Analysis
- Visualization

---

### Part 1. Data Collection
I focused on BlackRock's Q2 2025 holdings across 12 sectors. Raw data included:
- 4,231 holding records
- 78,000+ nested subsidiary relationships
- Market cap data for 1,892 companies

```sql
-- Sample SQL union for subsidiary resolution
WITH RECURSIVE SubsidiaryTree AS (
  SELECT parent_id, child_id, ownership_pct 
  FROM company_relations
  WHERE parent_id = 'BLACKROCK_INC'
  UNION ALL
  SELECT cr.parent_id, cr.child_id, cr.ownership_pct
  FROM company_relations cr
  JOIN SubsidiaryTree st ON cr.parent_id = st.child_id
)
SELECT * FROM SubsidiaryTree;
