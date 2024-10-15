Module 22 Challenge: Home Sales Data Analysis with SparkSQL
Overview
In this challenge, we use PySpark and SparkSQL to analyze home sales data. We'll calculate key metrics, create temporary tables, cache and uncache data, and evaluate performance improvements by partitioning and caching the data. The objective is to explore and demonstrate efficient data manipulation using SparkSQL in a large dataset.

Repository Setup
Create a new repository on GitHub named Home_Sales.
Clone the repository to your local machine using the following command:
bash
Copy code
git clone https://github.com/your-username/Home_Sales.git
Push changes regularly to your GitHub repository as you complete the challenge.
Files Required
The starter file Home_Sales_starter_code.ipynb
The dataset home_sales_revised.csv
Instructions
Step 1: Rename the Starter Code File
Rename the file Home_Sales_starter_code.ipynb to Home_Sales.ipynb.

Step 2: Import PySpark SQL Functions
Ensure that you import the necessary PySpark SQL functions at the beginning of the notebook:

python
Copy code
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, avg, round
Step 3: Load the Data
Read the home_sales_revised.csv file into a Spark DataFrame:

python
Copy code
df = spark.read.csv("home_sales_revised.csv", header=True, inferSchema=True)
Step 4: Create Temporary Table
Create a temporary table called home_sales from the DataFrame:

python
Copy code
df.createOrReplaceTempView("home_sales")
Step 5: Answer the Following Questions Using SparkSQL
Average price for four-bedroom houses sold each year:

Query to calculate the average price of four-bedroom houses sold in each year, rounded to two decimal places:
sql
Copy code
SELECT YEAR(date) AS year_sold, ROUND(AVG(price), 2) AS avg_price
FROM home_sales
WHERE bedrooms = 4
GROUP BY year_sold
ORDER BY year_sold;
Average price of homes with three bedrooms and three bathrooms for each year:

Query to calculate the average price of homes with 3 bedrooms and 3 bathrooms built in each year:
sql
Copy code
SELECT YEAR(date_built) AS year_built, ROUND(AVG(price), 2) AS avg_price
FROM home_sales
WHERE bedrooms = 3 AND bathrooms = 3
GROUP BY year_built
ORDER BY year_built;
Average price of homes with specific criteria:

Query to calculate the average price of homes with 3 bedrooms, 3 bathrooms, 2 floors, and at least 2,000 square feet:
sql
Copy code
SELECT YEAR(date_built) AS year_built, ROUND(AVG(price), 2) AS avg_price
FROM home_sales
WHERE bedrooms = 3 AND bathrooms = 3 AND floors = 2 AND sqft_living >= 2000
GROUP BY year_built
ORDER BY year_built;
Average price per view rating where average price ≥ $350,000:

Query to calculate the average price per view rating for homes where the average home price is greater than or equal to $350,000:
sql
Copy code
SELECT view, ROUND(AVG(price), 2) AS avg_price
FROM home_sales
GROUP BY view
HAVING AVG(price) >= 350000
ORDER BY avg_price;
Step 6: Cache the Table
Cache the home_sales temporary table and check if it is cached:

python
Copy code
spark.sql("CACHE TABLE home_sales")
spark.catalog.isCached("home_sales")
Step 7: Run Cached Query and Compare Runtime
Re-run the query from step 4 using the cached data, and record the runtime to compare it with the uncached runtime.

Step 8: Partition the Data by date_built
Partition the data by the date_built field and write it as a Parquet file:

python
Copy code
df.write.partitionBy("date_built").parquet("home_sales_partitioned")
Step 9: Create Temporary Table for Parquet Data
Create a temporary table for the partitioned Parquet data:

python
Copy code
parquet_df = spark.read.parquet("home_sales_partitioned")
parquet_df.createOrReplaceTempView("home_sales_parquet")
Step 10: Run Query on Parquet Data and Compare Runtime
Run the query from step 4 on the partitioned Parquet data and compare the runtime to the previous uncached and cached runtimes.

Step 11: Uncache the Table
Uncache the home_sales table and verify it has been uncached:

python
Copy code
spark.sql("UNCACHE TABLE home_sales")
spark.catalog.isCached("home_sales")
