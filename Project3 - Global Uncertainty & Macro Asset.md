## Idea: Use Text-based Macro Uncertainty to Predict MSCI World Index(ETF)

Shihan Zhou, 2023/10/26

#### 1.Main Idea: 

* Chosen ETF:  MSCI World Index (or other ETFs based on the global stock markets)
* Macroeconomic uncertainty and financial uncertainty have negative correlation with the returns of the Global Market.
* *Jurado, Ludvigson, and Ng (2015)* gives a measurement of macro and financial uncertainty using hundreds of macro indicators and financial time series, but only on monthly basis. 
* We may use Finance News like Wall Street Journal article titles to learn the uncertainty that the investors are aware of. The news data can be more frequent and timely to obtain. 
* Therefore, we can use the text-based Macro Uncertainty as an indicator to help predict the ETF price.

#### 2. Analysis of the Idea

##### 2.1 The relation between Macro, Financial Uncertainty and Global Markets

Macro uncertainty and financial uncertainty are usually high during the period of global financial crisis. Increment in the financial uncertainty is usually an indicator of bad economy. It can depress hiring, investment, and also consumption because investors and institutions might tighten their investment feeling unsure about the future.

In the below graphs, I use VIX as a proxy of investors' uncertainty about the financial market and plot its correlation with the price of MSCI World Index. We can see a negative correlation between the daily log returns of MSCI-W and VIX with no lag. 

![VIX_MSCI](.\code_pj3\VIX_MSCI.png)

<img src=".\code_pj3\VIX_MSCI_xcorr.png" alt="VIX_MSCI_xcorr" style="zoom:67%;" />

In the article shared by 2Sigma,  *Beyond the VIX: Alternative Measures of Macro and Financial Uncertainty*, the author plots the loadings of linear regression of macro and financial uncertainty on several macro assets or indexes as below. They follow the methodology of *Jurado, Ludvigson, and Ng (2015)* to calculate the macro uncertainty and financial uncertainty. We can also see a negative impact of uncertainty on the global market.

<img src=".\code_pj3\2sigma.png" alt="2sigma" style="zoom: 40%;" />

##### 2.2  *Jurado, Ludvigson, and Ng (2015)* 's Measurement of Uncertainty

* The common proxy of volatility, VIX, may too specially reflect the stock market uncertainty, and is driven by many other factors like sentiment, leverage changes instead of economic fundamentals.
* Their measure of Uncertainty are based on estimation of conditional expected volatility, using more than 100 macro indicators and financial time series. The macro uncertainty is a weighted average of individual uncertainty, which is estimated by FAVAR system. 
* One of the characteristics of this Uncertainty measurement is quantitatively important uncertainty episodes occur more infrequently and display larger and more persistent correlations with real activity.

Below is a figure of the value calculated by the 2-sigma article.

<img src=".\code_pj3\2sigma Uncertainty.png" alt="2sigma Uncertainty" style="zoom: 40%;" />

##### 2.3 Using news data to learn the uncertainty, Gain more timely estimation

* Use some popular news text, for example, using the front-page articles of the Wall Street Journal. Like the methodology of *Manela and Moreira (2017)*.

* let $U_t$ be the uncertainty at month $t$,  $x_{t}$ be the predicting vectors summarizing the word frequency of the month's articles.

  $x_{t,i} = \frac{number\ of\ n-gram\ i\ in\ month\ t }{total\ number \ n-grams\ in\ month\ t}$

  $U_t = w_0 + W \cdot x_t + v_t$

  Since $x_t$ is really high-dimensional,  use models like SVR regressions or PCA methods or MLP to fit the model.

* *Comments of the methods:* the training data is limited compared to the high dimensions.

*  Then we develop a way to gain more timely insights of the market uncertainty and concerns. 

##### 2.4 Prediction signal for the MSCI World Index

The VIX itself may not be a so strong indicator of the ETF returns. Below is the OLS regression results of the VIX on log returns of MSCI World Index. The R-square and significance is low.  

<img src=".\code_pj3\regression.png" alt="regression" style="zoom:30%;" />

But we can expect the macro uncertainty may be a more leading signal because it diggers deeper into the  economical fundamentals. We can use text-based uncertainty not simply replace but to modify our monthly estimation of uncertainty.

But we still need to combine the signal with other time series predicting indicators because combination makes better.



#### Reference:

* Kyle Jurado & Sydney C. Ludvigson & Serena Ng, 2015. "Measuring Uncertainty," *American Economic Review, American Economic Association*, vol. 105(3), pages 1177-1216, March.
* Manela, A. , & Moreira, A. . (2016). News implied volatility and disaster concerns. *Journal of Financial Economics*, S0304405X16301751.
* [Beyond the VIX: Alternate Measures of Macro and Financial Uncertainty - Two Sigma](https://www.twosigma.com/articles/beyond-the-vix-alternate-measures-of-macro-and-financial-uncertainty/)





### **Another Idea may worth careful thoughts:** 

* Some broad-based index ETF or some sector-specific index ETF consists of stocks trading in different countries (Time Zone), for example, Hong Kong and US. The US price change of certain industrial stocks might be a leading prediction of the corresponding Hong Kong stocks. The price adjustment may not be realized so quickly when the Hong Kong market is closed. Therefore, we can try to model the self-correlation of the index price to make predictions.

  