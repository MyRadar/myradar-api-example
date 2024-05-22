---
slug: "/apidocs/endpoints"
date: "2022-11-24"
title: "Endpoints"
---

<h2 id=points>Points</h2>

The "points" endpoint can be used for getting a single forecast cycle for one or more points.  For every point that is requested, you will receive a time series for each weather variable provided.  This endpoint has the following parameters:

  + latlons - 2d array of lat/lon points as JSON string
  + itime - date string in format YYYYmmdd.HHMM that will be the begining time of the forecast, can be in the past, default to current time
  + as_csv - if 'true' will return CSV data rather than JSON

The JSON response for a point request has the following structure:

    data : {
      valid_times:[...],
      cloud_cover:[[...]],
      t:[[...]],
      ...
    }

Each weather variable below 'data' will contain a 2d array whose length on the first dimension is the number of points requested.  The 'valid_times' variable is an array of epoch timestamps that matches the length of the time series data.  Each point's data will be at its corresponding index in the array.  See data dictionary below for description of variables and units

Alternatively, the ```points``` endpoint can be used to get a route if the ```latlons``` array provided is an array of ```[lat, lon, time]```.  Adding the time to each point will result in the response being a 1d time series for each variable where each point in the time series corresponds to each point in the route provided at the time requested.  In this scenario the ```itime``` parameter is not needed and will be ignored.  This will return a similar dictionary as above but each variable will be one dimension where each value is the valid time requested in the ```latlon``` array.

<h2 id=routes>Routes</h2>

The "routes" endpoint will perform a driving directions lookup between an origin and destination then provide a single time series that represents the weather forecast at each point along the route.  Unlike the 'points' endpoint, the 'routes' endpoint will always return only one time series for each weather variable.  This endpoint accepts the following parameters:

  + origin        - lat,lon pair for the origin point (eg 41.5,-105.1)
  + destination   - lat,lon pair for the destiantion point
  + itime         - initial time for the forecast (aka departure time)
  + include_route - if 'true' then include the complete driving directions response from mapping engine
  + as_csv        - if 'true' return results as CSV rather than JSON

The JSON response for a route request has the following structure:

    routes:[{
      valid_times:[...],
      points:"...",
      time_series:{
        cloud_cover:[...],
        t:[...],
        ...
      }
    }]

Each weather variable below 'time_series' will contain a 1d array that is the length of the route as provided by the driving directions enging.  'valid_times' is a list of epoch timestamps that represents the time the mapping engine believes you will arrive at that waypoint.  The 'points' string is an encoded polyline that contains the lat/lon points the mapping engine provided.


<h2 id=historical-data>Historical Data</h2>
The 'histdata' endpoint builds a continuous time series of data for one or more points between a begin time and end time.  This endpoint accepts the following parameters:

  + begin_time (url path parameter) - begin time in format YYYYmmdd.HHMM
  + end_time (url path parameter) - end time in format YYYYmmdd.HHMM (max 30 days)
  + latlons - 2d array of lat/lon points as JSON string (max 50 points)

This endpoint will return a NetCDF with the following structure

```
netcdf data {
dimensions:
        vtime_str = 99 ;
        string13 = 13 ;
        var = 17 ;
        point = 3 ;
        vtime = 108 ;
        string36 = 36 ;
        string11 = 11 ;
        string8 = 8 ;
variables:
        double vtime(vtime) ;
                vtime:_FillValue = NaN ;
        char vtime_str(vtime_str, string13) ;
                vtime_str:_Encoding = "utf-8" ;
        char var(var, string11) ;
                var:_Encoding = "utf-8" ;
        double data(var, point, vtime) ;
                data :_FillValue = NaN ;
        char run_time(string8) ;
                run_time:_Encoding = "utf-8" ;
        char point(point, string36) ;
                point:_Encoding = "utf-8" ;
data:

```

<h2 id=timetable>Time Table</h2>

```
POST /timetable?origin={lat1,lon1}&destination={lat2,lon2}
```

This endpoint will look up the driving route and provide the forecast over a period of departure times

```
  {
    "depart_times": //list of departure timestamps
    "route_forecasts": //list of route forecasts that match the depart_times
  }
```


<h2 id=data-dictionary>Data Dictionary</h2>

The 1d time series data dictionary is as follows:

## Data Dictionary

| Variable | Type | Definition |
| -- | -- | -- |
| rt | array | Road Temperature (C) |
| rc | array | Road Condition (0-10) |
| t | array | Air Temperature (C) |
| wspd | array | Wind Speed (m/s) |
| wdir | array | Wind Direction (0-360) |
| snod | array | Snow Depth (m) |
| prate | array | Precip Rate (m/s) |
| points | encoded str | Polyline encoded string of route points |
| valid_times | array | Timestamp of weather forecast, derived from DirectionsReport (seconds since epoch) |
| ptype | array | Precip Type 1=Wet,2=Mix,3=Frozen |
| delay_risk | array | Delay risk score decimal 0-3 |
| rc_colors | array | Colors for the road condition values that match the table below |
| cloud_cover | array | Cloud cover 0-100 |
| rh | array | Relative Humidity 0-100 |
| rad | array | Downward Short-Wave Radiation Flux w/m^2 |
| d_snod | array | Lagged snow depth |
| d_prate | array | Lagged precip rate |
| d_rad | array | Lagged rad |


## Road Condition Types

| Code | Value |
| -- | -- |
| DRY | 0|
| WET_SNOW | 1|
| WET_MELT | 2|
| WET_RAIN | 3|
| WET_PREV | 4|
| MIXED_FRZR | 5|
| MIXED_SNOW | 6|
| MIXED_MELT | 7|
| FRZ_SNOW | 8|
| FRZ_PREV | 9|
| FRZ_BLKICE | 10|

|Code|Definition|
|--|--|
| WET_SNOW | “Wet due to snow melting on contact”|
| WET_MELT | “Wet due to melting snow”|
| WET_RAIN | “Wet due to rain”|
| WET_PREVIOUS_RAIN | “Patches of wet due to previously fallen rain”|
| MIXED_FREEZING_RAIN | “Mixed (or slushy) conditions due to freezing rain”|
| MIXED_SNOW | “Mixed due to snow beginning to freeze”|
| MIXED_MELT | “Mixed due to slushy melting snow”|
| FROZEN_SNOW | “Frozen from snow accumulating”|
| FROZEN_PREV_PRECIP | “Patches of frozen from snow that fell previously”|
| FROZEN_BLKICE | “Potential for patches black ice (melted snow with refreezing)|

|Color|Code|Value|
| -- | -- | -- |
|"#ffffff"| DRY | 0|
|"#90EE90"| WET_SNOW | 1|
|"#32CD32"| WET_MELT | 2|
|"#008000"| WET_RAIN | 3|
|"#006400"| WET_PREV | 4|
|"#800080"| MIXED_FRZR | 5|
|"#9932CC"| MIXED_SNOW | 6|
|"#EE82EE"| MIXED_MELT | 7|
|"#87CEFA"| FRZ_SNOW | 8|
|"#00BFFF"| FRZ_PREV | 9|
|"#d3d3d3"| FRZ_BLKICE | 10|

