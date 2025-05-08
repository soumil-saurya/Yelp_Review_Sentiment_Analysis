# 📊 Yelp Reviews Sentiment Analysis Project

An end-to-end analytics project on 7.1M+ Yelp reviews using SQL, Python, and Snowflake. This case study covers data ingestion from large JSON files, sentiment classification using a Python UDF, and advanced SQL analysis to uncover business insights.

---

## 🧠 Overview

This project demonstrates how to handle large-scale semi-structured data, convert it into usable formats, classify review sentiment, and generate actionable business KPIs. The original Yelp dataset was 5GB+, requiring preprocessing and cloud-based handling.

---

## 🛠️ Tools & Technologies

- **SQL** (Snowflake)
- **Python** (TextBlob, Pandas)
- **AWS S3** (Data Staging)
- **Jupyter Notebook**
- **JSON** (Raw Data)
- **Excel** (Supplementary EDA)

---

## 📁 Dataset

- **Source**: [Yelp Open Dataset](https://www.yelp.com/dataset)
- **Format**: JSON
- **Size**: ~5GB
- **Files Used**: `review.json`, `business.json`, `user.json`

---

## 📦 Data Preprocessing – File Splitting

The 5GB JSON file was split into 10 smaller chunks for faster upload to AWS S3 and parallel processing in Snowflake.

```python
input_file = 'yelp_academic_dataset_review.json'
output_prefix = 'split_file_'
num_files = 10

with open(input_file, "r", encoding="utf8") as f:
    total_lines = sum(1 for _ in f)

lines_per_file = total_lines // num_files

with open(input_file, "r", encoding="utf8") as f:
    for i in range(num_files):
        output_filename = f"{output_prefix}{i+1}.json"
        with open(output_filename, "w", encoding="utf8") as out_file:
            for j in range(lines_per_file):
                line = f.readline()
                if not line:
                    break
                out_file.write(line)
```

---

## 🧩 Project Workflow

### 1. **Data Ingestion**
- Uploaded split files to **AWS S3**.
- Loaded data into **Snowflake** using the `COPY INTO` command and `VARIANT` data type for JSON.

### 2. **Schema Design & Transformation**
- Parsed JSON into structured tables using `SELECT`, `FLATTEN`, and `CAST`.
- Extracted key fields like business ID, user ID, review date, star rating, and review text.

### 3. **Sentiment Analysis**
- Created a Python UDF using **TextBlob** to score and classify reviews into:
  - **Positive**
  - **Neutral**
  - **Negative**

### 4. **SQL Analysis & Business Insights**
- Top business categories by review volume
- Cities with highest activity
- Top users by number of reviews
- % of 5-star reviews per business
- Monthly sentiment trends
- Most recent reviews per business (`ROW_NUMBER()`)

---

## 📊 Key Insights

- 65%+ of reviews were **positive**; food & fitness topped the sentiment charts.
- **Restaurants** dominated review count across regions.
- **July** had the highest number of reviews, indicating seasonal user engagement.
- Top 10 users reviewed 150+ distinct businesses — potential micro-influencers.

---

## 📷 Screenshots

> *(Add dashboard and query result images here once available)*

---

## 📂 Project Structure

```
yelp-sentiment-analysis/
│
├── preprocessing/
│   └── split_yelp_json.py
├── sql/
│   ├── create_tables.sql
│   ├── sentiment_udf.sql
│   └── analysis_queries.sql
├── notebooks/
│   └── sentiment_analysis_demo.ipynb
├── screenshots/
│   └── sample_results.png
└── README.md
```

---

## 🔚 Conclusion

This project showcases a real-world pipeline from raw data ingestion to advanced sentiment analytics using Snowflake and Python. It highlights practical SQL, cloud-based processing, and storytelling with data — valuable for roles in business analytics and operations.
