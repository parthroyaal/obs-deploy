1. Generate a large number of ticks (e.g., 1200 ticks = 20 minutes).using generateMockData(1200).
1. Set initial data with the first half of generated tickcs 600=10minute  after Aggregate all ticks into 10 1-minute candles. .
1. Use the remaining 600 ticks  (from index 600 to end ) for updates. in form of candle  -->

So the steps are:

1. Generate all the ticks (1200 ticks as before).
   Split into initial ticks (first 600) and update ticks (next 600).
1. Aggregate the initial ticks into candles and set them as initial data.
   For the update ticks, process each one by one, updating the current candle in real-time.
   But how to handle the initial candles and then the real-time updates? Let's see.

````html
<html>
    <head></head>
    <body>
        <div id="tv_chart_container" style="width: 100%; height: 100vh;"></div>
        
        <script src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/lightweight-charts.standalone.production.js"></script>
        <script>
            function* generateMockData(numRows) {
                const startTime = new Date('2024-02-27T01:00:00Z').getTime() / 1000;
                let tickValue = 100;

                for (let i = 0; i < numRows; i++) {
                    const timestamp = startTime + i;
                    yield { 
                        timestamp: new Date(timestamp * 1000).toISOString(),
                        tick: tickValue 
                    };
                    tickValue *= 1.001; // 0.1% increase per tick
                }
            }

            // Generate 1200 ticks (20 minutes)
            const allTicks = Array.from(generateMockData(1200));
            
            // Split into initial and update ticks
            const initialTicks = allTicks.slice(0, 600); // First 10 minutes
            const updateTicks = allTicks.slice(600);     // Last 10 minutes

            // Create initial candles from first 10 minutes
            function createInitialCandles(ticks) {
                const candles = [];
                let currentCandle = null;

                ticks.forEach(tick => {
                    const tickTime = new Date(tick.timestamp).getTime() / 1000;
                    const candleTime = tickTime - (tickTime % 60);

                    if (!currentCandle || candleTime > currentCandle.time) {
                        currentCandle = {
                            time: candleTime,
                            open: tick.tick,
                            high: tick.tick,
                            low: tick.tick,
                            close: tick.tick
                        };
                        candles.push(currentCandle);
                    } else {
                        currentCandle.high = Math.max(currentCandle.high, tick.tick);
                        currentCandle.low = Math.min(currentCandle.low, tick.tick);
                        currentCandle.close = tick.tick;
                    }
                });

                return candles;
            }

            // Chart setup
            const chart = LightweightCharts.createChart(document.getElementById('tv_chart_container'), {
                width: window.innerWidth,
                height: window.innerHeight,
                timeScale: {
                    timeVisible: true,
                    barSpacing: 25,
                }
            });

            const candleSeries = chart.addCandlestickSeries();
            const initialCandles = createInitialCandles(initialTicks);
            candleSeries.setData(initialCandles);

            // Real-time candle builder for updates
            let currentCandle = null;
            let lastCandleTime = initialCandles[initialCandles.length - 1].time;

            function processTick(tick) {
                const tickTime = new Date(tick.timestamp).getTime() / 1000;
                const candleTime = tickTime - (tickTime % 60);

                // New candle needed
                if (candleTime > lastCandleTime) {
                    if (currentCandle) {
                        candleSeries.update(currentCandle);
                        lastCandleTime = currentCandle.time;
                    }
                    currentCandle = {
                        time: candleTime,
                        open: tick.tick,
                        high: tick.tick,
                        low: tick.tick,
                        close: tick.tick
                    };
                } 
                // Update existing candle
                else {
                    currentCandle.high = Math.max(currentCandle.high, tick.tick);
                    currentCandle.low = Math.min(currentCandle.low, tick.tick);
                    currentCandle.close = tick.tick;
                }

                // Update chart with latest state
                candleSeries.update(currentCandle);
            }

            // Simulate real-time updates
            let tickIndex = 0;
            const interval = setInterval(() => {
                if (tickIndex < updateTicks.length) {
                    processTick(updateTicks[tickIndex]);
                    tickIndex++;
                } else {
                    clearInterval(interval);
                    console.log('All ticks processed!');
                }
            }, 50); // 50ms per tick (~20 ticks/second)
        </script>
    </body>
</html>

<!-- So the steps are:
1. Generate all the ticks (1200 ticks as before).
Split into initial ticks (first 600) and update ticks (next 600).
3. Aggregate the initial ticks into candles and set them as initial data.
For the update ticks, process each one by one, updating the current candle in real-time.
But how to handle the initial candles and then the real-time updates? Let's see. -->
````
