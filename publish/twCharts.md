
[[v1]]

[[v2]]

## 


```js
getBars: (symbolInfo, resolution, from, to, onData, onError) => {
            fetch(`/historic?from=${from}&to=${to}&resolution=${resolution}`)
                .then(r => r.json())
                .then(data => {
                    const bars = data.map(d => ({
                        time: d.time * 1000,
                        open: d.open,
                        high: d.high,
                        low: d.low,
                        close: d.close,
                        volume: d.volume || 0
                    })).filter(b => b.time >= from * 1000 && b.time <= to * 1000);
                    
                    onData(bars, {noData: bars.length === 0});
                })
                .catch(err => onError('Failed to load historical data'));
        },
```

```js
getBars: (symbolInfo, resolution, periodParams, onHistoryCallback, onErrorCallback) => {
        const { from, to } = periodParams;
        
        fetch(`/historic?from=${from}&to=${to}&resolution=${resolution}`)
            .then(r => r.json())
            .then(data => {
                const bars = data.map(d => ({
                    time: d.time * 1000,
                    open: d.open,
                    high: d.high,
                    low: d.low,
                    close: d.close,
                    volume: d.volume || 0
                })).filter(b => b.time >= from * 1000 && b.time <= to * 1000);
                
                onHistoryCallback(bars, { noData: bars.length === 0 });
            })
            .catch(err => onErrorCallback('Failed to load historical data'));
    },
```


The error is occurring because the signature of your `getBars` function does not match the one expected by TradingView’s widget. In your code, you’ve defined:


getBars: (symbolInfo, resolution, from, to, onData, onError) => { … }


However, TradingView’s charting library calls `getBars` with the following parameter order:



getBars(symbolInfo,resolution,periodParams,onHistoryCallback,onErrorCallback)

where periodParamsperiodParams is an object containing {from,to,firstDataRequest}{from,to,firstDataRequest}. In your function:

- The third parameter (which should be an object) is assigned to the variable from (expected to be a number).
- The fourth parameter (expected to be a function) becomes to.
- The fifth parameter gets assigned to onData (which actually becomes the error callback in your code).
- The sixth parameter (i.e. onError) ends up being undefined.

So when you call onError('Failed to load historical data') in your .catch block, you are effectively trying to call an undefined value as if it were a function, resulting in the error:



TypeError: onError is not a function

Adjust the function signature of getBars to correctly destructure the periodParams object and use the proper callback names. For example:

tv1.html1/1

- Function Signature:

The parameters have been renamed to reflect their purpose:

- periodParams (object with from and to)

- onHistoryCallback (the callback to send the historical bar data)

- onErrorCallback (the callback to handle errors)

- Destructuring periodParams:

We now extract from and to from the passed periodParams object.

- Using Correct Callbacks:

Instead of calling onData and onError, we’re calling onHistoryCallback and onErrorCallback, which are correctly passed by the TradingView widget.

## Summary

The error arose because the onError callback was not correctly received due to a parameter mismatch. By adjusting the getBars signature to match the expected TradingView API (and extracting from and to from the periodParams object), the callback functions are properly assigned, and the error will be resolved.



# GetBars issue 
- Issue: The candle timestamps from the /historic endpoint don’t match the requested range, so the filtering code returns an empty array.

- Fix: Either update your backend to generate candle data for the requested time range or remove/adjust the filtering logic in getBarsgetBars so that it returns the candles you have.

By applying either of these fixes, the TradingView chart should display the candles before any updates occur.
Fix : remove/adjust the filtering logic in getBarsgetBars so that it returns the candles you hav
OR :  update your backend to generate candle data for the requested time range
