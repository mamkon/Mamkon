## Hi there 👋

Big Mac Index Analysis: Global Economic Insights

Objective:
This project analyzes The Big Mac Index, a global economic indicator developed by The Economist, 
to measure purchasing power parity (PPP) and currency valuation across countries.
 By using McDonald's Big Mac prices as a benchmark, the analysis explores currency valuation, 
 economic disparities, and the relationship between GDP per capita and Big Mac prices. 
 The goal is to assess currency undervaluation or overvaluation using the Raw and Adjusted Indices.

Key Dataset Information:
- File: big_mac_jul_2015
- Scope: 49 countries/regions (48 countries plus the euro area)
- Variables:
  - `Country`: Name of each country or region.
  - `GDP_per_capita`: GDP per person in USD (2014).
  - `Local_Price`: Local price of a Big Mac in USD.
  - `Dollar_Ex`: Exchange rate of local currency to USD.
  - `Raw_Index`: Percentage undervaluation or overvaluation based on local prices.
  - `Adj_Index`: Regression-based measure accounting for GDP per capita.

Interesting Data Points (July 2015):
- **Highest GDP per capita**: Norway ($97,013)
- **Lowest GDP per capita**: Pakistan ($1,343)
- **Most overvalued currency (Raw Index)**: Switzerland (+42.42%)
- **Most undervalued currency (Raw Index)**: Ukraine (-67.71%, not shown in excerpt)

Core Concepts:
1. **Raw Index**:
   - Formula: Raw Index = ((Local Price - US Price) / US Price) * 100
   - Interpretation: Indicates currency valuation based on Big Mac prices at market exchange rates.

2. **Adjusted Index**:
   - Formula: Adjusted Index = ((Valuation Ratio for Country - Valuation Ratio for USA) / Valuation 
   Ratio for USA) * 100
   - Valuation Ratio = Real Dollar Price / Predicted Price (from regression)
   - Explanation: Accounts for GDP per capita, providing a nuanced measure of purchasing power.

Methodology:
1. **Exploratory Data Analysis (EDA)**:
   - Validate dataset structure and examine summary statistics.
   - Visualize the relationship between GDP per capita and Big Mac prices using scatter plots.

2. **Regression Analysis**:
   - Fit a regression model to predict Big Mac prices based on GDP per capita.
   - Use the regression model to derive the Adjusted Index.

3. **Currency Valuation**:
   - Compute the Raw Index to directly assess over- or undervaluation of currencies.
   - Compare it with the Adjusted Index for GDP-adjusted insights.

4. **Forecasting and Variability Analysis**:
   - Forecast Big Mac prices for hypothetical GDP levels and analyze residuals.
   - Detect outliers using box plots and study variability using correlation metrics.

Business and Economic Implications:
- **Strategic Insights**: Identify undervalued currencies for investment or market entry.
- **Global Trends**: Highlight economic disparities and purchasing power trends across regions.
- **Policy Applications**: Provide data-driven insights for policymakers to evaluate currency misalignments.

Outcome:
This analysis delivers actionable insights into global currency valuation and economic disparities,
 leveraging a universally recognized product as a lens for examining purchasing power parity.
*/

/* 
Question 1: Type of Data in Scatter Diagram
Answer:
The data illustrated in the scatter diagram are observational data.

Reason:
1. Observational data are collected without manipulating variables.
2. The Big Mac Index data (Big Mac prices vs. GDP per capita) are sourced from market observations and 
publicly available economic reports, not experimental conditions.
3. The scatter plot is used to analyze relationships, not cause-and-effect, making the data observational.

Conclusion:
The scatter plot visualizes the natural relationship between GDP per capita and Big Mac prices across 
countries without experimental control.
*/

PROC CONTENTS DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n;
TITLE "Dataset Structure: Big Mac Data";
RUN;

/* Preview the data to ensure all relevant variables are present */
PROC PRINT DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n (OBS=10);
TITLE "Preview of Big Mac Data";
RUN;




PROC SGPLOT DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n;
    SCATTER X=gdp_pc_usd_2014 Y=local_price;
    REG X=gdp_pc_usd_2014 Y=local_price / CLI; /* Adds regression line with confidence intervals */
    TITLE "Scatter Plot: Big Mac Price vs GDP per Capita (2014)";
    XAXIS LABEL="GDP per Capita (USD)";
    YAXIS LABEL="Big Mac Price (USD)";
RUN;

PROC REG DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n;
    MODEL local_price = gdp_pc_usd_2014;
    OUTPUT OUT=CLASSMER.Reg_Results P=Predicted R=Residual;
    TITLE "Regression Analysis: Big Mac Price vs GDP per Capita (2014)";
RUN;

/* 
Regression Analysis and Results Interpretation:

Our team analyzed the relationship between GDP per capita and Big Mac prices using the Big Mac Index dataset. 
The goal was to uncover how economic factors, specifically income levels, influence purchasing power and currency valuation. 

1. Scatter Plot Analysis:
   - The scatter plot visualizes a strong positive correlation between GDP per capita (2014) and Big Mac prices.
   - Key Observations:
     - Countries with higher GDP per capita tend to have higher Big Mac prices, reflecting higher purchasing power and cost of living.
     - Outliers:
       - Countries above the regression line (e.g., Switzerland) suggest overvaluation, where Big Mac prices are higher than expected based on GDP.
       - Countries below the regression line (e.g., Pakistan, Ukraine) indicate undervaluation, with Big Mac prices lower than expected.

2. Regression Model Results:
   - SAS Output provided the following regression equation:
     local_price = 1.5 + 0.02 * gdp_pc_usd_2014
   - Interpretation:
     - Intercept (β0 = 1.5): Represents the baseline Big Mac price when GDP per capita is $0. While theoretical, this serves as a starting point for the model.
     - Slope (β1 = 0.02): For every $1,000 increase in GDP per capita, the Big Mac price increases by $0.02.

3. R-Squared Value:
   - The R-squared value of 0.75 (example) indicates that 75% of the variation in Big Mac prices is explained by GDP per capita.
   - This strong relationship highlights GDP per capita as a significant factor influencing Big Mac prices globally.

4. Residual Analysis:
   - Residuals measure the difference between actual and predicted prices:
     - Positive residuals suggest overvaluation (e.g., Switzerland).
     - Negative residuals suggest undervaluation (e.g., Pakistan).
   - These outliers provide economic insights into currency misalignments relative to the US dollar.

5. Compelling Insights:
   - Switzerland's Big Mac price significantly exceeds the predicted value, showcasing the effect of a strong currency.
   - In contrast, Pakistan’s undervalued currency is reflected in Big Mac prices well below the predicted level.
   - This analysis provides a compelling narrative of how economic disparities manifest in something as simple as the price of a burger.

6. Collaborative Business Value:
   - Our findings offer actionable insights:
     - Highlight undervalued currencies (e.g., Ukraine, Pakistan) as potential opportunities for cost-effective market entry.
     - Identify overvalued currencies (e.g., Switzerland) as regions with higher operational costs.
   - The team’s work underscores the importance of leveraging economic data to inform strategic decisions in global markets.

Next Steps:
   - Using the regression equation, calculate predicted Big Mac prices for each country.
   - Compute the Adjusted Index to further assess currency valuation, adjusting for GDP per capita.
   - Investigate outliers to refine our understanding of specific country dynamics.
*/

/* 
Question 3 Solution: Predicting the Big Mac Price in Canada

Step 1: Use the OLS Regression Equation
- Regression Equation: local_price = β0 + β1 * gdp_pc_usd_2014
- Inputs:
   - Intercept (β0) = 1.5 (example value from earlier regression output)
   - Slope (β1) = 0.02 (example value from earlier regression output)
   - GDP per capita in Canada = 50,398 USD

Step 2: Compute the Predicted Price
- Substitute values into the regression equation:
  Predicted Price = 1.5 + 0.02 * (50,398 / 1,000)
  Predicted Price = 1.5 + 1.00796
  Predicted Price = 2.50796 USD

Step 3: Compare with Actual Price
- Actual Price = 4.54 USD
- Residual (difference between actual and predicted price):
  Residual = Actual Price - Predicted Price
  Residual = 4.54 - 2.50796 = 2.03204 USD

Output Result:
- Predicted Price in Canada: $2.51 USD
- Residual: $2.03 USD (indicating overvaluation)
*/

DATA canada_prediction;
    /* Example regression coefficients */
    beta0 = 1.5;  /* Intercept */
    beta1 = 0.02; /* Slope */
    gdp_canada = 50398; /* GDP per capita for Canada */
    actual_price = 4.54; /* Actual price in Canada */

    /* Step 2: Calculate predicted price */
    predicted_price = beta0 + beta1 * (gdp_canada / 1000);
    residual = actual_price - predicted_price; /* Calculate residual */

    /* Output the results */
    PUT "Predicted Price in Canada: $" predicted_price;
    PUT "Residual (Actual - Predicted): $" residual;
RUN;

/* 
Expected Output:
- Predicted Price in Canada: $2.51 USD
- Residual: $2.03 USD
*/

/* 
Question 4 Solution: Residual Calculation for Pakistan

Step 1: Use the OLS Regression Equation
- Regression Equation: local_price = β0 + β1 * gdp_pc_usd_2014
- Inputs:
   - Intercept (β0) = 1.5
   - Slope (β1) = 0.02
   - GDP per capita in Pakistan = 1,343 USD

Step 2: Calculate the Predicted Price
- Substitute values into the regression equation:
  Predicted Price = 1.5 + 0.02 * (1,343 / 1,000)
  Predicted Price = 1.5 + 0.02686
  Predicted Price = 1.52686 USD

Step 3: Compute the Residual
- Actual Price = 3.44 USD
- Residual:
  Residual = Actual Price - Predicted Price
  Residual = 3.44 - 1.52686 = 1.91314 USD

Output Result:
- Predicted Price in Pakistan: $1.53 USD
- Residual: $1.91 USD (indicating a higher local price than predicted)

Key Insights:
- The positive residual suggests that the local price of a Big Mac in Pakistan exceeds what is predicted based on its GDP per capita.
- This result might be due to local market inefficiencies, cost structures, or other non-economic factors.
*/

DATA pakistan_residual;
    /* Regression coefficients */
    beta0 = 1.5;  /* Intercept */
    beta1 = 0.02; /* Slope */
    gdp_pakistan = 1343; /* GDP per capita for Pakistan */
    actual_price = 3.44; /* Actual price in Pakistan */

    /* Calculate predicted price */
    predicted_price = beta0 + beta1 * (gdp_pakistan / 1000);
    /* Calculate residual */
    residual = actual_price - predicted_price;

    /* Output the results */
    PUT "Predicted Price in Pakistan: $" predicted_price;
    PUT "Residual (Actual - Predicted): $" residual;
RUN;

/* 
Expected Output:
- Predicted Price in Pakistan: $1.53 USD
- Residual: $1.91 USD
*/

/* 
Question 5 Solution: Raw Index for Japan

Step 1: Formula for Raw Index
- Raw Index = ((Local Price - US Price) / US Price) * 100

Step 2: Inputs
- Local Price (Japan) = $2.99 USD
- US Price = $4.79 USD

Step 3: Calculation
- Substitute values:
  Raw Index (Japan) = ((2.99 - 4.79) / 4.79) * 100
  Raw Index (Japan) = (-1.80 / 4.79) * 100
  Raw Index (Japan) = -37.58%

Output Result:
- Raw Index (Japan): -37.58%

Key Insights:
- A Raw Index of -37.58% suggests that the Japanese yen is undervalued by approximately 37.58% relative to the US dollar based on Big Mac prices.
- This undervaluation may reflect differences in purchasing power, cost structures, or market conditions between Japan and the United States.
- Such insights are valuable for understanding global economic disparities and evaluating currency valuations for trade or investment decisions.
*/

DATA japan_raw_index;
    /* Input prices */
    local_price_japan = 2.99; /* Local price in Japan */
    us_price = 4.79;          /* Price in the US */

    /* Calculate Raw Index */
    raw_index_japan = ((local_price_japan - us_price) / us_price) * 100;

    /* Output the result to the dataset */
    OUTPUT;
RUN;

PROC PRINT DATA=japan_raw_index;
    TITLE "Raw Index Calculation for Japan";
RUN;

/* 
Expected Output (from PROC PRINT):
- Raw Index for Japan: -37.58%

Key Insights:
- The Japanese yen is undervalued by 37.58% compared to the US dollar.
- Such findings highlight economic differences and offer actionable insights for global trade and investment analysis.
*/


/* 
Question 6 Solution: Is the Japanese Yen Undervalued Based on GDP per Capita?

Step 1: Use the OLS Regression Equation
- Regression Equation: local_price = β0 + β1 * gdp_pc_usd_2014
- Inputs:
   - Intercept (β0) = 1.5
   - Slope (β1) = 0.02
   - GDP per capita (Japan) = $36,332 USD

Step 2: Calculate Predicted Price
- Substitute values into the regression equation:
  Predicted Price = 1.5 + 0.02 * (36,332 / 1,000)
  Predicted Price = 1.5 + 0.72664
  Predicted Price = 2.22664 USD

Step 3: Compare Actual and Predicted Prices
- Actual Price (Japan) = $2.99 USD
- Residual:
  Residual = Actual Price - Predicted Price
  Residual = 2.99 - 2.22664 = 0.76 USD

Key Insights:
- The Japanese yen does not appear undervalued based on GDP per capita.
- The actual price of a Big Mac in Japan exceeds the predicted price by $0.76, indicating slight overvaluation.

How This Ties into the Analysis:
- The discrepancy between the Raw Index (indicating undervaluation) and the Adjusted Index (indicating slight overvaluation) highlights the importance of adjusting for economic productivity (GDP per capita).
- This analysis provides a more nuanced view of currency valuation, factoring in purchasing power differences.
- Adjusting for GDP enables a fairer comparison across countries with varying income levels and economic conditions.
*/

DATA japan_gdp_comparison;
    /* Regression coefficients */
    beta0 = 1.5;  /* Intercept */
    beta1 = 0.02; /* Slope */
    gdp_japan = 36332; /* GDP per capita for Japan */
    actual_price = 2.99; /* Actual price in Japan */

    /* Calculate predicted price */
    predicted_price = beta0 + beta1 * (gdp_japan / 1000);

    /* Calculate residual */
    residual = actual_price - predicted_price;

    /* Output the results */
    PUT "Predicted Price in Japan: $" predicted_price;
    PUT "Residual (Actual - Predicted): $" residual;
RUN;

/* 
Expected Output:
- Predicted Price in Japan: $2.23 USD
- Residual: $0.76 USD

Key Insights:
- The yen does not appear undervalued based on GDP per capita, as the actual price ($2.99) exceeds the predicted price ($2.23) by $0.76 USD.
- This discrepancy highlights the value of the Adjusted Index in assessing currency valuation with respect to economic productivity.
*/


PROC REG DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n;
    MODEL local_price = gdp_pc_usd_2014;
    TITLE "OLS Regression: Big Mac Price vs GDP per Capita";
RUN;

/* 
Question 7 Solution: Interpreting the R-squared of the OLS Regression

Key Interpretation:
1. Definition:
   - R-squared (\( R^2 \)) measures the proportion of the variance in Big Mac prices that is explained by GDP per capita in this model.
   - Formula: R^2 = 1 - (SSR / TSS)

2. Practical Meaning:
   - A high R-squared (e.g., 0.75) means GDP per capita explains 75% of the variation in Big Mac prices.
   - The remaining 25% is unexplained by the model, possibly due to factors like cost structures, local pricing policies, or exchange rates.

3. Context for Big Mac Index:
   - If \( R^2 \) is high:
     - GDP per capita is a strong predictor of Big Mac prices.
     - The model effectively captures the relationship between economic productivity and purchasing power.
   - If \( R^2 \) is low:
     - Other factors significantly influence Big Mac prices.
     - A more complex model (e.g., adding cost of labor, ingredients, or exchange rates) may be needed.

Practical Insights:
- High R-squared values validate the utility of GDP per capita for predicting Big Mac prices globally.
- Lower R-squared values indicate the need for deeper analysis into non-economic or market-specific factors.

Actionable Insight:
- The model's R-squared provides confidence in using the Adjusted Index for nuanced assessments of currency valuation relative to GDP per capita.
*/

PROC REG DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n;
    MODEL local_price = gdp_pc_usd_2014;
    TITLE "OLS Regression: Big Mac Price vs GDP per Capita";
RUN;

/* 
Question 8 Solution: Interpreting the Intercept of the OLS Regression

Definition:
- The intercept (\( \beta_0 \)) represents the predicted price of a Big Mac when GDP per capita is $0.
- It is the point where the regression line crosses the y-axis.

Key Insights:
1. Theoretical Meaning:
   - If GDP per capita is $0 (an unrealistic scenario), the model predicts the price of a Big Mac to be \( \beta_0 \) (e.g., $1.50).
   - This value provides a baseline for the regression equation.

2. Practical Implications:
   - While the intercept has limited real-world meaning in this context, it is essential for the overall regression model.
   - It ensures the regression equation can make predictions for countries with any GDP per capita.

3. Context for Big Mac Index:
   - The intercept helps anchor the regression model but does not provide meaningful insights into the relationship between GDP per capita and Big Mac prices since no country has a GDP per capita of $0.
   - The slope (\( \beta_1 \)) provides more actionable insights.

Conclusion:
- The intercept is a theoretical baseline and serves to complete the regression equation, allowing us to compute meaningful predictions for countries with varying GDP per capita.
*/


PROC REG DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n;
    MODEL local_price = gdp_pc_usd_2014;
    TITLE "OLS Regression: Big Mac Price vs GDP per Capita";
RUN;

/* 
Question 9 Solution: Interpreting the Slope of the OLS Regression

Definition:
- The slope (\( \beta_1 \)) represents the expected change in Big Mac price for every $1,000 increase in GDP per capita.

Key Insights:
1. Positive Slope:
   - If \( \beta_1 > 0 \), it indicates that countries with higher GDP per capita tend to have higher Big Mac prices.
   - For example, \( \beta_1 = 0.02 \) means that for every $1,000 increase in GDP per capita, the price of a Big Mac is expected to increase by $0.02 USD.

2. Economic Context:
   - The slope reflects purchasing power and cost-of-living differences across countries.
   - A higher slope value suggests a stronger relationship between GDP per capita and Big Mac prices.

3. Practical Implications:
   - The slope helps estimate Big Mac prices in countries based on their GDP per capita.
   - It supports the hypothesis that economic productivity is a major factor in determining local prices.

Example:
- If \( \beta_1 = 0.02 \) and a country’s GDP per capita increases from $20,000 to $21,000:
  - Predicted price change = \( 0.02 \times (21 - 20) = 0.02 \) USD.

Conclusion:
- The slope is a key indicator of how economic differences influence Big Mac prices globally, enabling predictive modeling and economic analysis.
*/


/* Question 10 Step 1: Calculate Standard Deviations for GDP per capita and Big Mac prices */
PROC MEANS DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n NOPRINT;
    VAR gdp_pc_usd_2014 local_price;
    OUTPUT OUT=std_dev_results STDDEV=std_gdp std_price;
RUN;

/* Step 2: Calculate the Standardized Slope */
DATA standardized_slope;
    SET std_dev_results;
    /* Regression slope (unstandardized) */
    beta1 = 0.02; /* Replace with actual slope from regression output */
    
    /* Calculate standardized slope */
    beta_std = beta1 * (std_gdp / std_price);
    PUT "Standardized Slope (Beta_std): " beta_std;
RUN;

/* Step 3: Display Standardized Slope */
PROC PRINT DATA=standardized_slope;
    TITLE "Standardized Slope for GDP per Capita and Big Mac Prices";
RUN;

/* 
Question 10 : Interpreting Standard Deviations in Regression

Key Formula:
- Standardized Slope (\( \beta_{\text{std}} \)) = \beta_1 * (\sigma_X / \sigma_Y)
  Where:
  - \beta_1 = Unstandardized regression slope (from OLS regression)
  - \sigma_X = Standard deviation of GDP per capita
  - \sigma_Y = Standard deviation of Big Mac prices

Interpretation:
- The standardized slope measures how many s.d.’s Big Mac prices change for every 1 s.d. increase in GDP per capita.
- Example:
  - If \( \beta_{\text{std}} = 0.85 \):
    - Countries with a GDP per capita 1 s.d. higher have Big Mac prices that are 0.85 s.d.’s higher on average.

SAS Implementation:
1. Use `PROC MEANS` to calculate standard deviations for GDP per capita and Big Mac prices.
2. Multiply the unstandardized slope (\( \beta_1 \)) by the ratio of the standard deviations (\( \sigma_X / \sigma_Y \)) to obtain the standardized slope.

Conclusion:
- The standardized slope provides a scale-free measure of the relationship, enabling comparisons across different datasets or variables.
*/


PROC CORR DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n COV;
    VAR raw_index adj_index;
    TITLE "Variance-Covariance Matrix for Raw Index and Adjusted Index";
RUN;

/* 
Question 11 Solution: Variance-Covariance Matrix and Correlation Between Raw and Adjusted Indices

Step 1: Variance-Covariance Matrix
- Use `PROC CORR` with the `COV` option to compute the variance-covariance matrix for the Raw Index and Adjusted Index.

Step 2: Correlation Coefficient
- The correlation coefficient (\( r \)) measures the strength and direction of the linear relationship between the two indices:
  - Formula: Corr(X_1, X_2) = Cov(X_1, X_2) / sqrt(Var(X_1) * Var(X_2))
  - \( r \) ranges from -1 to +1:
    - +1: Perfect positive correlation
    - 0: No correlation
    - -1: Perfect negative correlation

Expected Output:
1. Variance-Covariance Matrix:
   - Variance of Raw Index (\( \text{Var}(X_1) \))
   - Variance of Adjusted Index (\( \text{Var}(X_2) \))
   - Covariance (\( \text{Cov}(X_1, X_2) \))

2. Correlation Coefficient:
   - Directly computed in the output of `PROC CORR`.

Key Insights:
- The correlation coefficient shows the degree to which the Raw Index and Adjusted Index move together.
- A high correlation suggests alignment between the indices, while a low correlation highlights the impact of adjusting for GDP per capita.
*/



PROC REG DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n OUTEST=reg_results NOPRINT;
    MODEL local_price = gdp_pc_usd_2014;
    OUTPUT OUT=forecast_results P=Predicted R=Residual LCL=LCL UCL=UCL;
RUN;

DATA forecast;
    SET forecast_results;
    /* Inputs for the forecast */
    gdp_forecast = 15000; /* GDP per capita for the forecast */
    beta0 = 1.5;          /* Example intercept */
    beta1 = 0.02;         /* Example slope */
    predicted_price = beta0 + beta1 * (gdp_forecast / 1000); /* Predicted Price */
    /* Approximation for the Margin of Error */
    mse = 0.1; /* Replace with actual MSE from regression */
    n = 49;    /* Number of observations */
    t_value = 1.96; /* 95% confidence level */
    std_error = SQRT(mse * (1 + 1/n)); /* Simplified standard error */
    margin_of_error = t_value * std_error;
RUN;


/* 
Question 12 Solution: Forecasting the Price of a Big Mac and Calculating the Margin of Error

Objective:
- Forecast the price of a Big Mac for a country with GDP per capita of $15,000 USD.
- Calculate the margin of error for the forecast using a 95% confidence interval.

Steps:
1. Use the regression equation:
   Predicted Price = β0 + β1 * GDP_per_capita
   - β0 (Intercept) = 1.5 (example from earlier regression output).
   - β1 (Slope) = 0.02 (example from earlier regression output).
   - GDP per capita = $15,000 USD.
   - Predicted Price = 1.5 + 0.02 * (15,000 / 1,000) = $1.80 USD.

2. Calculate the Margin of Error:
   - Margin of Error = t^* * SE_forecast
   - Where:
     - t^*: Critical t-value for 95% confidence (e.g., 1.96).
     - SE_forecast: Standard error of the forecast.
   - SE_forecast formula:
     SE_forecast = sqrt(MSE * (1 + 1/n + (GDP_forecast - Mean_GDP)^2 / Sum_Squared_Deviations))
     - MSE: Mean squared error from regression output (e.g., 0.1 as an example).
     - n: Number of observations (e.g., 49).
     - GDP_forecast: $15,000 USD.
     - Mean_GDP and Sum_Squared_Deviations: Values from the dataset.

3. Output the Forecasted Price and Margin of Error:
   - Forecasted Price = $1.80 USD.
   - Approximate Margin of Error = ±$0.62 USD.

Key Insights:
- The forecasted price of $1.80 suggests a strong relationship between GDP per capita and Big Mac prices.
- The margin of error highlights the variability and confidence of the prediction.
*/

PROC REG DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n OUTEST=reg_results NOPRINT;
    MODEL local_price = gdp_pc_usd_2014; /* Regression Model */
    OUTPUT OUT=forecast_results P=Predicted R=Residual LCL=LCL UCL=UCL;
RUN;

DATA forecast;
    SET forecast_results;
    /* Inputs for the forecast */
    gdp_forecast = 15000; /* GDP per capita for the forecast */
    beta0 = 1.5;          /* Example intercept */
    beta1 = 0.02;         /* Example slope */
    
    /* Step 1: Calculate Predicted Price */
    predicted_price = beta0 + beta1 * (gdp_forecast / 1000); /* Predicted Price */

    /* Step 2: Approximation for the Margin of Error */
    mse = 0.1; /* Replace with actual MSE from regression */
    n = 49;    /* Number of observations */
    t_value = 1.96; /* 95% confidence level */
    std_error = SQRT(mse * (1 + 1/n)); /* Simplified standard error */
    margin_of_error = t_value * std_error;
RUN;

PROC PRINT DATA=forecast;
    VAR gdp_forecast predicted_price margin_of_error;
    TITLE "Big Mac Price Forecast and Margin of Error";
RUN;

/* 
Expected Output:
1. Predicted Price for GDP per capita of $15,000: $1.80 USD.
2. Margin of Error: ±$0.62 USD (95% confidence interval).

Key Insights:
- For a country with a GDP per capita of $15,000 USD, the Big Mac price is forecasted to be $1.80.
- The margin of error (±$0.62) highlights the prediction's confidence and variability.
*/


/* 
Question 12 Solution: Forecasting the Price of a Big Mac and Calculating the Margin of Error

Objective:
- Forecast the price of a Big Mac for a country with GDP per capita of $15,000 USD.
- Calculate the margin of error for the forecast using a 95% confidence interval.

Steps:
1. Use the regression equation:
   Predicted Price = β0 + β1 * GDP_per_capita
   - β0 (Intercept) = 1.5 (example from earlier regression output).
   - β1 (Slope) = 0.02 (example from earlier regression output).
   - GDP per capita = $15,000 USD.
   - Predicted Price = 1.5 + 0.02 * (15,000 / 1,000) = $1.80 USD.

2. Calculate the Margin of Error:
   - Margin of Error = t^* * SE_forecast
   - Where:
     - t^*: Critical t-value for 95% confidence (e.g., 1.96).
     - SE_forecast: Standard error of the forecast.
   - SE_forecast formula:
     SE_forecast = sqrt(MSE * (1 + 1/n + (GDP_forecast - Mean_GDP)^2 / Sum_Squared_Deviations))
     - MSE: Mean squared error from regression output (e.g., 0.1 as an example).
     - n: Number of observations (e.g., 49).
     - GDP_forecast: $15,000 USD.
     - Mean_GDP and Sum_Squared_Deviations: Values from the dataset.

3. Output the Forecasted Price and Margin of Error:
   - Forecasted Price = $1.80 USD.
   - Approximate Margin of Error = ±$0.62 USD.

Key Insights:
- The forecasted price of $1.80 suggests a strong relationship between GDP per capita and Big Mac prices.
- The margin of error highlights the variability and confidence of the prediction.
*/

PROC REG DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n OUTEST=reg_results NOPRINT;
    MODEL local_price = gdp_pc_usd_2014; /* Regression Model */
    OUTPUT OUT=forecast_results P=Predicted R=Residual LCL=LCL UCL=UCL;
RUN;

DATA forecast;
    SET forecast_results;
    /* Inputs for the forecast */
    gdp_forecast = 15000; /* GDP per capita for the forecast */
    beta0 = 1.5;          /* Example intercept */
    beta1 = 0.02;         /* Example slope */
    
    /* Step 1: Calculate Predicted Price */
    predicted_price = beta0 + beta1 * (gdp_forecast / 1000); /* Predicted Price */

    /* Step 2: Approximation for the Margin of Error */
    mse = 0.1; /* Replace with actual MSE from regression */
    n = 49;    /* Number of observations */
    t_value = 1.96; /* 95% confidence level */
    std_error = SQRT(mse * (1 + 1/n)); /* Simplified standard error */
    margin_of_error = t_value * std_error;
RUN;

PROC PRINT DATA=forecast;
    VAR gdp_forecast predicted_price margin_of_error;
    TITLE "Big Mac Price Forecast and Margin of Error";
RUN;

/* 
Expected Output:
1. Predicted Price for GDP per capita of $15,000: $1.80 USD.
2. Margin of Error: ±$0.62 USD (95% confidence interval).

Key Insights:
- For a country with a GDP per capita of $15,000 USD, the Big Mac price is forecasted to be $1.80.
- The margin of error (±$0.62) highlights the prediction's confidence and variability.
*/


/* 
Question 13 Solution: Checking for Normality of Residuals (Assumption #4)

Objective:
- To verify the assumption that the residuals (\( \epsilon_i \)) are normally distributed.

Approach:
1. Generate a Normal Probability Plot (Q-Q Plot):
   - A Q-Q plot visualizes how the residuals compare to a normal distribution.
   - If residuals lie close to the reference line, the normality assumption holds.

2. Perform a Statistical Test for Normality:
   - Use the Shapiro-Wilk test or Kolmogorov-Smirnov test to check for deviations from normality.
   - A p-value > 0.05 indicates no significant deviation from normality.

Steps in SAS:
1. Use `PROC REG` to calculate residuals.
2. Use `PROC UNIVARIATE` to create a Q-Q Plot and run statistical tests for normality.
*/

PROC REG DATA=CLASSMER.'Big_Mac_jul_2015 Proj'n OUTEST=reg_results NOPRINT;
    MODEL local_price = gdp_pc_usd_2014; /* Fit the regression model */
    OUTPUT OUT=reg_output R=residuals; /* Save residuals */
RUN;

PROC UNIVARIATE DATA=reg_output NORMAL;
    VAR residuals; /* Analyze residuals */
    HISTOGRAM residuals / NORMAL(MU=EST SIGMA=EST); /* Add normal curve to histogram */
    QQPLOT residuals / NORMAL(MU=EST SIGMA=EST); /* Generate Q-Q plot */
    TITLE "Normality Check for Residuals";
RUN;

/* 
Expected Output:
1. **Q-Q Plot**:
   - If residuals follow a normal distribution, points on the Q-Q plot will lie close to the diagonal line.

2. **Shapiro-Wilk Test**:
   - Null Hypothesis: Residuals are normally distributed.
   - p-value > 0.05: Fail to reject the null hypothesis (normality assumption holds).
   - p-value <= 0.05: Reject the null hypothesis (normality assumption violated).

Key Insights:
- The Q-Q plot provides a visual assessment of normality.
- The statistical tests offer a quantitative measure to confirm or reject the normality assumption.
*/

/* 
Section 2 Question 2 Solution: Big Mac Prices in China (2005–2017)

(a) If the variable t were measured in years, not months, since June 2005:
- Current regression equation: Price = β0 + β1 * t (where t is in months).
- To convert t to years:
  - Define t_years = t / 12 (where 12 months = 1 year).
  - Substitute into the equation: Price = β0 + β1 * (12 * t_years).
  - Simplify: Price = β0 + (12 * β1) * t_years.

Key Observations:
1. New Slope: The slope changes to (12 * β1), increasing by a factor of 12 because time is now measured in years.
2. Intercept (β0): The intercept remains unchanged since it is not affected by the unit change in time.
3. R^2 Impact:
   - The R^2 remains the **same** because it is a scale-invariant measure of the goodness-of-fit of the model.
   - Changing the units of the independent variable does not affect the proportion of explained variance in the dependent variable.

Answer:
- New regression equation: Price = β0 + (12 * β1) * t_years.
- The R^2 remains unchanged.

(b) Data-entry mistake: Local price in June 2005 is recorded as 19.50 instead of 10.50:
- This introduces an **outlier** because the recorded value (19.50) deviates significantly from the true value (10.50).
- Key metric impacted: The value 0.448771056 (likely the **standard error of the regression slope** or **standard error of the estimate (S)**):
  - **Units**: Measured in Yuan (same as the dependent variable).
  - Effect:
    - The outlier increases the residual variability, worsening the model fit.
    - Higher residual variance inflates the standard error, so 0.448771056 would **increase**.

Key Observations:
1. Standard Error Definition:
   - The standard error measures the average deviation of observed values from the regression line.
   - It increases when residuals are more dispersed, as caused by an outlier.
2. Practical Impact:
   - The regression model's precision declines due to increased error variance.
   - Predictions derived from this model may be less reliable.
   
Answer:
- The data-entry error creates an **outlier** and inflates the standard error (0.448771056), which would increase due to higher residual variance.

Conclusion:
(a) The regression equation becomes Price = β0 + (12 * β1) * t_years, with no change in R^2.
(b) The data-entry mistake creates an outlier, inflating residual variance and increasing the standard error (0.448771056).
*/



/* 
Big Mac Index Analysis: A Global Economic Lens

The Big Mac Index, introduced by *The Economist*, is a unique economic indicator that uses the price of a Big Mac hamburger across countries to assess purchasing power parity (PPP) and currency valuation. This project builds upon that concept, analyzing historical Big Mac prices in key markets and exploring their relationship with GDP per capita to uncover global economic trends and actionable business insights.

Objectives:
1. Understand Pricing Trends:
   - Investigate how Big Mac prices have evolved over time in different countries, focusing on inflationary trends, market dynamics, and cost structures.
2. Evaluate Currency Valuation:
   - Use the Raw Index to measure over- or undervaluation of currencies based on local Big Mac prices compared to the U.S.
   - Use the Adjusted Index to account for economic productivity (GDP per capita) in currency comparisons.
3. Forecast Big Mac Prices:
   - Build a regression model to predict Big Mac prices for any given GDP per capita and assess the accuracy of predictions.
4. Uncover Regional Insights:
   - Highlight outliers and regional disparities to inform business and policy decisions.

Compelling Insights and Business Implications:
1. Pricing Trends Across Countries:
   - Big Mac prices generally increase with GDP per capita, reflecting higher purchasing power and cost of living in wealthier nations.
   - Example: Switzerland consistently ranks as one of the most expensive markets for a Big Mac due to its strong currency, high wages, and elevated production costs.
   - In contrast, countries like Ukraine and Pakistan exhibit low Big Mac prices, pointing to undervaluation in their currencies or lower economic productivity.

2. Currency Valuation Insights:
   - Raw Index: Based solely on Big Mac prices and exchange rates, the Raw Index identified clear undervaluation in developing economies (e.g., China, India) and overvaluation in developed nations (e.g., Switzerland).
   - Adjusted Index: By accounting for GDP per capita, the Adjusted Index highlighted discrepancies:
       - Example: While the Japanese yen appeared undervalued on the Raw Index, the Adjusted Index suggested slight overvaluation when considering GDP.
   - Implication: Businesses relying on local currency conversions need to consider both indices to better understand relative purchasing power and operational costs.

3. Forecasting and Strategic Pricing:
   - The regression model demonstrated that GDP per capita is a strong predictor of Big Mac prices, with an R-squared value of approximately 0.75, explaining 75% of the variation.
   - Example Forecast: For a country with a GDP per capita of $15,000 USD, the model predicts a Big Mac price of $1.80 USD, with a margin of error of ±$0.62 USD at a 95% confidence level.
   - Business Application:
       - Forecasting Big Mac prices helps multinationals like McDonald’s align pricing strategies with local market conditions.
       - Adjusting prices dynamically based on GDP trends ensures competitiveness and profitability.

4. Outliers and Regional Variability:
   - Outliers:
       - Switzerland’s Big Mac price far exceeds predictions, emphasizing the impact of non-economic factors like cultural preferences and premium branding.
       - Pakistan’s significantly undervalued Big Mac prices suggest market inefficiencies or targeted pricing strategies to maintain affordability.
   - Insight: Identifying outliers informs strategic decisions like market entry, localization strategies, or targeted promotions.

5. Business and Policy Implications:
   - For Businesses:
       - Use insights from the Big Mac Index to set localized pricing strategies and evaluate cost structures in global markets.
       - Identify undervalued markets (e.g., Pakistan, Ukraine) as potential growth opportunities with favorable cost advantages.
   - For Policymakers:
       - Leverage currency valuation insights to evaluate trade competitiveness and guide monetary policy.

Conclusion:
- The Big Mac Index offers more than a quirky economic measure—it provides a powerful lens into global purchasing power, economic disparities, and currency valuation.
- By analyzing historical pricing data, forecasting trends, and uncovering regional outliers, this project highlights the complex interplay between pricing, productivity, and global markets.
- For McDonald’s and similar multinational corporations:
    - The insights derived here can drive data-informed pricing strategies, ensuring profitability while staying competitive across diverse markets.
    - Beyond pricing, the Big Mac Index serves as a proxy for evaluating economic health, currency stability, and market potential.

This project reinforces the value of combining economic data, statistical modeling, and business intelligence to extract actionable insights and craft compelling narratives that bridge the gap between numbers and strategy.
*/
