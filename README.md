 ELT Pipeline with dbt & Snowflake
This project is a guided code-along for building a complete ELT (Extract, Load, Transform) pipeline using dbt and Snowflake, based on the "Code Along - Build an ELT Pipeline in 1 Hour" tutorial. Airflow integration is not included in this version.

🧱 Tech Stack
dbt (data build tool) – for data transformations

Snowflake – for data warehouse and storage

SQL – for all data modeling and transformation logic

📦 Project Structure
bash
Copy
Edit
├── macros/                 # Custom reusable macros
│   └── pricing.sql         # Macro to calculate discounted amount
├── models/
│   ├── staging/            # Staging layer
│   │   ├── tpch_sources.yml
│   │   ├── stg_tpch_orders.sql
│   │   └── tpch/stg_tpch_line_items.sql
│   └── marts/              # Data marts and fact tables
│       ├── int_order_items.sql
│       ├── int_order_items_summary.sql
│       ├── fct_orders.sql
│       └── generic_tests.yml
├── tests/                  # Singular test cases
│   ├── fct_orders_discount.sql
│   └── fct_orders_date_valid.sql
└── dbt_project.yml         # dbt project configuration
🛠️ Setup Instructions
1. Snowflake Environment Setup
Use the following SQL commands in Snowflake to set up the environment:

sql
Copy
Edit
use role accountadmin;

create warehouse dbt_wh with warehouse_size='x-small';
create database if not exists dbt_db;
create role if not exists dbt_role;

grant role dbt_role to user <your_user>;
grant usage on warehouse dbt_wh to role dbt_role;
grant all on database dbt_db to role dbt_role;

use role dbt_role;
create schema if not exists dbt_db.dbt_schema;
2. Configure profiles.yml
Example profiles.yml:

yaml
Copy
Edit
snowflake_workshop:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: <account_name>
      user: <your_username>
      password: <your_password>
      role: dbt_role
      database: dbt_db
      warehouse: dbt_wh
      schema: dbt_schema
      threads: 1
3. Add Models
Sources & Staging: Define orders and lineitem from Snowflake sample data

Macros: Create a utility function to calculate discounts

Transformations: Generate intermediate and fact tables

Tests: Add generic and custom tests to validate data quality

✅ dbt Workflow
To run your transformations and tests:

bash
Copy
Edit
dbt debug               # Validate configuration
dbt deps                # Install dependencies
dbt run                 # Execute models
dbt test                # Run data quality tests
dbt docs generate       # Generate documentation
dbt docs serve          # View docs in browser
🧪 Tests Included
Generic Tests: Uniqueness, not null, relationships, accepted values

Singular Tests: Business logic validations for discounts and valid order dates
