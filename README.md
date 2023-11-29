# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

(1) Importing ecommerce data from files to postgreSQL tables.
(2) Cleaning and transforming data for analytics.
(3) Study patterns and find relationships across different tables
(4) Running queries to get business insights.
(5) Implementing the process of quality analysis to evince business insights. 


## Process
## Step 1:

//Syntax for creating and importing data:

//For example:
//Table for salesbysku:

```SQL
Create Table salesbysku
(
 productsku    character varying,
 totalordered  integer,
 Primary key   (productsku)
);

//Importing data using copy statement

```SQL
Copy salesbysku(productsku, totalordered)
From '/Users/yogithakandhi/Downloads/all_sessions .csv'
Delimiter ','
Csv header;

##Step 2: Data cleaning


## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)

## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)
