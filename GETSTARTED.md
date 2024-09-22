# Step-by-Step Guide to Set Up the Development Environment on Windows 11 for ***Proejct chatpm***

---

## 1. Prerequisites

Before starting, ensure you have administrative access to your Windows 11 machine. You will need to install the following:

- **Visual Studio Code (VSCode)**
- **Python 3.x**
- **Docker Desktop**
- **Node.js and npm**
- **Ollama**
- **Git** (optional, for version control)

---

## 2. Install and Configure Python

**Download and Install Python 3.x**

- Visit the official Python website: [Download Python](https://www.python.org/downloads/windows/)
- Download the latest Python 3.x installer for Windows.
- Run the installer:
  - Check the box **"Add Python to PATH"**.
  - Proceed with the installation.

**Verify the Installation**

- Open **Command Prompt**.
- Run:

  ~~~bash
  python --version
  ~~~

  You should see the installed Python version.

---

## 3. Install and Configure Docker Desktop

**Download and Install Docker Desktop**

- Visit the Docker website: [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/)
- Download the installer.
- Run the installer:
  - Accept the terms.
  - Select **"Use WSL 2 instead of Hyper-V"**.
- Restart your computer if prompted.

**Verify the Installation**

- Open **Command Prompt** or **PowerShell**.
- Run:

  ~~~bash
  docker --version
  ~~~

  You should see the Docker version installed.

**Enable WSL 2 Backend (if not enabled during installation)**

- Open Docker Desktop.
- Go to **Settings** > **General**.
- Check **"Use the WSL 2 based engine"**.

---

## 4. Set Up PostgreSQL with Docker

**Pull the PostgreSQL Docker Image**

- Open **Command Prompt** or **PowerShell**.
- Run:

  ~~~bash
  docker pull postgres
  ~~~

**Create a Docker Network (Optional)**

- For better container communication:

  ~~~bash
  docker network create project_network
  ~~~

**Run the PostgreSQL Container**

- Run:

  ~~~bash
  docker run --name postgres_db --network project_network -e POSTGRES_PASSWORD=your_password -p 5432:5432 -d postgres
  ~~~

  - Replace `your_password` with a secure password.

**Verify PostgreSQL is Running**

- Run:

  ~~~bash
  docker ps
  ~~~

  You should see the `postgres_db` container running.

---

## 5. Install Node.js and npm

**Download and Install Node.js**

- Visit the Node.js website: [Node.js Download](https://nodejs.org/en/download/)
- Download the LTS version for Windows.
- Run the installer and follow the prompts.

**Verify the Installation**

- Open **Command Prompt**.
- Run:

  ~~~bash
  node --version
  npm --version
  ~~~

---

## 6. Install Ollama and Set Up LLM

**Note:** As of my knowledge cutoff in September 2021, Ollama might not support Windows directly. Please verify on the official website.

**Download and Install Ollama**

- Visit the Ollama website: [Ollama Download](https://www.ollama.ai/download)
- Follow the installation instructions for Windows.

**Verify the Installation**

- Open **Command Prompt**.
- Run:

  ~~~bash
  ollama --version
  ~~~

**Set Up Llama 3.1:8b Model**

- Follow Ollama's documentation to download and set up the Llama 3.1:8b model.
- Ensure the model is properly installed and accessible.

---

## 7. Set Up Project Directory Structure

**Create the Project Directory**

- Open **Command Prompt**.
- Navigate to your preferred development folder.
- Run:

  ~~~bash
  mkdir project-management-chatbot
  cd project-management-chatbot
  ~~~

**Create Backend and Frontend Directories**

- Run:

  ~~~bash
  mkdir backend
  mkdir frontend
  ~~~

---

## 8. Set Up Backend Environment (FastAPI)

**Navigate to the Backend Directory**

- Run:

  ~~~bash
  cd backend
  ~~~

**Create a Virtual Environment**

- Run:

  ~~~bash
  python -m venv venv
  ~~~

**Activate the Virtual Environment**

- For **Command Prompt**:

  ~~~bash
  venv\Scripts\activate
  ~~~

- For **PowerShell**:

  ~~~bash
  venv\Scripts\Activate.ps1
  ~~~

**Install Required Packages**

- Run:

  ~~~bash
  pip install fastapi uvicorn[standard] sqlalchemy psycopg2-binary
  ~~~

**Freeze the Requirements**

- Run:

  ~~~bash
  pip freeze > requirements.txt
  ~~~

**Create a Basic FastAPI App**

- Create a file named `main.py` with a basic FastAPI app:

  ~~~python
  # backend/main.py

  from fastapi import FastAPI

  app = FastAPI()

  @app.get("/")
  async def root():
      return {"message": "Hello World"}
  ~~~

---

## 9. Set Up Frontend Environment (Vue.js)

**Navigate to the Frontend Directory**

- Go back to the project root:

  ~~~bash
  cd ..
  ~~~

- Then to the frontend directory:

  ~~~bash
  cd frontend
  ~~~

**Initialize a Vue.js Project**

- Install Vue CLI globally (if not already installed):

  ~~~bash
  npm install -g @vue/cli
  ~~~

- Create a new Vue.js project in the current directory:

  ~~~bash
  vue create .
  ~~~

  - When prompted, you can choose the default settings or customize as per your requirements.

**Run the Vue.js App**

- To verify, run:

  ~~~bash
  npm run serve
  ~~~

- Open a browser and go to `http://localhost:8080` to see the default Vue.js app.

---

## 10. Create Dockerfiles and Docker Compose

**Backend Dockerfile**

- In the `backend` directory, create a file named `Dockerfile`.

- Add the following content:

  ~~~dockerfile
  # backend/Dockerfile

  FROM python:3.9-slim

  WORKDIR /app

  COPY requirements.txt .

  RUN pip install --no-cache-dir -r requirements.txt

  COPY . .

  EXPOSE 8000

  CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
  ~~~

**Frontend Dockerfile**

- In the `frontend` directory, create a file named `Dockerfile`.

- Add the following content:

  ~~~dockerfile
  # frontend/Dockerfile

  FROM node:14

  WORKDIR /app

  COPY package*.json ./

  RUN npm install

  COPY . .

  EXPOSE 8080

  CMD ["npm", "run", "serve"]
  ~~~

**Create docker-compose.yml**

- In the root project directory (`project-management-chatbot`), create a file named `docker-compose.yml`.

- Add the following content:

  ~~~yaml
  version: '3'

  services:
    backend:
      build: ./backend
      container_name: backend
      volumes:
        - ./backend:/app
      ports:
        - "8000:8000"
      networks:
        - project_network
      depends_on:
        - postgres_db

    frontend:
      build: ./frontend
      container_name: frontend
      volumes:
        - ./frontend:/app
      ports:
        - "8080:8080"
      networks:
        - project_network

    postgres_db:
      image: postgres
      container_name: postgres_db
      environment:
        - POSTGRES_PASSWORD=your_password
      ports:
        - "5432:5432"
      networks:
        - project_network
      volumes:
        - postgres_data:/var/lib/postgresql/data

  networks:
    project_network:
      driver: bridge

  volumes:
    postgres_data:
  ~~~

---

## 11. Install VSCode Extensions

**Open Visual Studio Code**

- Install the following extensions:

  - **Python** (by Microsoft)
  - **Docker** (by Microsoft)
  - **ESLint** (for JavaScript linting)
  - **Vetur** (Vue tooling)
  - **Prettier - Code formatter** (optional)
  - **GitLens** (optional, for Git support)

---

## 12. Verify the Setup

**Build and Run the Docker Containers**

- Open a terminal in VSCode or Command Prompt.
- Navigate to the project root directory (`project-management-chatbot`).
- Run:

  ~~~bash
  docker-compose up --build
  ~~~

- Wait for the services to build and start.

**Verify Backend Service**

- Open a browser and go to `http://localhost:8000/docs`
  - You should see the automatic API documentation provided by FastAPI.

**Verify Frontend Service**

- Open a browser and go to `http://localhost:8080`
  - You should see the default Vue.js application.

**Verify PostgreSQL Service**

- Use a database client like **pgAdmin** or **DBeaver**.
- Connect to the database:
  - Host: `localhost`
  - Port: `5432`
  - User: `postgres`
  - Password: `your_password`

**Test Communication with Ollama**

- Ensure Ollama is installed and running.
- Refer to Ollama's documentation for testing.

---

## 13. References for Extended Reading

- **Dockerfile Reference**
  - Official documentation: [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
- **Docker Compose**
  - Official documentation: [Docker Compose overview](https://docs.docker.com/compose/)
- **FastAPI**
  - Official documentation: [FastAPI](https://fastapi.tiangolo.com/)
- **Vue.js**
  - Official documentation: [Vue.js Guide](https://vuejs.org/v2/guide/)
- **Ollama**
  - Official documentation: [Ollama Documentation](https://www.ollama.ai/docs)
- **PostgreSQL**
  - Official documentation: [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- **Python Virtual Environments**
  - Official documentation: [venv — Creation of virtual environments](https://docs.python.org/3/library/venv.html)
- **Visual Studio Code**
  - Official documentation: [VSCode Docs](https://code.visualstudio.com/docs)

---

## Why Each Step is Necessary

1. **Prerequisites**

   Ensuring all necessary tools are installed sets the foundation for the development environment. Without these, you cannot proceed with the setup or development of the project.

2. **Install and Configure Python**

   Python is required for backend development using FastAPI. Installing and configuring it ensures you can develop and run the backend services effectively.

3. **Install and Configure Docker Desktop**

   Docker allows for containerization of applications, ensuring consistency across different environments. It's essential for running services like PostgreSQL and deploying the application.

4. **Set Up PostgreSQL with Docker**

   PostgreSQL is the database used for data persistence. Running it in Docker simplifies setup and ensures a consistent environment across development and production.

5. **Install Node.js and npm**

   Node.js and npm are required for frontend development with Vue.js. They enable you to install dependencies and run the frontend application.

6. **Install Ollama and Set Up LLM**

   Ollama is used to run the local language model (LLM) Llama 3.1:8b, which powers the chatbot's natural language processing. Setting this up is crucial for the chatbot functionality.

7. **Set Up Project Directory Structure**

   Organizing the project into backend and frontend directories keeps the codebase clean and manageable, facilitating efficient development and collaboration.

8. **Set Up Backend Environment (FastAPI)**

   Establishes the Python environment and sets up a basic FastAPI app, laying the groundwork for backend development and API creation.

9. **Set Up Frontend Environment (Vue.js)**

   Initializes the Vue.js application, providing the foundation for frontend development and user interface design.

10. **Create Dockerfiles and Docker Compose**

    Dockerfiles define how to build Docker images for the backend and frontend. Docker Compose allows you to run multiple containers together, simplifying the development and deployment process.

11. **Install VSCode Extensions**

    Enhances the development experience by providing language support, debugging tools, and code formatting, making coding more efficient and reducing errors.

12. **Verify the Setup**

    Testing each component ensures they are correctly installed and can communicate with each other, preventing issues later in development.

13. **References for Extended Reading**

    Provides resources for deeper understanding, allowing you to learn more about each tool and resolve any issues that may arise.

---
