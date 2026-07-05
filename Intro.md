Here’s a practical beginner path for your Windows laptop.

Docker Desktop on Windows is officially supported with WSL 2, and Docker recommends enabling the WSL 2 backend for Linux containers. ([Docker Documentation][1]) Docker Compose lets you run multi-container apps from one YAML file. ([Docker Documentation][2])

## 1. Install Docker

Open **PowerShell as Administrator**:

```powershell
wsl --install
```

Restart your laptop, then install **Docker Desktop for Windows** from Docker’s official site. After opening Docker Desktop, verify:

```powershell
docker --version
docker compose version
docker run hello-world
```

## 2. Real example: Python web app in Docker

Create a folder:

```powershell
mkdir docker-demo
cd docker-demo
```

Create `app.py`:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello Alan! This Python app is running inside Docker."

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

Create `requirements.txt`:

```txt
flask
```

Create `Dockerfile`:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
```

Build and run:

```powershell
docker build -t my-python-app .
docker run -p 5000:5000 my-python-app
```

Open:

```text
http://localhost:5000
```

## 3. Add Docker Compose

Create `compose.yaml`:

```yaml
services:
  web:
    build: .
    ports:
      - "5000:5000"
```

Run:

```powershell
docker compose up
```

Stop:

```powershell
Ctrl + C
docker compose down
```

## 4. Commands to learn

```powershell
docker ps
docker images
docker stop <container_id>
docker rm <container_id>
docker rmi <image_id>
docker logs <container_id>
docker exec -it <container_id> sh
```

## What you just learned

A **Dockerfile** builds an image.
An **image** is the packaged app.
A **container** is the running app.
**Compose** runs one or more containers with one command.

[1]: https://docs.docker.com/desktop/features/wsl/?utm_source=chatgpt.com "Docker Desktop WSL 2 backend on Windows | Docker Docs"
[2]: https://docs.docker.com/compose/gettingstarted/?utm_source=chatgpt.com "Docker Compose Quickstart | Docker Docs"
