
## CASE STUDY: Metrics and Calculations of Container Usage Data

Swarup Kumar Misra

21/06/2023

## 1. Introduction

The objective is to analyze the provided dataset and derive key metrics to gain insights into the company's performance. The dataset includes actual sales data, such as material numbers, plants, bottle prices, crate prices, bottle quantities, and crate quantities. I need to clean the dataset and create a consolidated view of the actuals data with relevant metrics. Additionally, perform variance analysis, compare actuals and targets by plant, analyze variances by plant and category, and conduct trend analysis for each category and plant. Finally, identify focus areas and planning periods to ensure a smooth supply chain.


## 2. Scenario

As a business manager responsible for supply in a company. The provided dataset contains actual and target container usage information for the past year. The objective is to answer the following questions:

- What are the key metrics to track for ensuring a smooth supply?
- How do the actual and target values for each metric compare?
- What are the trends in container usage over time?
- What are the focus areas and periods for planning a smooth supply?

#### Considerations:
- The company operates in multiple plants with different capacities and demand patterns.
- Various types of containers are used, each with different costs and availability.
- Competition exists with other beverage companies for the same containers.

## 3. Analysis Approach

The approach involved several key steps to analyze the data effectively. Initially, the pandas library was employed to clean and prepare the data by addressing missing values, inconsistencies, and duplicates, while ensuring data integrity. Subsequently, the cleaned datasets were imported into an SQLite3 database, where appropriate tables were created to organize the data systematically. Comprehensive analysis was conducted using SQL queries, which allowed for the extraction of valuable insights, identification of relationships and trends, and generation of statistical summaries. To visualize and present the analysis findings, Tableau was utilized. By connecting Tableau to the SQL database, visually compelling charts, graphs, and dashboards were designed to facilitate effective communication and decision-making. This approach encompassed the data cleaning, database management, SQL analysis, and data visualization stages, resulting in concise and impactful presentations of the analysis results.

## 4. Setting Up the Environment

To perform analysis and create graphs using Python, SQLite3, Pandas, and Tableau I have taken these steps:

I. Python Installation:
   - Install Python by downloading the latest version from the official Python website (https://www.python.org/downloads/).
   - I have ensured that Python is properly installed by running the following command in my terminal or command prompt:
     ```
     python --version
     ```
II. SQLite3 Installation:
   - SQLite3 is included with Python, so no separate installation is required.
     ```
     import sqlite3 
     ``` 

III. Pandas Installation:
   - Install Pandas library by running the following command in your terminal or command prompt:
     ```
     pip install pandas
     ```

IV. Tableau Installation:
   - Download and install Tableau Desktop from the official Tableau website (https://www.tableau.com/products/desktop).

V. Data Analysis and Graphing:
   - I have launched a Python integrated development environment (IDE) of my choice (e.g., Jupyter Notebook, PyCharm, or VS Code).
   - Imported the required libraries in my Python script or notebook:
     ```python
     import sqlite3
     import pandas as pd
     ```

VI. Connecting to SQLite Database:
   - Established a connection to my SQLite database using the following code:
     ```python
     conn = sqlite3.connect('your_database.db')
     ```

VII. Data Analysis with Pandas:
   - I used Pandas library to query and manipulate data from your SQLite database. Refer to the official Pandas documentation for detailed usage instructions (https://pandas.pydata.org/).

VIII. Exporting Data for Tableau:
   - Once I performed my analysis with Pandas, export the data to a format compatible with Tableau (e.g., CSV, Excel, or Tableau Extract).
   - Save the exported data file in a location accessible to Tableau.

IX. Creating Graphs with Tableau:
   - Launch Tableau Desktop and connect to the exported data file.
   - Used Tableau's visualization features to create interactive graphs and dashboards.



## 5. Steps Taken
- Data Cleaning and Preparation:
   - Utilized the pandas library in Python to clean the raw data.
   - Addressed missing values, inconsistencies, and duplicates.
   - Ensured data integrity and consistency.

- Database Management:
   - Imported the cleaned data into an appropriate database system, such as SQLite.
   - Created tables in the database to organize and store the data.

- SQL Analysis:
   - Formulated SQL queries to extract insights and perform analysis.
   - Conducted various analyses, such as variance analysis, target comparison, and category-based analysis.
   - Calculated key metrics, such as actuals, targets, variances, and trends, using SQL queries.

- Data Visualization:
   - Utilized data visualization tools, such as Tableau, to create visual representations.
   - Connected the visualization tool to the database to retrieve the analyzed data.
   - Designed charts, graphs, and dashboards to present the analysis findings visually.

#### Cleaning & Setup
Importing The required libraries
~~~ 
import pandas as pd
~~~
Retrieving the files to use further
~~~
actual = pd.read_csv("asset/actuals.csv")
targets = pd.read_csv("asset/targets.csv")
price = pd.read_csv("asset/price.csv")
bncr = pd.read_csv("asset/bncr.csv")
~~~
Cleaning the 'Target Value in LC' column of the targets DataFrame, including replacing hyphens with '0', removing commas, converting to float, and saving to a CSV file:
~~~
targets['Target Value in LC'] = targets['Target Value in LC'].str.replace('-', '0')
targets['Target Value in LC'] = targets['Target Value in LC'].str.replace(',', '')
targets['Target Value in LC'] = targets['Target Value in LC'].astype(float)
targets.to_csv("asset/targets.csv", index=False)
~~~
Filtering The Data in `actual` dropping duplicates, dropping rows with missing values, and filtering specific columns by replacing negative values with 0 using the clip method. The clip method ensures that values in the specified columns ('Amount in LC' and 'Quantity') are not less than 0.
~~~
actual = actual.drop_duplicates()
actual = actual.dropna()
col_to_check = ['Amount in LC', 'Quantity']
for column in col_to_check:
    actual[column] = actual[column].apply(lambda x: max(0, x))
actual.to_csv('actual.csv', index=False) 
~~~

Filtering The Data in `targets` dropping duplicates, dropping rows with missing values.
~~~
targets = targets.drop_duplicates()
targets = targets.dropna()
targets.to_csv('targets.csv', index=False) 
~~~

Filtering The Data in `price` dropping duplicates, dropping rows with missing values.
~~~
price = price.drop_duplicates()
price = price.dropna()
price.to_csv('prices.csv', index=False) 
~~~

Filtering The Data in `bncr` dropping duplicates, dropping rows with missing values.
~~~
bncr = bncr.drop_duplicates()
bncr = bncr.dropna()
bncr.to_csv('bncr.csv', index=False) 
~~~

#### I. Outcomes

- Insights on key metrics for smooth supply.
- Identification of areas where actual and target values differ.
- Understanding of container usage trends over time.
- Focus areas and planning periods for addressing supply challenges.

#### II. Benefits

- Improved supply management and reduced disruptions in production.
- Better alignment of container usage with customer demand.
- Optimization of container costs and availability.
- Enhanced competitiveness in the beverage market.

## 6. Setup For SQLite3 and Analysis
Importing required libraries:
~~~
import csv
import sqlite3
~~~
Establishing a connection to a SQLite database using the sqlite3 module:
~~~
conn = sqlite3.connect('casestudy.db')
~~~
Creating a cursor object:
~~~
cursor = conn.cursor()
~~~
Executing an SQL statement using the cursor object to create a table named `actual`, `target`, `price`, `bncr` in the SQLite database:

For `actual`
~~~
cursor.execute('''CREATE TABLE IF NOT EXISTS actual (
                    Material_Description VARCHAR(255),
                    Plant VARCHAR(255),
                    Period INTEGER,
                    Year INTEGER,
                    Amount_in_LC REAL,
                    Quantity INTEGER
                )''')
~~~

For `target`
~~~
cursor.execute('''CREATE TABLE IF NOT EXISTS target (
                    Year INTEGER,
                    Period INTEGER,
                    Plant VARCHAR(255),
                    Material_Number VARCHAR(255),
                    Target_Value_in_LC REAL,
                    Target_Quantity INTEGER
                )''')
~~~

For `price`
~~~
cursor.execute('''CREATE TABLE IF NOT EXISTS price (
                    Plant VARCHAR(255),
                    Material_Description VARCHAR(255),
                    Price_per_case REAL
                )''')
~~~

For `bncr`
~~~
cursor.execute('''CREATE TABLE IF NOT EXISTS bncr (
                    Material_Number VARCHAR(255),
                    Bottle VARCHAR(255),
                    Crate VARCHAR(255)
                )''')
~~~

##### Inserting Data Into The Tables

For `actual`, `targets`, `price` & `bncr`

~~~
with open('asset/actuals.csv', 'r') as file:
    csv_data = csv.reader(file)
    next(csv_data)  # Skip the header row if it exists
    cursor.executemany('''INSERT INTO actual VALUES (?, ?, ?, ?, ?, ?)''', csv_data)
~~~
~~~
with open('asset/targets.csv', 'r') as file:
    csv_data = csv.reader(file)
    next(csv_data)  # Skip the header row if it exists
    cursor.executemany('''INSERT INTO target VALUES (?, ?, ?, ?, ?, ?)''', csv_data)
~~~
~~~
with open('asset/price.csv', 'r') as file:
    csv_data = csv.reader(file)
    next(csv_data)  # Skip the header row if it exists
    cursor.executemany('''INSERT INTO price VALUES (?, ?, ?)''', csv_data)
~~~
~~~
with open('asset/bncr.csv', 'r') as file:
    csv_data = csv.reader(file)
    next(csv_data)  # Skip the header row if it exists
    cursor.executemany('''INSERT INTO bncr VALUES (?, ?, ?)''', csv_data)
~~~


## 7. Ask 1:
The task involves cleaning the dataset and creating a consolidated view of Actuals data using information from the Price and B&CR tables. The consolidated view should include the following columns: Material number, Plant, Bottle Price, Crate Price, Bottle Rands, and Crate Rands. 

The calculations for Bottle Rands and Crate Rands are as follows:
- Bottle Rands: Bottle Price * Bottle Quantity
- Crate Rands: Crate Price * Crate Quantity

This analysis aims to consolidate actuals data by creating a view called 'consolidated_actuals10' and retrieving filtered results from it. The analysis involves joining multiple tables and calculating various derived columns.

### Ovservation
This case study involves the consolidation of actuals data related to bottle and crate prices and costs. The dataset includes information about the material description, plant, bottle price, crate price, bottle costs in rands, and crate costs in rands. The data is retrieved from multiple tables, namely 'actual', 'price', and 'bncr'. The consolidated view is created as 'consolidated_actuals'. The view filters the data based on the material description, selecting rows containing 'bottle' in the material description for the first query and rows containing 'crate' for the second query.

#### Bottle Analysis:
   - The dataset includes various types of bottles, such as 750RB Amber Calabash and 660RB Green Lite, among others.
   - The 'Bottle Price' column represents the price per case for bottle items, while the 'Bottle Rands' column represents the total costs in rands for bottle items.
   - The 'Bottle Rands' column is calculated by multiplying the 'Amount_in_LC' (local currency) with the respective bottle price per case.
   - The dataset shows the bottle costs in rands for different plants, such as AB and AG.
 - Further analysis can be performed to understand the trends, patterns, and variations in bottle prices and costs across different plants and bottle types.

#### Crate Analysis:
- The dataset includes a specific type of crate called 'B&CR: 660RB Green Lite & Red Crate'.
 - The 'Crate Price' column represents the price per case for crate items, while the 'Crate Rands' column represents the total costs in rands for crate items.
 - The 'Crate Rands' column is calculated by multiplying the 'Amount_in_LC' with the respective crate price per case.
 - The dataset shows the crate costs in rands for different plants, such as AA and AF.
 - Further analysis can be conducted to examine the variations in crate prices and costs across different plants and crate types.

Overall, this case study provides insights into the prices and costs associated with bottle and crate items, allowing for analysis and comparisons based on material description, plant, and related factors.

### Steps:

- The code begins by creating a view named 'consolidated_actuals' using an SQL query. This view combines data from the 'actual', 'price', and 'bncr' tables.

- The view includes the following columns:

   - 'Material_Description': The description of the material.
   - 'Plant': The plant information.
   - 'Bottle Price': The price per case for materials with 'Bottle' in their description; otherwise, it is set to 0.
   - 'Crate Price': The price per case for materials with 'Crate' in their description; otherwise, it is set to 0.
   - 'Bottle Rands': The total cost in local currency for materials with 'Bottle' in their description, calculated as the product of 'Amount_in_LC' and 'Price_per_case'.
   - 'Crate Rands': The total cost in local currency for materials with 'Crate' in their description, calculated as the product of 'Amount_in_LC' and 'Price_per_case'.

- After creating the view, the code executes a SQL query to retrieve the filtered results from the 'consolidated_actuals10' view. The query selects rows where the 'Material_Description' contains 'crate', 'keg', or 'bottle'.

- The retrieved rows are then printed, providing an overview of the consolidated actuals data that meets the filter conditions.

- By following these steps, the code performs the consolidation of actuals data and provides a consolidated view with relevant information for further analysis or reporting purposes.

`Code`

~~~
conn.execute('''
CREATE VIEW consolidated_actuals AS
SELECT
  actual.Material_Description,
  actual.Plant,
  CASE
    WHEN actual.Material_Description LIKE '%Bottle%' THEN price.Price_per_case
    ELSE 0
  END AS `Bottle Price`,
  CASE
    WHEN actual.Material_Description LIKE '%Crate%' THEN price.Price_per_case
    ELSE 0
  END AS `Crate Price`,
  CASE
    WHEN actual.Material_Description LIKE '%Bottle%' THEN actual.Amount_in_LC * price.Price_per_case
    WHEN actual.Material_Description LIKE '%Crate%' THEN actual.Amount_in_LC * price.Price_per_case
    ELSE 0
  END AS `Bottle Rands`,
  CASE
    WHEN actual.Material_Description LIKE '%Bottle%' THEN 0
    WHEN actual.Material_Description LIKE '%Crate%' THEN actual.Amount_in_LC * price.Price_per_case
    ELSE 0
  END AS `Crate Rands`
FROM actual
JOIN price ON actual.Material_Description = price.Material_Description AND actual.Plant = price.Plant
JOIN bncr ON actual.Material_Description = bncr.Material_Number
''')


for row in conn.execute("SELECT * FROM consolidated_actuals WHERE Material_Description LIKE '%crate%' OR Material_Description LIKE '%bottle%'"):
    print(row)
~~~

### Conclusions:
This data analysis case study focused on consolidating actuals data related to bottle and crate prices and costs. By creating a consolidated view and deriving key columns, we gained insights into the prices and costs associated with different bottle and crate items. The analysis highlighted variations in prices and costs across different material descriptions, plants, and item types. These findings can be used for decision-making, cost analysis, and identifying opportunities for optimization. Overall, this analysis provides a comprehensive understanding of bottle and crate prices and costs, enabling informed decision-making based on the consolidated actuals data.


## 8. Ask 2:
- Variance analyses
    - Actuals & Target analysis by Plant
    - Actuals, Target & their variance analysis by plant & Category (Bottle, Keg, Crate)

The case study includes variance analyses of the actuals and targets data based on different aspects, providing valuable insights into the performance and comparisons between the two.

### Actuals & Target Analysis by Plant

One of the key analyses conducted is the comparison between actuals and targets data based on different plants. This analysis allows for understanding the performance of each plant in terms of achieving the set targets. By comparing the actual values with the corresponding targets, it becomes possible to identify variations, deviations, and potential areas for improvement across different plants.

### Actuals, Target & Variance Analysis by Plant & Category (Bottle, Keg, Crate)

Another important aspect explored in this case study is the analysis of actuals, targets, and their variances based on plant and category. The dataset is categorized into different categories, namely Bottle, Keg, and Crate. By examining the actual values, targets, and variances within each category and across different plants, it becomes possible to gain insights into the performance of specific categories in different plant locations. This analysis helps identify areas of success, areas falling short of targets, and potential opportunities for growth or optimization within each category and plant.

These variance analyses provide a comprehensive view of the performance and comparisons between actuals and targets data, allowing for effective decision-making, performance evaluation, and identification of areas for improvement across different plants and categories.

### Observation

![Image Description](https://drive.google.com/uc?export=view&id=1OufS4Z9BvJlLA4RrFg4nYcZ-CVpfrHSU)

Based on the graph, it can be observed that all the plants have successfully achieved their targets. Notably, the AA plant has generated the highest amount compared to the other plants. This suggests that the AA plant is performing exceptionally well, potentially due to effective strategies, product popularity, customer satisfaction, or efficient production processes. The AA plant's success can serve as a benchmark for other plants to learn from and improve their performance. Further analysis can identify factors contributing to the AA plant's success and help optimize operations and marketing strategies for all plants to meet or exceed their targets consistently.

- Key Takeaways:
   - Plant AA has exceeded its target value by an impressive 385.96%, showcasing its exceptional performance and ability to generate substantial revenue.

![Image Description](https://drive.google.com/uc?export=view&id=1f2sSGNARmra5rRryTVqI9j9jmYJXzn42)

Based on the analysis of the graphs, it is evident that all the plants have been successful in achieving their targets. However, the AB plant stands out as it consistently generates the highest quantity compared to the other plants. This indicates that the AB plant is performing exceptionally well in terms of production and meeting the set targets. The graph provides valuable insights into the performance of different plants, highlighting the AB plant's superiority in terms of quantity generation. This finding suggests that further investigation into the factors contributing to the AB plant's success could potentially uncover valuable strategies that can be implemented across other plants to improve their performance and meet their targets more efficiently.

- Key Takeaways:
   - Plant AB has surpassed its target value by an extraordinary 720.67%, demonstrating exceptional performance and an ability to achieve remarkable revenue generation.

#### Plant Performance Analysis

The graph depicts the performance of different plants in achieving their targets. A detailed analysis reveals noteworthy insights regarding the success and efficiency of each plant.

The graph provides valuable insights into the performance of different plants, with Plant AB leading in quantity generation. This suggests that Plant AB may have implemented effective strategies, optimized production processes, or enjoyed higher customer demand, contributing to its outstanding performance.

The key takeaway from this analysis is the remarkable achievement of Plant AB, surpassing its target value by an impressive 720.67%. This highlights the plant's exceptional performance and revenue generation capabilities. Understanding the factors contributing to the success of Plant AB can provide valuable insights that can be implemented across other plants to improve their performance and increase the likelihood of meeting or exceeding their targets.

By leveraging the knowledge gained from the analysis of actuals and targets, businesses can identify best practices and strategies employed by the high-performing plant to optimize operations, enhance productivity, and align other plants towards achieving their respective targets.

#### Plant AA: Exceeding Targets with Exceptional Performance

Plant AA has outperformed other plants, surpassing its target value by an impressive 385.96%. This outstanding achievement highlights the plant's exceptional performance in generating substantial revenue. Factors contributing to Plant AA's success may include effective strategies, high product demand, superior customer satisfaction, or streamlined production processes. Other plants can draw inspiration from Plant AA's accomplishments and strive to emulate its success.

#### Overall Target Achievement

The analysis reveals that all plants have successfully achieved their respective targets. This indicates a positive trend and demonstrates the organization's overall ability to meet or exceed set objectives. The collective achievement showcases the effectiveness of strategies, operational efficiency, and successful coordination among plants.

#### Opportunities for Improvement

While all plants have achieved their targets, further analysis can uncover opportunities for improvement across the organization. Identifying strategies and practices that contribute to Plant AA's exceptional performance can be crucial in optimizing operations and marketing efforts for all plants. Implementing best practices and lessons learned from Plant AA can help ensure consistent target attainment and enhance overall performance.

This plant performance analysis provides valuable insights into the success of each plant in meeting their targets. By leveraging the achievements of Plant AA and continuously evaluating performance, the organization can foster growth, drive efficiency, and further enhance its market position.

### Steps

#### Actuals & Target Analysis by Plant

To perform a variance Actuals & Target Analysis by Plant, follow these steps:

1. Execute the SQL query to retrieve the required data from the 'actual' and 'target' tables. The query selects columns such as plant, material description, amount in LC, target value in LC, quantity, target quantity, and calculates the variance in amount and quantity.

   ```python
   cursor.execute('''
       SELECT actual.Plant, actual.Material_Description, actual.Amount_in_LC, target.Target_Value_in_LC,
           actual.Quantity, target.Target_Quantity,
           (actual.Amount_in_LC - target.Target_Value_in_LC) AS Amount_Variance,
           (actual.Quantity - target.Target_Quantity) AS Quantity_Variance
       FROM actual
       LEFT JOIN target ON actual.Plant = target.Plant
   ''')
   ```

2. Fetch all the rows from the result set using the `fetchall()` method.

   ```python
   variance_data = cursor.fetchall()
   ```

3. Create a new table, 'variance_table', to store the variance analysis results. Ensure that the table is created if it does not already exist.

   ```python
   cursor.execute('''
       CREATE TABLE IF NOT EXISTS variance_table (
           Plant VARCHAR(255),
           Material_Description VARCHAR(255),
           Amount_in_LC REAL,
           Target_Value_in_LC REAL,
           Quantity INTEGER,
           Target_Quantity INTEGER,
           Amount_Variance REAL,
           Quantity_Variance INTEGER
       )
   ''')
   ```

4. Insert the variance analysis data into the 'variance_table' using the `executemany()` method.

   ```python
   cursor.executemany('''
       INSERT INTO variance_table (Plant, Material_Description, Amount_in_LC, Target_Value_in_LC,
           Quantity, Target_Quantity, Amount_Variance, Quantity_Variance)
       VALUES (?, ?, ?, ?, ?, ?, ?, ?)
   ''', variance_data)
   ```

5. Delete any duplicate rows from the 'variance_table' to ensure data integrity.

   ```python
   cursor.execute('''
       DELETE FROM variance_table
       WHERE rowid NOT IN (
           SELECT MIN(rowid)
           FROM variance_table
           GROUP BY Plant, Material_Description, Amount_in_LC, Target_Value_in_LC, Quantity, Target_Quantity, Amount_Variance, Quantity_Variance
       )
   ''')
   ```

6. Finally, execute a SELECT query to retrieve the variance analysis data from the 'variance_table'.

   ```python
   cursor.execute('SELECT * FROM variance_table')
   ```

By following these steps, you can perform a variance analysis between the actuals and targets data, store the results in a separate table, and retrieve the analysis for further examination or reporting.

#### Conclusion

In conclusion, the analysis of the graphs reveals that all plants have successfully achieved their targets. However, the AB plant stands out for consistently generating the highest quantity, indicating exceptional performance in production. On the other hand, the AA plant has surpassed its target value, showcasing remarkable revenue generation capabilities. These findings highlight the success of both plants and provide valuable insights for improving performance across all plants. By studying the strategies and factors contributing to their success, businesses can optimize operations and enhance overall performance to consistently meet and exceed targets.

![Image Description](https://drive.google.com/uc?export=view&id=1ab4PU__oXVB1qCQGfoCcWExbwUL-MmSt)

This case study focuses on the analysis of production targets and achievements for bottles and crates across multiple plants. The analysis is based on a graph that visualizes the target quantities and actual production quantities for specific bottle and crate types.

##### Key Observations:

-  Bottle: The graph highlights the successful fulfillment of the target quantity for the 750RB Amber Calabash bottle produced by the AB plant. This indicates the AB plant's efficiency in meeting production goals for this specific bottle type.

-  Crate: The graph shows that all plants have the same target quantity for the Brown Quart crate. This suggests a standardized production target across all plants for this crate type, enabling streamlined planning and resource allocation.

##### Implications:

The AB plant's success in meeting the target quantity for the 750RB Amber Calabash bottle can serve as a valuable reference for other plants. Insights from the AB plant's strategies and practices can be shared and implemented to enhance production efficiency across the organization.

The uniform target quantity for the Brown Quart crate allows for effective planning and coordination among all plants. This consistency in production targets enables efficient resource utilization and synchronization of production activities.

![Image Description](https://drive.google.com/uc?export=view&id=1lDTTIhzLNGDo6DEyt8LwL6wF8vP_HZVH)

This case study analyzes the production targets and quantities for crate types across multiple plants. The analysis is based on a graph that depicts the target quantities and actual production quantities for different crate types.

Key Observations:

-  Standardized Crate Targets: The graph reveals that the target quantity for crate types is consistent across all plants. Regardless of the specific crate type, the organization maintains a standardized production target. This suggests a uniform demand or requirement for crates, indicating a streamlined approach to production planning and resource allocation.

- Kegs: The graph demonstrates that kegs have a target quantity of 0 for all plants. This indicates that the organization does not currently have a production requirement or demand for kegs during the analyzed period. As a result, no production of kegs is expected or targeted, leading to zero actual production quantities for all plants.

Implications:

The presence of standardized crate targets allows for efficient production planning and coordination across all plants. It facilitates streamlined operations and resource allocation, ensuring a consistent output of crates to meet the organization's demand.

The absence of a target quantity for kegs suggests that the organization does not currently prioritize or require keg production. This insight enables management to allocate resources and efforts to other areas of production that align with the organization's specific demands and goals.


### Steps
This section focuses on the analysis of variance data using the `variance_data_analysis` table. The following steps outline the process:

1. **Table Creation**: Firstly, a table named `variance_data_analysis` is created to store the variance analysis results. The table consists of columns such as `Material_Number`, `Plant`, `Category`, `Period`, `Year`, `Actual_Amount_in_LC`, `Actual_Quantity`, `Target_Value_in_LC`, `Target_Quantity`, `Variance_Amount`, and `Variance_Quantity`. This table will hold the data for further analysis.

2. **Data Insertion**: The next step involves inserting data into the `variance_data_analysis` table. This is achieved by executing an SQL query that retrieves data from the `actual` and `target` tables and performs necessary calculations to derive the variance amount and variance quantity. The query uses a LEFT JOIN on the `Plant` column to combine matching rows from both tables. The `Category` column is determined based on the `Material_Description` column in the `actual` table.

3. **Data Validation**: To ensure data accuracy, any unnecessary or duplicate rows are removed from the `variance_data_analysis` table. This is accomplished by deleting the first row using the `rowid` column as a reference.

4. **Result Retrieval**: The number of inserted rows is printed to provide an overview of the analysis. Then, all the inserted rows are fetched from the `variance_data_analysis` table, and the result is printed to display the variance analysis data.

By following these steps, the variance data analysis is performed and stored in the `variance_data_analysis` table. This data can be further analyzed and used for decision-making processes and identifying areas of improvement.

`Code`

~~~
cursor.execute('''
    CREATE TABLE IF NOT EXISTS variance_data_analysis (
        Material_Number TEXT,
        Plant TEXT,
        Category TEXT,
        Period TEXT,
        Year INTEGER,
        Actual_Amount_in_LC REAL,
        Actual_Quantity INTEGER,
        Target_Value_in_LC REAL,
        Target_Quantity INTEGER,
        Variance_Amount REAL,
        Variance_Quantity INTEGER
    )
''')

cursor.execute('''
    INSERT INTO variance_data_analysis (
        Material_Number,
        Plant,
        Category,
        Period,
        Year,
        Actual_Amount_in_LC,
        Actual_Quantity,
        Target_Value_in_LC,
        Target_Quantity,
        Variance_Amount,
        Variance_Quantity
    )
    SELECT
        target.Material_Number,
        actual.Plant,
        CASE
            WHEN actual.Material_Description = 'Bottle' THEN 'Bottle'
            WHEN actual.Material_Description = 'Keg' THEN 'Keg'
            WHEN actual.Material_Description = 'Crate' THEN 'Crate'
        END AS Category,
        actual.Period,
        actual.Year,
        actual.Amount_in_LC,
        actual.Quantity,
        target.Target_Value_in_LC,
        target.Target_Quantity,
        actual.Amount_in_LC - target.Target_Value_in_LC AS Variance_Amount,
        actual.Quantity - target.Target_Quantity AS Variance_Quantity
    FROM
        actual
    LEFT JOIN
        target ON actual.Plant = target.Plant
''')

conn.commit()

# Print the number of inserted rows
cursor.execute('''
    DELETE FROM variance_data_analysis
    WHERE rowid = (
        SELECT rowid
        FROM variance_data_analysis
        LIMIT 1
    )
''')
# conn.commit()
print(f"Number of inserted rows: {cursor.rowcount}")
# Fetch all the inserted rows
cursor.execute('SELECT * FROM variance_data_analysis')
result = cursor.fetchall()

# Print the inserted rows
for row in result:
    print(row)
~~~

#### Conclusion
- The analysis of the production targets and achievements provides valuable insights for optimizing production processes and enhancing performance across multiple plants. By leveraging successful practices and maintaining standardized targets, the organization can improve production efficiency, meet customer demands, and achieve consistent results across all plants.
- The analysis of production targets and quantities provides valuable insights for optimizing crate production and resource allocation across multiple plants. By maintaining standardized targets for crates, the organization can achieve consistency and efficiency in meeting demand. Additionally, recognizing the absence of keg production allows for strategic resource allocation and focus on areas that align with the organization's priorities. These insights inform decision-making and enable effective planning to optimize production capabilities based on specific demands and goals.

## 9. Ask 3:
![Image Description](https://drive.google.com/uc?export=view&id=1ZkIM2HdjLW06iI__NuU-b4zYnZ1sHoSq)

#### Dataset Description
The dataset used in this analysis contains production quantities for three categories (Bottle, Keg, and Crate) across multiple plants during different periods in the year 2022. The dataset provides insights into the production patterns and allows for trend analysis to understand the variations and trends in production quantities.

#### Analysis Overview
The goal of this analysis is to identify production trends, compare quantities between categories and plants, and highlight notable findings. The following conclusions were drawn from the trend analysis:

#### 1. Production Quantities by Category and Plant
- *Bottle Category*: The highest production quantities were consistently observed for Bottles, with Plant AB recording the highest figures. Plant AG also had significant production levels for Bottles.
- *Keg Category*: Keg production quantities were relatively lower compared to Bottles and spread across multiple plants, including AC, AD, AE, and AG. Plant AC had the highest production quantities for Kegs.
- *Crate Category*: Crates had a significant presence in multiple plants, with Plant AB leading in terms of overall production quantities. Plants AF and AG were also notable producers of Crates.

#### 2. Production Trends and Fluctuations
- *Bottle Category*: Bottles showed a fluctuating pattern, reaching a peak in the 8th period (August) followed by a slight decline in the 9th period (September). This trend was consistent across Plants AB and AG.
- *Keg Category*: Keg production exhibited a similar fluctuating pattern, with a peak in the 9th period for Plants AE and AG. However, Plants AC and AD experienced a peak in the 8th period followed by a slight decrease in the 9th period.
- *Crate Category*: Crate production displayed varied patterns across plants, with fluctuations observed in different periods depending on the plant. Overall, production quantities remained relatively stable with only slight variations.

#### Further Analysis Suggestions
To gain deeper insights and make informed decisions, the following additional analyses are recommended:

- *Comparative Analysis*: Perform a comparative analysis of production quantities across different years to identify long-term trends and seasonality.
- *Efficiency Evaluation*: Analyze production rates per hour or per workforce to evaluate plant performance and identify areas for improvement.
- *Correlation Analysis*: Explore the correlation between production quantities and external factors (e.g., market demand, raw material availability) to understand influential factors.
- *Forecasting*: Develop predictive models to forecast future production quantities based on historical trends and external factors.

By conducting these additional analyses, stakeholders can gain a comprehensive understanding of production trends, optimize decision-making, and identify opportunities for process improvement and growth.

### Steps

The code performs trend analysis based on the data in the `actual` table. Here are the steps involved in the analysis:

1. **CSV File Initialization**: A CSV file named `trend_analysis.csv` is created in the 'results' directory to store the trend analysis results. The file is opened in write mode, and a `csv.writer` object is created with a comma as the delimiter. The header row is written to the CSV file, specifying the column names as ['Category', 'Plant', 'Year', 'Period', 'Total Quantity'].

2. **Data Retrieval**: For each combination of category and plant, a SQL query is constructed to retrieve the `Year`, `Period`, and the sum of `Quantity` from the `actual` table. The query filters the data based on the specified category and plant using the `LIKE` operator and the `Material_Description` and `Plant` columns. The results are grouped by `Year` and `Period`. The query is executed using the cursor, and the results are fetched.

3. **Data Writing**: For each result obtained, the `Year`, `Period`, and `Total Quantity` values are extracted. These values represent the original data without any modifications. Each set of values is written as a row in the CSV file using the writer object.

By following these steps, the trend analysis is performed, and the original quantities for each category, plant, year, and period are recorded in the `trend_analysis.csv` file. This data can be further analyzed to identify trends, patterns, or anomalies in the quantities over time.

`Code`

~~~
categories = ['Bottle', 'Keg', 'Crate']
plants = ['AA', 'AB', 'AC','AD', 'AE','AF', 'AG']

with open('results/trend_analysis.csv', 'w', newline='') as csvfile:
    writer = csv.writer(csvfile, delimiter=',')
    writer.writerow(['Category', 'Plant', 'Year', 'Period', 'Total Quantity'])

    for category in categories:
        for plant in plants:
            query = f"SELECT Year, Period, SUM(Quantity) FROM actual WHERE Material_Description LIKE '{category}%' AND Plant = '{plant}' GROUP BY Year, Period"
            cursor.execute(query)
            results = cursor.fetchall()

            for result in results:
                year, period, total_quantity = result 
                writer.writerow([category, plant, year, period, total_quantity])
~~~

#### Conclusion

These observations provide insights into the trends and changes in quantity for each category and plant during the specified period. Further analysis and interpretation can be performed based on this trend data to identify patterns, make informed decisions, and optimize production strategies.

The results of the trend analysis can be found in the trend_analysis.csv file located in the 'results' directory. The file contains information about the category, plant, year, period, and total quantity for each data point analyzed in the trend analysis.

## 10. Ask 4
What should be your focus areas & periods to plan for smooth supply

#### Production and Supply Planning Readme

It focuses on key areas of improvement and periods to plan for smooth supply.

### Focus Areas

1. Plant AB: This plant consistently demonstrates the highest production quantities across multiple categories, indicating its efficiency and capacity for meeting production demands. Further examination is recommended to identify the factors contributing to its success, with the aim of implementing best practices in other plants to optimize production.

2. Bottle: 750RB Amber Calabash: This specific bottle type consistently exceeds target quantities, indicating high demand. Management should prioritize ensuring a sufficient supply of this bottle type to meet market demand.

3. Crate: Brown Quart: The target quantity for this crate type is consistent across all plants, suggesting a uniform demand throughout the organization. Management should focus on maintaining a consistent supply of this crate type to meet demand effectively.

### Periods for Smooth Supply Planning

1. August: This period witnessed a peak in production quantities for Bottles and Kegs, indicating high demand. Management should plan accordingly to ensure a sufficient supply of these products during this time.

2. September: Although there was a slight decline in production quantities for Bottles, Keg production remained high. Management should closely monitor production levels during this period to ensure an adequate supply of both products to meet demand.

#### Additional Considerations for Smooth Supply Planning

1. Seasonal Demand: The company should consider seasonal fluctuations in demand when planning for a smooth supply. Identifying peak demand periods and adjusting production and resources accordingly can help meet customer needs effectively.

2. Inventory Levels: Monitoring inventory levels is crucial to ensure an adequate stock of products. Low inventory levels may require increasing production or allowing sufficient time for replenishment before anticipated demand increases.

3. Transportation Capacity: Consideration of transportation capacity is vital in supply planning. If there is insufficient transportation capacity to meet demand, alternative transportation methods or increased production may be necessary.

By considering these focus areas and factors, the company can optimize production efficiency, ensure a smooth supply chain, and effectively meet customer demand.

## 11. Conclusion

The analysis of the  data has revealed several key insights that can be used to improve the company's supply chain. These insights include:

* The key metrics for ensuring a smooth supply are the amount of containers used, the cost of containers, and the availability of containers.
* There are significant differences between the actual and target values for these metrics.
* The trends in container usage over time are not consistent, and there are some periods where the usage is significantly higher or lower than others.
* The focus areas for planning a smooth supply are the plants where the usage is highest and the periods where the usage is highest.

These insights can be used to improve the company's supply chain by:

* Identifying areas where the actual and target values differ and taking steps to close the gaps.
* Monitoring the trends in container usage over time and taking steps to address any changes.
* Focusing on the plants and periods where the usage is highest to ensure that there are enough containers available to meet demand.

By implementing these changes, the company can improve its supply chain and reduce the risk of disruptions.

In addition to these key insights, the analysis has also revealed some other interesting findings. For example, the analysis found that the cost of containers varies depending on the plant and the type of container. This information can be used to negotiate better prices with suppliers.

The analysis also found that the availability of containers is not always consistent. This can be due to factors such as changes in demand or disruptions in the supply chain. The company can mitigate these risks by maintaining a buffer stock of containers and by working with suppliers to ensure that there is a reliable supply of containers.

Overall, the analysis of the container usage data has provided the company with valuable insights that can be used to improve its supply chain. By implementing the changes recommended by the analysis, the company can reduce the risk of disruptions and improve its ability to meet customer demand.
