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
Docker is an open-source software platform that helps developers and DevOps engineers build, share, and run application into standardized units called containers. It solves the problem of inconsistent environments ("But it works on my machine!")


## 2. Core Features & Capabilities  
List and briefly describe the main features or components of the tool.
> _Example:_  
> - **Prompt Templates:** Reusable prompt structures for consistent LLM queries.  
> - **Memory:** Enables context retention between user interactions.  
> - **Agents:** Allow LLMs to decide dynamically which tools or actions to use.

---

## 3. Role in Our Project  
Explain how this tool contributes to or enhances the **AI Knowledge Management Agent** system.  
- Which subsystem it affects (API, data, AI pipeline, etc.)  
- Why it was chosen  
- How it supports scalability, usability, or research goals  

> _Example:_  
> LangChain acts as the orchestration layer that connects our GPT-based reasoning model with ChromaDB (for memory storage) and FastAPI (for API exposure).

---

## 4. Installation / Setup Guide  
Document how to install and configure this tool.  
Include terminal commands, environment variables, or configuration steps.

```bash
# Example setup
pip install langchain openai chromadb
