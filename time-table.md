---
slug: "/apidocs/time-table"
date: "2023-07-23"
title: "Get a Time Table"
---


## Time Table

A time table is a route forecast for several different departure times


## Example

```python
import requests
import time

lat1, lon1 = 41.5, -105.1
lat2, lon2 = 36.1, -96.1

MYRADAR_API_KEY = "YOUR_KEY"
headers = {
  "Subscription-Key":MYRADAR_API_KEY
}

url = f"https://api.myradar.dev/v1/timeseries/timetable?origin={lat1},{lon1}&destination={lat2},{lon2}"

conn = requests.get(url, headers=headers)
data = conn.json()

for i in range(len(data['depart_times'])):
    fcst_data = data['route_forecasts'][i]
    depart_time = data['depart_times'][i]
    depart_time_str = time.strftime("%Y%m%d.%H%M", time.gmtime(depart_time))
    if fcst_data is None:
        continue
    max_temp = max(fcst_data['t'])
    min_temp = min(fcst_data['t'])
    print(depart_time_str, max_temp, min_temp)
```

```
20230724.0433 32.83 16.8054
20230724.0533 34.3944 16.4861
20230724.0633 36.2062 15.9421
20230724.0733 37.8701 15.3039
20230724.0833 39.173 14.7036
```