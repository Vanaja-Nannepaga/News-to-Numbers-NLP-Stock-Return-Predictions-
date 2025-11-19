# **News to Numbers: NLP-Based Stock Return Prediction**

This project focuses on predicting **short-horizon stock market reactions** using financial news headlines. Instead of forecasting raw returns—which are noisy and unstable—we predict the **10-minute volatility-normalized Jensen’s alpha**, a risk-adjusted measure that isolates the true impact of a news event on an asset’s price.

## **Goal**

Build an NLP + Quant Finance system that can estimate how the market will react *immediately* after a headline is published, using:

* Transformer-based text embeddings,
* Market-state financial features,
* Retrieval-augmented signals from historically similar news.

---

## **Key Idea: RAP (Retrieval Augmented Financial Prediction)**

Traditional NLP models try to infer market reaction from one headline.
Our **RAP framework** enhances this by retrieving **top-K semantically similar past headlines** (same ticker, earlier timestamps) and using their *actual* observed market reactions as additional features.

This helps the model:

* Understand historical analogs,
* Reduce noise,
* Improve stability in volatile conditions.

---

## **Data**

We use two main datasets:

### **1. News Data**

* 70,000+ headlines from Benzinga, CapitalIQ, and Polygon
* Filtered to 10 liquid stocks/ETFs
* Chronological split to avoid look-ahead bias
* Final usage:

  * **17,552 training headlines** (2010–2018)
  * **2,244 validation headlines** (2018–2020)

### **2. Market Data**

* TAQ minute-level trade data
* Calculations include:

  * Minute returns
  * Rolling CAPM betas
  * Jensen’s alpha
  * GARCH(1,1) volatility forecasts
* Target variable = **Volatility-normalized Jensen’s alpha**

---

## **Model Architecture**

### **1. Text Encoder**

Transformer models such as:

* FinBERT
* DistilRoBERTa-finance
* DeBERTa-finance

### **2. Financial Feature Aggregator**

Includes:

* Recent returns
* Volume shifts
* Volatility estimates
* Liquidity conditions

### **3. RAP Module**

Retrieves K nearest historical headlines using a FAISS vector index; aggregates their normalized returns.

### **4. Prediction Head**

* Ridge regression or MLP
* Huber loss for robustness

A **two-tier ensemble model** (stacking) further boosts performance by combining predictions from multiple encoders.

---

## **Results**

### **Baseline (One-Tier Models)**

Best encoder performance:

* **Sharpe Ratio:** 0.170
* **MDA:** 57.58%
* **Correlation:** 0.212

### **Two-Tier Meta-Ensemble**

* **Sharpe Ratio:** 0.373
* **MDA:** 60.29%
* **Correlation:** 0.403
* Sharpe improves to **0.834** for top 20% most confident predictions.

→ **57.8% improvement** over best baseline.

### **RAP Models**

Improved directional accuracy (up to 53.7%), especially for high-liquidity tickers.

---

## **Future Work**

* Better retrieval scoring and K-selection
* Real-time deployment for live trading
* Incorporating transcripts, social sentiment, and analyst reports
* Explainable RAP predictions

---

## **Team Semantics**

* Nidhi Singh (12241150)
* Nannepaga Vanaja (12241110)
* Himanshu (S24MA006)

