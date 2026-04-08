## 🌟 Funnel Analysis (DAX)

This script defines the funnel stages and calculates the number of distinct user sessions at each step of the bill payment journey.

### 1. Funnel Stage Definition

The funnel stages are created as a static DAX table to define the sequential payment journey.

```SQL
Funnel Stages =
DATATABLE(
    "StageNumber", INTEGER,
    "StageName", STRING,
    {
        {1, "1. Start Checkout"},
        {2, "2. Select Payment Type"},
        {3, "3. Enter Amount"},
        {4, "4. Review Payment"},
        {5, "5. Confirm Payment"},
        {6, "6. Authenticate Transaction"},
        {7, "7. View Receipt"}
    }
)
```


### 2. Funnel Stage Count Measure

This measure calculates the distinct number of sessions that successfully reached each funnel stage.

```SQL
M_CountStage =
VAR StageNum = SELECTEDVALUE('Funnel Stages'[StageNumber])

-- Get all sessions with Payment transactions
VAR SessionsWithPayment =
    CALCULATETABLE(
        VALUES('transactions_enhanced'[session_id]),
        'transactions_enhanced'[transaction_type] = "Payment"
    )

RETURN
SWITCH(
    StageNum,
    1,
        CALCULATE(
            DISTINCTCOUNT('event_logs'[session_id]),
            'event_logs'[event_id] = 7
        ),

    2,
        CALCULATE(
            DISTINCTCOUNT('event_logs'[session_id]),
            'event_logs'[event_id] = 11
        ),

    3,
        CALCULATE(
            DISTINCTCOUNT('event_logs'[session_id]),
            'event_logs'[event_id] = 14,
            TREATAS(SessionsWithPayment, 'event_logs'[session_id])
        ),

    4,
        CALCULATE(
            DISTINCTCOUNT('event_logs'[session_id]),
            'event_logs'[event_id] = 20,
            TREATAS(SessionsWithPayment, 'event_logs'[session_id])
        ),

    5,
        CALCULATE(
            DISTINCTCOUNT('event_logs'[session_id]),
            'event_logs'[event_id] = 24,
            TREATAS(SessionsWithPayment, 'event_logs'[session_id])
        ),

    6,
        CALCULATE(
            DISTINCTCOUNT('event_logs'[session_id]),
            'event_logs'[event_id] IN {26, 29},
            TREATAS(SessionsWithPayment, 'event_logs'[session_id])
        ),

    7,
        CALCULATE(
            DISTINCTCOUNT('event_logs'[session_id]),
            'event_logs'[event_id] = 34,
            TREATAS(SessionsWithPayment, 'event_logs'[session_id])
        ),

    BLANK()
)
```

### 3. Business Logic

Each funnel stage is mapped to a specific `event_id` from the `dim_event` table to represent the user journey in the payment flow.

| Stage | Funnel Step | Event ID | Business Meaning |
|---|---|---:|---|
| 1 | Start Checkout | 7 | User navigated to the payment menu |
| 2 | Select Payment Type | 11 | User selected a payment type |
| 3 | Enter Amount | 14 | User entered an amount and note |
| 4 | Review Payment | 20 | User reviewed payment details |
| 5 | Confirm Payment | 24 | User confirmed a payment |
| 6 | Authenticate Transaction | 26, 29 | User submitted a one-time password/ User returned from 3D Secure authentication |
| 7 | View Receipt | 34 | User viewed a transaction receipt |
```
