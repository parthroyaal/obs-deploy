import os
import csv

def update_csv(record):
\# Define the filename and the expected columns
csv_file = 'btc_data.csv'
fieldnames = \["price", "timestamp"\]

````
# Check if the file exists
file_exists = os.path.isfile(csv_file)

# Open the file in append mode and write the record
with open(csv_file, mode='a', newline='') as file:
    writer = csv.DictWriter(file, fieldnames=fieldnames)

    # Write the header if the file is new
    if not file_exists:
        writer.writeheader()

    # Write the data row
    writer.writerow(record)
````

Pandas (Less Efficient for Frequent Updates):

Every write operation requires loading the entire file into memory, appending data, and rewriting the file.
Overhead increases as the file size grows.
Easy to manipulate and analyze data (e.g., filtering, sorting, etc.).

CSV Module (More Efficient for Streaming Updates):
Writes directly to the file without loading the entire file into memory.
Minimal memory usage and faster for simple write operations.

Limited to basic write operations (you cannot analyze or manipulate data as easily).

Imperative Way (Non-ES6) js  code 

---

# Theory:

For nservice56=botService except : 

blocks Used :

1. Live tick to candle & tick bk app + render route   
   mdiagram:

## Structure

````mermaid
TD 
````

````py
def tickToCandle():
	pass



````

2. for render  route ChartBLocks

for service56 :

blocks Used :

For :
prototypeProgss : 
[plotlyBase](plotlyBase.md)
[servcie12](servcie12.md)=bybitLive&bk
[service34](service34.md)=fyers/angleOneLive&bk
[servcie56](servcie56.md)=angleOneEqlistLive&bk
*service78*=oandaApiLive&BknwsTicks
*service910*=bybitBotLive&bk

[jsStuff](jsStuff.md)
