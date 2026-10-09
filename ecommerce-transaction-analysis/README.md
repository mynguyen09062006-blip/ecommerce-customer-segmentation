# E-commerce Customer Segmentation (RF Model)

## What this project does

This project analyses one year of transactions (Dec 2010 – Nov 2011) from a UK online gift shop. It looks at how sales and customers change over time, then groups customers with a Recency–Frequency (RF) model based on how recently and how often they buy. I built it in Python during the Business Intelligence track at MCI Academy.

## Why it is useful

A shop that knows which customers are loyal and which are drifting away can spend its marketing budget better. It can reward loyal buyers, win back customers who have stopped ordering, and avoid spending too much on low-value one-time buyers. The EDA also shows *when* customers are most active, which helps with timing campaigns.

Main results:
- Revenue grew from about £0.59M (Dec 2010) to £1.19M (Nov 2011), with most of the growth in Sep–Nov.
- Customers order most on Thursdays and around midday (12:00–14:00).
- The UK brings ~82% of revenue, while the Netherlands, Australia and Singapore have the highest order values.
- Only 12% of customers are Loyal, and 42% are in the "Losing" segments, so win-back campaigns look like the biggest opportunity.

## How I processed the data

**1. Loading and checking**
- Loaded `transaction_data.csv` with pandas (516K rows, 8 columns) and checked data types and missing values with `info()` and `isnull().sum()`.

**2. Cleaning**
- Dropped rows with missing values, which were mostly transactions without a customer ID that cannot be used for segmentation.
- Converted `invoice_date` to datetime and `cust_id` to integer.
- Some quantities and prices were negative (returns), so I converted them to positive values with `abs()`.
- Added `amount = quantity × unit_price`.

**3. Feature engineering**
- Extracted `month`, `day`, `hour`, `year_month` and `week_days` from `invoice_date`.
- Found each customer's first purchase month and tagged every transaction as `new` or `old` (returning).

**4. Exploratory analysis**
- Monthly active users (unique customers per month), number of orders and revenue per month.
- Unique customers by weekday and by hour.
- Top 10 countries by revenue, and AOV (revenue ÷ number of orders) by country.
- Tracked how the Dec 2010 new customers kept buying in later months.

**5. RF segmentation**
- Recency = days between a customer's last order and the last date in the data; Frequency = number of unique invoices.
- Scored both from 1 to 3:

| Score | 1 | 2 | 3 |
|---|---|---|---|
| Recency (days) | > 48 | 15–48 | < 15 |
| Frequency (orders) | 1 | 2–5 | > 5 |

- Combined the two scores (e.g. `33`, `21`) and mapped them to 7 segments: Loyal, Potential loyal, New customer, Losing loyal, Losing potential loyal, Lost loyal and Low value.
- Visualised the segment sizes with a bar chart and an R × F heatmap.

![RF segment map](imgs/img2.png)

## Getting started

1. Clone the repository:
   ```bash
   git clone https://github.com/mynguyen09062006-blip/push-code.git
   cd push-code/ecommerce-transaction-analysis
   ```
2. Install the libraries:
   ```bash
   pip install pandas matplotlib seaborn jupyter
   ```
3. Download the [UCI Online Retail dataset](https://archive.ics.uci.edu/dataset/352/online+retail) and save it as `data/transaction_data.csv`.
4. Open `Cleaning and Visualizing Data.ipynb` in Jupyter and choose **Run All**.

**Files**
- `Cleaning and Visualizing Data.ipynb`: main notebook (cleaning → EDA → RF)
- `01_e_commerce_data_eda.ipynb`: project brief and guiding questions
- `Customer segment map.xlsx`: segment definitions
- `imgs/`: RF illustration and segment map

## Getting help

If you have a question or find a problem, please open an issue in this repository or contact me on [LinkedIn](https://www.linkedin.com/in/my-nguyen-anh/) or at mynguyen09062006@gmail.com.
