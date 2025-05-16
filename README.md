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

![image](https://github.com/user-attachments/assets/7c61d234-c8f7-4c76-a245-99bfbaf7bf3f)
![image](https://github.com/user-attachments/assets/e6cc2abc-3600-4866-b90d-697d0a20c307)

The images above show our current hardware implementation.

# Raspberry Pi (RPi) OS Install
![Raspberry_Pi_OS_Install](https://github.com/user-attachments/assets/62f728f9-0f50-4e1d-bcfd-d3fb5d24c8d4)

In order to install Raspberry Pi OS you need to install the Raspberry Pi Imager (https://www.raspberrypi.com/software/) and use a microSD card to install the OS on. In our config we selected the RPi 3, RPi OS 64-bit, and chose the microSD card as our storage. The OS version we are using is RPi OS 64-bit Debian 12.

# Docker Engine Installation
Since we are using Home Assistant Container, we need to install Docker Engine as various integrations will require us to run Docker applications.
https://docs.docker.com/engine/install/raspberry-pi-os/#install-using-the-repository

Once Docker Engine is installed we can proceed to the installation of Home Assistant Container.

# Home Assistant (HA) Configuration
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

## Added Home Assistant Features + Dashboard Configuration
### Alarm Clock Implementation



### Scheduler Component Implementation
In order to install the scheduler component we used the following components and installed it based on their documentation. The version of the scheduler card we are using at the time of this project is 3.12.3. We installed the scheduler component via the Home Assistant Community Store (HACS). At the time of this project we are using version 3.3.8 of the scheduler component.:

https://github.com/nielsfaber/scheduler-component
https://github.com/nielsfaber/scheduler-card

#### Light Scheduling Implementation
In order to allow the user to be able to schedule different configurations for a Matter enabled light bulb various scripts and input helpers were used. The config for these input helpers can be found in the .storage folder or you can create them via the Home Assistant UI. Also, the dashboard configuration can be found in .storage under Lovelace Dashboard 

##### Input Helper Creation
![image50](https://github.com/user-attachments/assets/afe6aefe-ff1e-42ed-9246-823eb1cb922a)

This shows the Scheduler card titled Scheduler along with the cards we made for the light configuration.  


![image26](https://github.com/user-attachments/assets/a4671082-bf1e-49b0-9258-788fc0a1639b)

First I created different input helpers to store values for each configuration. These are variables that take user input and store them. These input helpers can be created in the Home Assistant settings under "Devices & Services".  


![image16](https://github.com/user-attachments/assets/1f365a05-3758-4012-9f2a-7c611dc35ba2)

For the card on the dashboard to allow the user to select the input I created the following input helpers which display the levels that are able to be selected for the input as a slider and also has a dropdown menu to select which config to save the values to.  


![image17](https://github.com/user-attachments/assets/01d71cc4-9024-40f3-aac8-18c4a370733e)

These are the settings of the light brightness input helper. This will be used as user input for the user to set the light brightness and eventually save it to a config.  


![image4](https://github.com/user-attachments/assets/c50e47af-43a0-40e7-aa1d-0b6d7d068515)

These are the settings of the light color temp input helper. This will be used as user input for the uer to set the light color temperature and eventually save it to a config.  


![image57](https://github.com/user-attachments/assets/270dc098-8ae3-4b02-9917-4cb3bb25ea37)

This is an input number to save the selected color temp to config 1.  


![image58](https://github.com/user-attachments/assets/cb24c244-a8c3-4979-a484-46b08bf1a6c2)

This is an input number to save the selected brightness to config 1.  


We also had to create a button that runs a script to save the selected light settings to the specified config. The button configuration can be found in the .storage folder or can be made via the HA UI.

##### Script Creation
The scripts used can be found in the scripts.yaml file in the repo.

In order to save the selected values to the specified config, we had to create a button that runs a script that saves the values when pressed. This is the Save Current Light Settings to Scheduler shown in the original dashboard image. The light config selector drop down menu specifies which config variables to save the user input too.  


![image6](https://github.com/user-attachments/assets/ef3b93df-2f61-41ef-a5a6-5b19680c762c)

These are the names of the scripts used to apply various configs. These scripts take the values from the input helpers created earlier for the specific config and then performs a turn on action for the light, sending the brightness and color temp values as data. Make sure the entity id matches the light connected via the Matter integration. This script can be changed for each config by changing which input helper the values are taken from.

##### Adding the scheduler card to the dashboard
![image10](https://github.com/user-attachments/assets/5c7c9c01-83ef-4e73-ba26-31cb2b66fb85)

The scheduler card can be added to the dashboard via the HA UI. As you can see it appears under the custom card section.  

![image35](https://github.com/user-attachments/assets/749a369d-fd7a-4368-9c92-0b335722a40b)

We customized the scheduler card to show the different apply config scripts created. You can select which config to run and select the apply action, which will run the script to apply the config. In order to customize the displayed entities and actions we have to adjust the YAML for the card. The YAML for the customized section of the card can be found in the main dashboard configuration.  

![image](https://github.com/user-attachments/assets/44fc75a6-3138-41b8-a953-8428004b8a5d)

This shows scheduling one of the light config scripts to run at a certain time.

### Voice Assistant Implementation
The implementation of voice assistant functionality in HA requires various components in order to create the Assist pipeline. These components are all connected to Home Assistant via the Wyoming protocol integration which allows each of these components to communicate with the other components as well as HA. Also, the Ollama integration is required.
Here is a list of the required components:
- Wyoming integration enabled in HA
- Ollama integration enabled in HA
- Wyoming satellite (send audio/text data between the various voice services)
- Wyoming microwakeword (wake word detection)
- Wyoming faster whisper (speech to text recognition)
- Wyoming piper (text to speech conversion)
- Ollama (conversation agent). Note Ollama does not use the Wyoming protocol but rather has its own integration in HA.

#### WSL Setup
Since the RPi we are using is not powerful enough to run all of these services, we had to run them on a separate computer. We chose to run them on a separate computer in the Windows Subsytem for Linux (WSL).

The WSL install can be done using the following tutorial. You must also install Docker engine using the tutorial described earlier.: 
https://learn.microsoft.com/en-us/windows/wsl/install

Since WSL uses a virtualized network interface, we need to bridge the WSL network interface with the host computer's network interface so it will be easier to access the voice assistant services from the RPi.
This can be done using the following tutorial. You must enabled mirrored mode networking and then create firewall rules to forward the ports used for each voice assistant service.:
https://learn.microsoft.com/en-us/windows/wsl/networking

#### Wyoming Protocol Integration
![image47](https://github.com/user-attachments/assets/902aa9c2-6858-41f8-8a9d-557bee64d0b5)  
You must add the Wyoming protocol integration to HA which can be done via HA settings.

#### Ollama Integration:

