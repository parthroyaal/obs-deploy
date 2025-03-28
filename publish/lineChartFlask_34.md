

```python

from flask import Flask, render_template_string
from flask_sock import Sock
from datetime import datetime, timedelta
import time
import json

app = Flask(__name__)
sock = Sock(app)

# HTML Template (keep indentation for proper rendering)
HTML_TEMPLATE = """
<html>
    <head></head>
    <body>
        <div id="tv_chart_container" style="width: 100%; height: 100vh;"></div>
        
        <script src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/lightweight-charts.standalone.production.js"></script>
        <script>
            // Chart initialization
            const chart = LightweightCharts.createChart(document.getElementById('tv_chart_container'), {
                width: window.innerWidth,
                height: window.innerHeight,
                timeScale: {
                    timeVisible: true,
                    secondsVisible: true,
                }
            });
            
            const lineSeries = chart.addLineSeries();
            
            // Fetch initial historical data
            fetch('/historic')
                .then(response => response.json())
                .then(data => {
                    lineSeries.setData(data.map(entry => ({
                        time: entry.timestamp,
                        value: entry.tick
                    })));
                });

            // WebSocket connection for real-time updates
            const ws = new WebSocket(`ws://${window.location.host}/ws`);
            ws.onmessage = function(event) {
                const dataPoint = JSON.parse(event.data);
                lineSeries.update({
                    time: dataPoint.timestamp,
                    value: dataPoint.tick
                });
            };
        </script>
    </body>
</html>
"""

def generate_mock_data(num_rows, start_index=0):
    start_time = datetime(2024, 2, 27, 1, 0, 0)
    tick_value = 100.0
    
    for i in range(start_index, num_rows + start_index):
        yield {
            "timestamp": (start_time + timedelta(seconds=i)).timestamp(),
            "tick": tick_value
        }
        tick_value *= 1.10

@app.route("/")
def index():
    return render_template_string(HTML_TEMPLATE)

@app.route("/historic")
def historic_data():
    # First 60 data points
    historic_points = list(generate_mock_data(60))
    return json.dumps(historic_points)

@sock.route("/ws")
def push_latest_tick(ws):
    # Next 60 data points (index 60-119)
    for data_point in generate_mock_data(60, start_index=60):
        time.sleep(1)  # 1 second between updates
        ws.send(json.dumps(data_point))

if __name__ == "__main__":
    app.run(port=5000) ****
```

```html

```