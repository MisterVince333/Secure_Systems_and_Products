# **Task 2**
Through the steps below, you can run a Minecraft server locally using docker containers.
This guide consists of two parts. One for [Windows](#windows) and one for [Linux](#linux).

## **Windows**

### 1: Setting up
-   Go to https://www.minecraft.net/en-us/download/server and download the minecraft_server.[VERSION].jar
-   Create a folder and put the server.jar inside
-   Create a new text document document in the created folder and call it `eula.txt`.
-   Open the text document and add the following content: `eula=true`

### 2: Creating a Dockerfile
-   In the same folder, make a new file called `Dockerfile`
-   Now add following content to the Dockerfile:
    ```sh
	FROM openjdk
	COPY server.jar root/server.jar
	COPY eula.txt root/eula.txt
	WORKDIR /root
	CMD ["java","-jar","server.jar"]
    ```
    > Note: By default, Windows does not have a programme to open files without extensions, so you have to install one yourself.
    In this example, I will be using Visual Studio Code.
    
### 3: Building the Dockerfile
-   Open the folder in a terminal (e.g. CMD)
-   Now run the following command:
    ```sh
	docker build -t minecraft_server .
    ```
    If all goes well, the image should now being created. Once this process is complete, we can run the following command to check that we do indeed have the image:
    ```sh
	docker images
    ```
### 4: Starting up and connecting to the Minecraft server
-   In order to launch the minecraft server, we use the following command in the terminal:
    ```sh
	docker run minecraft_server -p 25565:25565 
    ```
    Once the world is generated, we should be able to connect to the server by entering _localhost_ as the IP address.
### 5: Upload the image to Docker Hub
-   Log in to the website https://hub.docker.com and create a new repository. In this case, I will call the repository _minecraft_server_
-   Go back to the terminal we used earlier. Use the command below to indicate/tag which image we want to upload.
    ```sh
	docker tag [IMAGE ID] [USERNAME]/[TAG]
	```
	In my case:
    ```sh
	docker tag 9c176df5ee53 mistervince333/minecraft_server
	```
-   Now we can push the tagged image to our repository:
    ```sh
	docker push [USERNAME]/[TAG]
	```
	In my case:
    ```sh
	docker push mistervince333/minecraft_server
	```
	> Note: You must be logged in to Docker via terminal to be able to push. If you haven't already done so, enter the command ```docker login``` first.
	
	If all went well, you should now be able to find the repository on Docker Hub.
### 6: Scanning the image for vulnerabilities
-   In order to scan the image for vulnerabilities, we use the following command:
	```sh
	docker run --rm -v $HOME/Library/Caches:/root/.cache/ aquasec/trivy image [USERNAME]/[TAG]
	```
	In my case:
	```sh
	docker run --rm -v $HOME/Library/Caches:/root/.cache/ aquasec/trivy image mistervince333/minecraft_server
	```
	The terminal will show a list of all vulnerabilities with the severity and the fixed version.
___
## **Linux**

### 1: Setting up
-   Open the terminal and create a new directory for the minecraft server using following command:
    ```sh
	mkdir minecraft_server
    ```
    and go inside the folder:
    ```sh
	cd minecraft_server
    ```
-   Download the latest version of the minecraft server (find the latest version [here](//www.minecraft.net/en-us/download/server)) using wget:
    ```sh
	wget https://piston-data.mojang.com/v1/objects/c9df48efed58511cdd0213c56b9013a7b5c9ac1f/server.jar
    ```
-   Create a new text document document in the created folder and call it `eula.txt`:
    ```sh
	nano eula.txt
    ```
    and add the following content: `eula=true`
-   Save the file

### 2: Creating a Dockerfile
-   In the same folder, make a new file called `Dockerfile`:
    ```sh
	nano Dockerfile
    ```
    and add following content:
    ```sh
	FROM openjdk
	COPY server.jar root/server.jar
	COPY eula.txt root/eula.txt
	WORKDIR /root
	CMD ["java","-jar","server.jar"]
    ```
- Save the file
    
### 3: Building the Dockerfile
-   Now run the following command in the folder:
    ```sh
	docker build -t minecraft_server .
    ```
    If all goes well, the image should now being created. Once this process is complete, we can run the following command to check that we do indeed have the image:
    ```sh
	docker images
    ```
### 4: Starting up and connecting to the Minecraft server
-   In order to launch the minecraft server, we use the following command in the terminal:
    ```sh
	docker run minecraft_server -p 25565:25565 
    ```
    Once the world is generated, we should be able to connect to the server by entering _localhost_ as the IP address.
    
### 5: Upload the image to Docker Hub
-   Log in to the website https://hub.docker.com and create a new repository. In this case, I will call the repository _minecraft_server_
-   Go back to the terminal and use the command below to indicate/tag which image we want to upload.
    ```sh
	docker tag [IMAGE ID] [USERNAME]/[TAG]
	```
	In my case:
    ```sh
	docker tag 9c176df5ee53 mistervince333/minecraft_server
	```
-   Now we can push the tagged image to our repository:
    ```sh
	docker push [USERNAME]/[TAG]
	```
	In my case:
    ```sh
	docker push mistervince333/minecraft_server
	```
	> Note: You must be logged in to Docker via terminal to be able to push. If you haven't already done so, enter the command ```docker login``` first.
	
	If all went well, you should now be able to find the repository on Docker Hub.
### 6: Scanning the image for vulnerabilities
-   In order to scan the image for vulnerabilities, we use the following command:
	```sh
	docker run --rm -v $(pwd):/root/.cache/ aquasec/trivy image  [USERNAME]/[TAG]
	```
	In my case:
	```sh
	docker run --rm -v $(pwd):/root/.cache/ aquasec/trivy image  mistervince333/minecraft_server
	```
	The terminal will show a list of all vulnerabilities with the severity and the fixed version.