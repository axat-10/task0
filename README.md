Trader Performance vs Market Sentiment

Setup
Requirements
pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
Data Files
Download both datasets and place them in this folder:

File	Rename to	Source
Bitcoin Fear/Greed CSV	fear_greed.csv	Link
Hyperliquid Trader CSV	trader_data.csv	Link
Run
jupyter notebook primetrade_analysis.ipynb
# Run All Cells (Kernel → Restart & Run All)
Methodology
Data Preparation (Part A)

Loaded both datasets, documented shape, missing values, and duplicates
Parsed trader timestamps (handles both epoch-ms and string formats auto-detected)
Normalized dates to daily level and inner-joined on date
Engineered per-trade features: win (PnL > 0), is_long (side = BUY/LONG)
Aggregated to daily market-level and per-account metrics: PnL, win rate, leverage, trade count, long/short ratio, drawdown proxy
Analysis (Part B)

Used Mann-Whitney U tests (non-parametric) to validate Fear vs Greed differences
3 trader segments: High/Low Leverage, Frequent/Infrequent, Consistent Winner/Inconsistent
3 insight charts: cumulative PnL timeline, leverage vs PnL by sentiment, long/short bias shift
Bonus

K-Means clustering (k=4) with elbow method to identify behavioral archetypes
Gradient Boosting classifier to predict next-day market profitability using lagged features + sentiment encoding
Key Insights
Fear days correlate with lower median PnL and win rate. The Mann-Whitney test reveals this is statistically significant, not random variance.

High-leverage traders amplify losses on Fear days. Traders using >10x leverage on Fear days have disproportionately negative mean PnL compared to the same group on Greed days. Low-leverage traders are far more resilient.

Long/Short bias shifts with sentiment. Traders increase their long-to-short ratio on Greed days and pull back on Fear days — but consistent winners maintain a more balanced ratio regardless of sentiment.

Strategy Recommendations
Strategy 1 — "Fear Day Leverage Cap"

On Fear days, cap leverage at ≤5x. High-leverage accounts lose significantly more on Fear days. Low-leverage traders' win rate drops only ~3–5% on Fear vs Greed days compared to ~15–20% for high-leverage accounts.

Strategy 2 — "Sentiment-Gated Frequency for Active Traders"

Frequent traders underperform on Fear days relative to infrequent traders. Rule: reduce position sizing by 30–50% during Fear, and increase trade frequency only after 2+ consecutive Greed days.

Rule of Thumb:

"Fear → reduce size, cap leverage, bias short. Greed → consistent winners can size up with long bias."

Output Charts
File	Description
chart_B1_performance.png	PnL, win rate, drawdown: Fear vs Greed
chart_B2_behavior.png	Trade count, leverage, long ratio, size by sentiment
chart_B3_segments.png	PnL distribution by 3 segments
chart_B3_segment_sentiment.png	Segment PnL split by Fear/Greed
chart_insight1_cum_pnl.png	Cumulative PnL timeline with sentiment background
chart_insight2_leverage_pnl.png	Leverage bucket vs mean PnL by sentiment
chart_insight3_long_short.png	Long/short ratio by sentiment
chart_bonus_elbow.png	K-Means elbow curve
chart_bonus_clusters.png	Trader archetype scatter
chart_bonus_feature_importance.png	Next-day profitability model features
