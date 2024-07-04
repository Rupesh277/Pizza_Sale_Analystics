<div align="center"> <h1>PIZZA SALES ANALYSIS</h1>
</div>

<div align="center">
    
![image](https://github.com/KARTIKEYASWAMI/Pizza_Sales_Analysis/blob/main/Images/PizzaPIC.png)

</div>     


----

__Tools used : Excel,MS SQL Server,Power BI__

[Dataset used](pizza_sales.csv)

[SQL Analysis-code](sql_analysis.txt) 

[Power BI Dashboard- click to view](Pizza_Sales_BI_Dashboard.pdf)

[Power BI Dashboard- click to interact](https://app.powerbi.com/view?r=eyJrIjoiMWQzOWNmNDItNzZhYS00MDMxLWI5ZDYtM2FmOWUwMzk3ZWIxIiwidCI6IjZmMTFlMWQzLTEyMTAtNDk5YS1iMjY0LTU2NzA0NTY4OGUyNyJ9)


__Business Problem__
-------
A pizza company needs a robust and scalable data analytics solution to handle the vast amount of data(approximately 50k rows of data combined) to effectively analyze and extract meaningful insights like trends, order patterns, menu performance, customer preferences and other key business metrics. Visualizations must also be created to identify trends and insights related to daily and monthly order volume, sales by pizza type and size, and top/bottom sellers. The end goal is to equip the business with data-driven insights that inform strategic decisions to improve operations and profitability.

__Solution Plan__
-------
  + In helping the pizza company gain valuable insights from their sales data CSV file, I will utilize SQL and data visualization with Power BI to extract relevant information and conduct insightful analyses.
    Data cleaning/processing, data transformation and analysis shall be done.
  + By leveraging SQL's functions, I can uncover key metrics like total revenue, average order size, and best/worst selling menu items.
  + Once the data has been extracted and prepared, I will leverage Power BI to present the findings through interactive visualizations. This will allow stakeholders at the pizza company to gain actionable 
    insights 
    into sales trends, order patterns, and menu performance through visually compelling charts, graphs, and reports.
  + I plan on creating a dynamic Power BI dashboard that enables users to explore details on daily order volume, sales by pizza type/size, or top/bottom selling items. The end goal is equipping the business with 
    data-driven insights to inform strategic decisions that improve operations and profitability.

__Execution__
---------

__Questions Answered from the Dataset__

__1) What are the Key Performance Indicators obtained from the Dataset?__

+ __Total Revenue:__
  
      SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;
   
![image](Images/TotalRevenue.png)

This metric provides a clear measure of the overall financial performance of the pizza sales. It indicates the total amount of money generated from pizza 
orders over a specific period, reflecting the revenue potential of the business.
      
+ __Average Order Value:__
    
        SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales
    
![image](Images/Avg_order.png)

Average order value helps in understanding customer spending habits and the effectiveness of marketing strategies. A higher average order value indicates that 
customers are spending more per transaction, which can contribute to increased revenue and profitability.

+ __Total Pizzas Sold:__
    
        SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales
     
![image](Images/total_pizza.png)
    
Total pizzas sold is a fundamental metric that directly reflects product demand. It provides insights into consumption patterns and helps in inventory management and production planning. Understanding total pizzas sold enables businesses to meet customer demand efficiently.
    
+ __Total Orders:__
    
        SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales
     
![image](Images/Total_Orders.png)

Total orders represent the volume of transactions processed within a specific period. Tracking total orders helps in evaluating sales performance and 
identifying trends in customer behavior. It provides a basis for assessing business growth and operational efficiency.

+ __Average Pizzas Per Order:__
    
        SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
        CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
        AS Avg_Pizzas_per_order
        FROM pizza_sales
        
![image](Images/AvgPizza_perOrder.png)

Average pizzas per order indicates the average quantity of pizzas purchased in each transaction. This metric offers insights into customer preferences and 
ordering behavior. Higher average pizzas per order may indicate upselling opportunities or popular menu items.
    
__2) Daily Trend for Total Orders__

        SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
        FROM pizza_sales
        GROUP BY DATENAME(DW, order_date)
        
![image](Images/table.png)

By analyzing the daily trend of total orders over a specific time period, we can identify patterns and fluctuations in order volumes on a daily basis. This 
helps in understanding the demand patterns throughout the week or month, enabling better inventory management and resource allocation.

 __3) Monthly Trend for Orders__

        select DATENAME(MONTH, order_date) as Month_Name, COUNT(DISTINCT order_id) as Total_Orders
        from pizza_sales
        GROUP BY DATENAME(MONTH, order_date)

![image](Images/Table2.png)

The monthly trend of total orders offers insights into the overall order activity throughout the month. Identifying peak periods of high order activity can aid in resource allocation, production planning, and promotional efforts. By recognizing monthly trends, businesses can adjust their operations to capitalize on high-demand periods and optimize efficiency during slower periods.

__4) % of Sales by Pizza Category__

        SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
        CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
        FROM pizza_sales
        GROUP BY pizza_category

![image](Images/Table3.png)

The distribution of sales across different pizza categories helps in understanding the popularity of each category and its contribution to overall sales. This 
insight informs menu optimization strategies, marketing campaigns, and inventory management decisions to maximize revenue and customer satisfaction.

 __5) % of Sales by Pizza Size__

        SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
        CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
        FROM pizza_sales
        GROUP BY pizza_size
        ORDER BY pizza_size

![image](Images/Table4.png)

The chart depicting the percentage of sales attributed to different pizza sizes provides insights into customer preferences regarding pizza size. 
Understanding these preferences allows for better inventory management and menu optimization to meet customer demand effectively.

__6) Total Pizzas Sold by Pizza Category__

        SELECT pizza_category, SUM(quantity) as Total_Quantity_Sold
        FROM pizza_sales
        WHERE MONTH(order_date) = 2
        GROUP BY pizza_category
        ORDER BY Total_Quantity_Sold DESC

![image](Images/Table5.png)

The metric showing total number of pizzas sold for each pizza category enables comparison of sales performance across different categories. This 
visualization helps in identifying popular and less popular pizza options, informing marketing strategies and menu adjustments accordingly.

__7) Top 5 Pizzas by Revenue__

        SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
        FROM pizza_sales
        GROUP BY pizza_name
        ORDER BY Total_Revenue DESC
        
![image](Images/Table6.png)

The chart highlighting the top 5 best-selling pizzas based on revenue, total quantity, and total orders identifies the most popular pizza options. This 
insight aids in understanding customer preferences and allows for targeted promotions or menu enhancements to capitalize on high-demand items.

 __8) Bottom 5 Pizzas by Revenue__

        SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
        FROM pizza_sales
        GROUP BY pizza_name
        ORDER BY Total_Revenue ASC

![image](Images/T7.png)

Conversely, the chart showcasing the bottom 5 worst-selling pizzas based on revenue, total quantity, and total orders helps identify underperforming or less 
popular pizza options. This information is valuable for making strategic decisions such as discontinuing unpopular items or adjusting pricing and marketing 
strategies to boost sales.
 
__9) Top 5 Pizzas by Quantity__

        SELECT Top 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
        FROM pizza_sales
        GROUP BY pizza_name
        ORDER BY Total_Pizza_Sold DESC

![image](Images/T8.png)

Identifying the top 5 pizzas by quantity sold provides insight into the most popular pizza varieties among customers. This information helps in understanding 
customer preferences and allows for targeted marketing efforts or menu optimizations to capitalize on high-demand items.
 
__10) Bottom 5 Pizzas by Quantity__

        SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
        FROM pizza_sales
        GROUP BY pizza_name
        ORDER BY Total_Pizza_Sold ASC

![image](Images/T9.png)

Conversely, identifying the bottom 5 pizzas by quantity sold highlights less popular or underperforming pizza options. This insight can inform menu 
adjustments, promotional strategies, or inventory management decisions to minimize waste and maximize profitability.
  
__11) Top 5 Pizzas by Total Orders__

        SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
        FROM pizza_sales
        GROUP BY pizza_name
        ORDER BY Total_Orders DESC

![image](Images/T10.png)

Identifying the top 5 pizzas by total orders sheds light on the most frequently ordered pizza varieties. This information helps in understanding customer 
behavior and preferences, enabling businesses to tailor marketing campaigns and menu offerings to meet customer demand effectively.

 __12) Bottom 5 Pizzas by Total Orders__

        SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
        FROM pizza_sales
        GROUP BY pizza_name
        ORDER BY Total_Orders ASC

![image](Images/T11.png)

Identifying the bottom 5 pizzas by total orders reveals less popular or niche pizza options that may require attention or adjustments. Understanding these trends allows businesses to make informed decisions regarding menu adjustments, promotional strategies, or inventory management to optimize sales and customer satisfaction.

__13) Generated various Charts and Visuals on Power BI and Excel to identify trends/patterns and gain more insights into the data.__

![image](Images/BI_Dashboard.png)
![image](Images/Excel_Dashboard.png)






__Conclusion__
---------

By delving into various facets of the pizza sales dataset, a comprehensive understanding of consumer behavior and market trends was attained. The analysis unveiled key insights, including the identification of peak and off-peak periods based on daily and monthly order trends. This knowledge equips businesses with the ability to optimize staffing and inventory management to meet varying demand levels effectively. Additionally, the examination of sales distribution across different pizza categories shed light on customer preferences, aiding in menu optimization and marketing strategies to enhance sales performance.

Moreover, calculating the average number of pizzas sold per order provided valuable insights into consumer behavior and consumption patterns, enabling businesses to tailor pricing strategies and portion sizes to maximize profitability and customer satisfaction. Overall, this analysis empowers the pizza establishment to make data-driven decisions, streamline operations, and enhance the overall customer experience, ensuring continued success in the competitive pizza market.
