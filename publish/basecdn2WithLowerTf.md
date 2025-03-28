
[[basecdn2]] + 

IN onready: function signature             supported_resolutions: ['5S','1', '5', '15', '30', '60', '240', '1D'],

In resolveSymbol function signaure :
inside onSymbolResolveCallbakc 
            has_intraday: true,

            has_daily: true,  // Added to explicitly support daily resolution

        

```html
<script>
has_seconds: true,
intraday_multipliers: ['1', '60'],
seconds_multipliers: ["5", "10", "15", "30"], // Updated with new timeframes
supported_resolutions: configurationData.supported_resolutions,
supported_resolutions: ['5S','1', '5', '15', '30', '60', '240', '1D']


enabled_features: ["dont_show_boolean_study_arguments", "seconds_resolution"],
time_scale: {
       visible: true,
 }
 timezone: "Asia/Kolkata",
        
</script>
```

#############

```js
const widget = new TradingView.widget({
enabled_features: ["seconds_resolution"]})
const datafeed = {
  onReady: (callback) => { callback({    supported_resolutions: ['5S', '10S', '15S','30S','45S','1','3' ,'5', '15', '30', '45', '60', '240', '1D'],  }) },
}
```


# Full 

```html
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
// Define datafeed object using standard object literal syntax
const datafeed = {
  onReady: function(callback) {
    callback({
      supported_resolutions: ['5S', '10S', '15S', '30S', '45S', '1', '3', '5', '15', '30', '45', '60', '240', '1D']
    });
  },
  resolveSymbol: function(symbolName, onResolve, onError) {
    /* Resolve symbol details */
  },
  getBars: function(symbolInfo, resolution, from, to, onData, onError) {
    /* Fetch historical data */
  },
  subscribeBars: function(symbolInfo, resolution, onTick) {
    /* Real-time updates */
  }
};
</script>


<script>
// Define configuration using standard object literal syntax
var widgetConfiguration = {
  container: 'chartContainer',
  symbol: 'AAPL',
  interval: '1D',
  datafeed: datafeed,
  library_path: 'https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/charting_library/charting_library.standalone.js',
  locale: 'en',
  theme: 'light',
  fullscreen: true,
  enabled_features: ["seconds_resolution"]
};

// Create widget instance
var widget = new TradingView.widget(widgetConfiguration);
</script>


</body>
</html>
```