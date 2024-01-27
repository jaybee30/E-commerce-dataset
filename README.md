# E-commerce-dataset
The aim of this analysis is to understand the e-commerce dataset, clean the raw dataset, solve the problem statements, generate insights from the dataset which will in turn guide in making useful business decision.


The dataset consists of main 2 tables, namely: ecommerce_data and us_state_long_lat_codes.
The first table ecommerce table consist of 21 columns and 113,271 rows  and the second table(us_state_long_lat_codes) consist of 4 columns and 52 rows.

**DATA CLEANING**

A new table was created namely CALENDAR. This was obtained from column (order_date) in e-commerce data. The calendar table consist of DATE, Year, Month and Month number.
•	Date = CALENDAR(MIN(ecommerce_data[order_date]),MAX(ecommerce_data[order_date]))
•	Year = YEAR('Calendar'[Date]). Created in order to extract just year from order date
•	Month = FORMAT('Calendar'[Date], "mmmm"). Created in order to extract just month name from order date
•	Month Number = month('Calendar'[Date]). This will be used to order the month name for sorting purpose.

Since the dataset consist of more than a table, a model was created.
Calendar table was linked to e-commerce table by creating a relationship using column name (Date) from Calendar table and column (order_date) from e-commerce table.
The cardinality of the relationship used was many to one. 
e-commerce table was linked to us_state_long_lat_codes by creating a relationship using column name (customer state) from e-commerce table and column (name) from us_state_long_lat_codes.
The cardinality of the relationship used was many to one. 

**PROBLEM STATEMENT**

•	Create a KPI banner showing Year to Date (YTD) sales, YTD Profit, YTD Quantity sold, YTD Profit margin
•	Find Year on Year growth for each KPI and show a YTD sparkline for each measure in the KPI to understand the monthly trend for each fact
•	Find YTD Sales, Previous Year to Date (PYTD) sales, Year on Year (YoY) sales growth for different customer category. Add a trend icon for each category
•	Find YTD Sales performance by each state
•	Top 5 and bottom 5 Products by Sales
•	YTD Sales by Region to know best and worst performing region all over the country
•	YTD Sales by shipping type to get the best shipping type percentage
TOOLS USED FOR THE ANALYSIS
•	MS EXCEL (POWER QUERY)
•	MS SQL SERVER version 19.1
•	POWER BI version 2.119

**DASHBOARD**

THE DASHBOARD CONSISTS OF:
•	TITLE 
•	SEGMENT (Consumer, Corporate and Office)
•	KPIs
•	Sales by category
•	Top 5 products YTD sales
•	Bottom 5 products YTD sales
•	YTD sales by customer region
•	YTD sales by shipping type
•	Sales by state

**CREATING KPI MEASURES**

•	YTD SALES (Year to date sales): expressed as sum of current year sales per order.
A measure was created to calculate Year to date sales. The DAX function can be found below. A total of $11.53M sales was made in 2022. As opposed the previous year sale (PYTD) which was $11.63. for further analysis, a Year on Year difference was calculated by expressing the value obtained from previous year sale to current year sale. Year on year value for YTD sales was -0.83%. This indicates a deficit in YTD sales as compared to previous year. The peak of sales was recorded in October ($1,112,452.07).
•	YTD PROFIT (Year to date profit): expressed as sum of current year profit per order.
A measure was created to calculate Year to date profit. The DAX function can be found below. A total of $1.34M profit was made in 2022. For further analysis, a Year on Year difference was calculated by expressing the value obtained from previous year profit to current year profit. Year on year value for YTD profit was 4.50%. This indicates an increase in YTD profit as compared to previous year. The peak of sales was recorded in October ($138,185.60) and the least at December ($87,411).
•	YTD QUANTITY (Year to date quantity): expressed as sum of current year order quantity.
A measure was created to calculate Year to date quantity. The DAX function can be found below. A total of 107,200K quantity of products purchased was recorded in 2022. For further analysis, a Year on Year difference was calculated by expressing the value obtained from previous year profit to current year quantity sold. Year on year value for YTD quantity was -7.29%. This indicates a decrease in YTD quantity as compared to previous year. The peak of quantity was recorded in October (11,688) and the least at December (4,654).

•	YTD PROFIT MARGIN (Year to date profit margin): expressed as sum of profit per order divided by sum of sales per order.

A measure was created to calculate Year to date profit margin. The DAX function can be found below. A total of 11.58% profit margin was recorded in 2022. For further analysis, a Year on Year difference was calculated by expressing the value obtained from previous year profit margin to current year profit margin. Year on year value for YTD profit margin was 5.37%. This indicates a increase in YTD quantity as compared to previous year. The peak of profit margin was recorded in April (0.13%) and the least at July (0.10%).



**DAX FUNCTIONS**

•	YTD SALES: TotalYTD = (SUM (ecommerce_data[sales_per_order]),’calender’[Date])
This represents the total sales recorded
•	YTDPROFIT:YTDprofit= TOTALYTD(SUM(ecommerce_data[profit_per_order]),'Calendar'[Date])
This represents the total profit recorded
•	YTD QUANTITY: YTD Qty = TOTALYTD(SUM(ecommerce_data[order_quantity]), 'Calendar'[Date])
This represents the total quanity recorded
•	YTD PROFIT MARGIN: YTD Profit Margin = TOTALYTD([Profit Margin], 'Calendar'[Date])
Profit margin represents sum of profit per order
•	YoY Sales = ([YTD Sales] - [PYTD Sales])/[PYTD Sales]
Year on Year sales is the difference in sales for the two years covered in the dataset
•	PYTD Sales = CALCULATE(SUM(ecommerce_data[sales_per_order]), DATESYTD(SAMEPERIODLASTYEAR('Calendar'[Date])))
PYTD is Previous Year to date sales

•	YoY Qty = ([YTD Qty] - [PYTD Qty])/[PYTD Qty]
Year on Year sales is the difference in sales for the two years covered in the dataset

•	PYTD Qty = CALCULATE(SUM(ecommerce_data[order_quantity]), DATESYTD(SAMEPERIODLASTYEAR('Calendar'[Date])))
PYTD Qty = previous year to date quantity

•	YoY Profit Margin = ([YTD Profit Margin] - [PYTD Profit Margin])/[PYTD Profit Margin]
Year on Year profit margin is the difference in profit margin for the two years covered in the dataset
PYTD Profit Margin = CALCULATE([Profit Margin], DATESYTD(SAMEPERIODLASTYEAR('Calendar'[Date])))

•	YoY Profit = ([YTD Profit] - [PYTD Profit])/[PYTD Profit]
Year on Year profit is the difference in profit margin for the two years covered in the dataset.
PYTD Profit = CALCULATE(SUM(ecommerce_data[profit_per_order]), DATESYTD(SAMEPERIODLASTYEAR('Calendar'[Date])))

The Year on Year values will be represented in whole number, but for the sake of this analysis, I changed it to percentage so as to see the difference in YoY values between the 2 years.
Next was creating a trend measure that will make it easier to differentiate if there is increase or decrease in the YoY VALUES.

•	SALES ICON = var positive_icon = UNICHAR(9650)
            var negative_icon = UNICHAR(9660)
                		var result = IF([YoY Sales]>0, positive_icon, negative_icon)
                			RETURN RESULT
The UNICHAR(9650) and UNICHAR(9660) are triangular shapes to represent increase or decrease  	  
This measure was repeated for the other 3 KPIs (Profit, Quantity and Profit margin)

After creating the trend icon, more formatting was done to make the visualization more detailed and self explanatory. Such as creating colours, icon colour for all trends of the KPIs. This was done by creating more measures

•	Sales Colour = IF([YoY Sales]>0, "Green", "Red"). 
This function will change the trend colour to either green or red depending on the Year on Year sales value created earlier.
This measure was repeated for the other 3 KPIs (Profit, Quantity and Profit margin)


*SALES BY CATEGORY*
Matrix card was used. 
The following measures were used for this problem statement:
On the row axis, Category name was used
On the values: YTD Sales, PYTD Sales, Year on Year sales and Trend were used.

The formula for Trend :
•	Trend = var positive_icon = UNICHAR(9650)
            var negative_icon = UNICHAR(9660)
                		var result = IF([YoY Sales]>0, positive_icon, negative_icon)
                			RETURN RESULT
                   
In this dataset, three products make up the category section namely: (Furniture, Office supplies and Technology). 
For furniture, $2.52M YTD sales was recorded, and $2.50M PYTD was recorded. There was increase in YoY sales with a value of 0.73%.
For Office supplies, $6.92M YTD sales was recorded, and $7.000M PYTD was recorded. There was decrease in YoY sales with a value of -1.22%.
For technology, $2.10M YTD sales was recorded, and $2.13M PYTD was recorded. There was increase in YoY sales with a value of -1.37%.
The least sales were recorded for technology category in the current year.


*TOP 5 PRODUCT YTD SALES*
Stacked bar chart was used
On the Y-Axis, product name was used
On the X-Axis, YTD Sales was used
I formatted the bar chart in order to limit the returned value to TOP 5. This was done by clicking filters> drag in product name> filter type TOP N> show items TOP, value 5> 
Filter by value, YTD Sales.
This will limit the value to Top 5 product in respects to sales.
Also, I added data labels. Position of data label used was inside end.

The result of this analysis depicts the top5 products by sales to be: (Staple envelope with a total sale of $57k, Staples $52k, Easy staple paper $47k, Staples in misc. colours $26k and the least K1 adjustable head chair with $22k.

*BOTTOM 5 PRODUCT YTD SALES*
Stacked bar chart was used
On the Y-Axis, product name was used
On the X-Axis, YTD Sales was used
I formatted the bar chart in order to limit the returned value to TOP 5. This was done by clicking filters> drag in product name> filter type TOP N> show items BOTTOM, value 5> 
Filter by value, YTD Sales.
This will limit the value to Top 5 product in respects to sales.
Also, I added data labels. Position of data label used was inside end.

The result of this analysis depicts the bottom 5 products by sales to be: (Eldon with a total sale of $379.89, Lexmark $269.98, Cisco $250, Xerox blank computer paper $26k and the least Rediform message books with $22k.


SALES BY STATE
Map was used to solve this problem statement.
After selecting the map chart type, the following measures were used:
Legend: customer region
LATITUDE: latitude
LONGITUDE: longitude 
BUBBLE SIZE: YTD sales
TOOL TIPS: first name and YoY sales
Additional formatting was done on the map under visual. 
Under map settings, style: dark, legend position: top left, bubble size: -5, colours: green (central), light blue (east), purple (south), deep blue (west). Also, under control, zoom buttons were turned ON.

Customer region: central, east, south and west.
CENTRAL: the highest sales was recorded in Texas ($1,169,658) and the least in south Dakota $8490
WEST: the highest sales was recorded in California ($2,335,532) and the least in Wyoming $1,069
EAST: the highest sales was recorded in New York ($1,286,687) and the least in West Virginia $5085
SOUTH: the highest sales was recorded in Florida ($449,323) and the least in Louisiana $45882


SALES BY CUSTOMER REGION
Donut chart was used for this purpose.
Under legend: customer region was used
Under values: YTD sales was used
Under visual: slices (different colour was used for regions)
Detail labels, position: outside, Label contents: category and percent of total.

The analysis takes into consideration FOUR customer regions namely: West, East, Central and South.
The highest sales per region was recorded in the west with a total of 32.22%, followed by East 28.42%, Central 23.19% and the least for South 16.17%.

SALES BY SHIPPING TYPE
Donut chart was used for this purpose.
Under legend: shipping type was used
Under values: YTD sales was used
Under visual: slices (different colour was used for shipping type)
Detail labels, position: outside, Label contents: category and percent of total.

The analysis takes into consideration FOUR shipping type namely: First class, second class, standard class and same day delivery
The shipping type widely used among customers was the standard class with a total of 60.51%, followed by Second class 19.22%, First class 15.10% and the least for Same day delivery 5.17%.


![E-COMMERCE RAW DATASET](https://github.com/jaybee30/E-commerce-dataset/assets/106179938/5a6801e3-8a3a-4863-a380-2d6c05d4ed13)
![US STATE LONG & LAT RAW DATA](https://github.com/jaybee30/E-commerce-dataset/assets/106179938/d86bf2e3-4ac6-4a98-840a-cefebb575d82)
![CREATING RELATIONSHIP](https://github.com/jaybee30/E-commerce-dataset/assets/106179938/a9fcfe5c-1b0f-44b7-aa2c-7cb7fb3d7966)
![DASHBOARD PIC](https://github.com/jaybee30/E-commerce-dataset/assets/106179938/49d65122-288a-4a0b-a1b1-9aaf1e2f709d)


To have access to the complete interactive dashboard, click on the link below:








