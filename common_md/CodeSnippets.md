---

# CodeSnippets

[NeonPostgresSql](NeonPostgresSql.md)

# Misc

[tv2Snips](tv2Snips.md)
[tv1Snips](tv1Snips.md)

# flaskStuff

## localhostStuff

````txt
 const socket = new WebSocket(`ws://${window.location.host}/ws`);
 
````

## libExpose

## Windows

````python
@app.route('/charting_library/<path:filename>')
def charting_library_files(filename):
    # Get the absolute path to the 'charting_library' folder
    charting_lib_path = os.path.join(app.root_path, 'charting_library')
    
    # Check if the requested file exists in the folder
    if os.path.exists(os.path.join(charting_lib_path, filename)):
        return send_from_directory(charting_lib_path, filename)
    else:
        return "File not found", 404
````

````
## Linux 

```python

````

## JsScriptInit

````javascript
 // Function to render chart
function chartRender() {
        lineSeries.setData(data);
    }
window.onload = chartRender;
````

1. **Date Only**: `2024-01-02` (YYYY-MM-DD)
1. **Date and Time**:
   * Full timestamp: `2024-01-02T12:34:56` (YYYY-MM-DDTHH:mm:ss)
   * With timezone: `2024-01-02T12:34:56Z` (UTC time, "Z" indicates Zulu/UTC timezone)

### Updated Code Using Minute-Level Data:

### FUll

````html
<html>
    <head>
        <script src="lib.js"></script>
    </head>
<body>
<script>
    // Data containing time-value pairs with ISO 8601 format including time
    var data = [
        { time: '2024-01-02T00:00:00', value: 101 },
        { time: '2024-01-02T00:01:00', value: 102 },
        { time: '2024-01-02T00:02:00', value: 103 },
        { time: '2024-01-02T00:03:00', value: 104 },
    ];

    // Create a chart object using LightweightCharts
    const chartObj = LightweightCharts.createChart(document.body, {
        width: window.innerWidth,
        height: window.innerHeight
    });

    // Add a line series to the chart
    const lineSeries = chartObj.addLineSeries();

    // Function to render chart
    function chartRender() {
        lineSeries.setData(data);
    }

    // Run the chartRender function on window load
    window.onload = chartRender;
</script>
</body>
</html>

````

## CharStyling

````javascript
  const chartObj = LightweightCharts.createChart(document.body, {
        width: window.innerWidth, // Changed to use proper JavaScript syntax for viewport width
        height: window.innerHeight // Fixed typo and corrected height syntax
    });
````

# tickToCandle

### Py ref: https://medium.com/@kchanchal78/converting-tick-data-to-ohlc-candlesticks-with-python-a064610348cc

res : https://notebooklm.google/

````py

def 

def tickToCandle():


````

### Js ref: https://parthroyaal.github.io/coinview/

````js

function tickToCandle() {

}



function processTradeData(trade) {
  const timestamp = new Date(trade.T);
  const price = parseFloat(trade.p);

  const candleTimestamp = new Date(timestamp.getFullYear(), timestamp.getMonth(), timestamp.getDate(), timestamp.getHours(), timestamp.getMinutes());

  let candle = candleData.find(c => c.time.getTime() === candleTimestamp.getTime());

  if (!candle) {
    candle = {
      time: candleTimestamp,
      open: price,
      high: price,
      low: price,
      close: price
    };
    candleData.push(candle);
  } else {
    candle.high = Math.max(candle.high, price);
    candle.low = Math.min(candle.low, price);
    candle.close = price;
  }

  if (candlestickSeries) { // Check if candlestickSeries is initialized
    candlestickSeries.update({
      time: candle.time.getTime() / 1000,
      open: candle.open,
      high: candle.high,
      low: candle.low,
      close: candle.close
    });
  }
}


````

# 
