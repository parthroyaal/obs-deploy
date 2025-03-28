```python
from flask import Flask, render_template_string
from flask_sock import Sock
import json
from datetime import datetime
from collections import deque
import pandas as pd
import threading

app = Flask(__name__)
sock = Sock(app)

# Global deque to store tick data for processing
DEQUE_MAXLEN = 5000
tick_data = deque(maxlen=DEQUE_MAXLEN)  # Adjust maxlen as needed


import logging


# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s [%(levelname)s] %(message)s',
    datefmt='%Y-%m-%d %H:%M:%S'
)


# Log initial state of the deque
logging.info(f"Initial tick_data: {list(tick_data)}")


def ws_client_connect():
    """
    Connect to Bybit WebSocket and populate the tick_data deque.
    """
    import websocket
    ws_url = "wss://stream.bybit.com/v5/public/linear"
    append_counter = 0  # Now a local variable inside ws_client_connect()

    def on_message(ws, message):
        nonlocal append_counter  # Refers to the variable inside ws_client_connect
        tick = json.loads(message)
        if tick.get("topic") == "publicTrade.BTCUSDT":
            trade_data = tick["data"][0]
            tick_time = datetime.fromtimestamp(trade_data["T"] / 1000)
            price = float(trade_data["p"])
            tick_data.append({'timestamp': tick_time, 'price': price})
            append_counter += 1

            logging.info(
                "Append #%d: deque size = %d\nDeque contents:\n%s\n",
                append_counter, 
                len(tick_data), 
                json.dumps(list(tick_data)[-5:], indent=4, default=str)
                )

   
            # Optionally, log the contents of the deque when it is full
            if len(tick_data) == tick_data.maxlen:
                logging.info("Deque reached maximum capacity. Current data: %s", list(tick_data))
                # flush the deque and also save the data to a file csv usind concatination

                # save the data to a file csv 
                df = pd.DataFrame(list(tick_data))



                # opt2
                df.to_csv('tick_data.csv', mode='a', header=False, index=False)
                



                # clear the deque
                tick_data.clear()



    def on_open(ws):
        subscribe_message = {
            "op": "subscribe",
            "args": ["publicTrade.BTCUSDT"]
        }
        ws.send(json.dumps(subscribe_message))
        logging.info("WebSocket connection opened and subscription message sent.")

    ws = websocket.WebSocketApp(
        ws_url,
        on_message=on_message
    )
    ws.on_open = on_open
    ws.run_forever()




# Start WebSocket client in a separate thread
threading.Thread(target=ws_client_connect, daemon=True).start()



def tick_to_candle(tick_data_deque, timeframe='1min'):
    """
    Converts tick data to OHLC candlestick data based on a given timeframe.
    """
    df = pd.DataFrame(list(tick_data_deque), columns=['timestamp', 'price'])
    df['timestamp'] = pd.to_datetime(df['timestamp'])
    ohlc = df.resample(timeframe, on='timestamp').agg({
        'price': ['first', 'max', 'min', 'last']
    })
    ohlc.columns = ['Open', 'High', 'Low', 'Close']
    return ohlc.dropna().reset_index()


# WebSocket route to push candlestick data to the client
@sock.route('/ws')
def push_candlestick_data(ws):
    while True:
        if len(tick_data) > 0:
            ohlc_df = tick_to_candle(tick_data)
            if not ohlc_df.empty:
                # Send the latest candlestick data to the client
                ws.send(ohlc_df.to_json(orient='records'))

# Serve the frontend
@app.route('/')
def index():
    html = """
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Live BTC/USDT Chart</title>
    <script src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/lightweight-charts.standalone.production.js"></script>
    </head>
    <body>
        <h1>Live BTC/USDT Candlestick Chart MknLtp</h1>
        <div id="chart" style="width: 100%; height: 500px;"></div>
        <script>
            const chart = LightweightCharts.createChart(document.getElementById('chart'), {
                width: window.innerWidth,
                height: window.innerHeight,
                priceScale: { borderColor: '#cccccc' },
                timeScale: { borderColor: '#cccccc',  timeVisible: true, secondsVisible: true, },
            });
            const candleSeries = chart.addCandlestickSeries();

            // Connect to the WebSocket
            const ws = new WebSocket('ws://' + location.host + '/ws');
            ws.onmessage = function(event) {
                const data = JSON.parse(event.data);
                const formattedData = data.map(candle => ({
                    time: new Date(candle.timestamp).getTime() / 1000,
                    open: candle.Open,
                    high: candle.High,
                    low: candle.Low,
                    close: candle.Close
                }));
                candleSeries.setData(formattedData); // Update the chart
                console.log(formattedData);
            };
        </script>
    </body>
    </html>
    """
    return render_template_string(html)

if __name__ == '__main__':
    app.run()
# wscat -c ws://127.0.0.1:5000/ws
```