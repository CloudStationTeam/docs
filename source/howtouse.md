# How to Use 

![CloudStation before logging in](images/cloudstation-first-access.png "CloudStation site screenshot when first accessed") 
1. In your web browser, go to your AWS instance's DNS address. You will see a map that takes up the whole screen. In the menu at the top, click "Log in/Sign up" and select "Sign up."
2. Sign up for an account, using a password that you are not using for any other accounts.
3. After logging in with your new account, you will see a box that says "Connect to Vehicle via ID."
    * Make sure your drone or SITL instance is connected to one of your CloudStation instance's open UDP ports.
    * Enter the number of the UDP port your drone is connected to (e.g. 14550). This will be that vehicle's drone ID.
![Drone popup menu](images/cloudstation-drone-popup.png "CloudStation site screenshot when first accessed") 
4. Controlling the drone:
    * You can arm/disarm the drone, change the flight mode, issue land or takeoff command, and issue RTL command by clicking on the controls on the left hand panel.
    * If using SITL, it can only be armed in GUIDED mode.
    * To set a fly-to point, click anywhere on the map to bring up the pop-up menu. Enter the final altitude of the destination and click "fly to". The drone will be put into guided mode and fly to the chosen destination. Note this will only work if the drone is already in the air and armed.

