---
title: 'Personal Dashboard(Real-time)'
description: 'Here is one of my work of Data collecting, cleaning, analyzing, and interactively presenting.'
pubDate: 'Apr 12 2025'
heroImage: '/dashboard4.png'
---

This is a data dashboard project I created while studying finance. Compared to the index provided by large websites, I am more eager to uncover the many details of economic operations.


## Step decomposition

I selected financial indicators on Yahoo, Alpha Vantage, and FMP, and then used Python's data crawling function to classify and reorganize them.
The reports on these websites are always incomplete, indicating that companies have not measured their respective indicators as expected. But we can still see what these companies have done from the ratio of cash flow to changes in their assets.

#### Catalogue
- Data Collection
- Analysis
  - Income and expenditure analysis
  - Asset proportion
  - Long term financial health
- Outcome

#### Part1.Data Collection

##### I have selected the top thirty Nasdaq stocks updated in real-time from yfinance. Please note that re running them will update the latest list

```markdown
Import yfinance as yf
import pandas as pd
# Define the tickers
# I have selected thirty stocks from the Nasdaq component page on March 22nd␣
↪2025
nasdaq_30 = ['OPTN', 'UONEK', 'SCOR', 'AXON', 'STRS', 'CMRX', 'BSBK', 'FNLC',␣
↪'NVCR', 'FNKO',
'NVEC', 'SLAB', 'MSBI', 'AGNC', 'NMRK', 'OPTX', 'IFRX', 'CVCO',␣
↪'TWST', 'STRT',
'SCNI', 'NMRA', 'SDA', 'KLXE', 'JAGX', 'CEAD', 'NVCT', 'STRR',␣
↪'BCG', 'SCNX']
combined_income = pd.DataFrame()
combined_balance = pd.DataFrame()
combined_cashflow = pd.DataFrame()
```
#### Part2.Data Cleaning
> Convert data format
> Handle missing value
> Process the outliers
> Validate

##### Before Clean

![blog placeholder](/yahoo.png)

##### After clean

![blog placeholder](/afterclean.png)


####### Don't forget to validate

```markdown
> In data collection, it is important to standardize the format at the beginning of the collection process and strictly classify the sources of data reporting
> def handle_missing(df, threshold=0.75):
      # Delete high missing rate columns
      missing_ratio = df.isnull().mean()
      cols_to_drop = missing_ratio[missing_ratio > threshold].index.tolist()
      df_cleaned = df.drop(columns=cols_to_drop)
      # Only process numerical columns
      numeric_cols = df_cleaned.select_dtypes(include=np.number).columns.tolist()
      for col in numeric_cols:
          # Step 1: Fill in missing values with the historical mean of the same␣
      ↪company's column
          df_cleaned[col] = df_cleaned.groupby('Ticker')[col].transform(
              lambda x: x.fillna(x.mean())
          )
          # Step 2: If the company has no historical data, fill in the entire␣
      ↪column with the mean
          df_cleaned[col] = df_cleaned[col].fillna(df_cleaned[col].mean())
      return df_cleaned
def validate_missing(df, name):
    total_missing = df.select_dtypes(include=np.number).isnull().sum().sum()
    print(f"{name}total missing: {total_missing}")
# Apply
```


#### Part3.Data Visualizaion


I want to analyze the financial situation of these companies from shallow to deep. I need to have a rough estimate of their cash flow, study their income and expenditure situation, and attempt to break down the various components of their cash flow and understand their decisions. And judge from the final financial impact whether these decisions are effective.


##### PROFITABILITY vs. Cost control

I choose a bar chart to horizontally compare the gross profit and sales management expenses of
different companies. High gross profit but a large proportion of expenses may imply efficiency issues.Many
companies have a high proportion of operating expenses, and the differences in operating
and revenue levels between these companies are often significant. Therefore, directly comparing
their operating amounts may not be a good choice. I chose to analyze their asset and cash flow
composition to determine the operational health of this company.

![blog placeholder](/provscost.png)


Excessive operating expenses do not necessarily mean that a company has the risk of cost control.
Companies also have similar goodwill assets that can prove their potential financing ability to
counter potential operating expenses. Therefore, we use scatter plots to observe which companies
have a higher proportion of goodwill.Through Group by Ticker, I calculate the total goodwill of
each Ticker, and then calculate the proportion of goodwill assets.

![blog placeholder](/scatter.png)



![blog placeholder](/SCNI1.png)

| Italics   | Bold     | Code   |
| --------- | -------- | ------ |
| _italics_ | **bold** | `code` |




![blog placeholder](/SCNI2.png)


## Other Elements — abbr, sub, sup, kbd, mark

### Syntax

```markdown
<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>Delete</kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.
```

### Output

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>Delete</kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.
