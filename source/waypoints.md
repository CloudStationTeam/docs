
### About How to use Waypoints. And [Demo1](https://youtu.be/aSjZk-p0Dkg) and [Demo2](https://youtu.be/mtgL0nYEi3Y).

When SITL runs slow, it maybe better to use 2 devices for SITL simulation and CloudStation Deployment.

#### Example Waypoints:
\<address\> \
\<lat\>, \<lon\>, \<alt\> (optional, default alt is at the ground level.) 

#### Example Waypoints for the Demo: 
Donald Bren Hall, Irvine, CA, USA. \
Calit2, Irvine, CA, USA. \
A random waypoint on the map nearby. \
RapidTech, Campus Drive, Irvine, CA, USA. 

#### Example Fly_To Waypoint for the Demo: 
A random waypoint on the map nearby.

### Explanation for the Demo:

#### Waypoints:

Donald Bren Hall, Irvine, CA, USA. (It's the return location. It could also be the start location.) \
Calit2, Irvine, CA, USA. (Waypoint 1) \
A random waypoint on the map nearby. (Waypoint 2) \
RapidTech, Campus Drive, Irvine, CA, USA. (Waypoint 3).

Demo1: It went to Waypoint 1, then Waypoint 2, and then Waypoint 3. And then Land.

Demo2: It went to Waypoint 1, then Waypoint 2, and then Waypoint 3. And then Land.

#### Fly_To Waypoint:

A random waypoint on the map nearby. (Waypoint 1).

Demo1: It went to Waypoint 1, and then Land.

Demo2: It went to Waypoint 1, and then Land.

### About How to use Waypoints:

#### Waypoints:

Confirm that it's connected to a drone. Send an example message first. For example, set mode to GUIDED. \
Select several waypoints. \
Send the waypoints. \
Set mode to AUTO, when SITL tells you to do it. If SITL shows a message about "switch to AUTO", then you could set mode to AUTO. If it doesn't work, then set mode to AUTO multiple times. \
It'll go to the waypoints.
Set mode to Land.

#### Fly_To Waypoint:

Confirm that it's connected to a drone. Send an example message first. For example, set mode to GUIDED. \
Select a waypoint on the map. \
Click fly_to. \
Set mode to AUTO, when SITL tells you to do it. If SITL shows a message about "switch to AUTO", then you could set mode to AUTO. If it doesn't work, then set mode to AUTO multiple times. \
Now it could fly to a waypoint. \
Set mode to Land.

That's all for waypoints.

#### Explanations for UI:
"Add": add a waypoint to a drone. \
"Update": update waypoints to a drone. \
"Send": send waypoints to a drone. \
"Remove": remove a waypoint for a drone. \
"Clear All": clear all waypoints for a drone. \
"Show All Waypoint Lists": show all waypoints for all drones. \
"Clear All Waypoint Lists": clear All waypoints for all drone. \
There could be different drones. 

Notes. \
"Note in SITL the drone will only arm in guided mode. However, for real drones, they will arm in other modes also."


