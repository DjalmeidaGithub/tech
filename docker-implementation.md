# Install Docker on Windows

---

## 1. Check System Requirements

- Windows 10 or 11, 64-bit.
- At least 4 GB RAM.
- Virtualization enabled in BIOS[web:11][web:12].

---

## 2. Download Docker Desktop

- Go to the official Docker website:  
  [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
- Click **Download for Windows**.
- Use this link to get more tutorials about docker:[https://www.docker.com/trainings/]
---

## 3. Run the Installer

- Locate the downloaded file (`Docker Desktop Installer.exe`), then double-click it to launch the installer
- When prompted, enable WSL 2 or Hyper-V (choose the option best suited for your system).
- Follow the on-screen wizard to complete installation.

---

## 4. Restart Your Computer

- After setup finishes, restart your computer if required.

---

## 5. Launch Docker Desktop

- Open Docker Desktop from the Start Menu.
- The Docker whale icon should appear in the taskbar.

---

## 6. Verify Installation

- Open a terminal (Command Prompt or PowerShell).
- Run:

docker --version

- You should see the installed Docker version.

---

Docker is now ready to use!

# How to Access a GitHub Repo in Visual Studio

This guide shows how to set up Visual Studio to access and work with the repository:  
`https://github.com/DjalmeidaGithub/tech/blob/main/docker-implementation.md`

---

## 1. Link Your GitHub Account to Visual Studio

- Open Visual Studio.
- Go to **File > Account Settings**.
- Add your GitHub account and sign in with your GitHub credentials.

---

## 2. Clone the Repository

- Go to **File > Open > Project from Source Control**.
- Select **Git**.
- Enter this repository URL:  

https://github.com/DjalmeidaGithub/tech.git

- Click **Clone**.

---

## 3. Open, Edit, and Sync

- After cloning, Visual Studio will open the repository as a local project.
- Browse the folders and open any file, such as `docker-implementation.md`.
- Make edits, commit changes, and push updates using the built-in Git window (**Git Changes**).

---

## 4. Git Workflow Features in Visual Studio

- **Branching:** Create and switch branches from the status bar.
- **Committing:** Edit files and commit changes directly.
- **Syncing:** Push and pull with GitHub easily.
- **Pull Requests:** Use Visual Studio's interface to make pull requests for collaborative work.

---

## Notes

- Ensure Git is installed (Visual Studio will prompt you if it’s missing).
- Your GitHub account should be authorized as described in Step 1.
- Visual Studio supports markdown editing and is suitable for collaboration on `.md` files.

---

# Docker Implementation on Windows Laptop: Frontend, Backend, and PostgreSQL

This guide provides a detailed, step-by-step process for setting up a Dockerized frontend and backend application connected to a PostgreSQL database on a Windows laptop. 
It also includes instructions for setting up the project on GitHub for collaborative changes.

The project structure includes a frontend, backend, and PostgreSQL database, all connected via a Docker network.

---

## 1. Install Docker on Windows

### 1.1. Download and Install Docker Desktop
- Go to [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/) and download the installer.
- Run the installer and follow the prompts.

### 1.2. Enable WSL 2 (Windows Subsystem for Linux)
- Open PowerShell as Administrator and run:
```
wsl --install

- Restart your computer if prompted.
- Set WSL 2 as the default version:

wsl --set-default-version 2
```
### 1.3. Enable Virtualization
- Ensure virtualization is enabled in your BIOS/UEFI settings.
- In Windows, enable the "Windows Subsystem for Linux" and "Virtual Machine Platform" features via **Turn Windows features on or off**.

### 1.4. Verify Installation
- Open a command prompt and run:
```
docker --version
docker-compose --version
```
- If both commands return version numbers, Docker is installed correctly.

---

## 2. Create a Dockerized Frontend and Backend with PostgreSQL

### 2.1. Project Structure
```
myapp/
├── backend/
│ ├── Dockerfile
│ ├── app.py
│ └── requirements.txt
├── frontend/
│ ├── Dockerfile
│ └── (your frontend code)
├── docker-compose.yml
└── .env (optional)
```

### 2.2. Backend Dockerfile (Python/Flask Example)

Use an official Python runtime as a parent image
```
FROM python:3.9-slim
Set the working directory in the container

WORKDIR /app
Copy the current directory contents into the container

COPY . /app
Install any needed packages

RUN pip install --no-cache-dir -r requirements.txt
Make port 5000 available to the world outside this container

EXPOSE 5000
Define environment variables

ENV FLASK_APP=app.py
ENV FLASK_ENV=development
Run app.py when the container launches

CMD ["flask", "run", "--host", "0.0.0.0"]

```

### 2.3. Frontend Dockerfile (React Example)

Use an official Node runtime as a parent image

```
FROM node:16-alpine
Set the working directory in the container

WORKDIR /app
Copy package.json and package-lock.json

COPY package*.json ./
Install dependencies

RUN npm install
Copy the rest of the application

COPY . .
Build the app

RUN npm run build
Serve the app

EXPOSE 3000
CMD ["npm", "start"]
```

### 2.4. Docker Compose File

```
version: '3.8'

services:
db:
image: postgres:13
environment:
POSTGRES_USER: postgres
POSTGRES_PASSWORD: postgres
POSTGRES_DB: postgres
volumes:
- postgres_data:/var/lib/postgresql/data
networks:
- mynetwork

backend:
build: ./backend
ports:
- "5000:5000"
depends_on:
- db
networks:
- mynetwork
environment:
- FLASK_ENV=development

frontend:
build: ./frontend
ports:
- "3000:3000"
depends_on:
- backend
networks:
- mynetwork

volumes:
postgres_data:

networks:
mynetwork:
driver: bridge

```
---

## 3. Build and Run the Containers

### 3.1. Build and Start the Containers
- Open a command prompt in the root folder and run:
```
docker-compose up --build
```
- This will build the images, create the containers, and start them.

### 3.2. Access the Services
- **Frontend:** Open [http://localhost:3000](http://localhost:3000) in your browser.
- **Backend:** Open [http://localhost:5000](http://localhost:5000) in your browser.
- **Database:** The PostgreSQL instance is accessible to the backend at `db:5432`.

### 3.3. Stop the Containers
- Press `Ctrl+C` in the terminal, then run:
```
docker-compose down
```

---

## 4. Set Up the Project on GitHub

### 4.1. Initialize a Git Repository
- In the root folder, run:

git init


### 4.2. Create a .gitignore File
Add the following contents to ignore Docker and local files:
```
Docker

.env
node_modules/
venv/
pycache/
*.pyc
Dockerfile
.dockerignore
docker-compose.override.yml
IDE

.idea/
.vscode/
```

### 4.3. Commit and Push to GitHub
Run the following commands:
```
git add .
git commit -m "Initial Docker setup for frontend and backend with PostgreSQL"
git branch -M main
git remote add origin https://github.com/yourusername/your-repo.git
git push -u origin main
```

---

## 5. Making Changes and Updating

### 5.1. Make Changes Locally
- Edit your frontend or backend code as needed.

### 5.2. Rebuild and Restart Containers
```
docker-compose build
docker-compose up
```

### 5.3. Commit and Push Changes
```
git add .
git commit -m "Updated frontend UI"
git push
```

### 5.4. Pull Changes on Another Machine
```
git pull
docker-compose build
docker-compose up
```

---

## 6. Troubleshooting Tips
- **Port conflicts:** Ensure ports 3000, 5000, and 5432 are free.
- **Database connection issues:** Check the backend logs for connection errors.
- **Docker Desktop issues:** Restart Docker Desktop if containers fail to start.

---

# Follow me at
- Newsletter: https://diamantinoalmeida.substack.com
- Website: https://diamantinoalmeida.com
- Mentorship: https://mentorcruise.com/mentor/diamantinoalmeida/
