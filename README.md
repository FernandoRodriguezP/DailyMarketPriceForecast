# Daily Market Price Forecast
## 1.Summary
This Master’s degree final project is a study about the forecast of the Electricity Market Price.<br>
Electricity prices in Europe are set daily (every day of the year) at 12 noon, for the twenty-four hours of the following day, in what is referred to as the Daily Market. The price and volume of energy over a specific hour are determined by the point at which the supply and demand curves meet, according to the marginal pricing model adopted by the EU. The complete information about the Electricity Market is available [here](http://www.omie.es/inicio/mercados-y-productos/mercado-electricidad/nuestros-mercados-de-electricidad).<br>
When the Daily Market Price is set, some of the factors that most affect are:
* *Electricity demand*: The total energy consumption for all purposes. The larger the electricity demand, the higher the Market Price will be.
* *Renewable energy generation*: Renewable energy have priority in the power grid and therefore always come first in the merit order. The larger of renewable energy generation, the lower the Market Price will be.
* *Date and time*: The Market Price will be increased in summer and winter, in addition peak values usually take place in the early morning and late afternoon.

### 1.1 Objective
The objective of this repository is to develop a Machine Learning model that allows to predict the Daily Market Price for 48 hours Forecast Horizon.<br>
For this, it will be necessary to obtain the information considered relevant for the development of the model.<br>
Once this information is available, it will be necessary to pre-process it, with the objective of adapting it to the requirements of the model.<br>
Next, a Machine Learning model based on the Random Forest Regressor algorithm will be developed.<br>
Finally, the project will have a visualization chapter developed with Tableu with which the results of the developed model can be seen graphically. 

## Data acquisition
To develop this model, two sources were mainly used.

### 2.1 Bank holidays
On the one hand, is necessary information about the holidays that takes place in Spain, for this is used a file called *“Bank_Holidays.csv”* that contains the information of the existing holidays in the last 4 years (since January 1, 2014 from December 31, 2017).<br>
The file *"Bank_Holidays.csv"* can be found in *"/DailyMarketPriceForecast/Bank_Holidays/Data/"* and consists of the following variables:
Variable     | Description


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
