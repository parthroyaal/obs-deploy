
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Real-Time BTCUSDT Candlestick Chart</title>
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
    // WebSocket URL for Bybit public trades
    const ws_url = "wss://stream.bybit.com/v5/public/linear";
    
    // Initialize an empty DataFrame to store tick data
    let df = [];
    
    // Create the chart
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
        candlestickSeries.setData(data);
    }

    // WebSocket setup
    const ws = new WebSocket(ws_url);

    ws.onopen = function () {
        console.log("WebSocket connected");

        // Subscribe to the BTCUSDT public trade channel
        const subscribeMessage = {
            op: "subscribe",
            args: ["publicTrade.BTCUSDT"]
        };
        ws.send(JSON.stringify(subscribeMessage));
    };

    ws.onmessage = function (event) {
        const data = JSON.parse(event.data);

        if (data.topic === 'publicTrade.BTCUSDT') {
            const tick = data.data[0];  // Get the latest trade data
            const tickTime = tick.T / 1000; // Convert timestamp to Unix format (seconds)
            const tickPrice = parseFloat(tick.p);  // The price of the trade

            // Create a new tick data point
            const newTick = {
                time: tickTime,  // Timestamp for x-axis
                open: tickPrice, // Open price (for simplicity using the trade price)
                high: tickPrice, // High price
                low: tickPrice,  // Low price
                close: tickPrice // Close price
            };

            // Add the new tick to the data array
            df.push(newTick);

            // Keep only the last 1000 data points for memory efficiency
            if (df.length > 1000) {
                df.shift();
            }

            // Resample to 1-minute OHLC data
            const resampledData = resampleToOHLC(df);

            // Update the chart with the resampled data
            updateChart(resampledData);
        }
    };

    // Function to resample tick data into OHLC data
    function resampleToOHLC(data) {
        let resampled = [];
        
        // Group the data into 1-minute intervals (you can change this to other timeframes)
        let currentMinute = Math.floor(data[0].time / 60);
        let ohlc = { 
            time: currentMinute * 60, 
            open: data[0].open, 
            high: data[0].high, 
            low: data[0].low, 
            close: data[0].close 
        };

        for (let i = 1; i < data.length; i++) {
            const tick = data[i];
            const minute = Math.floor(tick.time / 60);

            if (minute === currentMinute) {
                // Update the OHLC data for the current minute
                ohlc.high = Math.max(ohlc.high, tick.high);
                ohlc.low = Math.min(ohlc.low, tick.low);
                ohlc.close = tick.close;  // Always update the close
            } else {
                // Save the OHLC data for the previous minute and reset
                resampled.push(ohlc);
                currentMinute = minute;
                ohlc = {
                    time: currentMinute * 60, // Set the time to the beginning of the new minute
                    open: tick.open,           // Open price of the new minute
                    high: tick.high,           // High price
                    low: tick.low,             // Low price
                    close: tick.close          // Close price
                };
            }
        }

        // Add the last OHLC data point
        if (ohlc) {
            resampled.push(ohlc);
        }

        return resampled;
    }

    // Handle WebSocket errors
    ws.onerror = function (error) {
        console.error("WebSocket Error: " + error);
    };

    // Handle WebSocket close
    ws.onclose = function () {
        console.log("WebSocket connection closed");
    };

    // Optional: Handle browser window resize for chart responsiveness
    window.addEventListener('resize', () => {
        chart.resize(window.innerWidth, 500);
    });
</script>

</body>
</html>

```