# Overview
This is our CMPE 195B senior project. The goal of this project was to create a smart alarm clock/hub that utilized the Matter protocol to automate various Matter enabled devices. We also wanted to have basic voice assistant functionality.

# Hardware Used
In this project we used the following hardware:
- Raspberry Pi 3 Model B+
- Generic USB Microphone
- Speaker with 3.5 mm connection
- 5 inch Raspberry Pi Display
- Separate computer to run various services to process the voice assistant data

# Home Assistant Configuration
## Install + Setup:
1) download and configure Docker via https://www.docker.com/get-started/ or follow the instructions at https://docs.docker.com/engine/install/ubuntu/
2) run the following command for install:
   docker run -d \
  --name homeassistant \
  --privileged \
  --restart=unless-stopped \
  -e TZ=MY_TIME_ZONE \
  -v /PATH_TO_YOUR_CONFIG:/config \
  -v /run/dbus:/run/dbus:ro \
  --network=host \
  ghcr.io/home-assistant/home-assistant:stable

  This will install homeassistant core into docker. MY_TIME_ZONE is a tz database name (ex: Antarctica/South_Pole), find your timezone in https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
  /PATH_TO_YOUR_CONFIG is the name of the folder you want to store your config files in. If there is no existing directory with that name, a new one will be created
  d-bus command is for bluetooth capabilities, if the device is not bluetooth capable, this command can be ignored.
  
3) Home Assistant dashboard can now be accessed through http://<host>:8123 , where <host> = the hostname or ip of the device running home assistant
4) Copy the contents of this repository into the config folder
5) restart homeassistant
6) Confirm that the required integrations are functioningq
7) Commission matter devices according to the following procedure:
    ...TODO
8) Reconfigure certain device parameters in homeassistant
   - For the BrowserMod integration, go to the settings and register the device you want to play audio from (this device must either be capable of opening an internet browser or connected to a device capable of doing so)
      - click register, copy the device id that appears
      - Change any instances of a device name in the Alarm Trigger Automation to the device id you just copied
  ...TODO if any others

## Scheduler Component Integration
### Input helpers
