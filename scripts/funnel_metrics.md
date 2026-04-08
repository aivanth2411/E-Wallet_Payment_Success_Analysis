# Funnel Metrics (DAX)

This script calculates key funnel performance metrics, including:
- conversion rate from previous step
- conversion rate from first step
- average time from first step
- average time from previous step

## 1. Conversion Rate from Previous Step

Calculates the percentage of users who successfully moved from the previous funnel stage to the current stage.

```SQL
M_Percent of Previous =
VAR CurrentStage = MIN('Funnel Stages'[StageNumber])
VAR CurrentStageCount = [M_CountStage]

VAR PreviousStageCount =
    CALCULATE(
        [M_CountStage],
        FILTER(
            ALL('Funnel Stages'),
            'Funnel Stages'[StageNumber] = CurrentStage - 1
        )
    )

RETURN
    IF(
        CurrentStage = 1
            || ISBLANK(CurrentStageCount)
            || ISBLANK(PreviousStageCount)
            || PreviousStageCount = 0,
        BLANK(),
        DIVIDE(CurrentStageCount, PreviousStageCount, BLANK())
    )
```

## 2. Conversion Rate from First Step

Measures the overall conversion rate from **Start Checkout** to each funnel stage.

```SQL
M_Percent of First (step) =
VAR _Current = [M_CountStage]

VAR _First =
    CALCULATE(
        [M_CountStage],
        ALL('Funnel Stages'),
        'Funnel Stages'[StageNumber] = 1
    )

RETURN
    IF(
        ISBLANK(_First) || ISBLANK(_Current),
        BLANK(),
        DIVIDE(_Current, _First)
    )
```


## 3. Average Time from First Step

Calculates the average number of seconds from **Start Checkout (event_id = 7)** to each funnel stage.

```SQL
Average Time From First Step =
VAR CurrentStage = SELECTEDVALUE('Funnel Stages'[StageNumber])

VAR EventID_CurrentStage =
    SWITCH (
        TRUE(),
        CurrentStage = 2, 11,
        CurrentStage = 3, 14,
        CurrentStage = 4, 20,
        CurrentStage = 5, 24,
        CurrentStage = 6, -1,
        CurrentStage = 7, 34,
        BLANK()
    )

RETURN
SWITCH (
    TRUE(),
    CurrentStage = 1, 0,

    AVERAGEX (
        FILTER (
            'event_logs',
            IF (
                CurrentStage = 6,
                'event_logs'[event_id] IN {26, 29},
                'event_logs'[event_id] = EventID_CurrentStage
            )
        ),

        VAR CurrentEventTime = 'event_logs'[event_timestamp]
        VAR CurrentSessionID = 'event_logs'[session_id]

        VAR FirstEventTime =
            CALCULATE (
                MAX('event_logs'[event_timestamp]),
                FILTER (
                    ALL('event_logs'),
                    'event_logs'[session_id] = CurrentSessionID &&
                    'event_logs'[event_id] = 7 &&
                    'event_logs'[event_timestamp] < CurrentEventTime
                )
            )

        RETURN
            IF (
                ISBLANK(FirstEventTime),
                BLANK(),
                DATEDIFF(FirstEventTime, CurrentEventTime, SECOND)
            )
    )
)
```

## 4. Average Time from Previous Step

Calculates the average time taken to move from the previous funnel stage to the current stage.

```SQL
Average Time From Previous Step =
VAR CurrentStage = SELECTEDVALUE('Funnel Stages'[StageNumber])
VAR PrevStage = CurrentStage - 1

VAR EventID_CurrentStage =
    SWITCH(
        TRUE(),
        CurrentStage = 2, 11,
        CurrentStage = 3, 14,
        CurrentStage = 4, 20,
        CurrentStage = 5, 24,
        CurrentStage = 6, -1,
        CurrentStage = 7, 34,
        BLANK()
    )

VAR EventID_PrevStage =
    SWITCH(
        TRUE(),
        PrevStage = 1, 7,
        PrevStage = 2, 11,
        PrevStage = 3, 14,
        PrevStage = 4, 20,
        PrevStage = 5, 24,
        PrevStage = 6, -1,
        BLANK()
    )

RETURN
SWITCH(
    TRUE(),
    CurrentStage = 1, BLANK(),

    AVERAGEX(
        FILTER(
            'event_logs',
            IF(
                CurrentStage = 6,
                'event_logs'[event_id] IN {26, 29},
                'event_logs'[event_id] = EventID_CurrentStage
            )
        ),

        VAR CurrentEventTime = 'event_logs'[event_timestamp]
        VAR CurrentSessionID = 'event_logs'[session_id]

        VAR PrevEventTime =
            CALCULATE(
                MAX('event_logs'[event_timestamp]),
                FILTER(
                    ALL('event_logs'),
                    'event_logs'[session_id] = CurrentSessionID &&
                    'event_logs'[event_timestamp] < CurrentEventTime &&
                    IF(
                        PrevStage = 6 && CurrentStage = 7,
                        'event_logs'[event_id] IN {26, 29},
                        'event_logs'[event_id] = EventID_PrevStage
                    )
                )
            )

        RETURN
            IF(
                ISBLANK(PrevEventTime),
                BLANK(),
                DATEDIFF(PrevEventTime, CurrentEventTime, SECOND)
            )
    )
)
```

## 5. Metric Definitions

| Metric | Business Meaning |
|---|---|
| Previous Step % | Step-to-step conversion rate |
| First Step % | Overall funnel conversion |
| Avg Time from First | Time from checkout start |
| Avg Time from Previous | Time between consecutive steps |
