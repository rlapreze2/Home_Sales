#Module 22 Challenge – Big Data with SparkSQL**


## Project Overview

PySpark and SparkSQL is utilized to explore a dataset of King County home sales. Perform the following:

* Load a CSV of home‑sales data into a Spark DataFrame.
* Register it as a temp view and write SQL queries to answer business questions (average prices, filters by beds/baths/floors, etc.).
* Cache the view, partition output as Parquet, then measure caching vs. uncached runtimes.

By completing these steps you gain hands‑on experience with Spark’s DataFrame API, SparkSQL, caching, partitioning, and performance tuning in a distributed environment.

## Data Source

* **Home Sales Dataset**:
  URL: `https://2u-data-curriculum-team.s3.amazonaws.com/dataviz-classroom/v1.2/22-big-data/home_sales_revised.csv`
  
## Setup & Installation

1. Open a new Colab notebook.
2. Copy in the first cell from `home_sales.py` (cells 1–2) to install Spark:

   ```python
   import os
   spark_version = 'spark-3.5.5'
   os.environ['SPARK_VERSION'] = spark_version
   # ... install findspark / Java, then:
   from pyspark import SparkFiles
   from pyspark.sql import SparkSession
   spark = SparkSession.builder.appName("SparkSQL").getOrCreate()
   ```
3. Run the remaining cells as‑is.

## SparkSQL Exercises

In the notebook are numbered cells for each SQL task:

1. **Load & View Schema**

   ```sql
   df = spark.read.csv(..., header=True, inferSchema=True)
   df.printSchema()
   df.show(5)
   ```

2. **Create Temp View**

   ```python
   df.createOrReplaceTempView("home_sales")
   ```

3. **Queries 1–5**:

   * Avg price of 4‑bedroom houses per year (2 decimals).
   * Avg price per year for 3 bed/3 bath/2 floors with ≥ 2000 sqft.
   * Avg price/floor rating for homes ≥ \$350 K, etc.

4. **Caching**

   ```python
   spark.catalog.cacheTable("home_sales")
   ```

   Rerun a heavy query and record `cached_time`.

5. **Partition & Parquet**

   ```python
   df.write.partitionBy("date_built").parquet("partitioned_home_sales")
   p_df = spark.read.parquet("partitioned_home_sales")
   p_df.createOrReplaceTempView("p_home_sales")
   ```

6. **Performance Comparison**

   * Measure `uncached_time - cached_time` vs. `cached_time - partitioned_time`.

7. **Cleanup**

   ```python
   spark.sql("UNCACHE TABLE home_sales")
   spark.catalog.isCached("home_sales")  # should return False
   ```

## Performance Comparison

Print out three runtimes:

* **Uncached Runtime**
* **Cached Runtime**
* **Partitioned Parquet Runtime**

Compute the deltas to show the benefits of caching vs. partitioning.

## Rubric Alignment

Refer to **22\_BigData.html** for full criteria. Key points:

* **Data Loading (10 pts)**: Correct CSV ingestion & schema inference.
* **SQL Queries (40 pts)**: Five distinct queries with correct filtering, grouping, formatting.
* **Caching & Partitioning (25 pts)**: Demonstrate caching, partitioned write/read, and measure times.
* **Performance Analysis (25 pts)**: Report runtimes and clear comparison.

## Usage

1. **Launch** the Collab environment.
2. **Execute** each cell in order.
3. **Capture** the printed runtimes and include them in your write‑up or notebook comments.

## Limitations & Future Work

* **Cluster Scale**: This runs on a single‑node session; true cluster performance may differ.
* **Partitioning Field**: We partitioned by `date_built`, but alternate fields (e.g., `bedrooms`) are also considerations for future study.
* **Cache Persistence**: Cache is lost on session end—consider persisting intermediate DataFrames.

## References & Citations

- **Apache Spark Documentation**  
  https://spark.apache.org/docs/latest/  

- **PySpark API Reference**  
  https://spark.apache.org/docs/latest/api/python/  

- **Spark SQL, DataFrames & Datasets Guide**  
  https://spark.apache.org/docs/latest/sql-programming-guide.html  

- **Home Sales Dataset**  
  https://2u-data-curriculum-team.s3.amazonaws.com/dataviz-classroom/v1.2/22-big-data/home_sales_revised.csv  

- **Python `time` Module**  
  https://docs.python.org/3/library/time.html  

- **OpenAI's ChatGPT for help creating this README**  
  https://chatgpt.com  