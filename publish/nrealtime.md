
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Real-Time Candlestick Chart with Lightweight Charts</title>
    <style>
        /* Set the chart container size */
        #chart {
            position: relative;
            width: 100%;
            height: 500px;
        }
    </style>
</head>
<body>

<!-- The chart will be rendered here -->
<div id="chart"></div>

<script src="https://cdn.jsdelivr.net/npm/lightweight-charts@3.4.0/dist/lightweight-charts.standalone.production.js"></script>
<script>
    // Initialize the chart
    const chart = LightweightCharts.createChart(document.getElementById('chart'), {
        width: window.innerWidth,
        height: 500,
        crosshair: {
            vertLine: {
                color: '#9e9e9e',
                width: 1,
                style: 0, // Solid line
            },
            horzLine: {
                color: '#9e9e9e',
                width: 1,
                style: 0, // Solid line
            },
        },
        priceScale: {
            borderColor: '#ccc',
        },
        timeScale: {
            borderColor: '#ccc',
        },
    });

    // Create a candlestick series
    const candlestickSeries = chart.addCandlestickSeries();

    // Function to update the chart with new data (OHLC)
    function updateChart(data) {
        // Update the candlestick series with new data
        candlestickSeries.setData(data);
    }

    // Example data (OHLC)
    let ohlcData = [
        { time: 1675698000, open: 100, high: 110, low: 90, close: 105 },
        { time: 1675698060, open: 105, high: 115, low: 100, close: 110 },
        { time: 1675698120, open: 110, high: 120, low: 105, close: 115 },
        { time: 1675698180, open: 115, high: 125, low: 110, close: 120 },
        // More data points...
    ];

    // Call updateChart initially
    updateChart(ohlcData);

    // WebSocket-like mockup of live data (using setInterval for testing purposes)
    let interval = setInterval(() => {
        // Generate a new data point (mocking a tick)
        const lastTime = ohlcData[ohlcData.length - 1].time;
        const newTime = lastTime + 60; // Add 60 seconds (for 1-minute intervals)
        const newOHLC = {
            time: newTime,
            open: Math.random() * 10 + 120,  // Random price between 120 and 130
            high: Math.random() * 10 + 130,  // Random high
            low: Math.random() * 10 + 110,   // Random low
            close: Math.random() * 10 + 120  // Random close price
        };
        
        // Add the new data point and update the chart
        ohlcData.push(newOHLC);

        // Limit data to the last 100 data points for performance (like a sliding window)
        if (ohlcData.length > 100) {
            ohlcData.shift();
        }

        // Update the chart with new data
        updateChart(ohlcData);
    }, 2000); // Update every 2 seconds (simulating real-time data)
</script>

</body>
</html>

```