THESIS NOTES

Explainable Price Prediction of Consumer Electronics in Bangladeshi E‑Commerce

1. [The problem](#problem)
2. [Dataset](#dataset)
3. [Methodology](#method)
4. [Evaluation](#metrics)
5. [Explainability](#shap)
6. [Research gap](#gap)
7. [Expected outcome](#outcome)
8. [Limitations](#limits)
9. [Advantages / limits](#tradeoffs)
10. [Viva cheat‑sheet](#viva)
11. [Candidate references](#refs)

WORKING NOTES — DISTILLED FROM YOUR RESEARCH CHAT

# What the thesis actually does, in one page

A buyer can't tell if a listed electronics price is fair. This thesis predicts that price from product data, and uses SHAP to explain why the model predicted it — so it's never a black box.

Md. Samiul Alam · B.Sc. in CSE, Pundra University of Science & Technology

## The problem, concretely

Say Daraz lists a Samsung smartphone. For the same phone model, you'll typically see:

- One seller asking **৳25,000**, another asking **৳27,000**
- A different platform asking **৳29,000** for the same spec
- 10% discount on one listing, 25% on another
- Warranty included on some listings, absent on others

"Is this price actually reasonable?" — the buyer's question.\
"What price keeps me competitive?" — the seller's question.

This thesis answers both with data and machine learning, instead of guesswork.

## Dataset

\~6,500 electronics listings scraped from Daraz and Pickaboo, each with product name, brand, description, listed price, discounted price, discount %, warranty, and rating.

**Target variable: product price.** Because the output is a continuous number (৳25,000, ৳32,500 …) rather than a category, this is a *regression* problem, not classification.

## Methodology — five stages

#### Data preprocessing

Convert text prices ("৳25,000") to numbers, drop duplicates and incomplete rows, handle outliers.

#### Feature engineering

Derive platform from the product URL, category from the product name, plus discount, warranty type, and description length.

#### Log‑price target

Prices range from ৳1,000 to ৳100,000+ — heavily skewed. Using log(price) instead of raw price keeps training stable.

#### Model training

Linear Regression (simple baseline), Random Forest (many trees), and XGBoost (boosted, handles complex relationships) — tuned with cross‑validation.

#### SHAP explanation

Applied to the best model to show globally and per‑product which features pushed the price up or down.

## How "good" gets measured

MAE

Average rupee error. Predicted ৳28,000 vs. actual ৳30,000 → error of ৳2,000.

RMSE

Like MAE, but penalizes large misses more heavily than small ones.

R²

How much of the price variation the model explains — closer to 1 is better.

## Making the model explain itself

A bare prediction — "₹35,000" — isn't enough for a thesis about trust. SHAP breaks that number down into what pushed it up or down:

Brand = Samsung

Category = Smartphone

Warranty = 1 year

Discount applied

Rating

Illustrative only — real bar lengths come from the trained model, not from this page.

## Research gap

Most Bangladeshi e‑commerce research targets customer reviews and sentiment. Price modelling of electronics listings — and explaining *why* a model predicts a given price — is largely unexplored here. The contribution is combining four things that aren't usually combined: **Bangladesh + e‑commerce electronics + price prediction + explainable ML.**

## Expected outcome

| Price prediction model | Estimates a consumer electronics listing's price from its attributes. |
| --- | --- |
| Evidence on price drivers | Which of brand, category, platform, warranty, discount matter most — determined by SHAP, not assumed in advance. |
| Reusable cleaning pipeline | Documented steps other researchers can reapply to scraped e‑commerce data. |

## Limitations to state upfront

\~6,500 listings from only two platforms (Daraz, Pickaboo) may not represent the full Bangladeshi market. The data is scraped, so missing fields and scraping errors are possible. Important real‑world factors — demand, seller reputation, stock, seasonality — aren't in the dataset, and prices change over time while the model is trained on one snapshot. The study covers electronics only; results may not generalize to other categories. SHAP explains contribution, not causation — a feature mattering to the model doesn't prove it *causes* the price.

Data‑leakage check: if discounted price is engineered directly from listed price × discount %, don't feed that derived relationship back in as a feature — it makes prediction artificially easy.

## Advantages and limits, side by side

### Advantages

- Predicts a reasonable price for a given listing
- Explains *why*, via SHAP — not just a number
- Compares three models rather than relying on one
- Gives sellers/platforms a data‑driven pricing reference
- First Bangladesh‑focused work combining these elements
- Leaves behind a reusable cleaning pipeline

### Limits

- Two platforms only — not the whole market
- Scraped data: missing/duplicate/incorrect rows possible
- Prices drift over time; model reflects one snapshot
- Demand, stock, seller reputation aren't captured
- Three models only — others might perform differently
- SHAP shows contribution, not proof of causation

## If asked "what's your thesis?"

What

Predict consumer electronics prices

Where

Bangladesh e‑commerce — Daraz & Pickaboo

Data

\~6,500 scraped listings

How

Supervised regression on product attributes

Models

Linear Regression, Random Forest, XGBoost

Evaluation

MAE, RMSE, R²

Explain

SHAP — global and per‑prediction

Goal

Transparent, trustworthy pricing analysis

"My thesis predicts the price of consumer electronics on Daraz and Pickaboo using brand, category, discount, warranty, and rating, comparing Linear Regression, Random Forest, and XGBoost — then uses SHAP to explain which features drove each prediction. So it's not just price prediction, it's *explainable* price prediction."

## Candidate references

These titles came out of an AI‑assisted search in your uploaded chat, not a database search I ran myself — I haven't independently verified the authors, venues, or DOIs. Confirm each one on the publisher's site (or Google Scholar) before citing it in the actual thesis.

Machine Learning Study on Cross‑border E‑commerce Products Price Prediction (2024/25)

Closest methodological match — Lazada electronics data, same three models (Linear Regression, Random Forest, XGBoost).

Smart Pricing in Online Marketplaces: A ML and Analytics Framework (2025)

Direct electronic‑device price prediction, multiple regression models compared.

Product Pricing Solutions Using Hybrid Machine Learning Algorithm (2022/24)

Electronics pricing tied to product specs, XGBoost‑based ensemble.

Understanding Online Purchases with Explainable Machine Learning (2024)

SHAP methodology reference for the explainability section.

Explainable Machine Learning for E‑Commerce Purchase Intention (2026)

Not price prediction, but same model stack (RF/XGBoost + SHAP) for comparison.

### For establishing the Bangladesh‑specific gap

Predicting On‑Time Delivery: Bangladesh E‑Commerce

Daraz shipment data + ML; useful for local methodology context.

Sentiment Analysis of Bangladeshi E‑Commerce Site Reviews Using ML (2024)

Shows the existing local focus is reviews/sentiment — supports your stated research gap.

A Multimodal Deep Learning Framework for Retail Price Estimation (2025)

Bangladesh‑affiliated authors; relevant to price‑estimation methodology.

No verified published study was found combining Daraz/Pickaboo electronics price prediction with SHAP specifically — treat that as a possible framing for your gap statement, not a confirmed fact.

Distilled for quick reference while preparing the proposal and presentation — not a substitute for reading the full chat export or the cited papers themselves.