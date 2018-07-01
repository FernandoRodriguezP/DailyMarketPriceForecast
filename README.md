# Daily Market Price Forecast
## 1. Summary
This Master’s degree final project is a study about the forecast of the Electricity Market Price.<br>

Electricity prices in Europe are set daily (every day of the year) at 12 noon, for the twenty-four hours of the following day, in what is referred to as the Daily Market. The price and volume of energy over a specific hour are determined by the point at which the supply and demand curves meet, according to the marginal pricing model adopted by the EU. The complete information about the Electricity Market is available [here](http://www.omie.es/inicio/mercados-y-productos/mercado-electricidad/nuestros-mercados-de-electricidad).<br>

When the Daily Market Price is set, some of the factors that most affect are:
* *Electricity demand*: The total energy consumption for all purposes. The larger the electricity demand, the higher the Market Price will be.
* *Renewable energy generation*: Renewable energy have priority in the power grid and therefore always come first in the merit order. The larger of renewable energy generation, the lower the Market Price will be.
* *Date and time*: The Market Price will be increased in summer and winter, in addition peak values usually take place in the early morning and late afternoon.

### 1.1. Objective
The objective of this repository is to develop a Machine Learning model that allows to predict the Daily Market Price for 48 hours Forecast Horizon.<br>

For this, it will be necessary to obtain the information considered relevant for the development of the model.<br>
Once this information is available, it will be necessary to pre-process it, with the objective of adapting it to the requirements of the model.<br>
Next, a Machine Learning model based on the Random Forest Regressor algorithm will be developed.<br>
Finally, the project will have a visualization chapter developed with Tableu with which the results of the developed model can be seen graphically. 

## 2. Data acquisition
To develop this model, two sources were mainly used.

### 2.1. Bank holidays
On the one hand, is necessary information about the holidays that takes place in Spain, for this is used a file called *“Bank_Holidays.csv”* that contains the information of the existing holidays in the last 4 years (since January 1, 2014 from December 31, 2017).<br>

The file *"Bank_Holidays.csv"* can be found in *"/DailyMarketPriceForecast/Bank_Holidays/Data/"* and consists of the following variables:<br>

| Variable | Description |
| --- | --- |
| *Date* | *Date in format dd/mm/yyyy* |
| *Day* | *Weekday (Monday to Sunday)* |
| *Title* | *Name of the bank holiday* |
| *Bank Holiday* | *True (1) or False (0)* |

The complete dataset is made up of 4 variables and 55 samples, one for each bank holiday that took place during that period.

### 2.2. ESIOS
On the other hand, to access the information of REE, it is done through an API REST service to ESIOS (System Operator Information System). By using this service, you can download all the information in the system. The official documentation of the ESIOS API REST is available [here](https://api.esios.ree.es/).<br>

To access the system, it is necessary to request a token by email. Information about how to order the token is available [here](https://www.esios.ree.es/es/pagina/api).<br>

The indicators available in ESIOS can be found in the Excel file called "indicators.xlsx". This file consists of two fields:<br>

| Variable | Description |
| --- | --- |
| *Name* | *Name of the indicator as it appears on the ESIOS website* |
| *Indicator* | *Number associated with the name of the indicator used to download the indicator information* |

As seen in the previous section, the parameters that have most influence in the Daily Market Price are:<br>

| Indicator name | Indicator number |
| --- | --- |
| *Previsión diaria de la demanda eléctrica peninsular* | *Id: 460* |
| *Previsión de la producción eólica nacional peninsular* | *Id: 541* |
| *Generación prevista Solar* | *Id: 542* |
| *Precio medio horario componente mercado diario* | *Id: 805* |

In this way, information can be downloaded through the REST API service. Since the Electric Market Price is a value that is updated hourly, to develop the model it’s necessary that the rest of the parameters maintain the same scale, so the script is configured to perform the data download of the indicators previously mentioned with hourly frequency.<br>

To obtain the information from ESIOS, it is necessary to execute the Jupyter Notebook file *"Get_Indicators.ipynb"*, which is available in *"/DailyMarketPriceForecast/ESIOS/”*. Once the file is executed, four different *“.csv”* files will be generated (one for each indicator) in the path *"/DailyMarketPriceForecast/ESIOS/Data/"*.<br> 

The files obtained have a similar structure, formed by two fields:

| Indicator date | Indicator value |
| --- | --- |
| *Timestamp* | *Value* |

The complete dataset is made up of 2 variables and 35065 samples.

## 3. Preprocessing
It is necessary to make some transformations in the datasets in order to implement the desired Machine Learning model.<br>

As the dataset related to bank holidays has daily frequency, the information must be converted to hourly frequency in order to make a merge with the rest of the datasets. In addition, four series related to the dates (*"Hora"*, *"Dia"*, *"Mes"*, and *"Dia semana"*) will be created to analyze the contribution of these variables to the model.<br>

On the other hand, the ESIOS information comes from different files, so it must be combined in a single DataFrame whose variables will be the date in hourly format and also each of the indicators used in ESIOS.<br>

Once the preprocessing of the data is finished, it must be analyzed if there are outliers in the dataset. It is observed that there are only outliers in two series of the complete DataFrame (0.97% and 4.45% with respect to the total number of samples in each serie, respectively).<br>

A Machine Learning model based on the Random Forest algorithm will be developed, it should not be forgotten that this model is very robust in the presence of outliers because the model isolates them in small regions of the feature space. Then, since the prediction for each leaf is the average (in the case of regression), being isolated in separate leaves, outliers won't influence the rest of the predictions (in the case of regression for instance, they would not impact the mean of the other leaves).<br>

Despite the presence of a small number of outliers, it is decided not to act on them because it is considered that these samples have a normal behavior within the dataset.

## 4. Modelling
For the development of the model, a Random Forest Regressor algorithm has been implemented.<br>

The Random Forest algorithm is an ensemble of Decision Trees, trained with the “bagging” method, which means that the process of finding the root node and splitting the feature nodes will run randomly.<br>

The Random Forest algorithm is considered one of the most effective machine learning models for predictive. Some of it most important features are:
*	It runs efficiently on large data bases.
*	It can handle tabular data with numerical features, or categorical features with fewer than hundreds of categories. 
*	It gives estimates of what variables are important.
*	It generates an internal unbiased estimate of the generalization error as the forest building progresses.
*	It has an effective method for estimating missing data and maintains accuracy when a large proportion of the data are missing.<br>

To train the model, the dataset has been divided into 80% train and 20% test.

### 4.1. Hyperparameter tuning
Once the model is decided, before training the model it is necessary to obtain the best parameters of the Random Forest Regressor model with a hyperparameter tuning.<br>

The Randomized Search method has been chosen. With these method, the parameters of the estimator are optimized by cross-validated search over parameter settings. In contrast to GridSearchCV, not all parameter values are tried out, but rather a fixed number of parameter settings is sampled from the specified distributions.

### 4.2. Model evaluation
Once the hyperparameter tuning is done, the model is trained with the best parameters of the Random Forest Regressor, and it is tested with the test dataset (20% of total samples) to obtain the results of the prediction.

## 5. Visualization

## 6. How to Run
**6.1. Processing Bank Holidays data**<br>
> Execute "Bank_Holidays.ipynb"<br>
`$ jupyter notebook "./Bank_Holidays/Bank_Holidays.ipynb"`

**6.2. Getting ESIOS data**<br>
> Execute "Get_Indicators.ipynb"<br>
`$ jupyter notebook "./ESIOS/Get_Indicators.ipynb"`

**6.3. Preproccesing and Modelling**<br>
> Execute "Preprocessing_and_Modelling.ipynb"<br>
`$ jupyter notebook "./Model/Preprocessing_and_Modelling.ipynb"`

**6.4. Visualization**

## 7. About the technology
The technologies used to develop this project are:<br>

* Python: Programming language used for Preprocessing, Data Analysis and Modelling. During the project, two applications have been used to work with Python:
  * Jupyter Notebook: Application used for Preprocessing and Modelling.
  * Spyder IDE: Application mainly used in ESIOS module to import the libraries that allow extracting data from ESIOS API REST.
* Tableau: Software that produces interactive data visualization, used in the Data Visualitation module.

The main libraries used in this project are:<br>

* `numpy`
* `pandas`
* `seaborn`
* `sklearn`

## 8. About the autor
**Fernando Rodríguez Paler**
* https://www.linkedin.com/in/fernandorodriguezpaler

