# How to Use

After successful [installation](install.md) of OSRM we can use its HTTP API.

OSRM provides 6 service types:

* `route`: fastest path between given coordinates
* `nearest`: returns the nearest street segment for a given coordinate
* `table`: computes distance tables for given coordinates
* `match`: matches given coordinates to the road network
* `trip`: computes the fastest round trip between given coordinates
* `tile`: return vector tiles containing debugging info

The OSRM Documentation gives a detailed description of the API: 
https://github.com/Project-OSRM/osrm-backend/blob/5.3/docs/http.md

## Demo Server

The demo server is running 4 instances of OSRM, providing 4 different routing
profiles: `car`, `bicycle`, `foot` and `wheelchair`.
Each instance runs under a different port number and Nginx proxies each port to
a different URL endpoint.

* `car`: https://osrm.georepublic.net/car/<...>
* `bicycle`: https://osrm.georepublic.net/bicycle/<...>
* `foot`: https://osrm.georepublic.net/foot/<...>
* `wheelchair`: https://osrm.georepublic.net/wheelchair/<...>

Therefor the request URL follows the following pattern, which is slightly 
different to the OSRM documentation:

```
https://osrm.georepublic.net/{profile}/{service}/{version}/<...>
```

**Note**: the `{profile}` URL segment of the original request URL seems to have
no effect, even if the documentation says so: "Mode of transportation, is 
determined by the profile that is used to prepare the data".

## Sample Requests

* Car route with full overview:
  ```
  https://osrm.georepublic.net/car/route/v1/driving/135.43710,34.73392;135.42626,34.72967?overview=full
  ```
* Wheelchair route with step-by-step guidance, annotations and geometries in 
  GeoJSON format: 
  ```
  https://osrm.georepublic.net/wheelchair/route/v1/driving/135.43710,34.73392;135.42626,34.72967?overview=full&steps=true&geometries=geojson&annotations=true
  ```

## Helpful Tools

* [Interactive Polyline Encoder Utility](https://developers.google.com/maps/documentation/utilities/polylineutility)
* [Polyline encoding and decoding in Javascript](https://github.com/mapbox/polyline)
* Built-in [Vector Tile Support](https://github.com/Project-OSRM/osrm-backend/blob/5.3/docs/http.md#service-tile)

