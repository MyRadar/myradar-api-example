---
slug: "/apidocs"
date: "2022-11-24"
title: ""
---

MyRadar API Weather Forecasting Suite

API documentation and tutorials designed to jump start your weather application development needs.

Quick Start
===========

```python
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
  * [Make a point request and display a plot](/apidocs/make-a-point-request-and-display-a-plot)
  * [Get a route forecast and make a plot](/apidocs/get-a-route-forecast-and-make-a-plot)
  * [Make a Plot of Historical Data](/apidocs/make-a-plot-of-historical-data)
  * [Build a Power Prediction Model](/apidocs/build-a-power-prediction-model)
  * [Get a route from points](/apidocs/get-a-route-from-points)
  * [Get a time table for a route](/apidocs/time-table)
* Data Dictionaries and Reference
  * [Points](/apidocs/endpoints#points)
  * [Routes](/apidocs/endpoints#routes)
  * [Historical Data](/apidocs/endpoints#historical-data)
  * [Reference](/apidocs/endpoints#data-dictionary)