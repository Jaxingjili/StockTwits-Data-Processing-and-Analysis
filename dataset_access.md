# Tutorial for Accessing Dataset

## Introduction

This tutorial provides guidance on how to access various data files within the `repo/dataset` directory. The dataset will be hosted on an AWS S3 bucket, making it publicly accessible. This guide covers how to use the AWS CLI and Python (`pandas` and `dask`) to download and read the dataset from S3.

### **Recommendation: Use `Dask` for Large Datasets**

While `pandas` is a popular choice for data analysis, it is not always ideal for large datasets due to memory constraints. We highly recommend using `Dask` DataFrame, a parallel computing library that scales `pandas` operations, for large datasets. Dask allows for efficient memory usage and parallel processing, making it perfect for handling large CSV files in this repository.

To install `Dask`, use:

```bash
pip install dask
```

### Table of Contents

1. [Directory Structure](#directory-structure)
2. [Accessing Dataset from S3 using AWS CLI](#accessing-dataset-from-s3-using-aws-cli)
3. [Reading Files Directly from S3 using Python](#reading-files-directly-from-s3-using-python)
4. [File Descriptions and Data Access](#file-descriptions-and-data-access)
5. [Example Code Using `Dask`](#example-code-using-dask)
6. [Further Resources](#further-resources)
7. [Conclusion](#conclusion)

### 1. Directory Structure

The directory structure of the `dataset` folder is as follows:

```sh
dataset/
├── README.md
└── v1
    └── data
        └── csv
            ├── feature_wo_messages
            │   ├── feature_wo_messages_000.csv
            │   └── ... 
            ├── messages
            │   ├── msg_000.csv
            │   └── ... 
            ├── msg_info
            │   ├── msg_info_00.csv
            │   └── ...
            ├── sentiments
            │   ├── sentiment_00.csv
            │   └── ...
            ├── symbols
            │   ├── symbol_000.csv
            │   └── ...
            └── symbol_sentiments
                ├── symbol_sentiments_00.csv
                └── ...
```

Each folder in the `csv` directory contains CSV files. These files are crucial for various analytical and data processing tasks.

### 2. Accessing Dataset from S3 using AWS CLI

The dataset will be hosted on an AWS S3 bucket, and you can use the AWS CLI.

```bash
BASE_URL="s3://stocktwits-nyu"
CSV_URL="${BASE_URL}/dataset/v1/data/csv"
```

#### **Example Commands:**

- **Listing Files in the S3 Bucket:**

```bash
aws s3 ls --no-sign-request $CSV_URL
```

This command will list all files available in the specified path.

- **Downloading a Specific File:**

```bash
aws s3 cp --no-sign-request $CSV_URL/feature_wo_messages/feature_wo_messages_000.csv .
```

This command will download `feature_wo_messages_000.csv` to the current directory.

- **Synchronizing an Entire Directory:**

```bash
aws s3 sync --no-sign-request $CSV_URL/ .
```

This command will download all files from the specified S3 path to your local directory, maintaining the folder structure.

### 3. Reading Files Directly from S3 using Python

If you prefer to directly load the dataset from S3 without downloading, you can use Python libraries like `pandas` or `dask` to read CSV files from the S3 URL.

```python
BASE_URL = "s3://stocktwits-nyu" # or local path BASE_URL="local_path"
CSV_URL = f"{BASE_URL}/dataset/v1/data/csv"
```

#### **Reading with `pandas`:**

```python
import pandas as pd

data_url = f"{CSV_URL}/feature_wo_messages/feature_wo_messages_000.csv"
df = pd.read_csv(data_url, dtype={"sentiment": "object", "message_id": "object"})
print(df.head())
```

#### **Reading with `dask`:**

```python
import dask.dataframe as dd

data_url = f"{CSV_URL}/feature_wo_messages/*.csv"
df_dask = dd.read_csv(data_url, dtype={"sentiment": "object", "message_id": "object"})
print(df_dask.head())
```

### 4. File Descriptions and Data Access

Each folder in the `repo/dataset/v1/data/csv` directory contains CSV files specific to a particular aspect of the dataset. Below are descriptions and sample code for accessing the data.

- **Feature Without Messages**: Contains features extracted without the message content.
  
- **Messages**: Raw message data.
  
- **Message Information**: Metadata for the messages.
  
- **Sentiments**: Sentiment analysis results for each message.
  
- **Symbols**: Information about symbols (e.g., stocks) mentioned within the messages.
  
- **Symbol Sentiments**: Sentiment information mapped to individual symbols.

See code examples in the following sections for accessing each type of data.

### 5. Example Code Using `Dask`

Here is a consolidated example to load and explore all the datasets using Dask:

```python
import dask.dataframe as dd

# S3 paths to CSV files
file_paths = {
    "Feature Without Messages": f"{CSV_URL}/feature_wo_messages/*.csv",
    "Messages": f"{CSV_URL}/messages/*.csv",
    "Message Info": f"{CSV_URL}/msg_info/*.csv",
    "Sentiments": f"{CSV_URL}/sentiments/*.csv",
    "Symbols": f"{CSV_URL}/symbols/*.csv",
    "Symbol Sentiments": f"{CSV_URL}/symbol_sentiments/*.csv"
}

# Load and display each file using Dask
for key, path in file_paths.items():
    print(f"--- {key} ---")
    df = dd.read_csv(path, dtype={"sentiment": "object", "message_id": "object"})
    print(df.head(), "\n")
```

#### Output

```py
--- Feature Without Messages ---
  message_id  user_id            created_at sentiment  parent_message_id  in_reply_to_message_id symbol_list
0          4      593  2008-05-27T15:28:28Z       NaN                NaN                     NaN       ['V']
1          5     8687  2008-05-27T16:03:34Z       NaN                NaN                     NaN     ['NES']
2          6      549  2008-05-27T17:48:41Z       NaN                6.0                     NaN    ['AAPL']
3          7      170  2008-05-27T19:11:10Z       NaN                7.0                     NaN     ['XLE']
4          9      126  2008-05-27T22:39:09Z       NaN                NaN                     NaN    ['AAPL'] 

--- Messages ---
  message_id                                       message_body
0          4                        Sorry, I mean trading $V ;)
1          5  Following HEK ($HEK for stocktweets) this morn...
2          6  Wondering when the $AAPL rocket is going to ta...
3          7  Welcome early adopters! Remember to prefix the...
4          9  My $AAPL puts are now barely profitable.  I st... 

--- Message Info ---
  message_id  length                      important_words
0          4      27         ['sorry', 'mean', 'trading']
1          5      52  ['hek', 'stocktweets', 'following']
2          6      85             ['wwdc', 'rocket', 'im']
3          7      89       ['adopters', 'xle', 'welcome']
4          9     138         ['barely', 'ok', 'absorbed'] 

--- Sentiments ---
  message_id  user_id  created_at sentiment       symbol_list
0   10000059     6472  2012-10-15      -1.0  ['ZNGA', 'META']
1   10000071   148519  2012-10-15       1.0           ['FVI']
2   10000072    75026  2012-10-15       1.0            ['GS']
3   10000084   155028  2012-10-15       1.0          ['WYNN']
4   10000088    75026  2012-10-15       1.0           ['JPM'] 

--- Symbols ---
  message_id  user_id  created_at sentiment symbol_list  sym_number symbol
0          4      593  2008-05-27       NaN       ['V']           1      V
1          5     8687  2008-05-27       NaN     ['NES']           1    NES
2          6      549  2008-05-27       NaN    ['AAPL']           1   AAPL
3          7      170  2008-05-27       NaN     ['XLE']           1    XLE
4          9      126  2008-05-27       NaN    ['AAPL']           1   AAPL 

--- Symbol Sentiments ---
  message_id  user_id  created_at sentiment       symbol_list
0   10000059     6472  2012-10-15      -1.0  ['ZNGA', 'META']
1   10000071   148519  2012-10-15       1.0           ['FVI']
2   10000072    75026  2012-10-15       1.0            ['GS']
3   10000084   155028  2012-10-15       1.0          ['WYNN']
4   10000088    75026  2012-10-15       1.0           ['JPM'] 


```

### **Why Use Dask?**

- **Handles Large Files**: Dask efficiently handles files that are too large to fit in memory.
- **Parallel Computation**: It can utilize multiple cores, making data processing faster.
- **Scalable**: As data grows, Dask scales from a single machine to a distributed cluster.

### 6. Conclusion

This tutorial covers the basics of accessing and exploring the different types of data available in the `repo/dataset` directory. Each type of data serves a unique purpose, and by following the examples provided, you can easily access and analyze them using `Dask` for large-scale data or `pandas` for smaller datasets.

For additional support, feel free to reach out to the repository maintainers or check the `README.md` files in each subfolder for more details.
