# OpenFOAM Dockerfiles

This repository contains OpenFOAM Dockerfiles. Both openfoam.com and openfoam.org releases are included. Both are based on a Ubuntu Focal (20.04 LTS) image.

### What is Docker and why?

Docker is a set of tools to manage, build and run various software installations with a number of advantages. Using Docker we start containers (think of shipping containers)from images. Containers include everything needed to run a particular set of tasks (in this case OpenFOAM simulations). As containers are standardised, you can be confident that they run the same way on Windows, macOS and Linux. And yes, they are future proof and run similarly on major cloud solutions such as Amazon Web Services (AWS) and Microsoft Azure. This is convenient (!) because we can copy our setup to a cloud solution or a friend’s computer and still be confident it runs the same way.

### Docker containers, Docker images and Dockerfiles

Instead of shipping the complete container including the operating system, it’s more convenient to use what we call Dockerfiles. Think of Dockerfiles as recipes (simply just text file with set of commands) that outlines how to build an image. A container may then be started from the Docker image

### Contributing to this project

Feel free to fork these Docker files. If you make an improvement you are most welcome to make a pull request and you will be added to the author list. Comments are also welcome.

# Setup
This guide explains how to setup OpenFOAM with Docker. It contains both OpenFOAM fundation releases (from openfoam.org) and OpenFOAM ESI releases (from openfoam.com).

## 1. Prerequisites
*1a)* Install [Docker](https://www.docker.com/products/docker-desktop)

Windows only: You will be prompted to install WSL (Windows Subsystem for Linux) when installing Docker (in the instructions
please follow step 4 and 5).

*1b)* Install [Git](https://git-scm.com/downloads) for your operating system (Windows, macOS or Linux)

*1c)* Install the latest [Paraview](https://www.paraview.org/download/) version. Feel free to choose the MPI versions, which let's you run Paraview in parallel.

Windows only: Choose the .exe file

macOS: Choose the .pkg file

Linux: Choose the .tar.gz archieve and extract it

## 2. Preparing for OpenFOAM
*2a)* Decide on a OpenFOAM version to install. List available versions by folder names in this repository. Following the steps in guide will give you the latest ESI release of OpenFOAM. Replace ```esi``` with ```foundation``` to install the latest foundation release (e.g. 7, 8, 9)

*2b)* Open a Powershell (Windows) or a terminal (macOS or Linux) and run the following commands. First, make a folder to store your OpenFOAM data:

```shell
mkdir $HOME/openfoam-data
```

*2c)* Next, clone this repository by:

```shell
git clone https://github.com/jakobhaervig/openfoam-dockerfiles.git $HOME/openfoam-dockerfiles
```

You should now have two folder "openfoam-data" and "openfoam-dockerfiles" in your home folder.

*2d)* Now, make sure Docker is running before continuing.

*2e)* Build the OpenFOAM image:

```shell
docker image build -t openfoam/esi:latest $HOME/openfoam-dockerfiles/esi/latest/
```

*2f)* Build the FreeCad image:
```shell
docker image build -t openfoam/freecad:latest $HOME/openfoam-dockerfiles/freeCad
```

## 3. Run the Docker container

*3a)* Finally, start a Docker container with ``/data`` mapped to ``$HOME/openfoam-data``:

```shell
docker container run -ti --rm -v $HOME/openfoam-data:/data -w /data openfoam/esi:latest
```

*3b)* for the FreeCAD image.

```shell
docker container run -ti --rm -v $HOME/openfoam-data:/data -w /data openfoam/freecad:latest
```

*Please note* that everything in the container is deleted when you exit the container. Therefore you should save your simulation results and solver development in ``/data``, which is the only directory that persists when the container is closed.

Running the above command should leave you inside the Docker container with the username "foam". 
Also, you may access the container through ``$HOME/openfoam-data`` e.g.:

On a Windows system: ``C:\Users\jakob\openfoam-data``

On a macOS system: ``/Users/jakob/openfoam-data``

On most Linux systems: ``/home/jakob/openfoam-data``

## 4. Optional Save an alias for running the Docker container
Instead of using the command in docker container with the command in [3. Run the Docker container](#3-run-the-docker-container), we can save an alias for that command. So when you open a Powershell (Windows) or a terminal (macOS or Linux) you can simply type e.g. ```of``` (short for OpenFOAM) to start the Docker container.

### **Windows operating system**
*4a)* Open a Powershell with Administrator Rights and enter the follwing commands (copy/paste).

*4b)* Allow scripts to be run:
```shell
Set-ExecutionPolicy RemoteSigned
```

*4c)* Create a ```profile``` file to store our alias function:
```shell
New-Item -Path $profile -ItemType file -force
```

*4d)* Add the alias to the newly created file:
```shell
echo "function fcn-latest {
  docker container run -ti --rm -v $HOME/openfoam-data:/data -w /data openfoam/esi:latest
  }
Set-Alias of fcn-latest
" > $profile
```

### **macOS and Linux systems**
*4a)* 
Copy/paste the following code snippet in the terminal:
```shell
case "$OSTYPE" in
  linux*)   echo "alias of='docker container run -ti --rm -v $HOME/openfoam-data:/data -w /data openfoam/esi:latest'" >> $HOME/.bashrc ;;
  darwin*)  echo "alias of='docker container run -ti --rm -v $HOME/openfoam-data:/data -w /data openfoam/esi:latest'" >> $HOME/.zprofile ;;
  *)        echo "This function is not yet added for $OSTYPE" ;;
esac
```

## 4. Author list

Jakob Hærvig
