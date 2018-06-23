# Daily Market Price Forecast
## Summary
This Master’s degree final project is a study about the forecast of the Electricity Market Price.<br>
Electricity prices in Europe are set on a daily basis (every day of the year) at 12 noon, for the twenty-four hours of the following day, in what is refered to as the Daily Market. The price and volume of energy over a specific hour are determined by the point at which the supply and demand curves meet, according to the marginal pricing model adopted by the EU. The complete information about the Electricity Market is available [here](http://www.omie.es/inicio/mercados-y-productos/mercado-electricidad/nuestros-mercados-de-electricidad).<br>

#### Aim
The aim of this repository is to develop a Machine Learning model to predict the Daily Market Price for 48 hours Forecast Horizon.

## Technologies
The technologies used to develop this model are:<br>
* Python: Programming language used for Preprocessing, Data Analysis and Modeling.
  * Jupyter Notebook: Used for Preprocessing.
  * Spyder IDE: Used in the ESIOS module.
* Tableau: Software that produces interactive data visualization, used in the Data Visualitation phase.

## Data acquisition
There are two different data sources.<br>
The module named `/Bank_Holidays` contains a .csv file with all the bank holidays in Spain from 01/01/2014 to 01/01/2017.<br>
The module named `/ESIOS` contains the necessary .py files to extract
