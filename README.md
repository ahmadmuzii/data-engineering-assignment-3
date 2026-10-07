<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:1d4ed8,100:60a5fa&height=210&section=header&text=Online%20Retail%20II%20%E2%80%94%20Cleaning%20%26%20RFM&fontSize=40&fontColor=ffffff&fontAlignY=36&animation=fadeIn&desc=1.07%20million%20transactions%20%E2%86%92%20one%20row%20per%20customer&descSize=18&descAlignY=58" width="100%" alt="Online Retail II"/>
</p>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=2400&pause=700&color=60A5FA&center=true&vCenter=true&width=640&lines=2+Excel+sheets+%E2%86%92+1+DataFrame;Inspect+first%2C+clean+second+%F0%9F%94%8D;Monetary+%E2%80%A2+Frequency+%E2%80%A2+Recency" alt="typing"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white"/>
  <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white"/>
  <img src="https://img.shields.io/badge/Google_Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=black"/>
  <a href="https://colab.research.google.com/github/ahmadmuzii/data-engineering-assignment-3/blob/main/Assignment3_Data_Cleaning_Transformation.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open in Colab" height="28"/></a>
</p>

---

## 🧠 What I Built

For Assignment 3 of a **Data Engineering bootcamp**, I cleaned the **Online Retail II** dataset (a UK online gift retailer, Dec 2009 – Dec 2011) and transformed raw invoice lines into a **customer-level RFM summary** — the kind of table a marketing team uses for segmentation.

The approach throughout: **inspect the data before trusting the documentation**, make every cleaning decision explicit, and write down the trade-off (each phase ends with a reflection answering *why*).

**My role:** individual assignment — all code, decisions and reflections are my own.

---

## 🔁 Pipeline

```mermaid
flowchart TD
    A[(online_retail_II.xlsx<br/>2 worksheets)] -->|concat + reset index| B[1,067,371 rows × 8 cols]
    B --> C[Profile: nulls · uniques · describe]
    C --> D[Invoices: remove 19,494 cancellations 'C…'<br/>flag 'A…' bad-debt adjustments]
    D --> E[Stock codes: inspect admin codes<br/>POST · M · DOT · BANK CHARGES]
    E --> F[Drop 242,257 rows with no Customer ID]
    F --> G[Validate Price ≤ 0 and Quantity < 0]
    G --> H[LineTotal = Quantity × Price]
    H --> I[groupby Customer ID]
    I --> J[(customer_summary.csv<br/>Monetary · Frequency · Recency)]
```

| Phase | Decision | Why |
|:-:|---|---|
| 1 | Merge both yearly worksheets, reset index | One continuous table; avoids duplicate index values |
| 2 | Explore invoice formats before cleaning | Found `C` (cancellation) and `A` (bad-debt adjustment) prefixes the docs don't fully describe |
| 3 | Remove **19,494** cancellation lines | The goal is purchasing behaviour, not returns — cancellations would understate spend |
| 4 | Treat admin stock codes as non-products | Postage and bank charges aren't customer purchases (but would stay in a *revenue* report) |
| 5 | Drop **242,257** rows without a Customer ID | RFM is per customer; anonymous lines can't be attributed |
| 6 | Check zero/negative prices (71) and quantities | Giveaways, errors and refunds would distort spend |
| 7 | Compute `LineTotal` before aggregating | Each line contributes its own value; easy to sanity-check |
| 8 | Aggregate per customer | **Monetary** = Σ LineTotal · **Frequency** = unique invoices · **Recency** = days since last purchase, relative to the dataset's latest date (not today) |

### Output preview

| Customer ID | Monetary (£) | Frequency | Recency (days) |
|---|---:|---:|---:|
| 12346 | 77,556.46 | 12 | 325 |
| 12347 | 5,633.32 | 8 | 1 |
| 12348 | 2,019.40 | 5 | 74 |

---

## 🚀 Run It

1. Click **Open in Colab** above (or open the notebook in Jupyter).
2. Download `online_retail_II.xlsx` from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/502/online+retail+ii) and place it next to the notebook.
3. Run all cells — the notebook writes `customer_summary.csv`.

| File | Contents |
|---|---|
| `Assignment3_Data_Cleaning_Transformation.ipynb` | All 8 phases, code and written reflections |

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:60a5fa,50:1d4ed8,100:0f172a&height=100&section=footer" width="100%" alt=""/>
</p>
