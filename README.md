# Beijing-Air-Quality
kettle do Beijing Air Quality

Environmental:
kette/PDI version 8.0 (64-bit) and Cpython Plugin
python 3.6 (64-bit)
Anaconda3 (64-bit) **if you don't like Anaconda you can do The following operation**

Suppose you have completed the python installation and set up the pip environment variable
ctrl+R or open you cmd
(maybe you will look the pip 9.0 don't worry,just pip install --upgrade pip or Complete the operation according to the hint)
**if you PC at the same time, there are python2 and python3 change the order  that look like 'py -3 -m pip install pandas'**
python -m pip install pandas
python -m pip install scipy
python -m pip install sklearn
python -m pip install numpy
python -m pip install matplotlib
python -m pip install tensorflow
python -m pip install keras


First clearning your data 
If your doesn’t have the test data you can download Beijing_demo_beijing.txt
open kettle or PDI
run 2017all_clearn.ktr and allhistory_clearn.ktr  **Remember to modify your file path**
you will get the clearn Air quality data

next run Apply_LSTM_with_data_cleansing.ktr **Remember to adjust the output parameters**
I use the 4 month hisory air quality data for run the ktr. It's forecast So2 in the next 5 days. So you can run more and more data

this is python code for apply LSTM forecast.
# python script
from pandas import DataFrame
from pandas import Series
from pandas import concat
from pandas import read_csv
from pandas import datetime
from sklearn.metrics import mean_squared_error
from sklearn.preprocessing import MinMaxScaler
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import LSTM
import numpy as np


from keras.models import model_from_json


X = df.values



# load json and create model
json_file = open("F:\work\Pentaho\pentaholab\model.json", 'r')
loaded_model_json = json_file.read()
json_file.close()
loaded_model = model_from_json(loaded_model_json)
# load weights into new model
loaded_model.load_weights("F:\work\Pentaho\pentaholab\model.h5")

# make a one-step forecast
def forecast_lstm(model, batch_size, X):
	X = X.reshape(1, 1, len(X))
	yhat = model.predict(X, batch_size=batch_size)
	return yhat[0,0]

predictions = list()
for i in range(len(X)):
	# make one-step forecast
	X1 = X[i, :]
	yhat = forecast_lstm(loaded_model, 1, X1)
	predictions.append(yhat)

df['prediction'] = np.array(predictions)

later I will upload a readme.doc
maybe a month later（smile^_^） 2018 4 27

