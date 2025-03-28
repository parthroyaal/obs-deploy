
```python
import pandas as pd

  

fno_symbolList = pd.read_csv('https://public.fyers.in/sym_details/MCX_COM.csv', header=None, usecols=range(17))

  

# Explicitly named columns

named_columns = ['FyersToken', 'Name', 'Instruemnt', 'lot', 'tick', 'ISIN', 'TradingSession', 'Lastupdated', 'Expirydate', 'Symbol']

  

# Assign names to the first 10 columns

fno_symbolList.columns = named_columns + list(range(17-len(named_columns)))  

  
  

# Print all column names

print(fno_symbolList.columns.tolist())
```

```python
fno_symbolList.to_csv("MCX.csv")
  

df = pd.read_csv("MCX.csv")

df.head()
```
