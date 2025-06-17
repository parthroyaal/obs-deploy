## Typing rules

* see in froward
* type in burst 
* pre need auto location withtiout seeing both hand use j and k refrence 

# Dates

* [\__15JUN2025](__15JUN2025.md)

## junetypeFin

* [\__baseApp](__baseApp.md)

## service01

* nSpotBkdb
* nCeBkdb
* nPeBkdb
* live
* bk

service02
bybitBkdb

service03
tgramScreenr

[\__lwchart](__lwchart.md)

````js

<script src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/lightweight-charts.standalone.production.js"></script>

````

[\__advchart](__advchart.md)

````js

<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/charting_library/charting_library.standalone.js"></script>

````

````txt
after adding div container and script for lib   in  new script block declare widget object by calling tvchart widget constuctor before that define Datafeed varibable object    having fn signatures namely  : onReady passing supported resolutions list , resolveSymbol with some args 


      widget.onChartReady(() => {

        // Set the visible range to your data's range

        const firstBar = bars[0].time;

        const lastBar = bars[bars.length - 1].time;

        widget.activeChart().setVisibleRange({ from: firstBar, to: lastBar });

      });

    };
   
````
