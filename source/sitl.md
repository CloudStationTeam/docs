# SITL Deployment
For SITL deployment, you may want to start another Windows ec2 and connect to it by rdp, then
1. download https://github.com/ArduPilot/ardupilot/blob/master/Tools/environment_install/install-prereqs-windows.ps1, then right click to run it by ps (to download cygwin and mavproxy).
   
    ```
    curl https://raw.githubusercontent.com/ArduPilot/ardupilot/master/Tools/environment_install/install-prereqs-windows.ps1
    ```
2. open cygwin.exe on desktop.
3. download ardupilot (and update pymavlink).
   
    ```
    git clone https://github.com/ArduPilot/ardupilot.git
    cd ./ardupilot
    git submodule update --init --recursive
    ```
4. download https://github.com/CloudStationTeam/sitl_deployment/blob/main/start_sitl_fleet.sh and run it.
   
    ```
    cd ..
    curl https://raw.githubusercontent.com/CloudStationTeam/sitl_deployment/main/start_sitl_fleet.sh
    bash start_sitl_fleet.sh (add options)
    ```
start with 1 first. \
Then connect to it (14550+) on Cloud Station.
\
\
\
Tips:

For custom-location, you could use `-L` or `-l`.

For lidar sim, you could do `-A --uartF`, for example ```./ardupilot/Tools/autotest/sim_vehicle.py -A --uartF=sim:sf45b```. Then in mavproxy, do
```
param set SERIAL5_PROTOCOL 11
script /tmp/post-locations.scr
```
.

For battery param, you could do ```param set SIM_BATTERY 100``` in mavproxy.

