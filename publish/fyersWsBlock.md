
# PreWsAuth:
[[fyersAuth]]


# inMemoryDb: list


```python
from fyers_apiv3.FyersWebsocket import data_ws

data = []

def onmessage(message):
    global data
    try:
        print(f"Received message: {message}")
        data.append(message)
        print(f"Data length: {len(data)}")
    except Exception as e:
        print(f"Error in onmessage: {e}")


def onerror(message):
    """
    Callback function to handle WebSocket errors.

    Parameters:
        message (dict): The error message received from the WebSocket.


    """
    print("Error:", message)


def onclose(message):
    """
    Callback function to handle WebSocket connection close events.
    """
    print("Connection closed:", message)


def onopen():
    """
    Callback function to subscribe to data type and symbols upon WebSocket connection.

    """
    # Specify the data type and symbols you want to subscribe to
    data_type = "SymbolUpdate"
    # data_type = "DepthUpdate"


    # Subscribe to the specified symbols and data type
    symbols = ['MCX:CRUDEOIL25JAN5900CE']
    fyers.subscribe(symbols=symbols, data_type=data_type)

    # Keep the socket running to receive real-time data
    fyers.keep_running()

atx = "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJhcGkuZnllcnMuaW4iLCJpYXQiOjE3MzU4MTMxNjUsImV4cCI6MTczNTg2NDIyNSwibmJmIjoxNzM1ODEzMTY1LCJhdWQiOlsieDowIiwieDoxIiwiZDoxIiwiZDoyIiwieDowIl0sInN1YiI6ImFjY2Vzc190b2tlbiIsImF0X2hhc2giOiJnQUFBQUFCbmRtZ3RvT2N3QWlEYnZhZW44Q1BNM2dBWUlQSzdMUjJtVjhJenF1S09xYXRwVnJyVTBxMGtrSFBqWWxnOUhweU9VMHlITUlnRGpZSzBDc2JxLVRrLWFtLWtRT1ZKbGI4V0h2WHNXQlJxNnR1MjFPcz0iLCJkaXNwbGF5X25hbWUiOiJQQVJUSCBST1lBTCIsIm9tcyI6IksxIiwiaHNtX2tleSI6ImYyOTA3Njg3N2YwMTQ2M2YzY2RlODNiZDI1N2Q4ODcwYWY3Nzc3YTE3N2JiOTRkNDc4OTczZWRmIiwiZnlfaWQiOiJYUDEyMzI1IiwiYXBwVHlwZSI6MTAwLCJwb2FfZmxhZyI6Ik4ifQ.2Jv0UcQzVxjH-u56eWgaO5lRAhn9bLQfu0gQkw76R5M"

# Replace the sample access token with your actual access token obtained from Fyers
access_token = atx



```

```python
# Create a FyersDataSocket instance with the provided parameters
fyers = data_ws.FyersDataSocket(
    access_token=access_token,       # Access token in the format "appid:accesstoken"
    log_path="",                     # Path to save logs. Leave empty to auto-create logs in the current directory.
    litemode=False,                  # Lite mode disabled. Set to True if you want a lite response.
    write_to_file=False,              # Save response in a log file instead of printing it.
    reconnect=True,                  # Enable auto-reconnection to WebSocket on disconnection.
    on_connect=onopen,               # Callback function to subscribe to data upon connection.
    on_close=onclose,                # Callback function to handle WebSocket connection close events.
    on_error=onerror,                # Callback function to handle WebSocket errors.
    on_message=onmessage             # Callback function to handle incoming messages from the WebSocket.
)

# Establish a connection to the Fyers WebSocket
fyers.connect()

```

```python

import time 
# Iterate through the `data` list\
for item in data:
    print(item)  # Or replace with any other processing logic
    time.sleep(2)

```

## out 

```json
{'type': 'cn', 'code': 200, 'message': 'Authentication done', 's': 'ok'}
{'type': 'ful', 'code': 200, 'message': 'Full Mode On', 's': 'ok'}
{'type': 'sub', 'code': 200, 'message': 'Subscribed', 's': 'ok'}
{'ltp': 373.8, 'vol_traded_today': 1385, 'last_traded_time': 1735814785, 'exch_feed_time': 1735814810, 'bid_size': 1, 'ask_size': 1, 'bid_price': 373.8, 'ask_price': 375.1, 'last_traded_qty': 1, 'tot_buy_qty': 158, 'tot_sell_qty': 141, 'avg_trade_price': 341.07, 'low_price': 311.0, 'high_price': 381.1, 'lower_ckt': 0, 'upper_ckt': 0, 'open_price': 333.3, 'prev_close_price': 317.4, 'type': 'sf', 'symbol': 'MCX:CRUDEOIL25JAN5900CE', 'ch': 56.4, 'chp': 17.7694}
{'ltp': 373.8, 'vol_traded_today': 1385, 'last_traded_time': 1735814785, 'exch_feed_time': 1735814812, 'bid_size': 1, 'ask_size': 1, 'bid_price': 373.9, 'ask_price': 375.1, 'last_traded_qty': 1, 'tot_buy_qty': 160, 'tot_sell_qty': 141, 'avg_trade_price': 341.07, 'low_price': 311.0, 'high_price': 381.1, 'lower_ckt': 0, 'upper_ckt': 0, 'open_price': 333.3, 'prev_close_price': 317.4, 'type': 'sf', 'symbol': 'MCX:CRUDEOIL25JAN5900CE', 'ch': 56.4, 'chp': 17.7694}
{'ltp': 373.8, 'vol_traded_today': 1385, 'last_traded_time': 1735814785, 'exch_feed_time': 1735814812, 'bid_size': 1, 'ask_size': 1, 'bid_price': 374.1, 'ask_price': 375.3, 'last_traded_qty': 1, 'tot_buy_qty': 162, 'tot_sell_qty': 138, 'avg_trade_price': 341.07, 'low_price': 311.0, 'high_price': 381.1, 'lower_ckt': 0, 'upper_ckt': 0, 'open_price': 333.3, 'prev_close_price': 317.4, 'type': 'sf', 'symbol': 'MCX:CRUDEOIL25JAN5900CE', 'ch': 56.4, 'chp': 17.7694}
{'ltp': 373.8, 'vol_traded_today': 1385, 'last_traded_time': 1735814785, 'exch_feed_time': 1735814814, 'bid_size': 1, 'ask_size': 1, 'bid_price': 374.6, 'ask_price': 375.7, 'last_traded_qty': 1, 'tot_buy_qty': 165, 'tot_sell_qty': 133, 'avg_trade_price': 341.07, 'low_price': 311.0, 'high_price': 381.1, 'lower_ckt': 0, 'upper_ckt': 0, 'open_price': 333.3, 'prev_close_price': 317.4, 'type': 'sf', 'symbol': 'MCX:CRUDEOIL25JAN5900CE', 'ch': 56.4, 'chp': 17.7694}
{'ltp': 373.8, 'vol_traded_today': 1385, 'last_traded_time': 1735814785, 'exch_feed_time': 1735814817, 'bid_size': 1, 'ask_size': 1, 'bid_price': 374.5, 'ask_price': 375.8, 'last_traded_qty': 1, 'tot_buy_qty': 159, 'tot_sell_qty': 141, 'avg_trade_price': 341.07, 'low_price': 311.0, 'high_price': 381.1, 'lower_ckt': 0, 'upper_ckt': 0, 'open_price': 333.3, 'prev_close_price': 317.4, 'type': 'sf', 'symbol': 'MCX:CRUDEOIL25JAN5900CE', 'ch': 56.4, 'chp': 17.7694}
{'ltp': 373.8, 'vol_traded_today': 1385, 'last_traded_time': 1735814785, 'exch_feed_time': 1735814818, 'bid_size': 2, 'ask_size': 1, 'bid_price': 374.0, 'ask_price': 375.4, 'last_traded_qty': 1, 'tot_buy_qty': 160, 'tot_sell_qty': 144, 'avg_trade_price': 341.07, 'low_price': 311.0, 'high_price': 381.1, 'lower_ckt': 0, 'upper_ckt': 0, 'open_price': 333.3, 'prev_close_price': 317.4, 'type': 'sf', 'symbol': 'MCX:CRUDEOIL25JAN5900CE', 'ch': 56.4, 'chp': 17.7694}
{'ltp': 373.8, 'vol_traded_today': 1385, 'last_traded_time': 1735814785, 'exch_feed_time': 1735814819, 'bid_size': 1, 'ask_size': 1, 'bid_price': 374.0, 'ask_price': 375.4, 'last_traded_qty': 1, 'tot_buy_qty': 160, 'tot_sell_qty': 144, 'avg_trade_price': 341.07, 'low_price': 311.0, 'high_price': 381.1, 'lower_ckt': 0, 'upper_ckt': 0, 'open_price': 333.3, 'prev_close_price': 317.4, 'type': 'sf', 'symbol': 'MCX:CRUDEOIL25JAN5900CE', 'ch': 56.4, 'chp': 17.7694}
{'ltp': 373.8, 'vol_traded_today': 1385, 'last_traded_time': 1735814785, 'exch_feed_time': 1735814822, 'bid_size': 1, 'ask_size': 1, 'bid_price': 374.0, 'ask_price': 375.4, 'last_traded_qty': 1, 'tot_buy_qty': 160, 'tot_sell_qty': 144, 'avg_trade_price': 341.07, 'low_price': 311.0, 'high_price': 381.1, 'lower_ckt': 0, 'upper_ckt': 0, 'open_price': 333.3, 'prev_close_price': 317.4, 'type': 'sf', 'symbol': 'MCX:CRUDEOIL25JAN5900CE', 'ch': 56.4, 'chp': 17.7694}
{'ltp': 373.8, 'vol_traded_today': 1385, 'last_traded_time': 1735814785, 'exch_feed_time': 1735814824, 'bid_size': 2, 'ask_size': 1, 'bid_price': 374.0, 'ask_price': 375.6, 'last_traded_qty': 1, 'tot_buy_qty': 160, 'tot_sell_qty': 142, 'avg_trade_price': 341.07, 'low_price': 311.0, 'high_price': 381.1, 'lower_ckt': 0, 'upper_ckt': 0, 'open_price': 333.3, 'prev_close_price': 317.4, 'type': 'sf', 'symbol': 'MCX:CRUDEOIL25JAN5900CE', 'ch': 56.4, 'chp': 17.7694}
```


# Saveit

```python
import json

  

# Define the file path where you want to save the data

file_path = 'data.json'

  

# Save the list of dictionaries to the file

with open(file_path, 'w') as json_file:

    json.dump(data, json_file, indent=4)  # Use indent for readable formatting

    print(f"Data saved to {file_path}")
    
```


