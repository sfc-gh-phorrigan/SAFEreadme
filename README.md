# SAFEreadme# SAFE – Snowflake Access Flow Evaluator

> Project under development – name subject to change.

## Overview

SAFE is a Streamlit-based application designed to help Snowflake account administrators and users evaluate whether their current authentication methods are compliant with upcoming changes to Snowflake's password and MFA (Multi-Factor Authentication) policies.

This tool queries Snowflake account metadata and login history to flag users and service accounts that may need to update their authentication methods—such as switching from basic auth to supported MFA mechanisms.
![image](https://github.com/user-attachments/assets/3b7dc904-327b-4cd3-9607-1c7dc67c4b89)


## Key Features

- Identifies users who are still using password-based login without MFA
- 🔍Highlights service accounts that may break due to upcoming enforcement
- Provides a clear dashboard view of authentication factors across your Snowflake environment
-  Lightweight and quick to deploy using Streamlit

## Use Case

Snowflake will soon enforce stricter requirements on user authentication. SAFE helps you get ahead of the curve by:

- Running checks against `login_history` and `users` tables
- Flagging non-compliant authentication flows
- Helping you plan user remediation or automation updates

## Requirements

- Python 3.8+
- Snowflake account with `ACCOUNTADMIN` or sufficient privileges to query `login_history` and `users`
- [Snowflake Python Connector](https://docs.snowflake.com/en/developer-guide/python-connector)
- [Streamlit](https://streamlit.io/)
- `.env` file or secrets manager for credentials

## Installation

```bash
## Installation (Snowflake Native App Deployment)

This project is designed to run as a **Streamlit Native App** inside your Snowflake account.

### Step 1: Prepare Your Snowflake Environment

Ensure you have the following:

- A role with `CREATE APPLICATION` and `CREATE DATABASE` privileges
- Access to a working Snowflake warehouse
- ACCOUNTADMIN or a role with privileges to read from `SNOWFLAKE.ACCOUNT_USAGE` views (e.g., `login_history`, `users`)

### Step 2: Clone the Repo

```bash
git clone https://github.com/your-org/safe.git
cd safe


-- 1. Create a database for your Streamlit app (optional)
CREATE DATABASE SAFE_APP_DB;

-- 2. Create or use a schema
USE SCHEMA SAFE_APP_DB.PUBLIC;

-- 3. Create the Streamlit application
CREATE OR REPLACE STREAMLIT SAFE_APP
FROM '@your_stage/safe'
MAIN_FILE = '/app.py';


GRANT USAGE ON STREAMLIT SAFE_APP TO ROLE YOUR_ROLE;

