# GSoC'25 ExpectedReturns: Factor Formation Tests

## Prerequisites
- https://github.com/corsairier/ChatR-Tests-GSoC
- https://github.com/corsairier/ecotourism-GSoC_Test

These both are the tests of other GSoC projects coming under R language.

## Easy:

The installation of both the packages went smoothly. Just had to use 
`force=TRUE` option for the installation of FactorAnalysis package.

## Intermediate:

1. In `Commodities-long-run.Rmd`, there are many bugs related to bib and also many words flow out of the page.
2. In `expected-returns-replications.Rmd`, there are many warnings thrown related to bibtex and again many words flow out of the page.
3. In `Improving-time-series-momentum.Rmd`, there is some error related to the missing directory `sandbox/data/FuturesData/data/`.
4. In `Time-series-momentum.Rmd`, ff.dates error is encountered.
5. Some text flow out of the margins and the page.
6. In `Value-momentum-everywhere.Rmd`, we experience `object 'RF' not found`.
7. In `Which-factors.Rmd`, we experience this error,

`Error in attributes(xx) <- c(attributes(xx), dot.attrs):
! length of 'dimnames' [2] not equal to array extent`

## Harder

### Analysis of `Commodities-long-run.Rmd`

This report examines a replication of Levine et al.'s (2018) study on commodity futures returns over long time periods. The replication investigates whether commodity futures generate positive returns over the long run, how these returns relate to economic conditions, and whether investment styles can predict future returns. Based on the statistical evidence presented, there appear to be notable discrepancies between the replication results and the original paper's findings, particularly regarding the significance of economic factors in predicting returns.


The analysis reveals that the equal-weighted portfolio consistently generates positive returns across all time periods examined (1877-2015, 1877-1945, 1946-2015). Interestingly, the interest rate-adjusted carry returns contribute more significantly than excess spot returns to the overall performance. This portfolio also demonstrates lower volatility compared to its long-short counterpart, suggesting it may be more suitable for conservative investors seeking commodity exposure.


The long-short backwardation-based portfolio, on the other hand, shows mixed performance with generally positive returns but negative mean returns during the 1877-1945 period. This portfolio exhibits higher overall volatility, particularly in the interest rate-adjusted carry component, indicating greater sensitivity to changes in market conditions. These characteristics suggest the long-short approach might be better suited for investors with higher risk tolerance who seek to capitalize on specific market conditions.


The regression analysis examining macro factors reveals that for the equal-weighted portfolio, business cycles (measured by the recession indicator) show a significant negative correlation with returns, while inflation state demonstrates a significant positive correlation. However, these correlations weaken and lose statistical significance at longer horizons of one year and five years. This pattern suggests these factors may be useful for short-term tactical allocation decisions but less reliable for long-term strategic planning. 


For the long-short portfolio, the relationships between all three state variables (business cycle, inflation, backwardation) and returns appear weak across all time horizons. This limited predictive power contrasts with the original study by Levine et al. (2018), who found significant correlations between inflation and returns for both portfolios across all time horizons, persistent business cycle effects at the one-year horizon, and significant short-term effects from backwardation state.


The investment style analysis offers additional insights into potential return drivers. For the equal-weighted portfolio, momentum shows a significant positive correlation with returns in the short term, while value and aggregate carry show no significant correlation. This suggests that recent price trends may have predictive power for near-term performance, but longer-term valuation metrics and carry signals appear less useful.


The long-short portfolio exhibits more complex relationships with style factors. Momentum displays positive correlation in the short term but shifts to a negative correlation at the one-year horizon, suggesting a potential reversal effect. Aggregate carry only shows significant negative correlation at the five-year horizon. These findings differ from the original study, which found strong correlation between carry and returns across all time horizons for the equal-weighted portfolio.


The replication follows a robust methodology with extensive historical data spanning 1877-2015/2020 and multiple sub-periods to test for structural changes. The statistical approach employs multi-horizon regression analysis at zero, 11, and 59 months, along with factor model implementation via the FactorAnalytics package. The careful construction of investment style metrics and clear separation of spot returns and carry components allow for a thorough analysis of how these components perform in different economic environments.


Despite differences in factor relationships, the models consistently show significant alpha (intercepts) across specifications, suggesting commodity futures do generate excess returns regardless of the specific drivers. This consistency provides some confidence in the overall conclusion that commodities can serve as a valuable asset class.    


The significant discrepancies between the replication and original study warrant further investigation into potential differences in methodology, data sources, or variable construction. The long-short portfolio construction allows for more direct examination of how backwardation/contango states affect returns, which could provide valuable insights if further analyzed.  

**Conclusion**  
The replication study provides valuable insights into commodity futures returns but raises important questions about the robustness of previously reported relationships between economic factors and commodity returns. While the positive alpha across models suggests that commodity futures do generate positive returns over the long run, the specific drivers of these returns and their predictive power remain less clear than suggested in the original paper. A deeper investigation into the sources of these discrepancies would be valuable for both academic understanding and practical investment applications. Additionally, further analysis is needed to determine if the replication results change the overall conclusion that commodity futures can add value to a diversified portfolio from an asset allocation perspective.

### Repetitious Code

- The code that I found to be repetitive was in the `Which-factors.Rmd`.

```R
# The merging process was repeated multiple times

data.qff <- merge(FF6.monthly, Q5.monthly)
data.qff <- data.qff[complete.cases(data.qff), c(ff6.vars, q5.vars)]
data.qff <- data.qff['1967-01/2016-12']

data.qsy <- merge(SY4.monthly, Q5.monthly)
data.qsy <- data.qsy[complete.cases(data.qsy), c(q5.vars, sy4.vars)]
data.qsy <- data.qsy['1967-01/2016-12']

data.qdhs <- merge(DHS3.monthly, Q5.monthly)
data.qdhs <- data.qdhs[complete.cases(data.qdhs), c(q5.vars, dhs3.vars)]
data.qdhs <- data.qdhs['1972-07/2016-12']

data.qbs <- merge(BS6.monthly, Q5.monthly)
data.qbs <- data.qbs[complete.cases(data.qbs), unique(c(q5.vars, bs6.vars))]
data.qbs <- data.qbs['1967-01/2016-12']

```
Instead this could be reduced by creating a function,

```R
# Function to merge, clean, and subset data
prepare_data <- function(dataset1, dataset2, vars, period) {
  data <- merge(dataset1, dataset2)
  data <- data[complete.cases(data), vars]
  data <- data[period]
  return(data)
}

# Using the function
data.qff <- prepare_data(FF6.monthly, Q5.monthly, c(ff6.vars, q5.vars), '1967-01/2016-12')
data.qsy <- prepare_data(SY4.monthly, Q5.monthly, c(q5.vars, sy4.vars), '1967-01/2016-12')
data.qdhs <- prepare_data(DHS3.monthly, Q5.monthly, c(q5.vars, dhs3.vars), '1972-07/2016-12')
data.qbs <- prepare_data(BS6.monthly, Q5.monthly, unique(c(q5.vars, bs6.vars)), '1967-01/2016-12')

```

Other than that I did not find anything repetitious in the Vignettes.
