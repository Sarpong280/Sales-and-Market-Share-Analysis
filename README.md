# My Power BI Project: From Data to Dashboard

This article details my journey through a Power BI project, where I transformed raw data into insightful visualizations and an interactive dashboard. This project  uses real, anonymized data provided by ObviEnce, LLC.

[Interact with the report here](https://app.powerbi.com/view?r=eyJrIjoiZTY4YTk0ZTQtZTM5My00MjE2LWFlY2YtMjIwN2UxYzAyOWMzIiwidCI6IjM0Zjc2MzJlLWViZDQtNDNhMy1hYmE4LTk4YzQxOTgyM2UxZSIsImMiOjN9)

##  Data

In the first phase , I focused on preparing the data made up with sales data from the company and its competitors across seven countries, spanning seven years of transaction data by day, product, and zip code.


## Data Preparation in Power Query Editor

I heavily utilized the Power Query Editor for data shaping operations. Key transformations I performed included:
* **Changing Data Types**: I changed the data type of the "Zip" column from "Whole Number" to "Text" in both the "Sales" and "InternationalSales" queries to preserve leading zeros.
* **Renaming Queries/Tables**: I renamed queries for clarity (e.g., "Sales", "Product", "Geography", "Manufacturer", "International Sales").
* **Filling Empty Values**: In the "Product" query, I filled down empty cells in the "Category" column.
* **Splitting Columns**:
    * I split the "Product" column into "Product" and "Segment" using a pipe (|) delimiter.
    * I split the "Price" column into "MSRP" (price) and "Currency" using the "Column From Examples" feature.
* **Renaming Columns**: I renamed the newly split columns appropriately.
* **Removing Unwanted Rows**: I removed informational top and bottom rows from the "Geography" and "Manufacturer" queries respectively.
* **Promoting First Row as Headers**: I promoted the first row of the "Geography" query to become the column headers.
* **Transposing Data**: I transposed the "Manufacturer" query to reorient data from rows to columns, and then promoted the first row to headers.
* **Appending Queries**: Finally, I appended the rows from the "International Sales" query to the "Sales" query to create a single, comprehensive sales table.


This ensured the data was clean, correctly formatted, and properly structured for analysis in Power BI.

---

## Lab 2: Data Modeling and Exploration

* **Initial Visualization**: I began by analyzing sales by country, creating a Clustered column chart with `Country` on the X-axis and `Revenue` on the Y-axis.
* **Addressing Relationship Issues**: Initially, all countries showed the same `Sum of Revenue` due to a lack of an active relationship between `Sales` and `Geography` tables. An attempt to create a direct relationship on `Zip` resulted in a many-to-many cardinality warning.
* **Creating a Unique Key for Relationships**: To resolve the many-to-many issue, I created a new calculated column called `ZipCountry` in both the `Sales` and `Geography` tables by concatenating `Zip` and `Country`.
    * `ZipCountry = Sales[Zip] & "," & Sales[Country]`
    * `ZipCountry = Geography[Zip] & "," & Geography[Country]`
* **Establishing the Relationship**: With the new `ZipCountry` columns, I successfully created a 1-to-many relationship between `Sales` and `Geography` tables.
* **Verifying the Visual**: My clustered column chart then correctly displayed different sales figures for each country.
* **Date Table**: A dedicated Date table was created using the CALENDAR function to support time-based analysis.
CALENDAR(DATE(2014,1,1), DATE(2022,12,31)) 
* **Additional Measures**: Created sales and previous years then used these two to create percentage growth
    * Sales = SUM(Sales[Revenue])
    * PY Sales = CALCULATE(SUM(Sales[Revenue]),SAMEPERIODLASTYEAR('Date'[Date]))
    * % Growth = DIVIDE(SUM(Sales[Revenue])-[PY Sales],[PY Sales])

---

## Visualization
I arranged the visuals into a two-page report. The first page being the Market Share overview and the second being a breakdown by Manufacturer.
