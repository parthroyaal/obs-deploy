# obs-knowledBas

# Dev flow- how prjshould be coded from stratch  / systemDesignFlow Need memory dynniamc

# Project 's program flow

# Should be self contaitn to make use of dynamic memory more  meanign  complte pure fucntion modular also have main fucntion to map flows in app and in docs use   points  and ** subpoints\*\*

* app initi and sock init 
* 

````python
from flask import Flask, render_template_string, json

from flask_sock import Sock

  

app = Flask(__name__)

sock = Sock(app)
````

* routes  active \[/, /hist, /ws\]
* 

````python
  

###################################

@sock.route('/realtime')

def realtime(ws):

    with LOCK:

        CONNECTIONS.add(ws)

    try:

        while True:

            ws.receive()  # Keep connection open

    finally:

        with LOCK:

            CONNECTIONS.remove(ws)

  

###################################

@app.route('/historic')

def historic():

    # Serve complete candle history: initial candles + updates processed during live ticks

    return json.dumps(CANDLES)

  
  

###################################

# Minimal HTML for testing - unchanged for brevity, but uses candle updates on websocket

index_html = """

<!DOCTYPE html>

<html>

<head>

    <title>Real-Time Candlestick Chart</title>

    <script src="https://cdn.jsdelivr.net/npm/lightweight-charts@3.8.0/dist/lightweight-charts.standalone.production.js"></script>

</head>

<body>

    <div id="chart"></div>

    <script>

        const chart = LightweightCharts.createChart(document.getElementById('chart'), {

            width: window.innerWidth,

            height: window.innerHeight,

            timeScale: { timeVisible: true, secondsVisible: false }

        });

        const candleSeries = chart.addCandlestickSeries();

  

        fetch('/historic')

            .then(response => response.json())

            .then(data => {

                candleSeries.setData(data);

            });

  

        const ws = new WebSocket(`ws://${window.location.host}/realtime`);

        ws.onmessage = function(event) {

            const msg = JSON.parse(event.data);

            // Only update using the latest aggregated candle information

            const candle = msg.candle;

            candleSeries.update(candle);

        };

    </script>

</body>

</html>

"""

@app.route('/')

def index():

    return render_template_string(index_html)
````

* process start in seprate nMainflask thread of emulated websocket cleint receving tick data and on_message loop acess 
* basicall the background process of in case of TestApp only : tick_gen_thread 
* And the all apps Test, Bk,  liv candle_update_thread start 
* 

````python
import threading


def main()

    # Start the tick generator & candle update threads separately

    threading.Thread(target=tick_generator_thread, daemon=True).start()
    
    threading.Thread(target=candle_update_thread, daemon=True).start()
````

````
* tick_generator_thread

* candle_update_thread
````

````python
# Thread responsible for processing ticks and updating candles

def candle_update_thread():

    while True:

        time.sleep(1)  # Adjust interval as needed

        with LOCK:

            if LIVE_TICKS:

                for tick in LIVE_TICKS:

                    updated_candle = update_candle_with_tick(tick)

                    message = json.dumps({'candle': updated_candle})

                    # Broadcast the updated candle to all connected websocket clients

                    for ws in CONNECTIONS.copy():

                        try:

                            ws.send(message)

                        except:

                            CONNECTIONS.remove(ws)

                LIVE_TICKS.clear()
````

````

```python
# Description of fucntion to understand what it passed as args and what it returns

"""

This function takes in  num_rows and start_index as arguments and returns a list of mock ticks.

Input arg1: example : now_rows = 10 type: int

Input arg2: example : start_index = 0 type: int

Retun value exmaple: return ticks =  [{...}, {...}, ...] type: list

"""
current_price = None

def generate_mock_ticks(num_rows, start_index=0):

    global current_price

    base_time = int(time.time()) - 172800  # 2 days ago

  

    if current_price is None:

        current_price = 100

  

    ticks = []

    for i in range(num_rows):

        delta = random.uniform(-0.5, 0.5)

        current_price += delta

        timestamp = base_time + start_index + i

        ticks.append({

            "timestamp": timestamp,

            "value": round(current_price, 2)

        })

    return ticks
    
    ```


# To maintain state in bk need this 

```python
def create_candle_from_tick(tick):

    return {

        'time': tick['timestamp'] - (tick['timestamp'] % 60),

        'open': tick['value'],

        'high': tick['value'],

        'low': tick['value'],

        'close': tick['value']

    }
````

# fn doc:

fetch lines from filename=abc.txt by opening file in read mode using r flag of open funciton of python keyowrd as contxt manager and appending current  iteration  in loop   read to list list_of_lisnes and return this list  : but this keep all lins in memry not read one by one in memory 

````python
def fetch_lines(filename):
	with  open(filename, 'r') as file:
		list_of_lines = []
		for i in file:
			list_of_lines.append(i)
		return list_of_lines

output = fetch_lines('abc.txt')
print(output)
````

````python
def fetch_lines(filename):
	with  open(filename, 'r') as file:
		for i in file:
			list_of_lines.append(i)
		return list_of_lines

output = fetch_lines('abc.txt')
print(output)
````

````python
````

[mar17](mar17.md)
