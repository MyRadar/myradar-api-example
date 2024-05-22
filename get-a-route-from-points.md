---
slug: "/apidocs/get-a-route-from-points"
date: "2023-04-24"
title: "Get a Route Forecast from Points"
---


## Route From Points

A route is most simply a list of ```[lat, lon, time]```.  The ```/point``` endpoint can be used to query a route using a JSON array of ```[lat,lon,time]``` arrays where the ```time``` value in the array is a UTC timestamp.

## Example

```python
import time
import json
import requests

depart_time_str = "20230430.0000"
depart_time = time.mktime(time.strptime(depart_time_str, "%Y%m%d.%H%M"))

my_route = [
  [40.272097, -105.556849, depart_time],
  [40.275028, -105.564764, depart_time+2800],
  [40.270215, -105.578737, depart_time+5600],
  [40.268147, -105.587308, depart_time+7800],
  [40.267442, -105.611906, depart_time+9200]
]

points_str = json.dumps(my_route, separators=(',', ':')) # get a json str for the URL parameter

url = f"https://api.myradar.dev/v1/timeseries/point?latlons={points_str}" # format the URL

headers = {
    "Subscription-Key": MYRADAR_API_KEY
    }

response = requests.get(url, headers=headers)
route_data = response.json()

for i in range(len(my_route)):
    temperature = route_data['t'][i]
    point_time_str = route_data['valid_times_str'][i]
    print(f"temperature will be {temperature} C when you arrive at point {my_route[i][:2]} at {point_time_str}")
```

With a current time of ```Wed 03 Aug 2022 18:27:08 UTC``` you should see the following output:

```
temperature will be 17.6678 C when you arrive at point [40.272097, -105.556849] at 20220803 18:27:00
temperature will be 18.319 C when you arrive at point [40.275028, -105.564764] at 20220803 19:13:40
temperature will be 18.7466 C when you arrive at point [40.270215, -105.578737] at 20220803 20:00:20
temperature will be 17.6446 C when you arrive at point [40.268147, -105.587308] at 20220803 20:37:00
temperature will be 11.2602 C when you arrive at point [40.267442, -105.611906] at 20220803 21:00:20
```
