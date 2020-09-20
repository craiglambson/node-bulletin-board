Craig Lambson’s notes on System Engineer Job Interview Assignment
Writing a Dockerfile is the first step to containerizing an application. You can think of these Dockerfile commands as a step-by-step recipe on how to build up your image. The Dockerfile in the bulletin board app looks like this:
# Use the official image as a parent image.
FROM node:current-slim

# Set the working directory.
WORKDIR /usr/src/app

# Copy the file from your host to your current location.
COPY package.json .

# Run the command inside your image filesystem.
RUN npm install

# Add metadata to the image to describe which port the container is listening on at runtime.
EXPOSE 8080

# Run the specified command within the container.
CMD [ "npm", "start" ]

# Copy the rest of your app's source code from your host to your image filesystem.
COPY . .
The dockerfile defined in this example takes the following steps:
* Start FROM the pre-existing node:current-slim image. This is an official image, built by the node.js vendors and validated by Docker to be a high-quality image containing the Node.js Long Term Support (LTS) interpreter and basic dependencies.
* Use WORKDIR to specify that all subsequent actions should be taken from the directory /usr/src/app in your image filesystem (never the host’s filesystem).
* COPY the file package.json from your host to the present location (.) in your image (so in this case, to /usr/src/app/package.json)
* RUN the command npm install inside your image filesystem (which will read package.json to determine your app’s node dependencies, and install them)
* COPY in the rest of your app’s source code from your host to your image filesystem.
You can see that these are much the same steps you might have taken to set up and install your app on your host. However, capturing these as a Dockerfile allows you to do the same thing inside a portable, isolated Docker image.
The steps above built up the filesystem of our image, but there are other lines in your Dockerfile.
The CMD directive is the first example of specifying some metadata in your image that describes how to run a container based on this image. In this case, it’s saying that the containerized process that this image is meant to support is npm start.
The EXPOSE 8080 informs Docker that the container is listening on port 8080 at runtime.
What you see above is a good way to organize a simple Dockerfile; always start with a FROM command, follow it with the steps to build up your private filesystem, and conclude with any metadata specifications. There are many more Dockerfile directives than just the few you see above. For a complete list, see the Dockerfile reference.


1b.build and run the node-bulletin-board image using the Docker client on your machine
Introduction
Now that you’ve set up your development environment, you can begin to develop containerized applications. In general, the development workflow looks like this:
1. Create and test individual containers for each component of your application by first creating Docker images.
2. Assemble your containers and supporting infrastructure into a complete application.
3. Test, share, and deploy your complete containerized application.
In this stage of the tutorial, let’s focus on step 1 of this workflow: creating the images that your containers will be based on. Remember, a Docker image captures the private filesystem that your containerized processes will run in; you need to create an image that contains just what your application needs to run.

Set up
Let us download the node-bulletin-board example project. This is a simple bulletin board application written in Node.js.

Git
If you are using Git, you can clone the example project from GitHub:
git clone https://github.com/dockersamples/node-bulletin-board
cd node-bulletin-board/bulletin-board-app
Define a container with Dockerfile
After downloading the project, take a look at the file called Dockerfile in the bulletin board application. Dockerfiles describe how to assemble a private filesystem for a container, and can also contain some metadata describing how to run a container based on this image.

View of Dockerfile
PS C:\Users\micro\node-bulletin-board\bulletin-board-app> more dockerfile
FROM node:current-slim

WORKDIR /usr/src/app
COPY package.json .
RUN npm install

EXPOSE 8080
CMD [ "npm", "start" ]

COPY . .

build and run the node-bulletin-board image

Make sure you’re in the directory node-bulletin-board/bulletin-board-app in a terminal or PowerShell using the cd command. Run the following command to build your bulletin board image:
docker build --tag bulletinboard:1.0 .
PS C:\Users\micro\node-bulletin-board\bulletin-board-app> docker build --tag bulletinboard:1.0 .
Sending build context to Docker daemon  45.57kB
Step 1/7 : FROM node:current-slim
current-slim: Pulling from library/node
abb454610128: Pull complete                                                                                                        3dfc5a66c517: Pull complete                                                                                                        9d3cc0392eb2: Pull complete                                                                                                        269452c05570: Pull complete                                                                                                        4a5ad82bcf62: Pull complete                                                                                                        Digest: sha256:b79cd9570acf538e6ae8dec0a0e00bfb8060857270db7bfca85e028a19dce786
Status: Downloaded newer image for node:current-slim
 ---> 397ef203e8c4
Step 2/7 : WORKDIR /usr/src/app
 ---> Running in 37792a4a6834
Removing intermediate container 37792a4a6834
 ---> 58fee6765d3d
Step 3/7 : COPY package.json .
 ---> 4b0cb29335d9
Step 4/7 : RUN npm install
 ---> Running in 162fa9fab7c6

> ejs@2.7.4 postinstall /usr/src/app/node_modules/ejs
> node ./postinstall.js

Thank you for installing EJS: built with the Jake JavaScript build tool (https://jakejs.com/)

npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN vue-event-bulletin@1.0.0 No repository field.
npm WARN The package morgan is included as both a dev and production dependency.

added 91 packages from 168 contributors and audited 92 packages in 4.046s
found 0 vulnerabilities

Removing intermediate container 162fa9fab7c6
 ---> 973ce6322c85
Step 5/7 : EXPOSE 8080
 ---> Running in f2ab18b1fd88
Removing intermediate container f2ab18b1fd88
 ---> 4a65764ae07a
Step 6/7 : CMD [ "npm", "start" ]
 ---> Running in 2aacc8f728de
Removing intermediate container 2aacc8f728de
 ---> a4850c3942ab
Step 7/7 : COPY . .
 ---> 95e82d4a73fd
Successfully built 95e82d4a73fd
Successfully tagged bulletinboard:1.0
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
PS C:\Users\micro\node-bulletin-board\bulletin-board-app>
Doc manual suggests changing from windows to linux containers.  The documentation indicates that Linux containers are the default.  So no action required.

Run your image as a container
1. Run the following command to start a container based on your new image:
PS C:\users\micro\node-bulletin-board\bulletin-board-app> docker build --tag bulletinboard:1.0 .
Sending build context to Docker daemon  45.57kB
Step 1/7 : FROM node:current-slim
 ---> 397ef203e8c4
Step 2/7 : WORKDIR /usr/src/app
 ---> Using cache
 ---> 58fee6765d3d
Step 3/7 : COPY package.json .
 ---> Using cache
 ---> 4b0cb29335d9
Step 4/7 : RUN npm install
 ---> Using cache
 ---> 973ce6322c85
Step 5/7 : EXPOSE 8080
 ---> Using cache
 ---> 4a65764ae07a
Step 6/7 : CMD [ "npm", "start" ]
 ---> Using cache
 ---> a4850c3942ab
Step 7/7 : COPY . .
 ---> ee51ea79aacd
Successfully built ee51ea79aacd
Successfully tagged bulletinboard:1.0
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
PS C:\users\micro\node-bulletin-board\bulletin-board-app>There are a couple of common flags here:
o --publish asks Docker to forward traffic incoming on the host’s port 8000 to the container’s port 8080. Containers have their own private set of ports, so if you want to reach one from the network, you have to forward traffic to it in this way. Otherwise, firewall rules will prevent all network traffic from reaching your container, as a default security posture.
o --detach asks Docker to run this container in the background.
o --name specifies a name with which you can refer to your container in subsequent commands, in this case bb.
PS C:\Users\micro\node-bulletin-board\bulletin-board-app> docker run --publish 8000:8080 --detach --name bb bulletinboard:1.0
4c92b81aa2db75572e042056ad538a734ce4a93f74fa92a7e8ff46d200a172cd
PS C:\Users\micro\node-bulletin-board\bulletin-board-app>
2,  Visit your application in a browser at localhost:8000. You should see your bulletin board application up and running. 


At this step, you would normally do everything you could to ensure your container works the way you expected; now would be the time to run unit tests, for example.

Modify the index.html file welcome line ‘Welcome to the Bulletin Board’ to include your name or another greeting.
<div class="jumbotron">
    <h1>Welcome to the Bulletin Board</h1>
  </div>

How do I edit in powershell?  >vi doesn’t work because Im not running linux.
PS C:\Users\micro\node-bulletin-board\bulletin-board-app>  vi
vi : The term 'vi' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the
name, or if a path was included, verify that the path is correct and try again.
At line:1 char:2
+  vi
+  ~~
    + CategoryInfo          : ObjectNotFound: (vi:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

-----open cmd window as administrator.  Install chocolatey
C:\windows\system32>powershell                                                                                          Windows PowerShell                                                                                                      Copyright (C) Microsoft Corporation. All rights reserved.                                                               
Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\windows\system32>
>> Set-ExecutionPolicy Bypass -Scope Process -Force; iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
>>
Getting latest version of the Chocolatey package for download.
Getting Chocolatey from https://chocolatey.org/api/v2/package/chocolatey/0.10.15.
Extracting C:\Users\micro\AppData\Local\Temp\chocolatey\chocInstall\chocolatey.zip to C:\Users\micro\AppData\Local\Temp\chocolatey\chocInstall...
Installing chocolatey on this machine
Creating ChocolateyInstall as an environment variable (targeting 'Machine')
  Setting ChocolateyInstall to 'C:\ProgramData\chocolatey'
WARNING: It's very likely you will need to close and reopen your shell
  before you can use choco.
Restricting write permissions to Administrators
We are setting up the Chocolatey package repository.
The packages themselves go to 'C:\ProgramData\chocolatey\lib'
  (i.e. C:\ProgramData\chocolatey\lib\yourPackageName).
A shim file for the command line goes to 'C:\ProgramData\chocolatey\bin'
  and points to an executable in 'C:\ProgramData\chocolatey\lib\yourPackageName'.

Creating Chocolatey folders if they do not already exist.

WARNING: You can safely ignore errors related to missing log files when
  upgrading from a version of Chocolatey less than 0.9.9.
  'Batch file could not be found' is also safe to ignore.
  'The system cannot find the file specified' - also safe.
PATH environment variable does not have C:\ProgramData\chocolatey\bin in it. Adding...
WARNING: Not setting tab completion: Profile file does not exist at
'C:\Users\micro\OneDrive\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1'.
Chocolatey (choco.exe) is now ready.
You can call choco from anywhere, command line or powershell by typing choco.
Run choco /? for a list of functions.
You may need to shut down and restart powershell and/or consoles
 first prior to using choco.
Ensuring chocolatey commands are on the path
Ensuring chocolatey.nupkg is in the lib folder
PS C:\windows\system32>

Waste.
> Notepad.exe <filename>
> Notepad.exe index.html     viola!
> Save it. Build it.  Run it.


PS C:\users\micro\node-bulletin-board\bulletin-board-app> docker build --tag bulletinboard:1.0 .
Sending build context to Docker daemon  45.57kB
Step 1/7 : FROM node:current-slim
 ---> 397ef203e8c4
Step 2/7 : WORKDIR /usr/src/app
 ---> Using cache
 ---> 58fee6765d3d
Step 3/7 : COPY package.json .
 ---> Using cache
 ---> 4b0cb29335d9
Step 4/7 : RUN npm install
 ---> Using cache
 ---> 973ce6322c85
Step 5/7 : EXPOSE 8080
 ---> Using cache
 ---> 4a65764ae07a
Step 6/7 : CMD [ "npm", "start" ]
 ---> Using cache
 ---> a4850c3942ab
Step 7/7 : COPY . .
 ---> ee51ea79aacd
Successfully built ee51ea79aacd
Successfully tagged bulletinboard:1.0
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
PS C:\users\micro\node-bulletin-board\bulletin-board-app>

*  Tried to tag the build with bulletin-board:2.0, but it didn’t work.
Now I need to publish it:
PS docker run --publish 8000:8080 --detach --name bb bulletinboard:1.0
PS C:\users\micro\node-bulletin-board\bulletin-board-app> docker run --publish 8000:8080 --detach --name bb bulletinboard:1.0
docker: Error response from daemon: Conflict. The container name "/bb" is already in use by container "4c92b81aa2db75572e042056ad538a734ce4a93f74fa92a7e8ff46d200a172cd". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
** I’ll name the container bbc;  
PS C:\users\micro\node-bulletin-board\bulletin-board-app> docker run --publish 8000:8080 --detach --name bbc  bulletinboard:1.0
ad204dba0dff743e8af3a2d55ff13b4ecac8e93e4d788d0376f0b7527ed381f2
docker: Error response from daemon: driver failed programming external connectivity on endpoint bbc (6a3ceda1accab2c7632c770b49360dd5c58a54f815e7e77e374a94a84f3456db): Bind for 0.0.0.0:8000 failed: port is already allocated.

PS C:\users\micro\node-bulletin-board\bulletin-board-app> docker container rename bb craig

Try and rebuild with container name bb, which was renamed to craig.
docker run --publish 8080:8080 --detach --name bb bulletinboard:1.0
1f6990c2b62768c4fcb8bcec53e2cb1683fcdd22004a1141890b202003704cf2
PS C:\users\micro\node-bulletin-board\bulletin-board-app>
We published to local host 8080


Hurray!  It worked.

*** Modify the Dockerfile FROM line to use a different base image.  For example, you could use “current-alpine” instead of “current-slim”
This implies I’m building a new Dockerfile.
Current-slim is the default base image.
docker build --tag bulletinboard:1.0 .

*** edited the Dockerfile and changed the FROM line to current-alpine.
PS C:\users\micro\node-bulletin-board\bulletin-board-app> docker build --tag bulletinboard:1.0 .
Sending build context to Docker daemon  45.57kB
Step 1/7 : FROM node:current-alpine
current-alpine: Pulling from library/node
cbdbe7a5bc2a: Already exists                                                                                            f2ddd8e54216: Pull complete                                                                                             1a9fa31d8c27: Pull complete                                                                                             5882f170bc7b: Pull complete                                                                                             Digest: sha256:2d362c5c4185417536013b668517913684ff0c80e801f4f74b07d13c0b195e37
Status: Downloaded newer image for node:current-alpine
 ---> b85fc218c00b
Step 2/7 : WORKDIR /usr/src/app
 ---> Running in e9645304c972
Removing intermediate container e9645304c972
 ---> c99d9019cf77
Step 3/7 : COPY package.json .
 ---> 11a0e10f7964
Step 4/7 : RUN npm install
 ---> Running in 0feed11ff569

> ejs@2.7.4 postinstall /usr/src/app/node_modules/ejs
> node ./postinstall.js

Thank you for installing EJS: built with the Jake JavaScript build tool (https://jakejs.com/)

npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN vue-event-bulletin@1.0.0 No repository field.
npm WARN The package morgan is included as both a dev and production dependency.

added 91 packages from 168 contributors and audited 92 packages in 4.156s
found 0 vulnerabilities

Removing intermediate container 0feed11ff569
 ---> 3415efd2be68
Step 5/7 : EXPOSE 8080
 ---> Running in 17fd5567b732
Removing intermediate container 17fd5567b732
 ---> 5ca7862a703a
Step 6/7 : CMD [ "npm", "start" ]
 ---> Running in 36fcb2a11c96
Removing intermediate container 36fcb2a11c96
 ---> 4262a825f4ee
Step 7/7 : COPY . .
 ---> 88967a209e69
Successfully built 88967a209e69
Successfully tagged bulletinboard:1.0
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.

** Build the docker image locally and test your changes.
We built it.  Now we have to run it.
Docker run –publish 8080:8080 –detach –name bb bulletinboard:1.0
Must remove or rename running container craig
First stop running craig
Removed container craig.
PS C:\users\micro\node-bulletin-board\bulletin-board-app>  docker run --publish 8000:8080 --detach --name craig bulletinboard:1.0
de4f31dfc431d6216dd377ccd03e08f988d920cb653ba89b0621f36258e282ef
PS C:\users\micro\node-bulletin-board\bulletin-board-app>




