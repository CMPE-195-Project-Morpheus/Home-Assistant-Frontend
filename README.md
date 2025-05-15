# Overview
This is our CMPE 195B senior project. The goal of this project was to create a smart alarm clock/hub that utilized the Matter protocol to automate various Matter enabled devices. We also wanted to have basic voice assistant functionality.

# Hardware Used
In this project we used the following hardware:
- Raspberry Pi 3 Model B+
- 128 GB MicroSD card for OS install
- Generic USB Microphone
- Speaker with 3.5 mm connection
- 5 inch Raspberry Pi Display
- Separate computer to run various services to process the voice assistant data

# Raspberry Pi OS Install
![Raspberry_Pi_OS_Install](https://github.com/user-attachments/assets/62f728f9-0f50-4e1d-bcfd-d3fb5d24c8d4)

In order to install Raspberry Pi OS you need to install the Raspberry Pi Imager (https://www.raspberrypi.com/software/) and use a microSD card to install the OS on. In our config we selected the RPi 3, RPi OS 64-bit, and chose the microSD card as our storage. The OS version we are using is RPi OS 64-bit Debian 12.

# Docker Engine Installation
Since we are using Home Assistant Container, we need to install Docker Engine as various integrations will require us to run Docker applications.
https://docs.docker.com/engine/install/raspberry-pi-os/#install-using-the-repository

Once Docker Engine is installed we can proceed to the installation of Home Assistant Container.

# Home Assistant Configuration
## Install + Setup:
1) run the following command for install:
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

  ![image](https://github.com/user-attachments/assets/751f5bfe-e882-49fb-99e3-f1b26f38cf5b)
  
  This is the Home Assistant version we are using at the time of our project.

2) Home Assistant dashboard can now be accessed through http://<host>:8123 , where <host> = the hostname or ip of the device running home assistant
3) Copy the contents of this repository into the config folder
4) restart homeassistant
5) Confirm that the required integrations are functioningq
6) Commission matter devices according to the following procedure:
    ...TODO
7) Reconfigure certain device parameters in homeassistant
   - For the BrowserMod integration, go to the settings and register the device you want to play audio from (this device must either be capable of opening an internet browser or connected to a device capable of doing so)
      - click register, copy the device id that appears
      - Change any instances of a device name in the Alarm Trigger Automation to the device id you just copied
  ...TODO if any others

## Scheduler Component Integration
### Scheduler Component Install
In order to install the scheduler component we used the following components:
https://github.com/nielsfaber/scheduler-component
https://github.com/nielsfaber/scheduler-card




