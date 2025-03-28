
```python


import pandas as pd

import plotly.graph_objects as go

# Load the resampled data from the CSV file
df = pd.read_csv('data_1Min.csv', parse_dates=['Timestamp'])

# Ensure the 'Timestamp' column is in datetime format (if not already)
df['Timestamp'] = pd.to_datetime(df['Timestamp'])

# Create a candlestick chart using Plotly
fig = go.Figure(data=[go.Candlestick(
    x=df['Timestamp'],
    open=df['Price_first'],  # Opening price
    high=df['Price_max'],     # Highest price
    low=df['Price_min'],      # Lowest price
    close=df['Price_last'],   # Closing price
    name='BTCUSDT 1Min Candlestick'
)])

# Update layout of the chart for better visualization
fig.update_layout(
    title='1-Minute Candlestick Chart for BTCUSDT',
    xaxis_title='Timestamp',
    yaxis_title='Price (USDT)',
    xaxis_rangeslider_visible=False,  # Hide range slider for a cleaner view
    template='plotly_dark'  # Optional: Use a dark theme for the chart
)

# Show the figure
fig.show()

```

```txt
# Step 1 : import plotly.graph_objects as go

# Step 2: Create a Plotly FigureWidget

fig = go.FigureWidget()

```