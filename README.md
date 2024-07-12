<img width="838" alt="Screenshot 2024-07-10 at 9 53 36 PM" src="https://github.com/pchetry2/Retail_Price_Optimization/assets/168946426/1008279a-6ed9-4481-a91e-c3848bd33c08">

# Problem Statement
The fictional retail store noticed their prices were not optimized and sought a data analyst to investigate historical prices, identify crucial price factors, analyze the markets and economic contexts, and customer purchasing behavior to optimize pricing strategies.

# Objectives
* Select and analyze relevant pricing key factors and their influence on prices.
* Calculate the optimal selling prices for products to create efficient, data-driven recommendations.

# Methodology
* Data Collection: I obtained a comprehensive retail dataset from Kaggle containing 30 columns such as product categories, historical prices, quanity,volume, competitor prices.
* Data Extraction, Cleansing, and Transformation: To ensure the dataset was ready for analysis, I used Python for data extraction, cleansing, and transformation. The following were fixed in the dataset:
   * Changed column names "product_name_lenght" to "product_name_length", "product_description_lenght" to "product_description_length", "month_year to date" and "total_price to    revenue".
   * Converted the date format to a short date format for consistency and ease of analysis.
   * Created groups for product scores(1, 2, & 3) and consolidated them into a simplified product_score column .

* Analysis: To extract insights from the dataset I conducted various SQL queries in MySQL server and Tableau visualizations .
* Recommendations: Provided actionable recommendations based on insights from data analysis.

# SQL Queries and Tableau Visualizations

1. Identified top selling products:
SELECT product_category_name, ROUND(SUM(revenue), 2) AS total_revenue
FROM Retail_Data.Retail_Sales
GROUP BY product_category_name
ORDER BY total_revenue DESC;
2. Analyzing Monthly Revenue Trends:
SELECT
product_id, DATE_FORMAT(date, '%Y-%m') AS month,
Round(SUM(revenue),2) AS monthly_revenue
FROM Retail_Data.Retail_Sales
GROUP BY 1,2
ORDER BY 2;
4. Average Unit Price and Competitor Prices by Product Category:
SELECT 
    product_category_name, 
    ROUND(AVG(unit_price), 2) AS avg_unit_price, 
    ROUND(AVG(comp_1), 2) AS avg_comp_1_price, 
    ROUND(AVG(comp_2), 2) AS avg_comp_2_price, 
    ROUND(AVG(comp_3), 2) AS avg_comp_3_price
FROM 
    Retail_Data.Retail_Sales
GROUP BY 1;

5. Percentage of Products Priced Lower than Competitors:
SELECT 
    product_id, 
    product_category_name,
    ROUND(SUM(CASE WHEN comp_1 > unit_price THEN 1 ELSE 0 END) / CAST(COUNT(*) AS FLOAT) * 100, 2) AS percent_lower_than_comp_1,
    ROUND(SUM(CASE WHEN comp_2 > unit_price THEN 1 ELSE 0 END) / CAST(COUNT(*) AS FLOAT) * 100, 2) AS percent_lower_than_comp_2,
    ROUND(SUM(CASE WHEN comp_3 > unit_price THEN 1 ELSE 0 END) / CAST(COUNT(*) AS FLOAT) * 100, 2) AS percent_lower_than_comp_3
FROM 
    Retail_Data.Retail_Sales
GROUP BY 1,2;

6. Sales impact when priced higher or lower than competitors:
    SELECT 
    product_id,
    product_category_name,
    ROUND(AVG(unit_price), 2) AS avg_unit_price,
    ROUND(AVG(CASE WHEN comp_1 > unit_price THEN revenue ELSE NULL END), 2) AS avg_revenue_lower_than_comp_1,
    ROUND(AVG(CASE WHEN comp_1 < unit_price THEN revenue ELSE NULL END), 2) AS avg_revenue_higher_than_comp_1,
    ROUND(AVG(CASE WHEN comp_2 > unit_price THEN revenue ELSE NULL END), 2) AS avg_revenue_lower_than_comp_2,
    ROUND(AVG(CASE WHEN comp_2 < unit_price THEN revenue ELSE NULL END), 2) AS avg_revenue_higher_than_comp_2,
    ROUND(AVG(CASE WHEN comp_3 > unit_price THEN revenue ELSE NULL END), 2) AS avg_revenue_lower_than_comp_3,
    ROUND(AVG(CASE WHEN comp_3 < unit_price THEN revenue ELSE NULL END), 2) AS avg_revenue_higher_than_comp_3
FROM 
    Retail_Data.Retail_Sales
GROUP BY 1,2 ;
7. Sales volume impact by Price range:
    SELECT 
    product_category_name,
    SUM(CASE WHEN unit_price BETWEEN 0 AND 50 THEN qty ELSE 0 END) AS qty_sold_0_50,
    SUM(CASE WHEN unit_price > 50 THEN qty ELSE 0 END) AS qty_sold_abv_50
FROM 
    Retail_Data.Retail_Sales
GROUP BY 1;

 8. Customer Segmentation based on purchasing behavior
WITH customer_segments AS (
    SELECT
        product_id,
        SUM(qty) AS total_quantity,
        SUM(revenue) AS total_revenue,
        COUNT(DISTINCT customers) AS customers_count
    FROM
        Retail_Data.Retail_Sales
    GROUP BY
        product_id
)
SELECT
    product_id,
    customers_count AS unique_customers,
    total_quantity,
    total_revenue,
    CASE
        WHEN total_revenue >= 10000 THEN 'High Value Customer'
        WHEN total_revenue >= 5000 THEN 'Medium Value Customer'
        ELSE 'Low Value Customer'
    END AS customer_segment
FROM
    customer_segments
ORDER BY
    total_revenue DESC;
    
# Visualization of the data insights
https://github.com/pchetry2/Retail_Price_Optimization/assets/168946426/99471bb0-5873-4a32-9d91-023814f8dd40

# Observations & Recommendations
* Health & Beauty($212k)and Watches & Gifts($207k) generated the highest revenue,indicating strong presence and market demand while Consoles & Games($5.8k) and Perfumery($20k) had lower revenues.
   Focus marketing efforts on high-performing categories to boost the sales and Investigate & improve strategies for lower-performing categories.
* Some products like Health & Beauty and Watches & Gifts had consistent high revenues month over month, peak in December likely due to holiday season while others had sporadic sales.
   Aligning marketing strategies & inventory planning is recommended along with the offering promotions during low revenue months to increase sales.
* Computers Accessories, Furniture Decor, and Bed Bath Table are generally competitively priced against most competitors, while 
Watches Gifts, Cool Stuff, and Perfumery are priced higher than competitors, indicating a premium pricing strategy.
 Adjustment of prices for Watches Gifts, Cool Stuff, and Perfumery is recommended to better align with competitors and potentialy capture more market share.
* Customers have purchased Health Beauty, watches & gifts ,garden tools more frequently and low count in console games & perfume categories.
  To driven revenue on high-performing categories like garden tools, health beauty, and watches and gifts,focus marketing efforts and   promotions.
 Consider strategies to boost sales in underperforming categories like console games and perfumery, such as special discounts, bundling  offers, or targeted advertising.









