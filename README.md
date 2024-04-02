
# Enigma-Securities-Internship-Exercise

Python File Name: Enigma Securities Internship Exercise.ipynb

This Python script extracts real-time ticker data from the Bitfinex cryptocurrency exchange's WebSocket API and saves it to a CSV file. It allows users to pick their preferred cryptocurrency symbol and collects information such as bid and ask prices, trade volume, and many more metrics.

 Metrics including their Datatypes:
 
"timestamp": yyyy-mm-dd ,24 hour clock time,

"instrument" : Strings,

 "type": Strings,
 
"level_quantity": Integer ,

"avg_bid" : Float,

"avg_ask": Float,

"num_trades": Integer,

"total_volume" : Float,

"num_quotes" : Integer,

"avg_volume": Float,

"avg_spread": Float




Libraries used for this project:

import websocket

import json

import time

import csv


Installation of Libraries:

Install the required dependencies 
eg: pip install websocket-client


Ticker Information:

-BTCUSD: SPOT

-ETHUSD: SPOT

-BTCF0USTF0: PERPETUAL

-ETHF0USTF0: PERPETUAL


Code Details: 
1) Defining Websocket endpoint and Symbols:
   
   -socket = "wss://api-pub.bitfinex.com/ws/2"  : This is the WebSocket endpoint that we will connect and use it to recieve real time data.
   
   -symbols = {"BTCUSD": "tBTCUSD", "ETHUSD": "tETHUSD", "BTCF0USTF0": "BTCF0:USTF0", "ETHF0USTF0": "ETHF0:USTF0"} : The disctionary that is defied in the code maps the symbols "BTCUSD" to Symbols used in the api 
   like "tBTCUSD". This helps us to subscribe the correct data from the api.
   
2) Getting User Input for Symbol: This line takes in the symbol as an input variable to extract the correct data.
  
3)  Creating CSV File Path:
    csv_file_path = f'{symbol}.csv' : This line generates a file path for the CSV file depending on the user input symbol. For example, if the user types "BTCUSD," the file path will be "BTCUSD.csv"
    
4) Initializing Counters:
   num_trades, total_volume, num_quotes : These variables are initialized as 0 initially to keep track of the number of trades, total trade volume, and the number of quotes received and their count increases as      we increase as level quantity or keep running the real time json respone.
   
5) Processing Incoming Messages:
   - def on_message (ws, message): This function is called whenever a message is received from the WebSocket. It processes the message and extracts relevant data such as bid and ask prices, trade volume, etc.

   - Processing Ticker Data:

      - isinstance(ticker_data, list) and len(ticker_data) == 10 : This condition checks whether the ticker_data is a list with a length of 10. This is to check that the ticker data is complete and contains all           of the required information.

      - Calculating Bid and Ask Prices: Calculating bid and ask prices for different level quantities (1, 10, 25) using the extracted bid and ask prices from ticker_data

      - Calculating Averages: avg_bid and avg_ask : Dictionaries are created by calculating the average bid and ask prices for each level quantity based on the collected prices.

      - avg_spread  : The dictionary is created to contain the average spread (avg_ask - avg_bid) for each level amount.

      - Calculating Average Volume: avg_volume : total_volume / num_trades: Calculating the average volume based on the total volume and the number of trades

      - The indexation done throughout to extract data is done on the basis of predefined json format given by the bitfinex api in their documention, it reads the follows:
      
         #Example json format for btcusd:
         
         [CHANNEL_ID,[BID,BID_SIZE,ASK,ASK_SIZE,DAILY_CHANGE,DAILY_CHANGE_RELATIVE,LAST_PRICE,VOLUME,HIGH,LOW]]
         
         [261802,[70671,5.08870007,70672,2.95272452,647,0.00924088,70662,494.26904237,71471,69953]]
         
         In the above format, in order to extract the bid prices we would do double indexing eg:
         
         assume B = [261802,[70671,5.08870007,70672,2.95272452,647,0.00924088,70662,494.26904237,71471,69953]]
         
         then Bid_prices = B[1][0]
         
         output = 70671
         
 6) Creating Response JSON and Writing to CSV:
     
    - The response structure for spot and perpetual kinds differs according on the symbol type ("BTCUSD" or "ETHUSD"). For each level quantity (1, 10, 25), a response dictionary is constructed that includes the date, instrument, type, level amount, averages, and other calculated information mentioned above.
   
    - Once the Json respone is printed, the response is then writen to csv file using the "with open" function for further analysis.
     
 7) Sample Json Response for BTCUSD spot data:
    
      {
    "timestamp": "2024-04-01 17:21:08",
    
    "instrument": "BTCUSD",
    
    "type": "Spot",
    
    "level_quantity": 1,
    
    "avg_bid": 69810.9,
    
    "avg_ask": 69812.1,
    
    "num_trades": 1,
    
    "total_volume": 2647.69819543,
    
    "num_quotes": 1,
    
    "avg_volume": 2647.69819543,
    
    "avg_spread [1, 10, 25]": 1.2000000000116415
}

 
 
 - Sperate configuration File not required as configurations are already defined in the code
