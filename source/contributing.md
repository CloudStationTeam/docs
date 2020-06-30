# Contributing to the project
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

## Notes & Things to work on
1. **Front End** uses vanilla JS which should be good enough because the map from MapBox takes up most of the space on the page anyways. 
    * If I were to start working on this again, I'll probably clean up the front end code first. It is not structured in a way that can be maintined and improved upon easily. Using some kind of framework (Angular, React, Vue) should help.
2. **Back End**
    * The APIs we implemented don't follow the popular RESTful style. We decided to make them structured in a way that's very similar to MAVLink commands/messages. It makes initial development a lot easier but I imagine it will make it difficult to add some more advanced features. I think having RESTful APIs actually does make sense for this project. An example of the resources would be a JSON document of all the telemetry information of the vehicle. The backend should send the correct command according to the current speed in that document, etc. This design should make it easier to implement the front end and makes it more structured.
3. **Security**
    * This is probably the biggest problem of the current implementation. I don't have any experience in this field so I can't provide much information.