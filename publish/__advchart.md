```js
: console.log(TradingView.version());
CL v25.002 (internal id ea00b60e @ 2023-07-12T14:51:16.080Z)
```

#  v25.002


https://medium.com/@zeeshankhan8838/advance-trandingview-charting-library-integration-in-angular-f89d4bf12992

https://medium.com/@jonchurch/tradingview-js-api-integration-tutorial-introduction-5e4809d9ef36

https://medium.com/@jonchurch/tradingview-charting-library-js-api-setup-for-crypto-part-1-57e37f5b3d5a

https://medium.com/@jonchurch/tv-part-2-b8c2d4211f90



######################################


1. make chart-container div  and   add script src for library  


```html
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/charting_library/charting_library.standalone.js"></script>
```
2. in script section add widget object using TV Widget construct and pass option object with keys like
```js
{

    container_id: "container",

    datafeed: Datafeed,

    library_path: "https://cdn.jsdelivr.net/gh/parth-royale/cdn@main/charting_library/charting_library.standalone.js",

    symbol: "TMP",

    interval: "5S",

    timezone: "Asia/Kolkata", //"Etc/UTC"        ,

    theme: "dark",

    locale: "en",

    toolbar_bg: "#f1f3f6",

    hide_side_toolbar: false,

    allow_symbol_change: false,

    container_height: 600,

    save_image: false,

    details: true,

    studies: [

        "BB@tv-basicstudies",

        "Moving Averages@tv-basicstudies",

        "Volume@tv-basicstudies",

        "RSI@tv-basicstudies",

    ]

}
```