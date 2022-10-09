 
Quick Intro
The term docker is a revolution in the IT-infrastructure, Docker was introduced in 2013 to the real world. It gained popularity around the globe mainly in a couple of years and nowadays every organization is implementing the concepts of containerization and deploying the docker containers to DockerHub.
So, what exactly is Docker?
Docker is essentially a platform to package software so it can run on any hardware. This enables developers to work in standardized environments and consequently streamlines the lifecycle of development. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications.
Basic Lingo to understand Docker:
·       Dockerfile: You can think of the Dockerfile as the recipe. It is the blueprint to build the Docker image. It's made of multiple layers built on top of each other. The BaseLayers or Parent Image, is the core environment you want to work on, ex.: NodeJS. On top of it we add the layers for Dependencies, SourceCode, etc.
.    Docker image: The image is generated from the Dockerfile and it's a read only template with instructions for running a Docker container. The image provides the necessary filesystem that contains everything needed to run an application, that includes: dependencies, configurations, scripts, libraries and more. 
.    Docker container: A container is a running process of the image - just an instance of the image. It can run on your local machine, virtual machine or even in the cloud. 
·       Docker compose: Allows users to run multiple containers. Docker Compose is an excellent tool for optimizing the process of creating environments: development, testing, staging, and production. 
·       Share images using Docker Hub: the process of uploading Docker images to the public repository platform (called Docker Hub) provided by Docker's team. 
Work smarter, Not harder
A few things you might want to install before dockerizing your project, to make things a little easier for yourself, is the Docker CLI, the Docker VSCode extension & DockerHUB. 
-The Docker CLI gives you a nice overview of your images, running containers and container logs and in general makes it easier to manage your containers’ lifecycle. 
-The VSCode Docker extension makes it easier to build, manage and deploy containerized applications from your Visual Studio Code.
- Use Official Images from DockerHub.
Let’s get started
Ok, over to the more practical part. How would you go about dockerizing your project? Let’s say you want to containerize your nodeJS application. We tried to break it down into easy step for you:
The first step is to create a file called Dockerfile (that’s it, no extensions) in the same folder as your package.json. 
Next, write your image. You can write your own image, or you can use an image witten by others in the registry.
The first command in a dockerfile is FROM. FROM sets the baseimage to work from. There are a lot of different base images to choose from. In our case, building a node project, there is a specific baseimage just for that. So in this case we can write FROM node:16.17 . (The tag :12 after node specifies the version of Node we want to use). 
Next we can specify the working directory where we want the: ADD, COPY, RUN, CMD or ENTRYPOINT instructions to run. Let’s say we want to run the instructions in the app-folder, then the code would be WORKDIR /app.
In a node project, installing all of the dependencies is a good next step. To do this, we can use the COPY command. COPY takes two arguments; the file, and the location to be copied to. To install all of the dependencies we then copy the package.json-files and place them where we want them relative to the working directory. The code would look like this: COPY package*.json ./. 
Then we have to actually install them, which is done with the RUN command; RUN npm install. 
Now we want to copy over our local files. Though, there’s an issue; We don’t want to copy over our node modules. How do we solve that? 
With a .dockerignore file which works just like a .gitignore; simply write node_modules in it, and docker will ignore it. 
Okay, now we can copy over everything (that we want): COPY . . 
(To be clear, this means: Copy all of our local files to our working directory). 
When running the node project, we want to specify the port we want to be listening in to so we can run it in browser at localhost:// 
this is done with the ENV command; ENV PORT=5000 (or whatever port you want to be using) and EXPOSE 5000, which will define the network port. 
Lastly we have CMD. This is the one we use to tell docker how to actually start the program. CMD takes an array of commands like: CMD [“npm”, “start”]


So, what now?
To build the docker image, type docker build in the Terminal. There are alot of different tags to add to this command. The -t tag lets you name your docker image to whatever you want, making it easier to remember and to access later. 

To run the container you can use the command docker run in the terminal, followed by the id or tag name. To access the project on our localhost, we can pass a -p tag like this: docker run -p 8080:5000 . This tells docker to bind the container’s port 5000 to the host’s port 8080. 
Docker command glossary
Command
Description
Example
FROM
Sets the base image for Docker to work from.
FROM node:12
WORKDIR
Specifying the working directory where we want the subsequent ADD, COPY, RUN, CMD or ENTRYPOINT instructions to run.
WORKDIR /app
COPY
Takes two arguments; the file(s), and the location to be copied to.
COPY package*.json ./
RUN
Runs the following commands.
RUN npm install
ENV
Sets the environmental values.
ENV PORT=5000
EXPOSE
Defines the network port.
EXPOSE 5000
CMD
Takes an array of commands for docker to run.
CMD [“npm”, “start”]


Dockerizing Sample web application with ASP.NET
Create a sample  ASP.NET web application and run the application using the docker container with a specific port on localhost.
Initialize the asp application using the following commands
 dotnet new solution -n “WebApp” - - framework net5.0
 dotnet new  webapp -n “WebApp.App” - - framework net5.0
 dotnet sln add ./WebApp.App/WebApp.App.csproj
If you now run “dotnet run” inside the WebApp.App directory, a web server will start and you can open it on “http://localhost:5000 in your browser. You will see a homepage on your screen.
For containerizing the above application, we need to create a “dockerfile”. At the root level of the project create a file with a name Dockerfile and paste the below code in the docker file.

Sample docker file for ASP.NET core web app runs on dotnet version 5.0

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /source
COPY . .
RUN dotnet publish -c release -o /app
 
FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["dotnet", "MyWebApp.App.dll"]

The above docker file will create two docker images, first one contains dotnet sdk and another one with source code. To build the container run the command at the root of the project.

 docker build -t mywebapp .

The above command creates a new docker image named “mywebapp” and to get the list of images run “docker images” or  “docker image ls” in the terminal.

docker run -p 5020:80 -it mywebapp


This will create a container from the above image and the above container runs on port 5020. Now you can access the webapp on http://localhost:5020 in your browser and access the same webpage from the docker container.

docker ps command will give the list of containers available in your local machine and docker ps -a with passing an attribute will give results of available containers and also with exited containers
