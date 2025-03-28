
## ## lib 

```js
<script src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/lightweight-charts.standalone.production.js"></script>
```

##  ## ImDb

### ### lineChart

```js
const data = [

      { time: '2023-01-01', close: 100 },

      { time: '2023-01-02', close: 101 },

      { time: '2023-02-25', close: and so on  }

    ];

```

### ### candleChart

```js

```
##  ## Indicators Fns

###  ### Calling indicator fns


```js
// Example usage:
const sma = calculateSMA(data, 3);
console.log('SMA:', sma);

const rsi = calculateRSI(data, 14);
console.log('RSI:', rsi);

const macd = calculateMACD(data);
console.log('MACD:', macd);
```


### ## Indicator fns @commonJs

[[indiJs]]
[[indiPy]]

[[comparison]]

## ## lwChart   Basic+features

### ### LwChart initObj

```html
<div id="chart"></div>

<script>
const priceChart = LightweightCharts.createChart(document.getElementById('chart'), {
            width: window.innerWidth,
            height: window.innerHeight,
            timeScale: {
                timeVisible: true,
                borderColor: '#D1D4DC',
            },
            rightPriceScale: {
                borderColor: '#D1D4DC',
            },
            grid: {
                horzLines: {
                    color: '#F0F3FA',
                },
                vertLines: {
                    color: '#F0F3FA',
                },
            },
        });

</script>





```

### ### Object settings 

```js
        // Set the chart layout background to black

        layout: {

          backgroundColor: '#000',

          textColor: '#fff'

        },
```
### ### LwChart Panes


```html


<script>


</script>





```


## ## Event based chart init 
###  ### Create chart instance event lister callback  Dom load 

```js
document.addEventListener('DOMContentLoaded', function() {

});

```

###  ### Create chart instance event lister callback  on click button 

```js
document.addEventListener('DOMContentLoaded', function() {

});

```

## ## SetUpdate refresh copitable backend

```js
```