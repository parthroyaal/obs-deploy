
````js
<script>
        // Indicator functions implemented using plain JavaScript
// Assumption: Our "dataframe" is just an array of objects with properties "time" and "close".

/**
 * Calculate the Simple Moving Average (SMA) over a specific period.
 * @param {Array<Object>} df - The data array where each object contains a "time" and "close" property.
 * @param {number} period - The number of periods over which to calculate the SMA. Default is 50.
 * @returns {Array<Object>} - An array of objects containing the time and the corresponding SMA.
 */
function calculateSMA(df, period = 50) {
  const result = [];
  // Start when a full "period" of data is available.
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
 * Calculate the Relative Strength Index (RSI) with a given window length.
 * @param {Array<Object>} df - The data array where each object contains "time" and "close" properties.
 * @param {number} windowLength - The number of periods over which to calculate the RSI. Default is 14.
 * @returns {Array<Object>} - A copy of the input array with an added "RSI" property for each computed index.
 */
function calculateRSI(df, windowLength = 14) {
  // Create a copy of the dataframe converting the "time" to Unix timestamp (seconds).
  let modifiedDF = df.map(item => ({
    ...item,
    time: Math.floor(new Date(item.time).getTime() / 1000)
  }));

  // Create an array of price changes (delta). Set index 0 to 0.
  const delta = new Array(modifiedDF.length).fill(0);
  for (let i = 1; i < modifiedDF.length; i++) {
    delta[i] = modifiedDF[i].close - modifiedDF[i - 1].close;
  }

  // Separate positive gains and negative losses.
  const gains = new Array(modifiedDF.length).fill(0);
  const losses = new Array(modifiedDF.length).fill(0);
  for (let i = 1; i < delta.length; i++) {
    gains[i] = delta[i] > 0 ? delta[i] : 0;
    losses[i] = delta[i] < 0 ? -delta[i] : 0;
  }

  // Calculate rolling averages for gains and losses.
  const avgGains = new Array(modifiedDF.length).fill(null);
  const avgLosses = new Array(modifiedDF.length).fill(null);
  for (let i = windowLength; i < modifiedDF.length; i++) {
    let sumGains = 0;
    let sumLosses = 0;
    for (let j = i - windowLength + 1; j <= i; j++) {
      sumGains += gains[j];
      sumLosses += losses[j];
    }
    avgGains[i] = sumGains / windowLength;
    avgLosses[i] = sumLosses / windowLength;
  }

  // Compute RSI using the formula: RSI = 100 - (100 / (1 + RS))
  // where RS is the ratio of average gain to average loss.
  for (let i = windowLength; i < modifiedDF.length; i++) {
    const rs = avgLosses[i] === 0 ? Infinity : avgGains[i] / avgLosses[i];
    const rsi = 100 - (100 / (1 + rs));
    modifiedDF[i].RSI = rsi;
  }

  // Optionally convert the Unix timestamps back to Date objects.
  modifiedDF = modifiedDF.map(item => ({
    ...item,
    time: new Date(item.time * 1000)
  }));

  return modifiedDF;
}

/**
 * Helper function to calculate the Exponential Moving Average (EMA) for an array of numbers.
 * @param {Array<number>} prices - The array of prices.
 * @param {number} span - The span (period) for calculating EMA.
 * @returns {Array<number>} - The EMA for the prices.
 */
function calculateEMA(prices, span) {
  const ema = [];
  const multiplier = 2 / (span + 1);
  ema[0] = prices[0]; // initial EMA value is the first price.
  for (let i = 1; i < prices.length; i++) {
    ema[i] = (prices[i] - ema[i - 1]) * multiplier + ema[i - 1];
  }
  return ema;
}

/**
 * Calculate the Moving Average Convergence Divergence (MACD).
 * @param {Array<Object>} df - The data array where each object contains a "close" property.
 * @param {number} short_window - The period for the short-term EMA (default 12).
 * @param {number} long_window - The period for the long-term EMA (default 26).
 * @param {number} signal_window - The period for the signal line EMA (default 9).
 * @returns {Array<Object>} - The modified dataframe with added "MACD" and "Signal_Line" properties.
 */
function calculateMACD(df, short_window = 12, long_window = 26, signal_window = 9) {
  // Extract the closing prices.
  const prices = df.map(item => item.close);
  
  // Calculate the short-term and long-term EMA.
  const shortEMA = calculateEMA(prices, short_window);
  const longEMA = calculateEMA(prices, long_window);
  
  // MACD line is the difference between the short-term and long-term EMA.
  const macdLine = prices.map((_, i) => shortEMA[i] - longEMA[i]);
  
  // Signal line is an EMA of the MACD line.
  const signalLine = calculateEMA(macdLine, signal_window);
  
  // Attach MACD and Signal_Line to the original dataframe.
  for (let i = 0; i < df.length; i++) {
    df[i].MACD = macdLine[i];
    df[i].Signal_Line = signalLine[i];
  }
  return df;
}
</script>

</script>
    <!-- #######################################END OF INDICATOR FUNCTIONS###################################### -->


````
