[fyersAuth](fyersAuth.md)

````python
atx = app.get_token()
at = atx


````

````python
from fyers_apiv3 import fyersModel

  

import pandas as pd

# Initialize the FyersModel instance with your client_id, access_token, and enable async mode

  

data = {

    "symbol":"NSE:SBIN-EQ",

    "resolution":"D",

    "date_format":"1",

    "range_from":"2024-04-04",

    "range_to":"2024-04-05",

    "cont_flag":"1"

}

  

res = fyers.history(data=data)

# res

  

df = pd.DataFrame(res['candles'], columns=['date', 'open', 'high', 'low', 'close', 'volume'])

df = df.sort_values(by = 'date')

df

  

#  ------------------------------------------------------------------------------------------------------------------------------------------

# Sample Success Response

# ------------------------------------------------------------------------------------------------------------------------------------------

# {

#   "candles":[

#       [

#         1690934400,

#         609.85,

#         610.5,

#         594.1,

#         598.45,

#         14977497

#       ],

#       [

#         1691020800,

#         598.7,

#         600.85,

#         585,

#         590.5,

#         27774877

#       ]

#   ],

#   "code":200,

#   "message":"",

#   "s":"ok"

# }
````

# Misc

````python

data = {

    "symbol":"NSE:NIFTY2491225000PE",

    "resolution": "5S" , #"D",

    "date_format":"1",

    "range_from": str((datetime.now()  - timedelta(days=6)).date()),   # "2024-09-06",

    "range_to": str(datetime.now().date()) ,  # "2024-10-12",

    "cont_flag":"1"

}




data = {

    "symbols":"NSE:SBIN-EQ,NSE:IDEA-EQ"

}

# response =

fyers.quotes(data=data)

# print(response)
````
