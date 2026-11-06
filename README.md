# superstore-profit-analysis

What drives the profit of a retail company, and at what discount level do orders start to lose money? An exploratory analysis and regression study of the *Sample – Superstore* dataset with Python, pandas and statsmodels.

## Business questions

1. Which factors explain the profit of an order line?
2. At what discount level do orders stop being profitable?
3. Are some product groups or regions unprofitable because of the products themselves, or because of the discounts they get?

## Key findings

- **Discounts above 20% cause most of the losses.** At 20% discount, 86% of the order lines are still profitable; from 30% discount on, 87% to 100% of them lose money. The lines with more than 20% discount are only about 14% of all lines, but together they lose about 136,000 dollars, which cancels about a third of the profit made by all other lines.
- **Product type decides how much discount is possible.** Tables and Bookcases lose money in total, because a thin margin (about 21% to 22% without discount) meets high average discounts. Binders get the highest discounts but stay profitable thanks to a margin of about 47%.
- **Region and customer segment have almost no effect once the discount is known.** The Central region has the lowest margin, but only because it gives by far the most high discounts (28% of its order lines, compared to 4% in the West).

## Method

| Step | What I did |
| --- | --- |
| Data check | Missing values, duplicates, invalid sales, yearly trend |
| Exploration | Profit distribution, correlations, profit by category, segment, region and ship mode |
| Discount analysis | Share of loss-making lines and median margin for each discount level |
| Product and region analysis | Total profit, average discount and margin without discount per sub-category; share of high discounts per region |
| Modelling | Four OLS models compared with 5-fold cross-validation, all evaluated on profit in dollars |
| Diagnostics | Residual plots, Q-Q plot, Cook's distance |

## Model comparison

All models are linear regressions (OLS). Models 3 and 4 predict the profit margin (profit / sales) and convert it back to dollars.

| Model | Target | Cross-validated R² on profit | Mean absolute error |
| --- | --- | --- | --- |
| 1. Profit | profit | 0.14 | 58 dollars |
| 2. Log-profit | signed log of profit | unusable in dollars | 2,247 dollars |
| 3. Margin | margin | 0.63 | 31 dollars |
| 4. Margin with discount levels | margin | **0.84** | **18 dollars** |

The biggest improvement came from changing the target from profit to margin, and from treating each discount level as a category, because margins do not fall in a straight line with the discount. The signed log transformation gave a good-looking R² on the log scale (0.50), but its predictions are useless once they are converted back to dollars.

The data is fictional, so the patterns are probably cleaner than in real company data.

## Recommendations

- Use a maximum discount of 20% by default and require approval for higher discounts.
- Set discount limits per product group: lower for Tables, Bookcases and Supplies, higher for products with a large margin.
- Review the discount practice in the Central region first.

These are based on associations in the data: whether customers would still buy without the discount would need an experiment, for example an A/B test of discount levels.

## Files

```
superstore-profit-analysis/
├── superstore_profit_analysis.ipynb   # analysis notebook
├── data/
│   └── superstore.csv                 # raw data
├── README.md
└── requirements.txt
```

## How to run

```
git clone https://github.com/mina-motaghinasab/superstore-profit-analysis.git
cd superstore-profit-analysis
pip install -r requirements.txt
jupyter notebook superstore_profit_analysis.ipynb
```

## Data source

*Sample – Superstore*, a fictional retail dataset published by Tableau as sample data, downloaded from Kaggle. It contains 10,194 order lines from 2023 to 2026 for the United States and Canada.

## Use of AI tools

I wrote the first version of this analysis myself: the column selection, the exploratory plots, the OLS model with statsmodels, the cross-validation, the signed log transformation and the residual diagnostics with Cook's distance. I used Claude (an AI assistant) to review it and to turn it into a finished project. Claude suggested the discount-level analysis, the margin as target, the comparison of all models on the dollar scale, and the analysis by sub-category and region. It also helped write the explanations in the notebook and this README. I can explain every step of the code.

## Author

Mina Motaghinasab, M.Sc. Data Science student at TU Dortmund
