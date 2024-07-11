<img width="838" alt="Screenshot 2024-07-10 at 9 53 36 PM" src="https://github.com/pchetry2/Retail_Price_Optimization/assets/168946426/1008279a-6ed9-4481-a91e-c3848bd33c08">

# PROBLEM STATEMENT
The fictional retail store noticed their prices were not optimized and sought a data analyst to investigate historical prices, identify crucial price factors, analyze the markets and economic contexts, and customer purchasing behavior to optimize pricing strategies.

# Objectives
* Select and analyze relevant pricing key factors and their influence on prices.
* Calculate the optimal selling prices for products to create efficient, data-driven recommendations.

# Methodology
* Data Collection: I obtained a comprehensive retail dataset from Kaggle containing 30 columns such as product categories, historical prices, quanity,volume, competitor prices,time etc.
* Data Extraction, Cleansing, and Transformation: To ensure the dataset was ready for analysis, I used Python for data extraction, cleansing, and transformation. The following were fixed in the dataset:
   * Changed column names "product_name_lenght" to "product_name_length", "product_description_lenght" to "product_description_length", "month_year to date" and "total_price to    revenue".
   * Converted the date format to a short date format for consistency and ease of analysis.
   * Created groups for product scores(1, 2, & 3) and consolidated them into a simplified product_score column .

* Analysis: To extract insights from the dataset I conducted various SQL queries and Tableau visualizations .
* Recommendations: Provided actionable recommendations based on insights from data analysis.

# SQL Queries and Tableau Visualizations:

1. Identified top selling products
SELECT product_category_name, ROUND(SUM(revenue), 2) AS total_revenue
FROM Retail_Data.Retail_Sales
GROUP BY product_category_name
ORDER BY total_revenue DESC;
<img width="480" alt="Screenshot 2024-07-10 at 10 36 17 PM" src="https://github.com/pchetry2/Retail_Price_Optimization/assets/168946426/2e25dbe2-8d85-4098-8453-d13eff393218">


