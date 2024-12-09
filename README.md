# StockTwits: Comprehensive records of a financial social media platform from 2008 to 2022
We introduce the first publicly available comprehensive data set of posts on a social media platform: StockTwits. StockTwits is a financial social media platform where more than 7 million active users discuss financial markets and investing strategies across 550 million posts since 2008. We provide a complete record of all StockTwits posts up to 2022, including the poster's anonymous ID, the text and timestamp of the message, and whether the user tagged their own post as optimistic (``bullish'') or pessimistic (``bearish''). We study the temporal dynamics of this data set, analyzing it at both the ticker-level and the user-level to illustrate this data set's value. We show how to use this data set to (1) measure individual users' predictive accuracy, and (2) discover heterogeneity in how well sentiment predicts stock price movement. 
## Content
- [Data Release and Access](#data-release-and-access)
- [Dataset Description](#dataset-description)
- [Analysis Templates Overview](#analysis-templates-overview)
- [Data Analysis Tutorial](#data-analysis-tutorials)
- [Citing the Project](#citing-the-project)
- [Contect](#contect)

## Data Release and Access
The data set is stored in an Amazon S3 bucket and is available for public access through the [AWS Open Data Registry](fill later)
## Dataset Description

### Dataset Overview
This dataset contains several types of CSV files organized in a versioned structure. Below is the directory structure for version 1 of the dataset:

- `dataset/`
  - `README.md`: Overview of the dataset, this README file povides up-to-date information on the structure, usage, and content of the data.
  - `v1/`: Version 1 of the dataset
    - `data/`: Contains all CSV data files
      - `csv/`: Root directory for CSV files
        - `feature_wo_messages/`: Features excluding messages
          - `feature_wo_messages_000.csv`
          - Other files in the format `feature_wo_messages_###.csv`
        - `messages/`: Messages data
          - `msg_000.csv`
          - Other files in the format `msg_###.csv`
        - `msg_infos/`: Metadata about messages
          - `msg_info_00.csv`
          - Other files in the format `msg_info_###.csv`
        - `sentiments/`: Sentiment analysis data
          - `sentiment_00.csv`
          - Other files in the format `sentiment_###.csv`
        - `symbols/`: Symbol-related data
          - `symbol_000.csv`
          - Other files in the format `symbol_###.csv`
        - `symbol_sentiments/`: Sentiments tied to symbols
          - `symbol_sentiments_00.csv`
          - Other files in the format `symbol_sentiments_###.csv`
This structure may change as future versions of the dataset are released.

### Table Description
This dataset consists of multiple CSV files, each containing specific types of information. Below is a detailed explanation of each folder and the columns in the associated files:

- **feature_wo_messages**: features excluding meassge-content related ones
  - Contains files like `feature_wo_messages_000.csv`.
  - Columns:
    - `message_id`: Unique identifier for the message.
    - `user_id`: Identifier for the user who created the message.
    - `created_at`: Timestamp of message creation.
    - `sentiment`: Sentiment score or label.
    - `parent_message_id`: ID of the parent message, if it's a reply.
    - `in_reply_to_message_id`: ID of the message being replied to.
    - `symbol_list`: List of symbols mentioned in the message.

- **messages**: message_id - message content map
  - Contains files like `msg_000.csv`.
  - Columns:
    - `message_id`: Unique identifier for the message.
    - `message_body`: The text content of the message.

- **msg_infos**: message-content related features
  - Contains files like `msg_info_00.csv`.
  - Columns:
    - `message_id`: Unique identifier for the message.
    - `length`: Length of the message.
    - `important_words`: Key words extracted from the message.

- **sentiments**: the features for meassges that have user-labeled sentiment
  - Contains files like `sentiment_00.csv`.
  - Columns:
    - `message_id`: Unique identifier for the message.
    - `user_id`: Identifier for the user who created the message.
    - `created_at`: Timestamp of message creation.
    - `sentiment`: Sentiment score or label.
    - `symbol_list`: List of symbols mentioned in the message.

- **symbols**: the features for meassges that have user-tagged stock tickers
  - Contains files like `symbol_000.csv`.
  - Columns:
    - `message_id`: Unique identifier for the message.
    - `user_id`: Identifier for the user who created the message.
    - `created_at`: Timestamp of message creation.
    - `sentiment`: Sentiment score or label.
    - `symbol_list`: List of symbols mentioned in the message.
    - `sym_number`: Number of symbols in the message.
    - `symbol`: The actual symbol referenced.

- **symbol_sentiments**:the features for meassges that have both user-tagged stock tickers and user-labeled sentiment
  - Contains files like `symbol_sentiments_00.csv`, which mirrors the structure of the `sentiments` files.
  - Columns are the same as in the `sentiments` directory.

## Analysis Templates Overview
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
### E1&2_weekly_stock_network_visulization (ipynb and html)
- We produce filtered weekly ticker-ticker network visulizations.
- The visulizations are web-based(html) dynamic network looks like the following.
- 
### G_tutorial_for_using_data.ipynb
- This is a short tutorial for using the data, more details are explained in the following section. 


## Data Analysis Tutorials
### Access the dataset
Check the dataset_access.md for details. 

### Data Analysis Demo
Here is a short tutorial for using the dataset: (E_tutorial_for_using_data.ipynb)

This notebook stands as a tutorial(demo) for using the StockTwits data we provide to do some analysis. 
As the goal for this notebook is to show how to use the data, in this notebook, we provide a demo for a simple goal: how popular stocks' popularity V.S stock price looks like? This demo explains:
- how to choose a suitable dataset for a specific analysis
- how to access and manipulate the dataset
- how to complement the dataset with stock-related information with yfinance API
- NOTE: the demo is made in a python 3.11.2 kernal


## Citing the Project
Fill later
## Contect
- Xingji "Jax" Li: xl2860@nyu.edu
- Aaron Kaufman: aaronkaufman@nyu.edu
