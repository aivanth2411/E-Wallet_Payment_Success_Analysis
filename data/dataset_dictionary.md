# Dataset Dictionary

This dataset simulates transaction data, user behavior, and system errors of an **E-Wallet application** over a 2-month period  (01 Jul 2023 – 31 Aug 2023).

The main objective is to analyze the causes behind the significant decline in **bill payment success rates**, with a focus on:
- multi-level error tracking
- user journey analysis
- transaction performance drivers

## Data Sources

- `transactions.csv` – Detailed transaction history
- `sessions.csv` – User session information
- `event_logs.csv` – Step-by-step behavioral event logs
- `dim_user.csv` – User information
- `dim_event.csv` – Event reference table
- `dim_error_categories.csv` – 3-level error categorization


# Column Descriptions

### 1. transactions

- `transaction_id` – Unique transaction ID
- `user_id` – User ID associated with the transaction
- `session_id` – Session ID in which the transaction occurred
- `amount` – Transaction amount
- `category` – Main transaction category (e.g., Billing, Shopping)
- `sub_category` – Sub-category (e.g., Electricity, Water)
- `transaction_type` – Type of transaction (Payment, Transfer, Deposit, Withdrawal)
- `transaction_date` – Transaction timestamp
- `device_type` – Device used for the transaction (Android, iOS, Web)
- `payment_method` – Payment method (e.g., wallet_balance, linked_bank, credit_card)
- `status` – Transaction status (`success` / `failure`)
- `app_version` – App version at the time of transaction
- `os_version` – Device operating system version
- `platform` – Platform used (`android`, `ios`, `web`)
- `product_number` – Product / bill reference number
- `promo_code` – Applied promotion code (if any)
- `discount_amount` – Discount amount applied

### 2. dim_product

- `product_id` – Unique product ID
- `category` – Product category
- `sub_category` – Product sub-category
- `product_type` – Product type (e.g., electricity bill, water bill)
- `online_offline` – Online or offline product classification

### 3. dim_user

- `user_id` – Unique user ID
- `account_created_at` – Account creation timestamp
- `account_age_days` – Number of days since account creation
- `gender` – User gender
- `age_group` – Age group (`18-24`, `25-34`, `35-44`, `45+`)
- `device_type` – Primary device type
- `platform` – User platform (`android`, `ios`, `web`)
- `activity_level` – User activity level (`dormant`, `low`, `medium`, `high`)
- `balance` – Current wallet balance
- `loyalty_tier` – Membership tier (`bronze`, `silver`, `gold`, `platinum`)
- `os_version` – Device OS version
- `app_version` – Current app version
- `location_country` – User country
- `location_city` – User city
- `preferred_language` – Preferred app language
- `usual_connection_quality` – Typical network quality (`poor`, `fair`, `good`, `excellent`)


### 4. sessions

- `session_id` – Unique session ID
- `user_id` – User ID linked to the session
- `session_start` – Session start timestamp
- `session_end` – Session end timestamp
- `session_duration_seconds` – Session duration in seconds
- `device_type` – Device type used in session (`phone`, `tablet`)
- `operating_system` – Device operating system
- `app_version` – App version during session
- `location` – Usage location (city)
- `ip_address` – Session IP address
- `connection_quality` – Network quality during session
- `hour_of_day` – Hour when session started (`0–23`)


### 5. event_logs

- `event_log_id` – Unique event log ID
- `event_id` – Event type ID (linked to `dim_event`)
- `user_id` – User performing the event
- `session_id` – Session ID of the event
- `trans_id` – Related transaction ID (if applicable)
- `event_timestamp` – Event timestamp
- `platform` – Device platform used
- `event_attributes` – Additional event details in JSON format


### 6. dim_event

- `event_id` – Unique event ID
- `event_name` – Event name (e.g., `payment_status`, `app_opened`)
- `event_category` – Event category (`navigation`, `authentication`, `status`)
- `description` – Detailed event description


### 7. dim_error_categories

- `category_id` – Error category ID
- `category_level` – Error level (`1`, `2`, `3`)
- `category_name` – Error category name
- `parent_category_id` – Parent category ID (`NULL` if level 1)
- `weight` – Error severity weight for prioritization
- `error_message` – Detailed user-facing error message (level 3 only)
