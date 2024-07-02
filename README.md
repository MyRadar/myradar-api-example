
MyRadar API Weather Forecasting Suite

API documentation and tutorials designed to jump start your weather application development needs.

Quick Start
===========

```python
   # you will need to have these dependencies available in your python interpreter
   import pandas as pd
   import requests

   MYRADAR_API_KEY = "YOUR_KEY"
   headers = {
    "Subscription-Key":MYRADAR_API_KEY
    }

   url = f"https://api.myradar.dev/v1/timeseries/point?latlons=[[41.5,-105.1],[36.1,-96.34]]&as_csv=true"
   c = requests.get(url, stream=True, headers=headers) # make a request
   df = pd.read_csv(c.raw, parse_dates=['valid_time'])
```

Tutorials
=========

* Tutorials and Walk Throughs
  * [Make a point request and display a plot](make-a-point-request-and-display-a-plot.md)
  * [Get a route forecast and make a plot](get-a-route-forecast-and-make-a-plot.md)
  * [Make a Plot of Historical Data](make-a-plot-of-historical-data.md)
  * [Build a Power Prediction Model](build-a-power-prediction-model.md)
  * [Get a route from points](get-a-route-from-points.md)
  * [Get a time table for a route](time-table.md)
* Data Dictionaries and Reference
  * [Points](endpoints.md#points)
  * [Routes](endpoints.md#routes)
  * [Historical Data](endpoints.md#historical-data)
  * [Reference](endpoints.md#data-dictionary)
