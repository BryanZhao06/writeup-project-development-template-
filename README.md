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
> **Docker Image**: Treated as a read-only template that contains all of the technologies we need, runtimes, and the tools needed to run our code
> **Docker Container**: A runnable instance of the image that contains everything needed to run our application
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
3. Download the Docker extension onto any IDE you use (e.g. VSCode)
```powershell
# To check if you have Docker installed, type into your terminal
docker --version

# You should see something in the form of "Docker version ____, build ____"
# Example:

PS C:\Users\bryan> docker --version
Docker version 28.3.0, build 38b7060
