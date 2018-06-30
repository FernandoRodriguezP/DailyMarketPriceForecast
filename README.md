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
To obtain the information from ESIOS, it is necessary to execute the Jupyter Notebook file *"Get_Indicators.ipynb"*, which is available in *"/DailyMarketPriceForecast/ESIOS/”*. Once the file is executed, four different *“.csv”* files will be generated (one for each indicator) in the path *"/DailyMarketPriceForecast/ESIOS/Data/"*. The files obtained have a similar structure, formed by two fields:


## Technologies
The technologies used to develop this model are:<br>
* Python: Programming language used for Preprocessing, Data Analysis and Modeling.
  * Jupyter Notebook: Used for Preprocessing.
  * Spyder IDE: Used in the ESIOS module.
  * Libraries ->
* Tableau: Software that produces interactive data visualization, used in the Data Visualitation phase.

## Data acquisition
The most important factors that affect the Market Price are:
* *Electricity demand*: The total energy consumption for all purposes. The larger the electricity demand, the higher the Market Price will be.
* *Renewable energy generation*: Renewable energy have priority in the power grid and therefore always come first in the merit order. The larger of renewable energy generation, the lower the Market Price will be.
* *Date and time*: The Market Price will be increased in summer and winter, in addition peak values usually take place in the early morning and late afternoon.

There are two different data sources.<br>
The module named `/ESIOS` contains the necessary `.py` files to acquire the data from Red Eléctrica de España (REE).<br>
The module named `/Bank_Holidays` contains a `.csv` file with all the bank holidays in Spain from 01/01/2014 to 01/01/2017.<br>

## Data Preprocessing
