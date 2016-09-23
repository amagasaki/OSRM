# Hackathon - Routing for Wheelchair Users

The goal for the Amagasaki "Routing" Hackathon is to provide technology for 
custom routing for wheelchair users.

The focus of the Hackathon lies on the development of client-side applications, 
especially for mobile phones. Therefore a routing service with a ReSTful API is
provided to the participants.

## Routing API Services

When it comes to routing services, there is a large amount of choices. But their
target use case is usually motor vehicles, sometimes cyclists or pedestrians. 
For wheelchair users though the number of choices is tiny, the availability of 
suitable routing data is another problem.

### Open Route Service

**Open Route Service** (http://openrouteservice.org/) is one available service,
that provides routing profiles for wheelchair users. Its API provides a ReSTful
interface based on the **OpenLS** standard (http://openls.geog.uni-heidelberg.de/).
However, the server-side implementation of Open Route Service is not release 
under an Open Source license, and the project relies on research activities of
the university department.

Open Route Service requires an API key, which we received on request for this 
Hackathon. You can use the following API Key in your application:

```
ee0b8233adff52ce9fd6afc2a2859a28
```

This key has to be specified in a request such as
http://openls.geog.uni-heidelberg.de/route?api_key=ee0b8233adff52ce9fd6afc2a2859a28

### pgRouting

**pgRouting** (http://pgrouting.org/) extends the PostGIS / PostgreSQL 
geospatial database to provide geospatial routing functionality. Data changes 
can be reflected instantaneously through the routing engine. There is no need 
for pre-calculation. The “cost” parameter can be dynamically calculated through 
SQL and its value can come from multiple fields or tables.

There is a workshop (http://workshop.pgrouting.org/) to help you to get started,
and in general pgRouting should be suitable for wheelchair routing.
However, pgRouting is a low-level library, and it requires significant effort to 
develop an API service. This would be beyond the available time of this 
Hackathon.

### Graphhopper

**Graphhopper** (https://github.com/graphhopper/graphhopper) is an Open Source 
routing library and server using OpenStreetMap.

GraphHopper supports several routing algorithms like Dijkstra and A* and its 
bidirectional variants. Furthermore it allows you to use Contraction Hierarchies 
(CH) very easily, we call this speed mode and in contrast to the speed mode we 
call everything without CH the flexibility mode.

I was not able to find suitable information about wheelchair routing for 
Graphhopper though.

### Open Source Routing Machine (OSRM)

**Open Source Routing Machine (OSRM)** (http://project-osrm.org/) is a high 
performance routing engine written in C++11 designed to run on OpenStreetMap 
data. 

OSRM allows flexible import of OpenStreetMap data, handles continental sized 
networks within miliseconds, and it upports car, bicycle, walk modes, but is 
easily customized through profiles as well.

The project has a large user community, and there exist routing profiles for 
wheelchair users provided by Michael Maier: 
https://github.com/species/osrm-wheelchair-profiles
(Note: unfortunately these profiles are not 100% compatible with the recent
version of OSRM.)

OSRM is relatively easy to setup in a short time. Therefor OSRM is the 
preferred choice for this Hackathon.

Find more information on the following pages:

* [OSRM Installation](install.md)
* [API Documentation](use.md)

Have fun!