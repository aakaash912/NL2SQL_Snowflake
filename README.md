# NL2SQL_Snowflake
Natural Language to SQL Query Induced Chatbot For Personal Snowflake Databases using Llama 3.1 with Ollama Framework for LLMs and Langchain.

# Overview
The NL2SQL Model Project is designed to convert natural language queries into SQL commands, enabling users to interact with databases using everyday language. This project leverages advanced AI models to understand and translate user inputs into precise SQL queries, specifically optimized for Snowflake databases.

# Features
1. Natural Language Processing: Converts user queries in natural language to SQL statements.
2. Database Compatibility: Optimized for Snowflake, ensuring seamless database interaction.
3. Scalability: Capable of handling complex queries involving multiple tables and conditions.
4. User-Friendly: Designed to make database querying accessible to non-technical users.

# Installation
Prerequisites
1. Python 3.7+: Ensure you have Python installed on your system.
2. Jupyter Notebook: For running and testing the notebook.
   
# Data Base Used here:  
The BikeStores Database from sqlservertutorial.com has been used here. But Instead of 2 schemas, a single schema has been utilized here as ths version of the Chatbot has not been included with Cross Schema Support for Snowflake with SQLAlchemy. Other Database platforms might provide Cross Schema Support when used with SQLAlchemy.

Tables
1. Brands
Purpose: Stores information about bike brands.
Columns:
brand_id: Unique identifier for each brand.
brand_name: Name of the brand.
2. Categories
Purpose: Lists categories of bikes.
Columns:
category_id: Unique identifier for each category.
category_name: Name of the category.
3. Customers
Purpose: Stores customer information.
Columns:
customer_id: Unique identifier, auto-generated.
first_name, last_name, phone, email: Contact details.
4. Order Items
Purpose: Details items in an order.
Columns:
order_id: References parent order.
product_id: References product ordered.
quantity: Number of units ordered.
list_price, discount: Pricing details.
5. Orders
Purpose: Captures customer orders.
Columns:
order_id: Unique identifier, auto-generated.
customer_id: References customer.
order_date, required_date, shipped_date: Order dates.
store_id, staff_id: References store and staff.
6. Products
Purpose: Stores bike product details.
Columns:
product_id: Unique identifier for each product.
product_name: Name of the product.
brand_id, category_id: References brand and category.
model_year, list_price: Model and price information.
7. Staffs
Purpose: Information about store employees.
Columns:
staff_id: Unique identifier, auto-generated.
first_name, last_name, email, phone: Contact details.
active: Employment status.
store_id, manager_id: References store and manager.
8. Stocks
Purpose: Current stock levels at each store.
Columns:
store_id: References store.
product_id: References product.
quantity: Quantity in stock.
9. Stores
Purpose: Details of physical store locations.
Columns:
store_id: Unique identifier, auto-generated.
store_name, phone, email, street, city, state, zip_code: Store contact information.
Relationships
Brands to Products: One-to-many relationship.
Categories to Products: One-to-many relationship.
Customers to Orders: One-to-many relationship.
Orders to Order Items: One-to-many relationship.
Products to Stocks: Many-to-many relationship (through stocks).
Stores to Stocks: One-to-many relationship.
Staffs: Self-referencing relationship for managers.

# Entity-Relationship Diagram (ERD)
<img src="https://github.com/aakaash912/NL2SQL_Snowflake/blob/23f762c6270998c6b83852ce43dc34299a8a4c0d/Bikestores%20Database%20ER%20Diagram.png">

# Setup:

1. Clone the Repository
2. Install Required Packages by running the First Cell.
3. Follow the Next Section to Setup the Runtime for Google Colab. (Ignore for Windows)
   For Windows:
   	1. Download the Ollama Framework Windows Application from this: https://ollama.com/
   	2. Run the Ollama application to start up the Server.
   	3. Run cmd and run 'ollama pull llama3.1' (Model name Subject to Updates - Verify with Ollama on their web to get the suitable model)
   	4. The above would fetch the model and store it in your Windows PC.
   	5. Making use of Ollama due to Data Safety and easy integration with Python and Langchain.
4. Setup Environment Variables:

      LANGCHAIN_ENDPOINT="https://api.smith.langchain.com"

      LANGCHAIN_API_KEY="your_api_key_here"

      LANGCHAIN_TRACING_V2="true"

      LANGCHAIN_PROJECT="your_project_name_here"

In Jupyter Notebook:

5. Setup Snowflake Credentials for Connection:

      username = "your_username_here"

      password = "your_password_here"

      account = "your_account_identifier_here typically in the format account_name.region.cloud"
   
      database = "your_database_name_here"

      warehouse = "your_warehouse_name_here"

      role = "your_role_name_here"

      schema = "your_schema_name_here"

6. Few Shot Examples: The cell with the list named 'examples' has few shot examples for the 'Bikestores' Database. Hard coding is required for giving in Few shot examples that pertain to our own database.

7. Execute the Cells:

Follow the instructions within the notebook, executing each cell to process natural language queries into SQL statements.

Example
Input: "Who takes the Best Employee Award of the Year"

SQL Query automatically generated by LLM:
SELECT s.first_name, s.last_name, s.email AS staff_email, COUNT(o.order_id) AS total_orders, SUM(oi.quantity * oi.list_price * (1 - oi.discount)) AS total_sales FROM staffs s JOIN orders o ON s.staff_id = o.staff_id JOIN order_items oi ON o.order_id = oi.order_id GROUP BY s.first_name, s.last_name, s.email ORDER BY total_sales DESC, total_orders DESC LIMIT 10

Final Output printed: 
Based on the provided SQL result, it appears that there are not more than 20 records as requested in the question. However, I can still provide a beautiful table of the top employee(s) based on total sales and orders.

**Best Employee(s) of the Year Award**

| **Employee Name** | **Email** | **Total Orders** | **Total Sales** |
| --- | --- | --- | --- |
| Marcelene Boyer | marcelene.boyer@bikes.shop | 1617 | $2,625,843.82 |
| Venita Daniel | venita.daniel@bikes.shop | 1577 | $2,589,299.59 |
| Genna Serrano | genna.serrano@bikes.shop | 545 | $855,807.35 |

Note: The employee(s) listed above are the top performer(s) based on total sales and orders.



# Contributing
We welcome contributions to enhance the NL2SQL model. Please fork the repository and submit a pull request with your proposed changes.

# How the Code is Organized
Model Architecture
Data Input for LLM: Feeding the LLM with the Database Structure and memory for Follow Up questions and also Few Shot Prompts for relevancy selected with Dynamic Few Shot Prompt Selection based on Semantics.
Query Generation and Execution: Based on the inputs - Database Structure and the user's question, queries are generated and executed.
Post-Processing: The outputs from Query Execution is then sent to the LLM for phrasing the final output.
All the above are processed in a flow using Langchain's Features.

# Testing
Ensure you have a test database setup with mock data for validating the model outputs.
Utilize the included test cases to evaluate model performance.
