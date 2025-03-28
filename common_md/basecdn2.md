
````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>

<div id="chartContainer"></div>

<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/charting_library/charting_library.standalone.js"></script>


<script>
const datafeed = {
  onReady: (callback) => { callback({ supported_resolutions: ['1', '5', '60', '1D'] }) },
  resolveSymbol: (symbolName, onResolve, onError) => { /* Resolve symbol details */ },
  getBars: (symbolInfo, resolution, from, to, onData, onError) => { /* Fetch historical data */ },
  subscribeBars: (symbolInfo, resolution, onTick) => { /* Real-time updates */ },
};
</script>


<script>

const widget = new TradingView.widget({
  container: 'chartContainer', // ID of the container element
  symbol: 'AAPL',              // Default symbol
  interval: '1D',              // Default interval (1 day)
  datafeed: datafeed,
  library_path: 'https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/charting_library/charting_library.standalone.js',
  locale: 'en',                // Language ('en', 'es', 'zh', etc.)
  theme: 'light',              // 'light' or 'dark'
  fullscreen: true,
});
</script>


</body>
</html>
````
