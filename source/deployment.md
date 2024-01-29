# CloudStation Deployment
The instructions describe our setup process on an EC2 instance running Ubuntu 22.4 LTS. The steps should be similar if you use a server with Linux Distributions.

[CloudStation source code](https://github.com/CloudStationTeam/cloud_station_web)
## Step-by-step deployment guide

#### Prerequisites
Before you start, you will need an AWS account (free trial version is OK). The following deployment steps have been developed for a fresh EC2 instance. Although deployment on a local machine is possible, it may require additional steps and configuration changes not covered in the guide.

You will also need a free [MapBox](https://account.mapbox.com/) account. Make sure you can find your Mapbox public token, located on the home page after logging in.

#### Deployment Steps

1. Launch an EC2 instance on AWS with Ubuntu 22.4 LTS
    * t2.micro (free-tier eligible) is good enough to test the deployment.
    * Step 6: Configure Security Group (AWS EC2 Console)
      * SSH (TCP)   Port:22     Source:My IP
      * HTTP(TCP)   Port:80     Source:Anywhere
      * HTTPS(TCP)   Port:443     Source:Anywhere 
      * Custom UDP Rule  Port:14550  Source:Anywhere
        * MAVLink (vehicle messages) is routed to 14550 via UDP in the current configuration. Any available port can be used instead of 14550. If you want to connect multiple vehicles, enter a range of ports, such as 14550-14560.
    * Create or use existing key pairs. This is used for SSH.
2. Associate an Elastic IP to the EC2 (**Please take note of the IP/DNS address, you will need it in step 3 and 6) 
    * "An Elastic IP address is a static IPv4 address". It is a public address we use so the IP address of the deployed 
    CloudStation will stay the same. 
    > Learn more about it here: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html?icmpid=docs_ec2_console
    * This is only a temporary solution but it serves our testing purpose well. This step is optional but if elastic IP is not used, the server
    IP address will change from time to time.      
3. [Connect to Linux instance using an SSH client](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
4. Set up EC2 (run only once)  
    run 
    ```
    cd ~
    git clone https://github.com/CloudStationTeam/cloud_station_deployment.git 
    bash ~/cloud_station_deployment/setup_server.sh YOUR_MAP_BOX_KEY_HERE_NO_QUOTES
    ```
    This deploys an instance of the CloudStation web server based on the latest commit. Note that the latest commit may be developmental. For a more stable version, you can run setup_server.sh with a flag to indicate a certain release tag, e.g. `bash ~/cloud_station_deployment/setup_server.sh --tag=v4.0`.
    * You can see the list of releases [here](https://github.com/CloudStationTeam/cloud_station_web/releases). The tag for the release is indicated by the text following the tag icon to the left of the release icon, e.g. v4.0.
    * setup_server.sh does the following：   
      1. Update Ubuntu  
      2. Install NGINX and docker, Django , configures them 
      3. Clone cloud_station_web source code  
      4. Set up a Python virtual environment (install dependencies)
      5. Configures mapbox api key
      6. Write database migrations (Django)
      7. Configure NGINX with nginx.conf  
      8. Configure systemctl to automatically run Daphne as a service(daphne.service)  
      8. Download redis and start running redis in a docker container
5. In your web browser, go to your EC2 instance's DNS address (ec2-xx-xx-xxx-xxx.us-west-1.compute.amazonaws.com) and you should see the CloudStation website. Note that modern browsers default to https, but for this use http:// in front of the IP address.

#### Redeployment
To reload the server (after a code update)    
* Run ```bash ~/cloud_station_deployment/reload_server.sh```  
* The script does the following:  
    1. Pull latest version of the src code
    2. Write database migrations  
    3. Collect staticfiles  
    4. Reload NGINX and Daphne  
    5. Run django_background_tasks

Note that If frontend didn't work, for example If JS files are not updated, clear browser cache. (Typically control shift R to reload).

#### Testing Using SITL
To test CloudStation with a simulated drone instead of a real drone, you can install and run [SITL](https://ardupilot.org/dev/docs/sitl-simulator-software-in-the-loop.html). If you have a Windows computer running Cygwin, you can also use our [SITL deployment script](https://github.com/CloudStationTeam/sitl_deployment) to automate running multiple SITL instances.

For the example SITL command,

cd ~/ardupilot/ArduCopter;  sim_vehicle.py --console --map --osd --out=udp:35.94.121.200:14550 --custom-location=33.64586111,-117.84275,25,0

the address of the cloudstation instance is 35.94.121.200. You can use your cloudstation instance IP address instead.

Professor Burke's github has a single script that will deploy SITL to a linux machine, see: https://github.com/PeterJBurke/CreateSITLenv.

#### Restarting Ubuntu
If you need to restart Ubuntu, run the following:
```bash ~/cloud_station_deployment/configure_web_server.sh```
This procedure is needed to restart redis.

## Configuring a Drone for CloudStation
In order for the drone to connect to CloudStation, the drone needs to be set up to send Mavlink packets over UDP to the CloudStatoin IP address. The UDP port is also the "ID" of the drone in the CloudStation UI. As far as Cloudstation is concerned, it does not matter how the drone is configured to send/receive Mavlink traffic over UDP.

There are many ways to configure the drone to do this. The easiest is to have a on-board ["companion computer"](https://ardupilot.org/dev/docs/companion-computers.html). For example, the companion computer, e.g., can connect to the flight controller over UART. The companion computer can run Mavproxy, and the Mavproxy configuration file can be set to point send Mavlink packets over UDP to the IP address of the CloudStation. You will need to make sure firewalls are appropriately configure. A detailed example of how to do this over 4G network is also at [4guav](https://gitlab.com/pjbca/4guav) and in ref:

[J70] Peter J. Burke “A Safe, Open Source, 4G Connected Self-Flying Plane With 1 Hour Flight Time and All Up Weight (AUW) <300 g: Towards a New Class of Internet Enabled UAVs”
IEEE Access, 7(1), 67833 – 67855 (2019).



## Authors
  * Lyuyang Hu
  * Omkar Pathak
  * Peter Burke

## Troubleshooting 
1. How do I know my hardware setup is correct (the vehicle is sending mavlink messages to the server)?  
   ```sudo tcpdump -n udp port 14550 -X``` will print the messages received at port 14550 (UDP).
2. How do I know NGINX and Daphne is running? How do I know if there are errors?
     ```
     service nginx status  
     service daphne status
     ```
3. The telemetry textbox shows that the websocket connection between server and browser has been disconnected. What do I do?  
    * This usually means Redis fails. Following the Django Channels recommendation, we use Redis as the backing store for the channel layer. We use Docker to run Redis.
    * To check status of Docker: ```service docker status```  
    * To show all Docker containers on the machine: ```sudo docker ps -a```  
    * To restart Docker and Redis:  
    ```
    sudo systemctl start docker
    sudo systemctl enable docker
    sudo docker run -p 6379:6379 -d redis:2.8
    ```

