Examination of the Impact of Changes in the Fed Funds Rate on Savings

This project examines whether changes in the Fed Funds Rate impact household savings behavior (personal savings rate), the savings rate offered by banks (average national savings deposit rate), or the broader money supply (M2). The data was pulled from the FRED (Federal Reserve Economic Data) api. This project asks three questions using economic data:

1. Does the Fed Funds Rate move with the personal savings rate?  
2. Does it move with the M2 money supply?  
3. Does it move with the rates banks actually pay savers, and if so, how quickly?

Data

Pulled via the fredapi Python package from FRED:

| Series | FRED code | Description |
| :---- | :---- | :---- |
| Federal Funds Rate | FEDFUNDS | The Fed's benchmark interest rate |
| Personal Savings Rate | PSAVERT | Share of disposable income saved |
| M2 Money Supply | M2SL | Broad measure of money supply |
| National Savings Deposit Rate | SNDR | Average bank savings account rate |

Two dataframes were built: one including all four series, restricted to 2021–present (the window where SNDR data is available), and one excluding SNDR to preserve the full historical range of the other three series.

Methods

* Cleaned and merged time series with pandas, dropping missing values  
* Computed level correlations (raw series vs. raw series) and month-over-month change correlations (first differences), since two series can trend together without their changes being related  
* Tested lagged correlations (0, 1, 3, 6, 12 months) to check whether the Fed Funds Rate leads other series rather than moving with them simultaneously  
* Visualized each relationship with matplotlib, including a dual-axis plot (Fed Funds Rate vs. Personal Savings Rate) and a shared-axis plot (Fed Funds Rate vs. Deposit Rate)

Key Findings

1\. Personal savings rate: essentially unrelated to the Fed Funds Rate

* Correlation: 0.367 (looks mildly correlated, but likely driven by shared long-run trends, not a real link)  
* Month-over-month change correlation: −0.047  
* Lagged correlations (1–12 months): all near zero (−0.002 to 0.015)

Takeaway: households don't appear to adjust their savings rate in response to interest rate changes, at least not in a simple, linear way.

2\. M2 money supply: weak, inconsistent relationship

* Level correlation: −0.469  
* Month-over-month change correlation: −0.083  
* Correlation with M2's year-over-year growth rate: 0.014

Takeaway: money supply growth is driven by other forces more than by the policy rate itself.

3\. Bank savings deposit rates: a strong, lagged relationship

* Average spread between Fed Funds Rate and deposit rates: 3.11 percentage points (range: 0.0–4.9 points)

Correlation of month-over-month changes, by lag:

| Lag | Correlation |
| :---- | :---- |
| 0 Months | 0.669 |
| **1 Month** | **0.815** |
| 3 Months | 0.675 |
| 6 Months | 0.527 |
| 12 Months | 0.038 |

Takeaway: banks pass Fed rate changes through to savers, but with a roughly one-month lag, and the relationship fades out by a year. This lag means banks reprice deposit products shortly after a Fed move rather than instantly or gradually over the long run.

Conclusion

The Fed Funds Rate has a clear, measurable, lagged effect on what banks pay savers. However, they have little detectable effect on how much people choose to save or on broad money supply growth. This suggests the rate's influence travels more directly through the banking system's pricing decisions than through household saving psychology, at least over the sample period studied.

Limitations & Next Steps

* Deposit rate data only covers 2021–present, a period dominated by one rate-hiking cycle so results may not generalize to other rate environments  
* Correlation ≠ causation; a regression model with controls (inflation, unemployment) would strengthen the story  
* Could extend to regional or credit union deposit rates, or compare against CD/money market rates for a fuller picture of pass-through

How to Run

pip install fredapi pandas matplotlib

Set a FRED\_API\_KEY (free from the FRED website) and run the notebook top to bottom.