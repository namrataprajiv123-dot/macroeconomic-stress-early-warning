# macroeconomic-stress-early-warning
An empirical machine-learning early-warning model for identifying future GDP contractions in India.
# Early warning of GDP contractions in India

I and my friend Manasa C Atul,  built the first version of this for the CHRIST University research competition, where it placed first. 
I have since rebuilt it on official data and tested it out of sample.
Next steps: add a term-spread variable and test horizons of 1, 3 and 6 months.
Acknowledgements
The project was developed by the us, with technical guidance from

## Question

Can GDP growth, industrial output and stock-market returns give a warning of a GDP contraction three months before it shows up?

## Data

- **Real GDP growth:** quarterly, % change on the previous quarter, seasonally adjusted. OECD via FRED, series `NAEXKP01INQ657S`.
- **Industrial production:** monthly, % change on a year earlier, seasonally adjusted. OECD via FRED, series `INDPRINTO01GYSAM`, averaged to quarters.
- **Equity returns:** monthly change in the OECD share price index for India, `SPASTT01INM661N`. This is a broad index, not the Nifty 50.

The script downloads all three when it runs.

## What counts as a contraction

A quarter is a contraction if real GDP growth is negative. The sample has four: 2009Q1, 2011Q3, 2020Q2 and 2021Q2. None of them are back to back, so the usual two-quarter definition of a recession would find no episodes at all for India. This is why contraction is used not regression

## Method

The quarterly figures are spread over their three months and then shifted back by five months. Five is our own rough guess at how long a figure takes to be published, and the point is that the model only sees what would have been known at the time. The target is a contraction three months ahead.

I fit a logistic regression as the baseline and a random forest next to it. Both are tested walk-forward: train on the past, predict the next 12 months, move forward and repeat, with a three-month gap between training and test so that overlapping labels cannot leak across. AUC, recall, precision and the confusion matrix are all computed from out-of-sample predictions only.

## Results

*To fill in after running the script on real data: the table of out-of-sample AUC, recall and precision for both models, plus the chart `oos_stress_probability.png`.*

## Limitations

- There are only four episodes, and the test period is mostly COVID, which no model built on lagged data would have seen coming. The metrics are suggestive
- There are no yield-curve or credit variables, even though related work finds them useful.
- The five-month release lag is an assumption.
- The OECD seasonally adjusted quarterly figures are not the same as the year-on-year numbers MoSPI publishes.
- This is a prediction exercise. The feature importances doesnt say anything about what causes a contraction.

## Related work

- Camacho and Palmieri (2021), *Journal of Forecasting*, on OECD indicators as recession signals.
- Senapati and Kavediya (2020), RBI Working Paper 09/2020, on measuring financial stress in India.



. Data from the OECD, accessed through FRED.

Namrata P Rajiv, BSc Economics, Mathematics and Statistics, CHRIST (Deemed to be University).
