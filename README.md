# SAFE – Snowflake Access Flow Evaluator

> Project under development – name subject to change.

## Overview

SAFE is a Streamlit Native App designed to run directly inside Snowflake. It helps administrators evaluate whether current authentication methods comply with Snowflake’s upcoming password and MFA policy changes.

The app analyzes login behavior, authentication types, and user classifications using `ACCOUNT_USAGE` views, and presents actionable insights to improve security posture.

## Key Features

- Displays authentication methods used across all users
- Flags accounts still relying on password-only access
- Identifies missing MFA enrollment and outdated user types
- Recommends and executes actions such as unsetting passwords or changing user classifications
- Runs entirely within the Snowflake environment—no external servers required

## Use Case

This tool helps Snowflake administrators prepare for:

- Enforcement of multi-factor authentication
- Retirement of password-only login flows
- Transitioning users to supported methods like SAML, OAuth, or RSA key pairs

SAFE uses account metadata to guide remediation and reduce manual audit effort.

## Requirements

- Snowflake account that supports Streamlit Native Apps
- A role with:
  - `CREATE STREAMLIT` and `CREATE DATABASE` privileges
  - Read access to:
    - `SNOWFLAKE.ACCOUNT_USAGE.login_history`
    - `SNOWFLAKE.ACCOUNT_USAGE.users`
    - `SNOWFLAKE.ACCOUNT_USAGE.policy_references`
  - Access to a compute warehouse
- A Snowflake stage for uploading the app

## Deployment

### Step 1: Upload the App

Use SnowSQL or the UI to upload the `app.py` file into a Snowflake stage.

```sql
CREATE OR REPLACE STAGE safe_stage;
PUT file://app.py @safe_stage AUTO_COMPRESS=FALSE;
```

### Step 2: Create the Application

```sql
CREATE OR REPLACE DATABASE SAFE_APP_DB;
USE SCHEMA SAFE_APP_DB.PUBLIC;

CREATE OR REPLACE STREAMLIT SAFE_APP
FROM '@safe_stage/app.py'
MAIN_FILE = '/app.py';

GRANT USAGE ON STREAMLIT SAFE_APP TO ROLE <your_admin_role>;
```

Once created, the app can be accessed directly from the Snowflake UI under Streamlit Apps.


## How to Contribute

SAFE is actively being developed, and we welcome feedback, bug reports, and feature suggestions.

You can contribute in one of two ways:

1. **Email Feedback**
   Reach out to us directly at `safe@snowflake.com` with your questions, ideas, or bug reports.

2. **Open a GitHub Issue**
   If you're using this project via GitHub, you can open an issue to:

   * Report a bug
   * Request a new feature
   * Share improvements or use cases

Please include as much detail as possible to help us understand the context of your request. If submitting a code-related suggestion, linking to specific lines or snippets is appreciated.



## License

This project is licensed under the Apache 2.0 License.

```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0
```

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
