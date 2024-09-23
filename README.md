# StockTwits: Comprehensive records of a financial social media platform from 2008 to 2022
We introduce the first publicly available comprehensive data set of posts on a social media platform: StockTwits. StockTwits is a financial social media platform where more than 7 million active users discuss financial markets and investing strategies across 550 million posts since 2008. We provide a complete record of all StockTwits posts up to 2022, including the poster's anonymous ID, the text and timestamp of the message, and whether the user tagged their own post as optimistic (``bullish'') or pessimistic (``bearish''). We study the temporal dynamics of this data set, analyzing it at both the ticker-level and the user-level to illustrate this data set's value. We show how to use this data set to (1) measure individual users' predictive accuracy, and (2) discover heterogeneity in how well sentiment predicts stock price movement. 
## Content
- [Data Release and Access](#data-release-and-access)
- [Dataset Description](#dataset-description)
- [Analysis Templates Overview](#analysis-templates-overview)
  

## Data Release and Access
## Dataset Description

### A_data_process.ipynb
In this notebook, we process the raw data:
- Repack and clean the raw grabbed StockTwis data.
- Produce a series of "analysis datasets", which include parts of the data with smaller size for easier accessiblily and analysis. For example, it produce a dataset that include all features but the messages bodies, which is smaller in size an more accessible for analysis that disregard message bodies.
- Conduct analysis on the "missing data": the pipleine of are data grabber get the data records by messages' sequential ids, by cheking the difference beteen the ids, we analysis the pattern of "missing data" in our dataset. For example, two contingent messages have id 10 and 14, we know that ther are 3 messages missing(11,12,13), we interpret this as by the time we grab the data, the 3 missing messages have been deleted by their senders.

### B_analysis_on_popularity_dynamics.ipynb
In this notebook, we do analysis on the popularity dynamics on StockTwits platform:
- The general popularity dynamics in the plotform over time.
- Case study on two kinds of popular stocks: some stocks are stably popular over a long period of time, while some stocks become phenomenally poipular over a shor period of time.

### C_analysis_on_sentiment-popularity-stock_relation.ipynb
In this notebook, we do analysis on the correlation between sentiment, populatity, and actual stock prive movement:
- Measuring sentiment by "bullish rate" considering the message count about a stock as the measuremennt of popularity, we analysis their relation with the actual stock prices.
- User-accuracy: consider each user is a predictive "trading machine", we analysis how well users on StockTwits platform in "predicting" the stock price movements.

### D_analysis_on_message_body.ipynb
- We analysis the difference in users' posts about a stock during different(bullish and bearish) time periods.
## Analysis Templates Overview
