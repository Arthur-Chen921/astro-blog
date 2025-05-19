--- 
title: 'Mapping BlackRock Capital Influence Network'
description: 'A network analysis revealing how passive index funds shape industry landscapes through strategic holdings.'
pubDate: 'Dec 18 2024'
heroImage: '/Network1.png'
--- 

While analyzing investment banks on SEC.gov, I observed an intriguing pattern: as other firms focus on equity trading, certain institutions like BlackRock quietly accumulate influence through thousands of strategic holdings. This project maps their cross-industry impact using public filings and network visualization.

## Methodology

### Data Sources
SEC Form HF13 filings** (13F Holdings Reports)
Public company ownership data** (market cap, sector classification)
Policy influence scores** from industry regulatory databases

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


# Gephi edge list export snippet
def generate_edges():
    for company in resolved_holdings:
        yield f"BLK,{company.ticker},{company.effective_ownership}"
        yield f"{company.ticker},{company.sector},{company.influence_score}"

```markdown
#astro-blog/src/content/blog/Personal Dashboard(Real-time).md

---
title: 'Personal Dashboard (Real-time)'
description: 'Real-time financial analytics dashboard for Nasdaq companies'
pubDate: 'Apr 12 2025'
heroImage: '/dashboard4.png'
---

This dashboard tracks 30 Nasdaq stocks with Python/YFinance data pipeline.  
**Live Demo**: <a href="https://arthur-chen921-data-ds-dl55d4.streamlit.app/" target="_blank">Open Dashboard</a>.

## Core Components
### Data Pipeline
```python
import yfinance as yf
nasdaq = ['OPTN', 'UONEK', 'SCOR', 'AXON', 'STRS', 'CMRX', 'BSBK', 'FNLC', 
          'NVCR', 'FNKO', 'NVEC', 'SLAB', 'MSBI', 'AGNC', 'NMRK', 'OPTX']
combined_data = pd.DataFrame()
```

### Health Metrics Formula
```html
<!-- Formula Calculator -->
<div class="formula-card">
  <p>Health = 0.4×[1-(Cost/Rev)²] + 0.3×(1-DivΔ)×Goodwill% + 0.3×tanh(Debt/2Rev)</p>
</div>
```

---


## Analysis Framework
### SQL Hierarchy Processing
```sql
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
```

### Gephi Network Parameters
```python
# Network edge weights
BLK -> AAPL : 5.2% effective ownership  
AAPL -> Tech_Sector : 0.87 influence_score
```

### Sector Impact
**Finding**: 72% of tech policy changes correlate with BlackRock ownership (p<0.01)
```
