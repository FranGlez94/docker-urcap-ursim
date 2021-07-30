
# DOCKER-URCAP-URSIM
This is a simple way for running a docker container with a [Universal Robots](https://www.universal-robots.com/)'s URCaps environment, along with an URSim.


## 1. Pre-requisites
- Docker (Docker Desktop in Windows)
- Visual Studio Code


## 2. Installation
Just fill up _".env"_ file with the config you need:
- URCAP_SDK - Version of the SDK will be downloaded
- TZ - Timezone
- ROBOT_MODEL - UR Model for the simulator
- TAR_GZ_URSIM_URL - URL of the URSim_Linux installer, it must be __TAR.GZ__ .

Then run the command:
```cmd
docker-compose up -d --build
```


## 3. How to use
This section is divided in two points:
- URSim Polyscope Access
- URCap development

Just be sure both containers are running.


## 3.1 URSim Polyscope Access
There is two ways to connect to URSim:
- Using RDP on port 33890.
- Using _"http://localhost:8080"_ on a web browser.

Due to problems with the resolution, initial width and height must be 1280*800, later you can use the zoom, resize from the RDP client or resizing the browser. 

Example on Windows:
```cmd
mstsc.exe /v:localhost:33890 /w:1280 /h:800
```

Note: Its possible to create a shortcut with this content (Windows).


## 3.1.2 Host ports and URSim ports 
All ports are written on the dockerfiles and the docker-compose file.
You are freely to modify it. 

Actual are listed here:
```
Published ports (Host:URSim): 
- 8080:8080
- 33890:3389
- 502:502
- 29999:29999
- 30001-30004:30001-30004

URSim ports:
- Modbus Port: 502
- Interface Ports: 29999 30001-30004
- Guacamole web browser viewer: 8080
- Guacamole RDP server: 3389
```


## 3.2 URCap development

To develop an URCap a Linux system its needed, also a IDE to help us developing.

We will use VSCode to connect to the container and start developing in it.


## 3.2.1 VSCode as IDE

Install this VSCode extension:
- [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers). To connect to the container.

Connect to the container pressing __F1 key__ and find _"Remote-Containers: Attach to Running Container"_.
Select it and look for the URCap container. 

It will take some time to start the VSCode server

Now I recommend to install this extension on the remote:
- [Java Extension Pack](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack). Recommended to develop in Java using VSCode

You can develop on this wonderful IDE on the remote container.


## 3.2.2 Ready URCaps Environment

Using VSCode, open the folder __"/root/workspace/"__, inside there are one file named _"newURCap.sh"_.
To create a new URCap execute the file using __BASH__, like this:
```bash
bash ./newURCap.sh
```
Fill up the data its asking, after that you will get clean URCap project ready. 


## 3.2.3 Testing directly into URSim container
On _"pom.xml"_ Change _"ursim.home"_ entry like this:
```xml
<!--Install path for the UR Sim-->
<ursim.home>/root/ursim</ursim.home>
```

Then run the command:
```cmd
mvn clean install -P ursim
```

It will automatically install the URCap JAR file into the URSim folder.

After the command is completed, restart the URSim using Polyscope GUI


## CREDITS

My dockerfiles are a modified version of these developers: 
- [ahobsonsayers](https://github.com/ahobsonsayers/DockURSim)
- [mohamedghita](https://github.com/mohamedghita/dockerfiles)
