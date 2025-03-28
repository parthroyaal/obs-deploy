

```python



import pandas as pd
from datetime import datetime, timezone, timedelta

def calculate_sma(df, period: int = 50):
    return pd.DataFrame({
        'time': df['time'],
        f'SMA {period}': df['close'].rolling(window=period).mean()
    }).dropna()


def calculate_rsi(df):
    # Calculate RSI only using the relevant columns
    df['time'] = (pd.to_datetime(df['time']).view('int64') // 10 ** 9)

    # Calculate the price change
    delta = df['close'].diff()

    # Define the time period for RSI calculation (usually 14 days)
    window_length = 14

    # Calculate gain and loss
    gain = (delta.where(delta > 0, 0)).rolling(window=window_length).mean()
    loss = (-delta.where(delta < 0, 0)).rolling(window=window_length).mean()

    # Calculate Relative Strength
    rs = gain / loss

    # Calculate RSI
    rsi = 100 - (100 / (1 + rs))

    # Convert 'time' back to datetime object
    df['time'] = pd.to_datetime(df['time'], unit='s', utc=True)

    # Add RSI column to the dataframe
    df['RSI'] = rsi

    return df


def calculate_macd(df, short_window=12, long_window=26, signal_window=9):
    # Calculate short-term and long-term exponential moving averages
    short_ema = df['close'].ewm(span=short_window, adjust=False).mean()
    long_ema = df['close'].ewm(span=long_window, adjust=False).mean()

    # Calculate MACD line
    macd_line = short_ema - long_ema

    # Calculate signal line
    signal_line = macd_line.ewm(span=signal_window, adjust=False).mean()

    # Add MACD and signal line columns to the dataframe
    df['MACD'] = macd_line
    df['Signal_Line'] = signal_line

    return df


df = pd.read_csv('ohlcv.csv')
print(df)
df = df.rename(columns={"timestamp": "time", "Open": "open", "High": "high", "Low": "low", "Close": "close","Volume": "volume"})
# Convert the 'timestamp' column to a datetime object
df['time'] = pd.to_datetime(df['time'], utc=True)
df['time'] = df['time'] - timedelta(hours=5)

# Calculate RSI
df = calculate_rsi(df)

# Calculate MACD
df = calculate_macd(df)

```
