# Data-Mart-Design-and-Implementation-of-OLAP-Queries-with-SQL
This work involves designing a data mart for a UK-based multinational e-commerce retail store and implementation of OLAP queries on the created data mart using Pandas SQL

## Data Mart Design
A data mart is a subset of a data warehouse, and it stores information that is specific to a single unit of a business. It enables teams to gain insight faster since they do not have to search for it in the data warehouse. 
The data mart was designed based on the CRM data of the company retrieved from UCI repository website: https://archive.ics.uci.edu/dataset/502/online+retail+ii. 

## Analysis of the CRMS data and identification of entities
The CRMS data has nine columns, and they are Invoice Number, Stock Code, Description, Quantity, Invoice Date, Price, Customer ID, Country, and Amount.  This data is not sufficient to support a broader analysis to cater for the needs of a multinational online retail business as there are other useful dimensions and attributes of the existing dimensions of sales that are not captured by the CRMS.
This data mart is designed to fit the future information requirements of the multinational online retail business as it provides a broader dimension for analyzing the business’ sales. Two new attributes were added, namely currency and promotion. The table below shows the list of all the entities identified in the CRMS data and the new attributes added with their respective entity attributes and domain.

|Entity Name | Entity Attribute | Entity Domain|
|---------| ----------| ---------|
|Sales	|Invoice Number	|Unique Identifier|
|	|Description|	Text|
|	|Price|	Currency|
|	|Quantity|	Numeric|
|	|Stock Code|	Unique Identifier|
|	|Invoice Date|	Date|
|	|Customer ID|	Unique Identifier|
|	|Country|	Text|
|	|Amount|	Currency|
|Product|	Stock Code|	Unique Identifier|
|	|Description|	Text|
|Date|	Date ID|	Unique Identifier|
|	|Date|	Date|
|	|Day|	Text|
|	|Month|	Text|
|	|Quarter|	Text|
|	|Year|	Numeric|
|Customer|	Customer ID|	Unique Identifier|
|	|Customer Name|	Text|
|	|Customer Email|	Text|
|	|Address|	Text|
|Country|	Country Code|	Unique Identifier|
|	|Country Name|	Text|
|Promotion|	Promotion ID|	Unique Identifier|
|	|Promotion Name|	Text|
|	|Start Date|	Date|
|	|End Date|	Date|
|	|Discount Percentage|	Numeric|
|	|Discount Percentage|	Numeric|
|	|Promotion Type|	Text|
|	|Target Audience|	Text|
|	|Pomotion Channel|	Text|
|	|Product Categories|	Text|


## Conceptual ERD
Figure 8 below is the conceptual ERD of the multinational online retail store. In this ERD, 0..* represents zero to many relationship, 0..1 represents zero to one relationship, 1..* represents one to many relationship, 1..1 represents one to one relationship, *...1 represents many to one relationship and *..* represents many to many relationship.
 
![](https://github.com/2-88/Data-Mart-Design-and-Implementation-of-OLAP-Queries-with-SQL/blob/main/Screenshot%20(5).png)

Explanation to the Conceptual ERD:
A customer is associated with one or more sales and each sales record is for a particular customer.
Each sale has one or more products and zero or more of a product is present in a one sales.
Many Sales is done on a certain date and a date is associated with many sales.
A single sale involves one currency, and one currency is used for many sales.
Each sale is from a specific country and a country can have zero or more sales at a given time.
A sale zero or more promotions and many promotions can be associated with a particular sale.

## Logical ERD
The Figure below is the logical ERD for the conceptual ERD above.  “PK” denotes the primary keys and “FK” denotes the foreign keys.
 
![](https://github.com/2-88/Data-Mart-Design-and-Implementation-of-OLAP-Queries-with-SQL/blob/main/Screenshot%20(5).png)

Explanation to the Logical ERD:
From the above logical ERD, Customer ID is the primary key for Customer table, Country Code is the primary key for Country table, Date ID is the primary key for Date table, Currency ID is the primary key for Currency table, Promotion ID is the primary key for Promotion table and Stock Code is the primary key for Product table.
All the afore-mentioned keys are captured as foreign keys in the Sales table. The primary key to the Sales table is the invoice number.

##Fact Table
The sales table stores the most relevant information the business wants to keep and analyze to gain insight. The sales table is the most relevant information for the business and is therefore the fact table. It has the primary keys of all the dimensions as its foreign keys and can be queried to obtain sales information with respect to all the dimensions. Thus, sales can be analyzed with respect to customer, product, country, date, or currency. Figure 10 below shows the sales fact table.
•	Fact Table: Sales
•	Invoice Number: A unique identifier for each invoice.
•	Description: A brief description of the purchased items.
•	Price: The price per unit of the item.
•	Quantity: The quantity of items purchased.
•	StockCode: A unique identifier for each product.
•	InvoiceDate: The date when the invoice was generated.
•	CustomerID: A unique identifier for each customer.
•	Country: The country where the customer is located.
•	Dimension Tables:
•	Product Dimension
•	StockCode: A unique identifier for each product.
•	Description: A detailed description of the product.
•	Other product-related attributes as needed.
•	Date Dimension
•	DateKey: A unique identifier for each date.
•	Date: The date in YYYY-MM-DD format.
•	Day: The day of the week (e.g., Monday, Tuesday).
•	Month: The month (e.g., January, February).
•	Quarter: The quarter of the year.
•	Year: The year.
•	Customer Dimension
•	CustomerID: A unique identifier for each customer.
•	CustomerName: The name of the customer.
•	CustomerEmail: The email address of the customer.
•	Address: The customer's address.
•	Phone Number: The customer's phone number.
•	Country Dimension
•	Country Code: A unique identifier for each country.
•	Country Name: The name of the country.
•	Promotion Dimension
•	Promotion ID: A unique identifier for each promotion
•	Promotion Name: It provides the name or title of the promotion
•	Start Date: Date
•	End Date: Date
•	Discount Percentage: The amount by which the product's price is reduced during the promotion
•	Promotion Type: The type or category of the promotion, such as a seasonal sale, flash sale, discount code, or other promotion types.
•	Target Audience: The intended audience or customer segment for the promotion. It may include values like "Best Customers," "Loyal Customers,” “Potential Customers," or other specific segments.
•	Promotion Channel: The marketing channel or platform through which the promotion is offered.
•	Product Categories: Information about the product categories or specific products included in the promotion
 
Figure 10







##Data Definition Language (DDL) statements to create the proposed data mart 
Below are the DDL statements to create the proposed data mart and shown in Table 10.

 
Table 10
Fact Table	Dimensions
CREATE TABLE Sales (
    InvoiceNumber TEXT PRIMARY KEY,
    Description TEXT,
    Price REAL,
    Quantity INTEGER,
    StockCode TEXT,
    InvoiceDate DATE,
    CustomerID INTEGER,
    CountryID TEXT,
    PromotionID INTEGER,
    CurrencyCode INTEGER,
    Amount REAL
);
	CREATE TABLE Product (
    StockCode TEXT PRIMARY KEY,
    Description TEXT
);

	CREATE TABLE Date (
    DateKey INTEGER PRIMARY KEY,
    Date DATE,
    Day TEXT,
    Month TEXT,
    Quarter TEXT,
    Year INTEGER
);
	CREATE TABLE Customer (
    CustomerID INTEGER PRIMARY KEY,
    CustomerName TEXT,
    CustomerEmail TEXT,
    Address TEXT,
    PhoneNumber TEXT
);
	CREATE TABLE Country (
    CountryCode TEXT PRIMARY KEY,
    CountryName TEXT
);
	CREATE TABLE Promotion (
    PromotionID INTEGER PRIMARY KEY,
    PromotionName TEXT,
    StartDate DATE,
    EndDate DATE,
    DiscountPercentage REAL,
    PromotionType TEXT,
    TargetAudience TEXT,
    PromotionChannel TEXT,
    ProductCategories TEXT
);
	CREATE TABLE IF NOT EXISTS Currency (
    CurrencyID INTEGER PRIMARY KEY,
    CurrencyCode TEXT NOT NULL UNIQUE,
    CurrencyName TEXT NOT NULL,
    CurrencyExchangeRate REAL NOT NULL,
    CurrencyConversion TEXT NOT NULL
);

 
##Supported Queries of the Designed Data Mart
Below is the list of queries that can be support by the designed data mart
1. Sales Analysis:

1.1. What were the total sales for a specific product category in each time?

1.2. Which products had the highest sales revenue in the last quarter?

1.3. How do sales vary by product description and country?

1.4. What is the average order value for each country?

1.5. Can you identify trends in sales over the past year?

2. Customer Insights:

2.1. Who are the top-spending customers, and what products do they buy?

2.2. How does customer purchasing behavior differ between new and returning customers?

2.3. What is the customer retention rate for each country?

2.4. Which customers have made the most purchases in the last month?

3. Date Analysis:

3.1. What were the sales trends for a specific product over a specific number of years, for instance, the last five years?

3.2. How do sales vary by day of the week or month?

3.3. What is the seasonality of product sales?

4. Product Performance:

4.1. Which products have the highest and lowest sales quantities?

4.2. What is the inventory turnover rate for each product category?

4.3. Can you identify slow-moving or overstocked products?

5. Country-Based Insights:

5.1. What are the top-selling products in each country?

5.2. How do exchange rate fluctuations impact sales revenue in different countries?

5.3. Which countries have the highest and lowest sales growth rates?

6. Promotion Analysis:

6.1. What is the ROI for specific promotions in terms of revenue and profit?

6.2. Which promotions had the highest redemption rates?

6.3. Can you identify the most effective promotion channels for driving sales?

6.4. How does the duration of a promotion impact its effectiveness?

7. Cross-Functional Analysis:

7.1. How do promotions affect sales for different product categories?

7.2. What is the correlation between customer location and their response to promotions?

7.3. Does the time of year (season) affect customer buying behavior?

8. Financial Reporting:

8.1. What is the total revenue and profit for each year and quarter?

8.2. How do currency fluctuations impact financial results?

8.3. Can you generate a balance sheet or income statement for a specific year or quarter?

##Implementation of the data mart
Some of the queries listed above were answered using designed data mart with only five rows of randomly generated data that fit the data mart. The queries and outcomes are shown below:
•	What were the total sales for a specific product category?
Using the SQL query:
SELECT SUM(s.Price * s.Quantity) AS TotalSales
FROM Sales s
INNER JOIN Product p ON s.StockCode = p.StockCode
WHERE (p.Description = 'Hand Warmer Scotty Dog Design'
OR p.Description = 'Hand Warmer Union Jack'
OR p.Description = 'White Hanging Heart T-Light Holder'
OR p.Description = 'Hand Warmer Owl Design');

The output of the query:
 

•	Which products had the highest sales revenue in Q1 in the year 2023?

Using the SQL query:
SELECT p.Description, SUM(s.Price * s.Quantity) AS TotalRevenue
FROM Sales s
INNER JOIN Product p ON s.StockCode = p.StockCode
INNER JOIN Date d ON s.InvoiceDate = d.Date
WHERE d.Year = '2023'
AND d.Quarter = 'Q1'
GROUP BY p.Description
ORDER BY TotalRevenue DESC;









The output of the query:
Table 11
Product Description	Total Revenue
Hand Warmer Scotty Dog Design	87.92
White Hanging Heart T-Light Holder	79.96
Hand Warmer Union Jack	31.98
Hand Warmer Owl Design	24.99

The output as shown in Table 11 above revealed that Hand Warmer Scotty Dog Design contributed the most to revenue in the first quarter and Hand Warmer Owl Design was the least contributor to revenue in the first quarter. The output is visualized in Figure 13 below.
 
Figure 11



•	What are the top-selling products in each country?

Using the SQL query:
SELECT
p.Description AS ProductDescription,
s.Country,
SUM(s.Price * s.Quantity) AS TotalSales
FROM Sales s
INNER JOIN Product p ON s.StockCode = p.StockCode
GROUP BY ProductDescription, Country
ORDER BY TotalSales DESC;

The output of the query:
 
Figure 12
The output as shown in Figure 11 above indicated that Whit Hanging Heart T- Light was the most sold product in United Kingdom. Hand Warmer Union Jack was the most sold product in Canada. Hand Warmer Scotty Dog Design was the most sold product in both Denmark and United States of America and Hand Warmer Owl Design was the most sold product in France.




•	Which customers have made the most amount of purchases in January?
Using the SQL query:
SELECT
c.CustomerName,
SUM(s.Price * s.Quantity) AS TotalSalesAmount
FROM Sales s
INNER JOIN Customer c ON s.CustomerID = c.CustomerID
INNER JOIN Date d ON s.InvoiceDate = d.Date
WHERE strftime('%Y-%m', d.Date) = '2023-01'
GROUP BY c.CustomerName
ORDER BY TotalSalesAmount DESC;
The output of the query:
As shown in table 12 below, the output of the query showed that Customer C contributed the highest revenue to the business representing 79.76 of the total revenue realized in January 2023. Customer D contributed the least to the total revenue in January 2013 with 24.99. The

Table 12
Customer Name	Total Sales
Customer C	79.96
Customer E	54.95
Customer A	32.97
Customer B	31.98
Customer D	24.99

 
Figure
