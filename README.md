# Retail Sales Forecasting
## Why this project

Retailers need to know roughly how much they'll sell in the coming weeks to plan stock, staffing, and promotions properly. This project builds a forecasting model on real daily sales data from a 1,115-store retail chain, predicting sales several weeks ahead based on each store's own history.

## The data

- [Rossmann Store Sales (Kaggle)] — daily sales for 1,115 stores, Jan 2013 to July 2015
- Two files: daily sales/promo data, and store metadata (type, assortment, competition, promo participation)

## Results

| Model | MAE | RMSE | R² |
|---|---|---|---|
| Baseline (store + day-of-week avg) | 1,252 | 1,660 | 0.704 |
| Linear Regression | 818 | 1,142 | 0.860 |
| **Random Forest (best)** | **669** | **994** | **0.894** |
| Gradient Boosting | 699 | 1,035 | 0.885 |

The best model predicts within about **9-10% of average daily sales**, and clearly beats a strong baseline this isn't just fitting to an average, it's picking up real, useful signal.

## What drives sales, based on the data

- **Recent sales history matters most** — lag and rolling average features account for ~85% of the model's decision-making, more than store type, promos, or calendar features combined
- **Promotions genuinely lift sales** — about 39% higher on promo days
- **Performance holds up across store types** — error stays in a consistent 7-10% range, not wildly better for one segment and worse for another


## A note on model size

The originally benchmarked model (100 trees, depth 15) came out at 242MB too big for GitHub. I swapped to a leaner version (50 trees, depth 10) that's 99% smaller (2.5MB) for a barely-there drop in accuracy (R² 0.894 → 0.887). Worth it.

## Limitations

- Forecasts a fixed 6-week horizon; a real production system would need to handle rolling forecasts and refresh features as new data comes in
- Competition distance turned out to be a weak predictor competitor pricing or promo activity (not in this dataset) might matter more than raw distance

## What's next

- Try store clustering to see if grouping similar stores improves accuracy
- Extend to multi-horizon forecasting
