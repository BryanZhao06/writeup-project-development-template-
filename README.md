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
1. Go to [https://it-tools.tech](https://it-tools.tech) and find the "Docker run to Docker compose converter"
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
4. Congratulations! You have installed modules in your container :D

#### How to utilize the modules you installed:
1. Open up a notebook and import pipeline from our transformers library
2. Create a translation model and assign it to pipeline, which specifies the task of translating English to French
```Translator.ipynb

from transformers import pipeline

translation = pipeline("translation_en_to_fr")
```
3. Run the cell and download all of the dependencies
4. In a separate cell, create an fr instance that uses your translation model, which takes in any string of your choice
5. Call the instance by typing fr
```Translator.ipynb
fr = translation("Hello, I like to code")
fr

#Output: [{'translation_text': "Bonjour, j'aime coder"}]
```
6. To access just the text from our dictionary output, focus on the index 0 to cancel the list and focus on the key of the translation text to cancel the dictionary
```Translator.ipynb
fr[0]['translation_text']

#Output: "Bonjour, j'aime corder"
```
7. Congratulations! You successfully used a pipeline to translate English text to French :D

#### How to work with SRT files in our local machine
1. Download the `captions_english.srt` file from [https://drive.google.com](https://drive.google.com/file/d/16bCS1wbllBEyIAf70OweiWQXSKV_C2sl/view)
2. Move the file to the folder you have everything saved in and open it in your notebook using pysrt
3. Create a variable named subs that stores your srt file being opened
4. Loop through all of the english subs and translate it using the translation model we've created
5. Replace the subtitles in the srt file with the French translations
6. Save the newly translated file into a new file and run the cell
```Translator.ipynb
import pysrt

subs = pysrt.open("captions_english.srt")

for sub in subs:
    fr_text = translation(sub.text)[0]['translation_text']
    sub.text = fr_text

subs.save("captions_french.srt")
```
7. Congratulations! You now have an entirely new file that has all of the subtitles translated :D

#### How to copy files from system to image:
1. Go back to your Dockerfile and write the following:
```Dockerfile
COPY captions_english.srt
Translator.ipynb ./

#"COPY": takes files from our project folder and stores them directly on our image
#"./": our root directory that we are saving our files to
```
2. Navigate to your terminal and remove all of your stopped containers by calling `docker container prune`
3. Call `docker compose up` again to build our new image
4. Congratulations! You have successfully copied files from your system to image :D

#### How to push local image to remote repository
1. Navigate to Docker Hub -> Repositories
2. Create a repository by giving it a name and description and click Create
3. Find our local repository name by navigating back to our terminal and calling `docker images`
4. Look below REPOSITORY to find the name of our machine and copy it
5. Change the name of our local repository to match the name of our remote repository by calling
```powershell
docker image tag [local_repository_name]:[tag_name] [remote_repository_name]:1.0`
```
6. Call `docker images` again and your remote repository should show up
7. Upload our repository to Docker Hub by calling `docker push [remote_repository_name]:1.0`
8. Navigate back to our browser and hit refresh
9. Congratulations! You have pushed your local image to a remote repository on Docker Hub :D

#### How to clean up containers and images 
1. Prune our containers once more with `docker container prune`
2. Remove the local instance of our image with `docker rmi [remote_repository_name]:1.0`
3. Check it has been removed by calling `docker images`
4. Change the current directory of our terminal to have no files in it by calling `mkdir test`
5. Navigate there with `cd test`
6. Pull our remote image from Docker Hub with `docker pull [remote_repository_name]:1.0`
7. Run the image WITHOUT using compose by calling `docker run -p [port number of your choice]:[container port] [remote_repository_name]:1.0`
```powershell
docker run -p 5000:8888 bryanzhao786/srt-translator:1.0
```
8. Copy the URL from Jupyter and replace the container port number with the port number of your choice
9. Congratulations! You have cleaned up your containers and images while still keeping all of your file activity saved :D
