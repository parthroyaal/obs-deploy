

So the steps are:

1. Generate 120 data points using generateMockData(120).

Set initial data with the first 60.

3. Use the remaining 60 (from index 60 to 119) for updates. -->


```html
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
                    tickValue *= 1.10;
                }
            }

            // Generate double the initial data (120 points)
            const mockData = Array.from(generateMockData(120));
            
            // Chart setup
            const chart = LightweightCharts.createChart(document.getElementById('tv_chart_container'), {
                width: window.innerWidth,
                height: window.innerHeight,
                timeScale: {
                    timeVisible: true,
                    secondsVisible: true,
                }
            });

            const lineSeries = chart.addLineSeries();

            // Set initial 60 points
            lineSeries.setData(
                mockData.slice(0, 60).map(data => ({
                    time: new Date(data.timestamp).getTime() / 1000,
                    value: data.tick,
                }))
            );

            // Replay function for remaining data
            function startUpdateReplay() {
                let index = 60; // Start from 61st data point
                const interval = setInterval(() => {
                    if (index < mockData.length) {
                        const dataPoint = mockData[index];
                        lineSeries.update({
                            time: new Date(dataPoint.timestamp).getTime() / 1000,
                            value: dataPoint.tick,
                        });
                        index++;
                    } else {
                        clearInterval(interval);
                        console.log('All updates completed!');
                    }
                }, 1000); // 1 second per update
            }

            // Start updating after 1 second delay
            setTimeout(startUpdateReplay, 1000);
        </script>
    </body>
</html>

<!-- 
So the steps are:
1. Generate 120 data points using generateMockData(120).
Set initial data with the first 60.
3. Use the remaining 60 (from index 60 to 119) for updates. -->
```