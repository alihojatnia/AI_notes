# Apple News–Stock Sentiment Research

A research project exploring the relationship between Apple Inc. (AAPL) news sentiment and stock market behavior. The pipeline scrapes news (e.g., Google News results), enriches it with Yahoo Finance pricing data, engineers predictive features, evaluates a simple trading strategy, and serves insights via a Streamlit dashboard.

---

## Key Objectives

* Collect recent Apple-related news with metadata (URL, title, source, publish date, content).
* Perform sentiment analysis on news titles.
* Merge news sentiment with AAPL OHLCV data from Yahoo Finance.
* Engineer return/volatility and lagged sentiment features.
* Visualize relationships and evaluate a basic sentiment-driven strategy.
* Provide an interactive Streamlit dashboard.

---

## Final Dataset Columns

```
['url', 'title', 'source', 'published_dt_only',
 'Open_AAPL', 'High_AAPL', 'Low_AAPL', 'Close_AAPL', 'Volume_AAPL',
 'sentiment', 'sentiment_score', 'Volatility', 'Daily_Return',
 'Sentiment_Lag1', 'Sentiment_Lag7', 'Sentiment_Binary', 'Day_of_Week',
 'Next_Day_Return', 'Strategy_Return']
```

### Column Descriptions

* **url**: Link to the news article.
* **title**: Article headline.
* **source**: Publisher/source name.
* **published\_dt\_only**: Article publish date (date-only, normalized to local/UTC as configured).
* **Open\_AAPL, High\_AAPL, Low\_AAPL, Close\_AAPL, Volume\_AAPL**: Daily OHLCV from Yahoo Finance.
* **sentiment**: Categorical sentiment label (e.g., `positive`/`neutral`/`negative`).
* **sentiment\_score**: Continuous sentiment polarity score (e.g., from VADER/TextBlob).
* **Volatility**: Rolling or intraday proxy (e.g., `(High - Low) / Open`), configurable.
* **Daily\_Return**: Daily log or simple return of AAPL for the publish date.
* **Sentiment\_Lag1 / Sentiment\_Lag7**: Lagged sentiment scores to test delayed effects.
* **Sentiment\_Binary**: Thresholded sentiment signal (e.g., `1` for bullish, `0` for not-bullish).
* **Day\_of\_Week**: 0–6 encoding (Mon–Sun) for seasonality controls.
* **Next\_Day\_Return**: Return from `t` to `t+1`, used as the prediction target.
* **Strategy\_Return**: Return of a naive strategy (e.g., long if bullish sentiment, flat/short otherwise).

> **Note:** Feature definitions are configurable in `config.py`/notebook; see code comments for exact formulas used.

---

## Pipeline Overview

1. **News Scraping**

   * Query Apple-related keywords from news sources (e.g., Google News results).
   * Parse: `url`, `title`, `source`, `published_date`, `content` (if available).
   * Clean & deduplicate by URL/title and normalize publish timestamps.

2. **Market Data (Yahoo Finance)**

   * Download daily AAPL OHLCV around the news window.
   * Align trading days to publish dates; forward/backfill as needed for weekends/holidays.

3. **Sentiment Analysis**

   * Run sentiment over **titles** (optionally content).
   * Save both **label** and **score**.
   * Tools: VADER/TextBlob/HF models (select one in `config`).

4. **Feature Engineering**

   * Daily returns, volatility, day-of-week dummies.
   * Lagged sentiment features (1 & 7 days).
   * Binary trade signal from sentiment threshold.

5. **Backtest (Simple)**

   * `Strategy_Return = signal_t * Next_Day_Return` (long-only or long/short).
   * Aggregate performance metrics: hit rate, average return, cumulative return.

6. **Visualization & App**

   * Matplotlib/Plotly static charts.
   * **Streamlit** dashboard for interactive exploration.

---

## Streamlit Dashboard

Run the dashboard to browse news, filter by date/source/sentiment, and compare sentiment vs. returns.

```bash
streamlit run app.py
```


**Volatility & Returns:**

```python
df['Volatility'] = (df['High_AAPL'] - df['Low_AAPL']) / df['Open_AAPL']
df['Daily_Return'] = df['Close_AAPL'].pct_change()
df['Next_Day_Return'] = df['Close_AAPL'].pct_change().shift(-1)
```

**Lagged Sentiment & Signal:**

```python
df['Sentiment_Lag1'] = df['sentiment_score'].shift(1)
df['Sentiment_Lag7'] = df['sentiment_score'].rolling(7).mean().shift(1)
df['Sentiment_Binary'] = (df['sentiment_score'] > 0.05).astype(int)
```

**Strategy Return (long-only):**

```python
df['Strategy_Return'] = df['Sentiment_Binary'] * df['Next_Day_Return']
```

---

## Visualizations

* Price with rolling average vs. average daily sentiment.
* Histogram of **Next\_Day\_Return** by sentiment bucket.
* Rolling correlation between sentiment and next-day returns.
* Cumulative **Strategy\_Return** vs. buy-and-hold.

---

## Results

* Scraped Apple-related news and assembled a clean dataset with: URL, title, source, publish date, content (when available).
* Joined news to AAPL OHLCV from Yahoo Finance by date.
* Ran sentiment analysis on titles; stored both labels and scores.
* Engineered return/volatility, lagged sentiment features, day-of-week.
* Evaluated a simple sentiment-driven next-day strategy and computed **Strategy\_Return**.
* Built an interactive **Streamlit** dashboard for exploration.

---
