---
title: 'Personal Dashboard(Real-time)'
description: 'Here is one of my work of Data collecting, cleaning, analyzing, and interactively presenting.'
pubDate: 'Apr 12 2025'
heroImage: '/dashboard4.png'
---

This is a data dashboard project I created while studying finance. Compared to the index provided by large websites, I am more eager to uncover the many details of economic operations.

Check final <a href="https://arthur-chen921-data-ds-dl55d4.streamlit.app/">Dashboard</a>.

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
# I have selected thirty stocks from the Nasdaq component page on March 22nd
↪2025
nasdaq = ['OPTN', 'UONEK', 'SCOR', 'AXON', 'STRS', 'CMRX', 'BSBK', 'FNLC',
'NVCR', 'FNKO','NVEC', 'SLAB', 'MSBI', 'AGNC', 'NMRK', 'OPTX', 'IFRX', 'CVCO',
...]
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


####### Don't forget to validate.

```markdown
> In data collection, it is important to standardize the format at the beginning of the collection process and strictly classify the sources of data reporting
> def handle_missing(df, threshold=0.75):
      # Only process numerical columns
      numeric_cols = df_cleaned.select_dtypes(include=np.number).columns.tolist()
      for col in numeric_cols:
          # Step 1: Fill in missing values with the historical mean of the same␣
          # Step 2: If the company has no historical data, fill in the entire␣

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

The asset liability ratio is usually determined by the profitability and cost of a company. I compared
the profit trend and cost control trend of SCNI to identify the underlying reasons for the decline
in the asset liability r.But it still needs to be compared whether it has stickiness to shareholders
and whether it really invests costs in the work environment ,to check if these changes are made for
long term.

![blog placeholder](/SCNI1.png)

After analyzing the funding trends of SCNI, I selected five more segmented categories of funding
flows to examine the specific numbers. I directly selected from the previously defined table, using
the date as the first column, and used red and black colors on three items to distinguish positive
and negative numbers, in order to make better judgments.It can be seen that the consumption of
working capital has surged ,and the Dividends remained the same amount.


![blog placeholder](/SCNI.png)

After analyzing the flow of funds, I wanted to assess the company’s ability to withstand risks, so
I introduced a long term debt issue outside of operating cash to observe its fund composition. As it was to observe the proportion of fund composition, I chose a pie chart.It can be seen that long
term debt occupies a very exaggerated proportion.


#### Outcome & Link

In summary, in order to evaluate the operational status of the company, I have divided the overall calculation into four parts: calculating the proportion of operating costs, measuring the health of cash flow, the degree of decline in dividends and reputation assets, and long-term debt issuance.

You can calculate by this formula also, this website will not collect any information from you :)

<body>
    <!-- 公式展示区 -->
    <div class="formula-card">
        <h2>📈 Financial Health Formula </h2>
        <div class="formula-code">
            <p>Health rate = </p>
            <p>0.4 × [1 - (Cost/Revenue)²] (When ≤70%)</p>
            <p>0.4 × e⁻²⁽Cost rate⁻⁰⋅⁷⁾ (Cost>70%)</p>
            <p>+ 0.3 × (1 - Dividend volatility) × goodwill%</p>
            <p>+ 0.3 × tanh(Debt/(2×Revenue))</p>
        </div>
    </div>
    <div class="calculator">
        <div class="input-grid">
            <div>
                <label>Revenue（Million）</label>
                <input type="number" id="revenue" value="1000" step="100">
            </div>
            <div>
                <label>Operation Cost（Million）</label>
                <input type="number" id="opCost" value="700" step="50">
            </div>
            <div>
                <label>Current dividends（Million）</label>
                <input type="number" id="dividend" value="200" step="10">
            </div>
            <div>
                <label>Previous dividend distribution（Million）</label>
                <input type="number" id="dividendPrev" value="200" step="10">
            </div>
            <div>
                <label>Goodwill rate（0-100）</label>
                <input type="number" id="reputation" value="80" min="0" max="100">
            </div>
            <div>
                <label>Long debt（Million）</label>
                <input type="number" id="longDebt" value="1500" step="100">
            </div>
        </div>
        <div class="score-gauge">
            <div class="gauge-fill" style="--score: 0"></div>
            <div class="score-label">Health Score：<span id="finalScore">0</span></div>
        </div>
    </div>
    <script>
    class FinanceCalculator {
        constructor() {
            this.inputs = [
                'revenue', 'opCost', 'dividend', 
                'dividendPrev', 'reputation', 'longDebt'
            ];
            this.init();
        }
        init() {
            // 绑定事件监听
            this.inputs.forEach(id => {
                document.getElementById(id).addEventListener('input', () => this.update());
            });
            this.update(); // 初始计算
        }
        getValue(id) {
            const el = document.getElementById(id);
            return el.value ? parseFloat(el.value) : 0;
        }
        update() {
            // 获取输入值
            const [R, C, D, Dp, A, L] = this.inputs.map(id => this.getValue(id));
            // 计算各因子
            const costRatio = C / R;
            const costFactor = this.calcCostFactor(costRatio);
            const reputationFactor = this.calcReputation(D, Dp, A);
            const debtFactor = this.calcDebt(L, R);
            // 总评分
            const total = (costFactor + reputationFactor + debtFactor) * 100;
            this.display(total);
        }
        calcCostFactor(ratio) {
            return ratio <= 0.7 ? 
                0.4 * (1 - Math.pow(ratio, 2)) : 
                0.4 * Math.exp(-2 * (ratio - 0.7));
        }
        calcReputation(D, Dp, A) {
            const delta = Dp ? Math.abs((D - Dp) / Dp) : 0;
            return 0.3 * (1 - delta) * (A / 100);
        }
        calcDebt(L, R) {
            return 0.3 * Math.tanh(L / (2 * R));
        }
        display(score) {
            // 更新仪表盘
            document.documentElement.style.setProperty('--score', Math.min(score, 100));
            document.getElementById('finalScore').textContent = score.toFixed(1);
        }
    }
    // 初始化计算器
    new FinanceCalculator();
    </script>
</body>

![blog placeholder](/dashboard1.png)
![blog placeholder](/dashboard2.png)
![blog placeholder](/dashboard3.png)

I have created interactive websites for various indicators of this analysis. Please click to view my <a href="https://arthur-chen921-data-ds-dl55d4.streamlit.app/">Dashboard</a>.
