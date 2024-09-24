# Forecasting Commodity Futures Curve: A Comparative Analysis of LSTM Models with Raw and Parameterized Data Inputs

## **Research Question** <br>

How does the forecasting performance of Long Short-Term Memory (LSTM) models differ when using raw commodity prices as input compared to parameterized data inputs based on the Nelson-Siegel model?

### **Abstract** <br> 

This paper aims to assess the effectiveness of Long Short-Term Memory (LSTM) models for generating optimal forecasts of commodity future prices. Specifically, two LSTM models, LSTM-Base and LSTM-Nelson-Siegel (LSTM-NS), are compared to evaluate the impact of parameterizing commodity futures curves on the forecasting ability. The study utilizes raw commodity prices as well as parameters derived from the Nelson-Siegel model as inputs to the LSTM models. The research question addressed is whether parameterization of the commodity futures curve enhances the predictive performance of the LSTM model due to reduced input dimensionality. The findings have implications for portfolio managers, hedgers, and corporations seeking to improve risk management and decision-making based on commodity price forecasts.

### **Data** <br>

Three different commodity futures data were retrieved from the Bloomberg Terminal and used in this study. These futures include the ICE Brent Crude future (Brent), the NYMEX West Texas Intermediate Crude Oil future (WTI), and the NYMEX Henry Hub Natural Gas future (NG).  

The dataset composes daily prices of the three commodities spanning from Jan/3/2000 to Jun/13/2023, or a total of 6117 observation after excluding non-trading days. Each observation consists of the price data of 13 contracts with different time to maturity, 1-12 months, and two years to maturity. In other words, each observation represents a futures curve containing 13 points on a given date.

### **Methodology** <br>

This paper builds upon the research from Figueiredo and Saporito (2022) to compare the predictive performance of the Long Short-Term Memory (LSTM-Base) model against the LSTM-Nelson-Siegel (LSTM-NS) model proposed by the authors. This comparison is performed on various commodity futures while examining the predictiveness of the LSTM model. The following sections discuss in detail: 1. Nelson-Siegel Model, 2. LSTM Model General Overview, 3. LSTM-Base & LSTM-NS Model.

**Nelson-Siegel Model**

The Nelson-Siegel model was introduced by Nelson and Siegel (1987) to predict yield curves. Scholars later adapted this method for predicting commodity futures due to the similar structures between yield and futures curves. This study examines whether feeding LSTM models with futures curve parameter values as inputs improves the predictive performance of the LSTM model compared to using raw price data as inputs.

The Nelson-Siegel (NS) model parameterizes the futures curve data into three parameters as shown below:
![NS Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/Diagrams/NS_Formula.png)

Where: <br>

  * yt is the price of the contract at time t
  * τ represents the remaining time to maturity of a contract <br>

The β parameters are time-varying and estimated recursively for each observation via OLS (Ordinary Least Squares). They represent the **level**, **slope**, and **curvature** components of the futures curve, respectively. The parameter λ governs the speed of decay of the factor coefficients and varies in this study to capture potential shape changes in the futures curves. This allows LSTM models to learn the temporal structures of the data. The parameter λ is also estimated recursively using OLS.

**LSTM Model General Overview**<br>

The LSTM (Long Short-Term Memory) model is a special kind of recurrent neural network (RNN) capable of learning long-term dependencies and processing sequential data. First introduced by Hochreiter and Schmidhuber (1997), it was designed to address the vanishing or exploding gradient problem, making it well-suited for financial time series data.

LSTM Cell Operation <br>

Within an LSTM cell, different gates (forget gate, input gate, and output gate) combine with activation functions such as sigmoid and tanh to determine the cell state and regulate the network. Key steps include:

  * The forget gate decides what information to discard or keep.
  * The input gate adds new information to the current cell state.
  * The output gate produces the hidden state for the next time step.

Figure below demonstrates how an LSTM cell works, xt and ht is the input and output at time t. ht-1 is the output from the previous cell which is also an input in this current cell. it, ft, and ot indicate the input gate, forget gate, and output gate respectively where ct demonstrated the memory cell that 
store the past state. Sigmoid and tanh are the activation functions that map non-linearity.<br>

![LSTM CELL Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/LSTM_Cell.png)

Following equations summarize the the operation of the LSTM layers adapted from previous research and articles (Olah 2015, Vijayaprabakaran and Sathiyamurthy 2022, and Figueiredo and Saporito 2022)

![LSTM CELL Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/Gate_act_fcts.png)

This section describes the two LSTM models used in the study: LSTM-Base and LSTM-NS.

### **Model Architecture** <br>

Both models share the same design, consisting of:<br>

 * 2 LSTM layers with 50 units each, using Glorot initialization to prevent gradient issues.<br>
 * 2 Dropout layers to prevent overfitting, with a 20% dropout rate.<br>
 * Early stopping to monitor validation loss and reduce training iterations if improvement stalls (patience parameter: 5).<br>
 * The models are compiled using the Adam optimizer with an 80%-10%-10% train-validation-test split.

![Model Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/Model_arch.png) <br>

![Model Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/Model_arch_2.png) <br>


**Input and Output Differences**

**LSTM-Base**: The model takes raw price data with 13 features (one for each maturity) as inputs. The input shape for the LSTM-Base model is [# of samples, 300, 13]. <br>

**LSTM-NS**: The raw price data are parameterized into 4 parameters (beta0, beta1, beta2, and lambda). The input shape for the LSTM-NS model is [# of samples, 300, 4] <br>



### Target Forecast Curves 

![Curve Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/NG_curve_ot.png) <br>

![Curve Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/Brent_curve_ot.png) <br>

![Curve Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/WTI_curve_ot.png) <br>


### **Sample Result Display for WTI Futures**

**Within-model Performance**

*LSTM-Base* <br>

![Result Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/LSTM_Base_result.png)

*LSTM_NS* <br>

![Result Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/LSTM_NS_result.png)


**Cross-model Performance**

*Natural Gas*<br>

![Result Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/NG_result.png)

*Brent* <br>

![Result Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/Brent_result.png)

*WTI*<br>

![Result Display](https://github.com/StevenYangts/Research-LSTM_CommFutCurve_Fcst/blob/main/WTI_result.png)
