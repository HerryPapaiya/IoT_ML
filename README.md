# IoT_ML
Implementation of ML on IoT based data.

The industrial scope for the convergence of IoT and ML is wide and informative. IoT renders an enormous amount of data from various sensors. 
On the other hand, ML opens up insight hidden in the acquired data.

The implementation includes three major parts as described.

1. Internet of Things

IoT part includes,
Sensors – Measurement of multiple parameters can be done using primary sensing elements or electronic sensors. For Project, DHT11
An IoT device – It is used to establish a stable connection with the Internet. For Project, ESP8266(NodeMCU).

Data transmission from DHT11 to the ESP8266 board done in 5 bytes.
2 bytes for RH(Relative Humidity),                                                                
2 bytes for Temperature, and 
1 byte for checksum 

In addition, it can make a connection with any data logger for data-storage and data-analysis.(Google sheets using Google scripts) 

2. Data Logger

The main function of the data-logger is to store the acquired data. Moreover, it provides,
    • Graphical Representation,
    • On-field and Off-field monitoring,
    
Data-logger for the project is Google sheets which can be accessed from Google drive. Stable connection can be achieved between Google Sheets and 
ESP8266 using Google scripts code.

3. Machine Learning

The main aim of ML is to observe data and perceive upcoming events. The Random Forest algorithm can be used for both Classification and Regression problems 
in ML. ML has been implemented in Jupiter notebook in Python. 

It applies 75% of the data to the Training set and the remaining 25% of the data to the Test Set. Consequently, the decision tree comes up, 
and at the end prediction takes place with ±error and specific percent of accuracy.
