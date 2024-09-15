# Forecasting Commodity Futures Curve: A Comparative Analysis of LSTM Models with Raw and Parameterized Data Inputs

## **Research Question** <br>

How does the forecasting performance of Long Short-Term Memory (LSTM) models differ when using raw commodity prices as input compared to parameterized data inputs based on the Nelson-Siegel model?

### **Abstract** <br> 

This paper aims to assess the effectiveness of Long Short-Term Memory (LSTM) models for generating optimal forecasts of commodity future prices. Specifically, two LSTM models, LSTM-Base and LSTM-Nelson-Siegel (LSTM-NS), are compared to evaluate the impact of parameterizing commodity futures curves on the forecasting ability. The study utilizes raw commodity prices as well as parameters derived from the Nelson-Siegel model as inputs to the LSTM models. The research question addressed is whether parameterization of the commodity futures curve enhances the predictive performance of the LSTM model due to reduced input dimensionality. The findings have implications for portfolio managers, hedgers, and corporations seeking to improve risk management and decision-making based on commodity price forecasts.

### **Data** <br>

Three different commodity futures data were retrieved from the Bloomberg Terminal and used in this study. These futures include the ICE Brent Crude future (Brent), the NYMEX West Texas Intermediate Crude Oil future (WTI), and the NYMEX Henry Hub Natural Gas future (NG).  

The dataset composes daily prices of the three commodities spanning from Jan/3/2000 to Jun/13/2023, or a total of 6117 observation after excluding non-trading days. Each observation consists of the price data of 13 contracts with different time to maturity, 1-12 months, and two years to maturity. In other words, each observation represents a futures curve containing 13 points on a given date.

### **Model Architecture**
![Model Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/Model_arch.png)
![Model Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/Model_arch_2.png)

