# Docker Research Report

---

## Basic Information

| Field | Details |
|-------|----------|
| **Name** | Bryan Zhao |
| **Project** | AI Knowledge Management Agent |
| **Role** | DevOps Engineer |
| **Tool / Skill** | Docker |
| **Date** | 11 October 2025 |
| **Links / Sources** | [Official Docs](https://www.docker.com) · [GitHub Repo](https://github.com/docker/docs) · [YouTube Tutorial](https://www.youtube.com/watch?v=DQdB7wFEygo) |
---

## 1. Overview  
Provide a concise explanation of what this tool or skill is.  
- What problem does it solve?  
- What is its main purpose or use case?  
- Who typically uses it?
> **Docker** is an open-source software platform that helps developers and DevOps engineers build, share, and run application into standardized units called containers. It solves the problem of inconsistent environments ("But it works on my machine!")


## 2. Core Features & Capabilities  
List and briefly describe the main features or components of the tool. 
> **Docker Image**: Treated as a read-only template that contains all of the technologies we need, runtimes, and the tools needed to run our code (blueprint)
> **Docker Container**: A runnable instance of the image that contains everything needed to run our application (the actual home)
> **Docker Compose**: A tool for defining and running multi-container applications using a single YAML file to configure all the application's services 
---

## 3. Role in Our Project  
Explain how this tool contributes to or enhances the **AI Knowledge Management Agent** system.  
- Which subsystem it affects (API, data, AI pipeline, etc.)  
- Why it was chosen  
- How it supports scalability, usability, or research goals  

> Docker acts as the glue that bridges the gap between our development and operations so that we can run everything through one platform without having to deal with the "But it works on my machine!" issue. It affects all of our subsystems by being the binding agent that connects our backend, database, and frontend altogether into multiple containers.

---

## 4. Installation / Setup Guide  
Document how to install and configure this tool.  
Include terminal commands, environment variables, or configuration steps.

1. Go to [https://www.docker.com](https://www.docker.com)
2. Select "Download Docker Desktop" and download for the system you use (Mac/Windows/Linux)
3. Download the Docker extension onto VSCode
```powershell
# To check if you have Docker installed, type into your terminal
docker --version

# You should see something in the form of "Docker version ____, build ____"
# Example:

PS C:\Users\bryan> docker --version
Docker version 28.3.0, build 38b7060
```
4. Download the Dev Containers Extension on VSCode (Allows us to easily modify any container we want)
6. Congrats! You now have Docker installed :D

### Configuration Demo using Jupyter Notebook Image
1. Open Docker Desktop and make sure that it says `"Engine running"` on the bottom left of the screen
2. Go to [https://hub.docker.com]([https://hub.docker.com]) and type in the search bar "Jupyter Tensorflow"
3. Copy the Docker Pull Command and paste the command into your terminal `docker pull jupyter/tensorflow-notebook`
```powershell
#How to read what the terminal command means:
docker pull jupyter/tensorflow-notebook

"jupyter": the name of the community that maintains this image
"tensorflow-notebook": the name of the image itself
"jupyter/tensorflow-notebook": the name of the repository
```
5. Turn our Jupyter image into a container by typing the command "docker run", followed by the name of the repository
```powershell
docker run jupyter/tensorflow-notebook
```
7. Copy one of the URLS Jupyter provides us and paste it into our browser

Oh no! Big error! How come?

Reason: We are not communicating with our own operating system! The terminal says that the file is from a person named Jovyan, which is not who we are. These are the propertiees of our container, an isolated process on our computer, and because it is isolated, we get an error when we try to access it from the outside

How do we solve it?
8. Collapse our notebook with `Ctrl + C` and press the up key to fetch our most recent terminal command `docker run jupyter/tensorflow-notebook`
9. In front of of our repository name, add -p (ports), choose a port from our host system (e.g. 8000), and then the port from our container (8888)
```powershell
docker run -p 8000:8888 jupyter/tensorflow-notebook
```
10. Return to your browser and type `localhost:8000`
11. Copy our token from Jupyter (the part in the URL after lab?token=) and paste it as our password
12. Congratulations! You're in! :D

#### How to set a password to access your notebook
1. Go to [https://it-tools.tech]([https://it-tools.tech]) and find the "Docker run to Docker compose converter"
2. Paste in your terminal command and download the docker-compose.yml file that is generated
3. Open up the docker-compose.yml file in VSCode and add the following key to your file
```docker-compose.yml
environment:
  - JUPYTER_TOKEN=[password of your choice]
```
4. Navigate to `localhost:8000` again and type in the password you set
5. Congratulations! You set up the password! :D

#### How to preserve our files through Drive Mounting (exposing a folder from our container to a folder on our host machine)
1. In the same file, add the following key to your file
```docker-compose.yml
volumes:
  - ./:/home/jovyan

#How to read what this key means:
"./": the current directory of our terminal (aka the folder where your compose file lives)
":/home/jovyan": the directory of our container (Remember when we ran docker run without our port and we ran into an error because we weren't Jovyan? That's this directory!)
```
2. Move the file into the folder you wish to store it in and open up a terminal instance inside the current directory
3. Run your container in Docker Compose by typing `docker compose up` in your terminal
4. Congratulations! You now have docker-compose.yml

#### How to install modules in your container:
1. Go to your .yml file and replace your existing image with `build: ./` (specifies the location of the file)
2. Go back to VSCode and create a new file called Dockerfile
3. In Dockerfile, write the following:
```Dockerfile
FROM jupyter/tensorflow-notebook

USER $NB_UID

RUN pip install --upgrade pip && \
    pip install "transformers[sentencepiece]" pysrt && \
    pip install --index-url https://download.pytorch.org/whl/cpu torch && \
    fix-permissions "/home/${NB_USER}"

#"FROM": specifies the parent image on which our container is based
#"USER $NB_UID": Jovyan, the username with the right permissions (usernames differ depending on the base image, replace $NB_UID with root in case you don't know the specific username)
#"RUN": allows us to install our necessary modules
```
