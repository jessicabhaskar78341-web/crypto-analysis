# Measures and DAX

This file includes all the measures and DAX used during the project.

## Growth Momentum
```dax
Growth Momentum =
VAR weekly =
    SELECTEDVALUE(crypto[price_change_percentage_7d_in_currency])
VAR daily =
    SELECTEDVALUE(crypto[price_change_percentage_24h_in_currency])
RETURN
IF(
    ISBLANK(weekly) || ISBLANK(daily),
    BLANK(),
    DIVIDE(weekly - daily, ABS(daily))
)
```

## Momentum Rating
```dax
Momentum Rating =
VAR M = [Growth Momentum]
RETURN
SWITCH(
    TRUE(),
    ISBLANK(M), "No Data",
    M >= 0.50, "Strong Uptrend",
    M >= 0.20, "Moderate Uptrend",
    M >= 0.00, "Sideways/stable",
    M >= -0.20, "Weak Downtrend",
    "Strong Downward / Reversal Risk"
)
```

## ForecastDates
```dax
ForecastDates =
DATATABLE(
    "Label", STRING,
    "DayIndex", INTEGER,
    {
        {"7 Days Ago", -7},
        {"Today", 0},
        {"7 Days Ahead", 7}
    }
)
```