# How to Contribute
This contributor guide aims to list all the technologies used for this project to help student developers get started quickly. If 
you just want to quickly set up a development environment, please follow the instructions in **WebApp->Getting Started**.  

## Technologies
1. Hardware:    
    The focus of the project isn't on the hardware. However, CloudStation is built for the hardware so it is important to have a good understanding of how it works and where to find answers if you run into issues.  

    1. [Ardupilot](https://ardupilot.org/ardupilot/) (autopilot software) 
        * [Mission Planner](https://ardupilot.org/planner/index.html)
            * You will find it helpful to debug with this GCS that runs on local computer
    2. [MAVLink](https://mavlink.io/en/): messaging protocol used for drone-to-drone and drone-to-GCS communication
        * documentation for all message types can be found here 

2. Web development
    * Front end: html, javascript, bootstrap, [MapBox](https://www.mapbox.com/)
    * Back end: Django
        * SQLite is used in the code for simplicity. Django provides interfaces for various flavors of SQL databases. We also have instructions in **Deployment -> AWS RDS** to help you get started with AWS RDS
3. Deployment  
    Please refer to the bash scripts in [CloudStation Deployment](https://github.com/CloudStationTeam/cloud_station_deployment). Some technologies we used are:
    * NGINX, Daphne

## General Notes
1. The front end currently uses vanilla JS. It would be best to refactor it in a way that is more sustainable for future development. Using some kind of framework (Angular, React, Vue) should help.
2. **Back End**
    * The APIs we implemented don't follow the popular RESTful style. We decided to make them structured in a way that's very similar to MAVLink commands/messages. It makes initial development a lot easier but I imagine it will make it difficult to add some more advanced features. I think having RESTful APIs actually does make sense for this project. An example of the resources would be a JSON document of all the telemetry information of the vehicle. The backend should send the correct command according to the current speed in that document, etc. This design should make it easier to implement the front end and makes it more structured.
3. **Security**
    * This is probably the biggest problem of the current implementation. I don't have any experience in this field so I can't provide much information.
    * Currently, CloudStation cannot be used safely for multiple users. User accounts do not actually link drones to their respective accounts, and there are no safeguards in place to prevent one user from entering the drone ID of a different user. Editing the additional telemetry data displayed in "Other Data" will also affect settings for all users.
4. **Debug**
    * To reload the server, you could do `bash reload_server.sh`. However, if you updated requirements.txt, you have to do `rm -r ENV` and deploy everything again.
    * You could use print statements or Python logging, however, it's recommended to use Python logging because it's a good practice. You could configure the logs in settings.py. The logs are not displayed by Django even if you run it in front-end, it's configured by daphne.service. So after you reload the server, you could do `journalctl -u daphne.service | tail` for print statements. You may output it to a log file or use Splunk for it to be more convenient.
    * Debugger Tools:
      FG: browser debugger tools, log msgs;
      BG: print logs, website log msgs, SITL console, etc.

    
## Feature Improvements
1. Enhancing multi-user support and security (see General Notes - Security)
    * Saving drone IDs for each user
    * Some sort of safeguard to prevent a user from controlling a drone that is not theirs
    * Saving telemetry data displayed in "Other Data" individually for each user
2. There is currently no support for takeoff, landing, etc., so vehicle types other than rovers cannot be controlled through CloudStation.
3. The fly-to feature currently sets all fly-to points to 0 altitude, which means it can only be used for rovers. If support for flying vehicles is added, the fly-to feature will need altitude controls.
4. Waypoint missions are currently partially implemented in the backend but have no front-end interface and have not been tested.
5. Controlling multiple drones at a time feels very unintuitive and clunky. Improvements could include:
    * Automatically selecting the tab of a drone when it is clicked on
    * Automatically bringing up the popup for a drone when its tab is selected
    * Automatically selecting the tab of a drone when a pin belonging to that drone is clicked
    * Color-coding or otherwise differentiating pins belonging to different drones
    * etc.
6. Adding an HUD (with an artificial horizon, etc.) for the currently selected drone
7. Adding video streaming support for the currently selected drone
8. Using different vehicle icons based on the type of vehicle detected
