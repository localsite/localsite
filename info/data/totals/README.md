
# Input-Output Totals

## Samples of Merging Input-Output JSON Data for Totals

1. [New Commodity Chart](/useeio.js/test/example_tabulator.html) from Micheal ([Fork without COR error](https://github.com/modelearth/useeio.js) [Github](https://github.com/USEPA/useeio.js/blob/dev/test/example_tabulator.html)) - Two JSON files are combined in Javascript and displayed with Tabulator. Javascript multiplies the total commodity output `q` and the `jobs` indicator values per 1 USD output from&nbsp;[matrix&nbsp;`D`](../../../../io/about/matrix/) - Update to show matrix D data grid.<br><br>**Occasional CORS Issue:** Resolved by updating the API endpoint and model in config.js<br><!--<span style="color:red">The link above does not work due to a CORS restriction on the API: 'https://smmtool.app.cloud.gov/api/USEEIOv2.0/matrix/q' from origin 'http://localhost:8887' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.</span>-->

2. View the same chart in a [React page with Tabulator](https://model.earth/useeio-widgets/src/html/jobsTable.html) from Leo ([Github](https://github.com/TheTisiboth/useeio-widgets/blob/IoChartTabulator/src/html/jobsTable.html)).<br><br>**To Fix:** The following error can be fixed by using Leo's branch containing useeio.jobsGrid<br><span style="color:red">useeio.jobsGrid is not a function</span>

## Update to Inflow-Outflow Chart

In the EPA's [USEEIO-widgets repo](https://github.com/USEPA/useeio-widgets/), the existing [Inflow-Outflow Chart](/io/charts/) (which uses Material UI) will be updated to displaying totals by default, <span style='white-space: nowrap;'>rather than just per-dollar.</span>

Here a [NYT Paywall article](https://www.nytimes.com/interactive/2022/12/13/climate/climate-footprint-map-neighborhood.html) on a Footprint Calculator from UC Berkeley CoolClimate Network’s EcoDataLab spinoff which uses USEEIO data.  
