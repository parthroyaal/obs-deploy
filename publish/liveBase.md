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
tick_data = deque(maxlen=1000)  # Adjust maxlen as needed for buffering


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


def ws_client_connect():
    """
    Connect to Bybit WebSocket and populate the tick_data deque.
    """
    import websocket
    ws_url = "wss://stream.bybit.com/v5/public/linear"

    def on_message(ws, message):
        tick = json.loads(message)
        if tick.get("topic") == "publicTrade.BTCUSDT":
            trade_data = tick["data"][0]
            tick_time = datetime.fromtimestamp(trade_data["T"] / 1000)
            price = float(trade_data["p"])
            tick_data.append({'timestamp': tick_time, 'price': price})

    def on_open(ws):
        subscribe_message = {
            "op": "subscribe",
            "args": ["publicTrade.BTCUSDT"]
        }
        ws.send(json.dumps(subscribe_message))

    ws = websocket.WebSocketApp(
        ws_url,
        on_message=on_message
    )
    ws.on_open = on_open
    ws.run_forever()


# Start WebSocket client in a separate thread
threading.Thread(target=ws_client_connect, daemon=True).start()


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

    # Modularized templates

    js_html = """
    <script src="https://unpkg.com/lightweight-charts/dist/lightweight-charts.standalone.production.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', () => {
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
            };
        });
    </script>

    """

    main_template = """
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Live BTC/USDT Candlestick Chart</title>
        {{ js|safe }}
    </head>
    <body>
        <h1>Live BTC/USDT Candlestick Chart</h1>
        <div id="chart" style="width: 100%; height: 500px;"></div>
    </body>
    </html>
    """

    # Render the full HTML using modular components
    return render_template_string(main_template, js=js_html)

if __name__ == '__main__':
    app.run(debug=True)

```