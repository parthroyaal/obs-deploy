

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Technical Indicators - Candlestick Chart</title>
  <style>
    /* Add your custom styles here */
  </style>
</head>
<body>
  <h1>Technical Indicators Demo</h1>
  <div id="chart"></div>

  <script src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/lightweight-charts.standalone.production.js"></script>
  <script>
    // Load CSV file using fetch and process as text
    fetch('mar10.csv')
      .then((response) => response.text())
      .then((csvText) => {
        // Split CSV text into lines.
        // Assumes CSV rows like: "2025-03-07 09:59:55,22535.25"
        const lines = csvText.trim().split('\n');
        const tickData = lines.map((line) => {
          const [time, close] = line.split(',');
          // Convert CSV time to a UTC ISO string (adds "T" and "Z")
          const isoTime = time.trim().replace(' ', 'T') + 'Z';
          // Convert to Unix timestamp in seconds
          return {
            time: Math.floor(new Date(isoTime).getTime() / 1000),
            close: parseFloat(close.trim())
          };
        });
        // Aggregate tick data into candlestick bars.
        const candlestickData = aggregateToCandlestick(tickData);
        // Now initialize the chart using the candlestick data.
        initializeCharts(tickData, candlestickData);
      })
      .catch((error) => console.error("Error loading CSV file:", error));

    /**
     * Aggregates tick data (each with a close price) into candlestick data.
     * Groups ticks into buckets of a specified timeframe (in seconds).
     * For each bucket, the first tick is used as the open, the last tick as the close,
     * and high/low are the maximum/minimum prices in the bucket.
     *
     * @param {Array} data - Array of tick data objects { time: <Unix timestamp>, close: <price> }.
     * @param {number} timeframe - The aggregation interval in seconds. (Default is 5 seconds.)
     * @returns {Array} - Array of candlestick objects sorted by time.
     */
    function aggregateToCandlestick(data, timeframe = 5) {
      const candles = {};
      data.forEach((item) => {
        // Calculate the bucket for the given timeframe (5 seconds in this case)
        const bucket = Math.floor(item.time / timeframe) * timeframe;
        if (!candles[bucket]) {
          // Initialize a new candle for this bucket
          candles[bucket] = {
            time: bucket,
            open: item.close,
            high: item.close,
            low: item.close,
            close: item.close
          };
        } else {
          // Update candle for bucket
          candles[bucket].high = Math.max(candles[bucket].high, item.close);
          candles[bucket].low = Math.min(candles[bucket].low, item.close);
          candles[bucket].close = item.close; // the latest tick becomes the close
        }
      });
      // Convert the candles object into an array and sort by time
      return Object.values(candles).sort((a, b) => a.time - b.time);
    }

    /**
     * Creates the chart and adds the candlestick series.
     * Optionally, additional indicator series (like SMA) can be overlaid.
     */
    function initializeCharts(tickData, candlestickData) {
      const chart = LightweightCharts.createChart(document.getElementById('chart'), {
        width: window.innerWidth,
        height: window.innerHeight,
        timeScale: {
          timeVisible: true,
          borderColor: '#D1D4DC'
        },
        rightPriceScale: {
          borderColor: '#D1D4DC'
        },
        grid: {
          horzLines: { color: '#F0F3FA' },
          vertLines: { color: '#F0F3FA' }
        }
      });

      // Replace the previous price line series with a candlestick series.
      const candlestickSeries = chart.addCandlestickSeries({
        upColor: '#4caf50',
        downColor: '#f44336',
        borderUpColor: '#4caf50',
        borderDownColor: '#f44336',
        wickUpColor: '#4caf50',
        wickDownColor: '#f44336'
      });
      candlestickSeries.setData(candlestickData);

      // OPTIONAL: If you'd like to overlay a moving average (SMA) on top of the candlestick chart,
      // you can calculate it using your tickData (or aggregated data) and add a line series.
      const sma12 = calculateSMA(tickData, 12);
      const sma12Data = sma12.map((item) => ({ time: item.time, value: item['SMA 12'] }));
      const sma12Series = chart.addLineSeries({
        title: 'SMA 12',
        color: '#FFEB3B',
        lineWidth: 2
      });
      sma12Series.setData(sma12Data);

      // Similarly, other indicators (RSI, MACD) could be added to separate panes.
    }

    // ------------------------------- INDICATOR FUNCTIONS -------------------------------

    /**
     * Calculate the Simple Moving Average (SMA) over a given period.
     */
    function calculateSMA(df, period = 50) {
      const result = [];
      for (let i = period - 1; i < df.length; i++) {
        let sum = 0;
        for (let j = i - period + 1; j <= i; j++) {
          sum += df[j].close;
        }
        const sma = sum / period;
        result.push({ time: df[i].time, [`SMA ${period}`]: sma });
      }
      return result;
    }

    /**
     * Calculate the Relative Strength Index (RSI) for a given window length.
     * Assumes that the passed df already has time as a Unix timestamp.
     */
    function calculateRSI(df, windowLength = 14) {
      const delta = new Array(df.length).fill(0);
      for (let i = 1; i < df.length; i++) {
        delta[i] = df[i].close - df[i - 1].close;
      }
      const gains = new Array(df.length).fill(0);
      const losses = new Array(df.length).fill(0);
      for (let i = 1; i < delta.length; i++) {
        gains[i] = delta[i] > 0 ? delta[i] : 0;
        losses[i] = delta[i] < 0 ? -delta[i] : 0;
      }
      const avgGains = new Array(df.length).fill(null);
      const avgLosses = new Array(df.length).fill(null);
      const modifiedDF = df.slice();
      for (let i = windowLength; i < df.length; i++) {
        let sumGains = 0, sumLosses = 0;
        for (let j = i - windowLength + 1; j <= i; j++) {
          sumGains += gains[j];
          sumLosses += losses[j];
        }
        avgGains[i] = sumGains / windowLength;
        avgLosses[i] = sumLosses / windowLength;
      }
      for (let i = windowLength; i < df.length; i++) {
        const rs = avgLosses[i] === 0 ? Infinity : avgGains[i] / avgLosses[i];
        const rsi = 100 - (100 / (1 + rs));
        modifiedDF[i].RSI = rsi;
      }
      return modifiedDF;
    }

    /**
     * Calculate the Exponential Moving Average (EMA) for an array of prices.
     */
    function calculateEMA(prices, span) {
      const ema = [];
      const multiplier = 2 / (span + 1);
      ema[0] = prices[0];
      for (let i = 1; i < prices.length; i++) {
        ema[i] = (prices[i] - ema[i - 1]) * multiplier + ema[i - 1];
      }
      return ema;
    }

    /**
     * Calculate the Moving Average Convergence Divergence (MACD).
     */
    function calculateMACD(df, short_window = 12, long_window = 26, signal_window = 9) {
      const prices = df.map((item) => item.close);
      const shortEMA = calculateEMA(prices, short_window);
      const longEMA = calculateEMA(prices, long_window);
      const macdLine = prices.map((_, i) => shortEMA[i] - longEMA[i]);
      const signalLine = calculateEMA(macdLine, signal_window);
      for (let i = 0; i < df.length; i++) {
        df[i].MACD = macdLine[i];
        df[i].Signal_Line = signalLine[i];
      }
      return df;
    }
  </script>
</body>
</html>
```