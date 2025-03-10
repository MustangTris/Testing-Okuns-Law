### Table of Contents

1. [Research Question](#Research-Question)
2. [Abstract](#Abstract)
3.  [Theoretical Framework](#Theoretical-Framework)
4.  [Empirical Framework](#Empirical-Framework)
      - [Weak Dependence Test](#Weak-Dependence-Test)
      - [Interpretation](#Interpretation)
5. [Final Regression](#Final-Regression)
      - [Interpretations](#Interpretations)
6. [Works Cited](#Works-Cited)

# Research Question:

*Does Okun's Law hold true at the national level in the United States?*

## Abstract

Using time-series data from FRED, Okun's Law is tested on a national scale, reaching back from quarterly reports starting Jan 1948. In attempt of identifying a relationship between **Unemployment Rate (UNRATE)** and **Gross Domestic Product (GDP)**, we fail to reject the null hypothesis due to mixed results between the lagged and current variables representing **Gross Domestic Product (GDP)**. For future research, this could mean restructuring analysis to support further lagged variables, reaching back up to a year rather than a quarter.

## Theoretical Framework

***Okun's Law:***
 This macroeconomic law posits a negative relationship between unemployment and economic growth.

 A law is defined as a principle of nature; A state of matter that cannot be false, given constraints are met. I chose to research an economic law for a stronger hypothesis. A higher probability of rejecting the null hypothesis means a more clear-cut research process. If we fail to reject the null hypothesis, there might be an error in the process. Okun's law robustness has been tested in the past [[Texas A&M University](https://d1wqtxts1xzle7.cloudfront.net/32945963/JMACRO_2000-libre.pdf?1392043528=&response-content-disposition=inline%3B+filename%3DThe_robustness_of_Okuns_law_Evidence_fro.pdf&Expires=1732175263&Signature=YBkeWfFMUe2-gXZ5A~f9venh4lYzaVbOOTN~zwE7-vWBnxc~9eXlq5OKImHq4DvMM35P9fD0cTte9yca6CckAoe9vsJDmTlcEOOULYnUkI4~c6JqKWQ0ogj4ao3JAXqEi296KwD2DZqFWe~Dkyx85FbFxCDgfCXswhYdFZMiala84M2Orn--vwaoo~dkdl1GoepItymcsgL6HgoWIsVMbwFL3PqCfVOfrefMaG56eqBsMmlJVR0h9gjIvUdCqEa9PZWzjPGV091uR1I-cIf3e1e5Km975hcGDYS2Wom-UWFW4dpsbDd42zAbVnUb4VEP2LmBLOx6m61EIsIp6aq33Q__&Key-Pair-Id=APKAJLOHF5GGSLRBV4ZA)]. The results concluded strong correlation, although rocky in the 1907's recession. 

## Empirial Framework

 All data is available via [FRED](https://fred.stlouisfed.org/graph/?g=1BLqN). This data is seasonally adjusted and reported quarterly. We're using data pertaining from Q1 1948 to Q3 2024. The data starts on the first quarter in which all variables were reported-Q2 1948, and ends on the most recently reported data-Q3 2024. Gross domestic product will measure the expected growth in the economy. The consumer price index will tell us if unemployment has a tangible effect on inflation. Finally, the labor force participation rate catches various discrepancies-like an aging population-resulting in an economic correction.
 
$$
\begin{align}
Unemployment_i &= \beta_0 + \beta_1GDP_t + \beta_2CPI_t + \beta_3Labor_t + \epsilon_t
\end{align}
$$
| Variable | Description |
|----------|-------------|
| Unemployment Rate | dependant, Percent |
| Gross Domestic Product | independant, Percent change from Preceding Period ($t-1$) |
| Consumer Price Index | independant, Percent change from Year Ago ($t-4$) |
| Labor force participation rate | independant, Percent, Based on Jan 1947 |

First the data is imported as a dataframe and printed for a visual analysis of possible formatting errors. Then the date column is being reformatted to a standardized library by quarter. Change in unemployment is then plotted on a line graph for visual representation of the dependent variable.


```python
import pandas as pd
import statsmodels.formula.api as smf
import numpy as np
```


```python
df=pd.read_excel(r"C:\Users\trist\OneDrive\Documents\College\Current classes\Econometrics\Final\Econometrics FRED Data New.xlsx", sheet_name='FRED Graph')
print(df)
print(df.describe())
```

        Observation_Date    UNRATE   GDP      LFP      CPI
    0         1948-01-01  3.733333   9.6  4.07187  8.83257
    1         1948-04-01  3.666667  10.7  4.07471  9.01106
    2         1948-07-01  3.766667  10.1  4.07810  8.47784
    3         1948-10-01  3.833333   1.7  4.07471  4.52580
    4         1949-01-01  4.666667  -7.4  4.07527  1.38320
    ..               ...       ...   ...      ...      ...
    302       2023-07-01  3.700000   7.7  4.13889  3.56176
    303       2023-10-01  3.733333   4.8  4.13783  3.23615
    304       2024-01-01  3.800000   4.7  4.13623  3.24919
    305       2024-04-01  4.000000   5.6  4.13677  3.19431
    306       2024-07-01  4.200000   4.7  4.13836  2.64001
    
    [307 rows x 5 columns]
                        Observation_Date      UNRATE         GDP         LFP  \
    count                            307  307.000000  307.000000  307.000000   
    mean   1986-04-01 09:46:19.153094464    5.688056    6.487296    4.139493   
    min              1948-01-01 00:00:00    2.566667  -29.100000    4.068460   
    25%              1967-02-15 00:00:00    4.366667    4.150000    4.090450   
    50%              1986-04-01 00:00:00    5.500000    6.000000    4.141020   
    75%              2005-05-16 12:00:00    6.750000    8.700000    4.188895   
    max              2024-07-01 00:00:00   13.000000   40.000000    4.209160   
    std                              NaN    1.702485    5.374826    0.046349   
    
                  CPI  
    count  307.000000  
    mean     3.533016  
    min     -2.787270  
    25%      1.657215  
    50%      2.893890  
    75%      4.460630  
    max     14.425770  
    std      2.875790  
    


```python
df['Observation_Date'] = pd.PeriodIndex(df['Observation_Date'], freq='Q')
df.plot(kind='line', x="Observation_Date", xlabel='Year', ylabel='Unemployment Rate', y=['UNRATE', 'GDP'])
```




    <Axes: xlabel='Year', ylabel='Unemployment Rate'>




    
![png](output_4_1.png)
    


This graph shows a visualization of the relationship between the Unemployment and GDP. Now a regression can be done to see if this is true.

### Weak Dependance Test

To set up weak dependance tests, an alternative variable with adjusted time (t-1) must be created for each original variable.


```python
df['CPI_t'] = df['CPI'].shift(1)
df['GDP_t'] = df['GDP'].shift(1)                    
df['LFP_t'] = df['LFP'].shift(1)
```

Now that the time shifted variables are created, we can check for weak dependance by running a regression on the 3 sets of shifted variables on their original variables.


```python
CPI_wd=smf.ols(formula="CPI ~ CPI_t", data=df)
print(CPI_wd.fit().summary())
GDP_wd=smf.ols(formula="GDP ~ GDP_t", data=df)
print(GDP_wd.fit().summary())
LPR_wd=smf.ols(formula="LFP ~ LFP_t", data=df)
print(LPR_wd.fit().summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                    CPI   R-squared:                       0.906
    Model:                            OLS   Adj. R-squared:                  0.905
    Method:                 Least Squares   F-statistic:                     2922.
    Date:                Thu, 28 Nov 2024   Prob (F-statistic):          5.83e-158
    Time:                        11:53:37   Log-Likelihood:                -394.35
    No. Observations:                 306   AIC:                             792.7
    Df Residuals:                     304   BIC:                             800.1
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept      0.1688      0.080      2.115      0.035       0.012       0.326
    CPI_t          0.9466      0.018     54.054      0.000       0.912       0.981
    ==============================================================================
    Omnibus:                       43.423   Durbin-Watson:                   1.109
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):              359.970
    Skew:                          -0.014   Prob(JB):                     6.82e-79
    Kurtosis:                       8.313   Cond. No.                         7.44
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                    GDP   R-squared:                       0.073
    Model:                            OLS   Adj. R-squared:                  0.069
    Method:                 Least Squares   F-statistic:                     23.78
    Date:                Thu, 28 Nov 2024   Prob (F-statistic):           1.75e-06
    Time:                        11:53:37   Log-Likelihood:                -937.11
    No. Observations:                 306   AIC:                             1878.
    Df Residuals:                     304   BIC:                             1886.
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept      4.7290      0.465     10.162      0.000       3.813       5.645
    GDP_t          0.2692      0.055      4.876      0.000       0.161       0.378
    ==============================================================================
    Omnibus:                      153.825   Durbin-Watson:                   2.114
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):             5458.675
    Skew:                           1.367   Prob(JB):                         0.00
    Kurtosis:                      23.510   Cond. No.                         13.3
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                    LFP   R-squared:                       0.993
    Model:                            OLS   Adj. R-squared:                  0.993
    Method:                 Least Squares   F-statistic:                 4.519e+04
    Date:                Thu, 28 Nov 2024   Prob (F-statistic):               0.00
    Time:                        11:53:37   Log-Likelihood:                 1273.1
    No. Observations:                 306   AIC:                            -2542.
    Df Residuals:                     304   BIC:                            -2535.
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept      0.0285      0.019      1.473      0.142      -0.010       0.067
    LFP_t          0.9932      0.005    212.590      0.000       0.984       1.002
    ==============================================================================
    Omnibus:                      251.925   Durbin-Watson:                   2.140
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):            10917.833
    Skew:                          -2.921   Prob(JB):                         0.00
    Kurtosis:                      31.674   Cond. No.                         391.
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    

### Interpretation

$$
\begin{array}{|c|c|c|}
\hline
\textbf{Weak Dependance Variable Tested} & \textbf{Significance} & \textbf{Correlation} \\ 
 & \textbf{P>|t|} & (\beta_n) \\
\hline
\text{Gross Domestic Product} & \text{0.000 (***)} & 26.92\% \\ 
\text{Consumer Price Index} & \text{0.000 (***)} & 94.66\% \\ 
\text{Labor force participation rate} & \text{0.000 (***)} & 99.32\% \\ 
\hline
\end{array}
$$
These statistically very significant relations to varying degrees gives us a clear representation of what results should look like when looking for strong dependance. These results are not ideal. To combat this, the time shifted variables will be included in the final regression formula.

### Final Regression

Now, with the final regression formula out of date, let's update our previous formula to include the time lagged variables.
$$
\begin{align}
Unemployment_i &= \beta_0 + \beta_1GDP_t + \beta_2CPI_t + \beta_3Labor_t + \beta_4GDP_{t-1} + \beta_5CPI_{t-1} + \beta_6Labor_{t-1} + \epsilon_t
\end{align}
$$


```python
reg=smf.ols(formula="UNRATE ~ CPI + CPI_t + GDP + GDP_t + LFP + LFP_t", data=df)
print(reg.fit().summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                 UNRATE   R-squared:                       0.143
    Model:                            OLS   Adj. R-squared:                  0.126
    Method:                 Least Squares   F-statistic:                     8.338
    Date:                Thu, 28 Nov 2024   Prob (F-statistic):           2.29e-08
    Time:                        11:53:38   Log-Likelihood:                -572.68
    No. Observations:                 306   AIC:                             1159.
    Df Residuals:                     299   BIC:                             1185.
    Df Model:                           6                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept    -31.1484      8.392     -3.712      0.000     -47.663     -14.634
    CPI           -0.1864      0.115     -1.624      0.105      -0.412       0.040
    CPI_t          0.2541      0.109      2.325      0.021       0.039       0.469
    GDP            0.0264      0.020      1.318      0.188      -0.013       0.066
    GDP_t         -0.0444      0.019     -2.369      0.018      -0.081      -0.008
    LFP          -80.3937     25.726     -3.125      0.002    -131.021     -29.766
    LFP_t         89.2679     25.748      3.467      0.001      38.597     139.938
    ==============================================================================
    Omnibus:                       13.883   Durbin-Watson:                   0.247
    Prob(Omnibus):                  0.001   Jarque-Bera (JB):               15.014
    Skew:                           0.541   Prob(JB):                     0.000549
    Kurtosis:                       2.919   Cond. No.                     5.30e+03
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 5.3e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
    

# Interpretations

$$
\begin{array}{|c|c|c|c|}
\textbf{Variable} & \textbf{P-Value} & \textbf{Correlation} & \textbf{Significance Level} \\ 
 & \textbf{P>|t|} & (\beta_n) & \\ 
\hline
\text{Gross Domestic Product} & \text{0.188} & 2.64\% & \text{Not significant} \\ 
\text{Gross Domestic Product, last period} & \text{-0.081 (ns)} & -4.44\% & \text{Not significant} \\ 
\text{Consumer Price Index} & \text{0.105} & -18.64\% & \text{Not significant} \\ 
\text{Consumer Price Index, last period} & \text{0.021} & 25.41\% & \text{* Significant} \\ 
\text{Labor force participation rate} & \text{0.002} & -8039.37\% & \text{** Very significant} \\ 
\text{Labor force participation rate, last period} & \text{0.001} & 8926.79\% & \text{*** Highly significant} \\ 
\hline
\end{array}
$$

- **Lagged CPI vs. Current CPI:**
  - Lagged **Consumer Price Index (CPI)** is significant ($p < 0.05$, $\beta_2=25.41\%$), while the current CPI is not ($p = 0.105$), showing inflation effects take time.

- **Lagged GDP vs. Current GDP:**
  - Neither current nor lagged **Gross Domestic Product (GDP)** is significant ($p = 0.188$ and $p = 0.081$).

- **Lagged LFP vs. LFP:**
  - Current **Labor Force Participation Rate (LFP)** shows a strong negative correlation ($\beta_6=-8039.37\%$, $p < 0.01$), while the lagged rate has a strong positive correlation ($\beta_5=8926.79\%$, $p < 0.001$).

Because of the mixed **Gross Domestic Product (GDP)** results, we fail to reject the null hypothesis. This all means lagged variables outperform current ones, emphasizing the delayed effects of economic factors on unemployment. The lack of significance for GDP suggests that unemployment may be driven more by structural factors (e.g., labor force participation) than by aggregate output. The insignificance of the current CPI suggests that inflation’s immediate impact on unemployment may be weaker than traditionally expected. Perhaps a more robust model capturing the nuanced Distributed Lag Model (DLM).

# Works Cited

1. Texas A&M University. "The Robustness of Okun's Law: Evidence from OECD Countries." [Access the paper here](https://d1wqtxts1xzle7.cloudfront.net/32945963/JMACRO_2000-libre.pdf?1392043528=&response-content-disposition=inline%3B+filename%3DThe_robustness_of_Okuns_law_Evidence_fro.pdf&Expires=1732175263&Signature=YBkeWfFMUe2-gXZ5A~f9venh4lYzaVbOOTN~zwE7-vWBnxc~9eXlq5OKImHq4DvMM35P9fD0cTte9yca6CckAoe9vsJDmTlcEOOULYnUkI4~c6JqKWQ0ogj4ao3JAXqEi296KwD2DZqFWe~Dkyx85FbFxCDgfCXswhYdFZMiala84M2Orn--vwaoo~dkdl1GoepItymcsgL6HgoWIsVMbwFL3PqCfVOfrefMaG56eqBsMmlJVR0h9gjIvUdCqEa9PZWzjPGV091uR1I-cIf3e1e5Km975hcGDYS2Wom-UWFW4dpsbDd42zAbVnUb4VEP2LmBLOx6m61EIsIp6aq33Q__). Accessed November 27, 2024.

2. Federal Reserve Bank of St. Louis. "Economic Data Graph: Unemployment Rate and Real GDP Growth." [Access the graph here](https://fred.stlouisfed.org/graph/?g=1BLqN). Accessed November 27, 2024.

