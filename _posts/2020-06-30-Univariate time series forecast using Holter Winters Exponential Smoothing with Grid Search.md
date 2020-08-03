---
layout: post
title: Holter-Winters exponential smoothing model with grid search on univariate time series predictions
image: /img/stock.png
---

## Introduction: 

We recently completed a project of building a platform which empowers our stakeholder [Sauti East Africa Organization](https://sautiafrica.org/) to make timely decisions on where to direct their relief fund and effort when their monitored market prices are likely to increase beyond our predicted price alert boundary. This project is one of the lab projects initiated by [Lambda School](https://lambdaschool.com/), along with its partner organizations/companies, to get their students work on real-world problems in team working environment of modern industry.  I was happy to learn time series forecasting though the project and report some of my methods/findings that might be useful for anyone who is in a similar learning path toward univariate time series forecasting. Due to proprietary issue, only part of the information is shown in this post.  

One of the task for us data science students is to select and predict daily market price for 100 markets and thousands of products. 
Based on my exploratory test results, statistical methods outperform [deep learning models](https://github.com/qianjing2020/Sauti-Africa-Market-Monitoring-DS/tree/master/hw-forecast/app/test). This finding also agrees with previous studies which compared classical statistic methods and machine learning methods on univariate time series forecasting [1, 2]. Based on all those preliminary findings, I selected Holt-Winters Exponential Smoothing for my final forecast model. 

Luckily [Statsmodels](https://www.statsmodels.org/dev/generated/statsmodels.tsa.holtwinters.ExponentialSmoothing.html) library has provided Holter-Winters model and some optimization option. When the Holter-Winters model optimization is set to true, some of the parameters are automatically tuned during fitting. These parameters are _smoothing level_, _smoothing slope_, _smoothing seasonal_, and _damping slope_.  To find the best combination for the rest of the unoptimized parameters, We can implement search method, combined with a forward sliding window validation on our time series dataset. In our project, I used grid-search which works better for the domains of searched parameters, ie, _trend_, _dampening_, _seasonality_, _seasonal period_, _Box-Cox transform_, _removal of the bias at fitting_). The **RMSPE** (root mean squared percentage error) is used to evaluate the model performance and the smallest **RMSPE** indicates the best combination for all model parameters. 

## Methods description: 
1. Data preprocess. Time series sequences are preprocessed (null removed, zero removed, duplication removed) and stored in our analytical database in AWS Cloud. The test sequences are extracted from analytical database giving the unique combination of product_name, market_id, and source_id. Related classes and functions for data preprocess can be found in [data_process_v2.py]('https://github.com/qianjing2020/Sauti-Africa-Market-Monitoring-DS/blob/master/hw-forecast/app/code/data_process_v2.py').

2. Grid-searching with slide window forecast. First we remove the outliers and augmented to a day-by-day timeframe, then we interpolated the time series to fill data gaps from days to months. Then we split the data into initial train, initial test, and validation periods. A forecast window, with default length of 30 days, slides down the end of the initial train at a default pace of 14 days per slide. The 144 combinations of model parameters are fed into the model for each window forecast and model scoring.  Finally, the best score returned from all valid windows and its correspondent model parameters are recorded and saved to our [AWS](https://aws.amazon.com/) database.  In this process, each time series from unique source-market-product combination can get its own optimized model configuration. Related classes and functions can be found in [classic_forecast_v2.py](https://github.com/qianjing2020/Sauti-Africa-Market-Monitoring-DS/blob/master/hw-forecast/app/code/classic_forecast_v2.py).

3. Exceptions. Poor configuration of model parameters can results in failure during model fitting. Sometimes model fitting does not converge during a certain slide, so the window is discard during the grid search. For time series with very poor data quality (sparse data with gap in years), the model fitting fails at all windows and the grid search will return nothing. 

## Configurations:
As a first approach, [test_65_sequence.py](https://github.com/qianjing2020/Sauti-Africa-Market-Monitoring-DS/blob/master/hw-forecast/app/test/test_65_sequence.py) selects 65 sequences from our 4-year East Africa Market Price Dataset provided by our stakeholder. The default configurations for model are as follows:

* Data split (in days):  
    train (start) = 692,
    test(start) = 1038,
    val =30,
    window length = 30, 
    sliding distance = 14

* Model parameters:  
    A total of 144 configurations (_144=3x3x2x2x2x2_).  

    The searched parameters *t, d, s, p, b, r* are trend type, dampening type, seasonality type, seasonal period, Box-Cox transform, removal of the bias at fitting, respectively. Their search domains are:  
    t_params = ['add', 'mul', None]  
    d_params = [True, False]  
    s_params = ['add', 'mul', None]  
    p_params = [12, 365]  
    b_params = [True, False]  
    r_params = [True, False]  

    The 'add' (additive) method is preferred when the seasonal variations are roughly constant through the series, while the 'mul' (multiplicative) method is preferred when the seasonal variations are changing proportional to the level of the series. 

## Model output: 
* Example of one time series: sale type = retail, market = 'Dar Es Salaam', product = ' Morogoro Rice': 
  
    Best configuration = ['add', True, 'add', 12, False, False]
 
    Root mean square percentage error (RMSPE):
         5.40% for 30 days forecasting, test dataset and predictions
         1.17% for 30 days forecasting, validation data and predictions

* All the time series metadata, best parameter configuration, model forecast for the validation period and the correspondent RMSPE are saved to database table 'hw_params_wholesale' and 'hw_params_retail' for future reference. 
  
    <img src="https://github.com/qianjing2020/qianjing2020.github.io/blob/master/img/hw_model_result.png"  width="800">
    
## Methodology Pros and Cons:
Pros: 
* Model parameters can be customized. User has total control over window size (number of days to predict), sliding pace, train-test-val split, and grid-search domain. 
* Model is highly tolerant, suitable for all time series, even the flat data. Will always return the best model configuration when data pass QC check. 
* Model forecast results are much more accurate than **Facebook Prophet** forecast method, and several [tested Deep Learning methods](https://github.com/qianjing2020/Sauti-Africa-Market-Monitoring-DS/tree/master/hw-forecast/app/test). 
* Model is also adaptive. User can add own model evaluation metrics, add random search instead of grid search, etc.
   
Cons: 
* User needs to be familiar with python and database basics.
* Time consuming. The grid search method desires large computational power. To get a better idea of computational intensity for the grid search method, we tested one time sequence: Dar Es Salaam Morogoro Rice, which has a total length of 1760 days after interpolation. On a stand-alone machine (2.6 GHz processor, 8 GB RAM), it took 108.73 min to finish grid searching of 144 configurations; while on a virtual machine with 64 GB RAM and 16 GPU cores (an *AWS EC2 instance type m5ad.4xlarge*), it took 17.38 min to complete the same task. For 65 sequences, it took 11.38 hours on the stand-alone machine, and *3.06 hours* using the *AWS EC2 virtual machine*.
   
## Improvement suggestion:
1. Since we have built data quality index for every time series in our AWS database, the future developer can use DQI metric to define prediction resolution for each time series, instead of using an universal day-by-day time frame. This customized forecast resolution feature could reduce uncertainty due to interpolation and reduce runtime and improve overall accuracy. 
   
2. For time series where a large data gap exists, the interpolation method can result in a flat line, and the search method will favor dampening this flat trend. Since we apply a sliding window, and select best parameters based on smallest error over all sliding steps, this data gap effect could have been largely reduced. At this point, the data gap effect is still not clear and need further investigation.
   
3. Random search and Bayesian Optimization could reduce the computation time, and are best-suited for optimization over continuous domains. These search methods worth exploring, keeping in mind of the discrete nature of the Holter-Winters model parameter domains (except for seasonal period _p_, which can be set as continuous integers). 
        
### Reference:

1. [Statistical and Machine Learning forecasting methods: Concerns and ways forward](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0194889)

2. [The M4 Competition: Results, findings, conclusion and way forward](https://www.sciencedirect.com/science/article/abs/pii/S0169207018300785)
