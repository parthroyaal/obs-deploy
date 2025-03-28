You can provide this prompt to the LLM, and it will generate:

* A **step-by-step implementation guide** explaining the functionality of the code at a high level.
* A **Mermaid diagram** visually depicting the logical flow and data transitions in the program.

This generic system prompt ensures adaptability and usability across different domains and codebases.

````html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/lightweight-charts@3.5.0/dist/lightweight-charts.standalone.production.js"></script>
    <title>Candlestick Chart</title>
</head>
<body>
<script>

// Updated data with candlestick schema and second-level granularity
const data = [
    { time: '2019-04-11T09:00:00', open: 80.01, high: 85.00, low: 78.50, close: 82.00 },
    { time: '2019-04-11T09:01:00', open: 82.00, high: 86.00, low: 81.00, close: 84.50 },
    { time: '2019-04-11T09:02:00', open: 84.50, high: 87.00, low: 83.50, close: 86.00 },
    { time: '2019-04-11T09:03:00', open: 86.00, high: 88.00, low: 85.50, close: 87.50 },
    { time: '2019-04-11T09:04:00', open: 87.50, high: 89.00, low: 86.50, close: 88.00 }
];

// Main function to orchestrate chart initialization and updates
function main() {
    // Create a chart with additional options for second-level granularity
    const chart = LightweightCharts.createChart(document.body, { 
        width: 400, 
        height: 300,
        timeScale: {
            timeVisible: true,   // Enable time visibility
            secondsVisible: true // Ensure seconds are displayed on the time axis
        }
    });

    // Add candlestick series to the chart
    const candlestickSeries = chart.addCandlestickSeries();

    // Set the candlestick data
    candlestickSeries.setData(data);
}

// Execute the main function
main();

</script>
</body>
</html>

````

### Key Changes:

1. **Data Schema**:
   
   * The `data` array now contains objects with `time`, `open`, `high`, `low`, and `close` properties.
   * `time` is in ISO 8601 format with second-level granularity (e.g., `2019-04-11T09:00:00`).
1. **Chart Options**:
   
   * The `timeScale` option is added to the `createChart` call:
     * `timeVisible: true` ensures the time is displayed.
     * `secondsVisible: true` enables seconds to be visible on the x-axis.
1. **Series Type**:
   
   * Replaced `addLineSeries` with `addCandlestickSeries` to render candlestick charts.
1. **Set Data**:
   
   * The `setData` method now accepts the updated `data` array with candlestick attributes.

---

This code will render a candlestick chart with second-level timestamps, providing a detailed visualization of the open, high, low, and close values for each second. Let me know if you need further enhancements!

## V2

Here’s the updated version of your code with the utility function to convert `time` values from ISO 8601 strings to Unix timestamp seconds before passing the data to `.setData`.

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/lightweight-charts@3.5.0/dist/lightweight-charts.standalone.production.js"></script>
    <title>Candlestick Chart</title>
</head>
<body>
<script>

// Utility function to convert ISO string to Unix timestamp (UTC)
function stringToUnixTimestampUTC(dateString) {
    return Math.floor(new Date(dateString + 'Z').getTime() / 1000);
}

// Original data with ISO 8601 time format
const rawData = [
    { time: '2019-04-11T09:00:00', open: 80.01, high: 85.00, low: 78.50, close: 82.00 },
    { time: '2019-04-11T09:01:00', open: 82.00, high: 86.00, low: 81.00, close: 84.50 },
    { time: '2019-04-11T09:02:00', open: 84.50, high: 87.00, low: 83.50, close: 86.00 },
    { time: '2019-04-11T09:03:00', open: 86.00, high: 88.00, low: 85.50, close: 87.50 },
    { time: '2019-04-11T09:04:00', open: 87.50, high: 89.00, low: 86.50, close: 88.00 }
];

// Convert the `time` field in the data to Unix timestamp seconds
const data = rawData.map(entry => ({
    time: stringToUnixTimestampUTC(entry.time),
    open: entry.open,
    high: entry.high,
    low: entry.low,
    close: entry.close
}));

// Main function to orchestrate chart initialization and updates
function main() {
    // Create a chart with additional options for second-level granularity
    const chart = LightweightCharts.createChart(document.body, { 
        width: 400, 
        height: 300,
        timeScale: {
            timeVisible: true,   // Enable time visibility
            secondsVisible: true // Ensure seconds are displayed on the time axis
        }
    });

    // Add candlestick series to the chart
    const candlestickSeries = chart.addCandlestickSeries();

    // Set the candlestick data
    candlestickSeries.setData(data);
}

// Execute the main function
main();

</script>
</body>
</html>

````

### Key Updates:

1. **Utility Function**:
   
   * Added `stringToUnixTimestampUTC` to convert ISO 8601 strings to Unix timestamp seconds.
1. **Data Conversion**:
   
   * Used `Array.map` to preprocess the `rawData`, converting `time` values to Unix timestamps using the utility function.
   * The transformed `data` array now has `time` values as Unix timestamps.
1. **Integration with Chart**:
   
   * The preprocessed `data` is passed to `.setData` for the candlestick series.

---

This ensures compatibility with the `LightweightCharts` library, which uses Unix timestamp seconds for the `time` field in candlestick series data. Let me know if you need further refinements!

## ConciseOptimize

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/lightweight-charts@3.5.0/dist/lightweight-charts.standalone.production.js"></script>
    <title>Candlestick Chart</title>
</head>
<body>
<script>

// Utility function to convert ISO string to Unix timestamp (UTC)
const stringToUnixTimestampUTC = (dateString) => Math.floor(new Date(dateString + 'Z').getTime() / 1000);

// Main function to orchestrate chart initialization and updates
function main() {
    // Original data with ISO 8601 time format
    const rawData = [
        { time: '2019-04-11T09:00:00', open: 80.01, high: 85.00, low: 78.50, close: 82.00 },
        { time: '2019-04-11T09:01:00', open: 82.00, high: 86.00, low: 81.00, close: 84.50 },
        { time: '2019-04-11T09:02:00', open: 84.50, high: 87.00, low: 83.50, close: 86.00 },
        { time: '2019-04-11T09:03:00', open: 86.00, high: 88.00, low: 85.50, close: 87.50 },
        { time: '2019-04-11T09:04:00', open: 87.50, high: 89.00, low: 86.50, close: 88.00 }
    ];

    // Create a chart with additional options for second-level granularity
    const chart = LightweightCharts.createChart(document.body, { 
        width: 400, 
        height: 300,
        timeScale: {
            timeVisible: true,   // Enable time visibility
            secondsVisible: true // Ensure seconds are displayed on the time axis
        }
    });

    // Add candlestick series to the chart
    const candlestickSeries = chart.addCandlestickSeries();

    // Set the candlestick data, transforming time values inline
    candlestickSeries.setData(
        rawData.map(entry => ({
            ...entry,
            time: stringToUnixTimestampUTC(entry.time)
        }))
    );
}

// Execute the main function
main();

</script>
</body>
</html>

````

### Key Improvements:

1. **Inline Transformation**:
   
   * The `map` function is used directly within the `candlestickSeries.setData` call to transform the `rawData`. This avoids creating an intermediate `data` variable.
1. **Spread Operator**:
   
   * The `{ ...entry }` syntax copies all properties of each `entry` object (like `open`, `high`, `low`, and `close`) while modifying the `time` field to use the `stringToUnixTimestampUTC` function.
1. **Arrow Function for Utility**:
   
   * The utility function `stringToUnixTimestampUTC` is now a concise arrow function for brevity.

This version is cleaner, avoids intermediate variables, and uses modern JavaScript syntax effectively. Let me know if you have more suggestions!

## StyleGuide

also the flow should only be in main functyion not data varualbe the3 flow of what is happeinign in each step and do every thing in step to able to make mermaoid flow diagram and implemantaion docs that can be used to code same app from styracth using diagram and docs

Here’s a revised version where the entire flow is encapsulated within the `main` function to ensure the steps are clearly delineated. This approach ensures that the flow can be directly translated into a **Mermaid diagram** and **implementation documentation**.

---

### Updated Code

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://unpkg.com/lightweight-charts@3.5.0/dist/lightweight-charts.standalone.production.js"></script>
    <title>Candlestick Chart</title>
</head>
<body>
<script>

// Main function to orchestrate chart creation and data transformation
function main() {
    // Step 1: Define raw data with ISO 8601 time format
    const rawData = [
        { time: '2019-04-11T09:00:00', open: 80.01, high: 85.00, low: 78.50, close: 82.00 },
        { time: '2019-04-11T09:01:00', open: 82.00, high: 86.00, low: 81.00, close: 84.50 },
        { time: '2019-04-11T09:02:00', open: 84.50, high: 87.00, low: 83.50, close: 86.00 },
        { time: '2019-04-11T09:03:00', open: 86.00, high: 88.00, low: 85.50, close: 87.50 },
        { time: '2019-04-11T09:04:00', open: 87.50, high: 89.00, low: 86.50, close: 88.00 }
    ];

    // Step 2: Convert ISO time to Unix timestamp seconds inline
    const transformedData = rawData.map(entry => ({
        ...entry,
        time: Math.floor(new Date(entry.time + 'Z').getTime() / 1000)
    }));

    // Step 3: Create the chart instance
    const chart = LightweightCharts.createChart(document.body, { 
        width: 400, 
        height: 300,
        timeScale: {
            timeVisible: true,   // Ensure time is displayed
            secondsVisible: true // Display seconds on the time axis
        }
    });

    // Step 4: Add candlestick series to the chart
    const candlestickSeries = chart.addCandlestickSeries();

    // Step 5: Set transformed data to the candlestick series
    candlestickSeries.setData(transformedData);
}

// Execute the main function
main();

</script>
</body>
</html>

````

### **Mermaid Flow Diagram**

This flow mirrors the execution steps in the `main` function.

````mermaid
flowchart TD
    A[Start] --> B[Define Raw Data]
    B --> C[Transform Time to Unix Timestamp]
    C --> D[Create Chart Instance]
    D --> E[Add Candlestick Series to Chart]
    E --> F[Set Transformed Data to Candlestick Series]
    F --> G[End]

````

### **Implementation Documentation**

Below is the step-by-step guide to recreate the app from scratch, based on the flow diagram.

#### **Step 1: Define Raw Data**

* Initialize an array containing candlestick data points.
* Each object in the array should have the following properties:
  * `time`: An ISO 8601 timestamp string (e.g., `2019-04-11T09:00:00`).
  * `open`, `high`, `low`, `close`: Numeric values representing the candlestick data.

#### **Step 2: Transform Time to Unix Timestamp**

* Use `Array.map` to transform the raw data into a new array.

* Convert the `time` property of each object to Unix timestamp seconds using:
  
  javascript
  
  CopyEdit
  
  `Math.floor(new Date(entry.time + 'Z').getTime() / 1000)`

#### **Step 3: Create Chart Instance**

* Use `LightweightCharts.createChart` to initialize a new chart instance.
* Pass the following options:
  * `width` and `height`: Set the chart dimensions.
  * `timeScale` options:
    * `timeVisible: true`: Enable time visibility on the x-axis.
    * `secondsVisible: true`: Ensure seconds are displayed on the x-axis.

#### **Step 4: Add Candlestick Series to Chart**

* Use `chart.addCandlestickSeries()` to create a candlestick series for the chart.

#### **Step 5: Set Transformed Data to Candlestick Series**

* Pass the transformed data array (from Step 2) to `candlestickSeries.setData()`.

#### **Final Step: Execute Main Function**

* Call the `main` function to execute all the steps and render the chart.

---

This structure keeps everything within the `main` function, making it easy to follow the flow and map directly to the Mermaid diagram and documentation. Let me know if you need further refinements!

````style
html, body {
    margin: 0;
    padding: 0;
    height: 100%;
}

``
````
