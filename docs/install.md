# Installation

For this hackathon OSRM is installed on a dedicated server, hosted in Germany, 
running Ubuntu 16.04.

* OSRM is used in version 5.3
* OpenStreetMap is available for whole Japan
* Routing profiles are available for `car`, `bicycle`, `foot` and `wheelchair`
* The HTTPS connection uses certificates provided by Let's Encrypt
* NGinx is used to make all profiles available through port 443

## OSRM Server

First software dependencies must be installed:

```
sudo apt install build-essential git cmake pkg-config \
      libbz2-dev libstxxl-dev libstxxl1v5 libxml2-dev \
      libzip-dev libboost-all-dev lua5.1 \
      liblua5.1-0-dev libluabind-dev libtbb-dev
```

Then checkout the project repository, in our case v5.3 branch:

```
git clone https://github.com/Project-OSRM/osrm-backend.git --branch 5.3 
cd osrm-backend
```

Compile OSRM using CMake:

```
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build .
cmake --build . --target install
```

OSRM binaries should be now installed in `/usr/local/bin/`.


## Routing Profiles

As routing profiles we will use 3 profiles, that are included by default (`car`, 
`bicycle` and `foot`), and a modified profile for `wheelchair`, that can be found
in the `profiles` directory of this repository.
(Derived from https://github.com/species/osrm-wheelchair-profiles, see 
[wheelchair.lua](../profiles/wheelchair.lua).)

Make all profiles available in the `osrm-backend/profiles` directory.

## Data Preparation

First create a directory `data` in the repository root directory.

```
cd path/to/osrm-backend
mkdir data
cd data
```

Then download OSM data from Geofabrik's download server. In our case we download
OpenStreetMap extract of Japan in `.pbf` format:

```
wget http://download.geofabrik.de/asia/japan-latest.osm.pbf
```

After downloading the data it needs to be processed, which can take a while.
The first step is extracting the data, and it must be done separately for each
profile. The following is the command for the `car` profile:

```
osrm-extract japan-latest.osm.pbf -p ../profiles/car.lua
```

After extraction follows the contraction:

```
osrm-contract japan-latest.osrm
```

After completion we can test OSRM server by starting it with:

```
osrm-routed japan-latest.osrm --port 5000
```

**Note:** If we want to provide routing for multiple profiles, we need to run 
`osrm-routed` multiple times under different port numbers and with different 
data files, which need to be organized (named) accordingly.

## Running OSRM continuously 

To keep OSRM running continuously `supervisor` (http://supervisord.org/) is used 
with the following settings, in this example for the `wheelchair` profile:

```
[program:"osrm-wheelchair"]
directory=/path/to/data
user=osrm
command=/usr/local/bin/osrm-routed wheelchair-japan-latest.osrm
  --port 5000 
autostart=true
autorestart=true
stdout_logfile=/srv/bin/osrm-wheelchair.log
stderr_logfile=/srv/bin/osrm-wheelchair.error.log
```

See the [Docmentation](use.md) for usage notes.
