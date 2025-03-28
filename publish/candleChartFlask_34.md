

```python
from flask import Flask, render_template_string
from flask_sock import Sock
from datetime import datetime, timedelta
import time
import json

app = Flask(__name__)
sock = Sock(app)

HTML_TEMPLATE = """
<html>
    <head></head>
    <body>
        <div id="tv_chart_container" style="width: 100%; height: 100vh;"></div>
        
        <script src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/lightweight-charts.standalone.production.js"></script>
        <script>
            let currentCandle = null;
            let lastCandleTime = null;
            let candleSeries;

            // Chart initialization
            const chart = LightweightCharts.createChart(document.getElementById('tv_chart_container'), {
                width: window.innerWidth,
                height: window.innerHeight,
                timeScale: {
                    timeVisible: true,
                    barSpacing: 25,
                }
            });
            
            candleSeries = chart.addCandlestickSeries();

            // Fetch initial historical candles
            fetch('/historic')
                .then(response => response.json())
                .then(data => {
                    candleSeries.setData(data);
                    if(data.length > 0) {
                        lastCandleTime = data[data.length - 1].time;
                    }
                });

            // WebSocket connection for real-time ticks
            const ws = new WebSocket(`ws://${window.location.host}/ws`);
            ws.onmessage = function(event) {
                const tick = JSON.parse(event.data);
                processTick(tick);
            };

            function processTick(tick) {
                const tickTime = tick.timestamp;
                const candleTime = tickTime - (tickTime % 60);

                if (!lastCandleTime || candleTime > lastCandleTime) {
                    if (currentCandle) {
                        candleSeries.update(currentCandle);
                        lastCandleTime = currentCandle.time;
                    }
                    currentCandle = {
                        time: candleTime,
                        open: tick.value,
                        high: tick.value,
                        low: tick.value,
                        close: tick.value
                    };
                } else {
                    currentCandle.high = Math.max(currentCandle.high, tick.value);
                    currentCandle.low = Math.min(currentCandle.low, tick.value);
                    currentCandle.close = tick.value;
                }
                candleSeries.update(currentCandle);
            }
        </script>
    </body>
</html>
"""

def generate_mock_ticks(num_rows, start_index=0):
    start_time = datetime(2024, 2, 27, 1, 0, 0)
    tick_value = 100.0
    
    for i in range(start_index, num_rows + start_index):
        yield {
            "timestamp": (start_time + timedelta(seconds=i)).timestamp(),
            "value": tick_value
        }
        tick_value *= 1.001  # 0.1% increase per tick

def create_initial_candles(ticks):
    candles = []
    current_candle = None
    
    for tick in ticks:
        candle_time = tick["timestamp"] - (tick["timestamp"] % 60)
        
        if not current_candle or candle_time > current_candle["time"]:
            if current_candle:
                candles.append(current_candle)
            current_candle = {
                "time": candle_time,
                "open": tick["value"],
                "high": tick["value"],
                "low": tick["value"],
                "close": tick["value"]
            }
        else:
            current_candle["high"] = max(current_candle["high"], tick["value"])
            current_candle["low"] = min(current_candle["low"], tick["value"])
            current_candle["close"] = tick["value"]
    
    if current_candle:
        candles.append(current_candle)
    
    return candles

@app.route("/")
def index():
    return render_template_string(HTML_TEMPLATE)

@app.route("/historic")
def historic_data():
    # First 600 ticks (10 minutes)
    initial_ticks = list(generate_mock_ticks(600))
    initial_candles = create_initial_candles(initial_ticks)
    return json.dumps(initial_candles)

@sock.route("/ws")
def push_realtime_ticks(ws):
    # Next 600 ticks (index 600-1199)
    for tick in generate_mock_ticks(600, start_index=600):
        time.sleep(0.05)  # 50ms between ticks (~20/sec)
        ws.send(json.dumps({
            "timestamp": tick["timestamp"],
            "value": tick["value"]
        }))

if __name__ == "__main__":
    app.run(port=5000)
```
