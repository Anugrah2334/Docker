# Microservices Architecture with Docker Swarm ⚓

This guide explains how to deploy a microservices architecture using Docker Swarm with an API Gateway and a Backend Service.


![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/docker-swarm.png)



## 🚀 Step 1: Install Docker & Initialize Docker Swarm

### 1.1 Install Docker
Ensure Docker is installed on your machine. Verify with:
```sh
docker --version
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot1.png)
If not installed, download it from Docker's official site and install it.

**Note:** Docker Desktop should be running in the background for Docker Swarm to work properly.

### 1.2 Initialize Docker Swarm
Start Docker Swarm:
```sh
docker swarm init
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot2.png)
This makes your machine the Swarm Manager.

## 📁 Project Structure
```
microservices-docker-swarm/
│── backend-service/
│   ├── backend.py
│   ├── Dockerfile
│
│── api-gateway/
│   ├── api_gateway.py
│   ├── Dockerfile
│
│── docker-compose.yml
│── README.md
```

## 🛠 Step 2: Create the Microservices Code

### Backend Service
Create `backend.py`:
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "rajput_tarakk"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```
Create `Dockerfile`:
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY backend.py /app
RUN pip install flask
CMD ["python", "backend.py"]
```

### API Gateway Service
Create `api_gateway.py`:
```python
from flask import Flask
import requests

app = Flask(__name__)

@app.route('/')
def hello():
    backend_response = requests.get('http://backend-service:5000')
    return f"API Gateway: {backend_response.text}"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=8080)
```
Create `Dockerfile`:
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY api_gateway.py /app
RUN pip install flask requests
CMD ["python", "api_gateway.py"]
```

## 📦 Step 3: Build Docker Images
```sh
docker build -t backend-service ./backend-service
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot3.png)
```sh
docker build -t api-gateway ./api-gateway
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot4.png)
```sh
docker images
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot5.png)

## 📜 Step 4: Create Docker Compose File for Swarm
Create `docker-compose.yml`:
```yaml
version: '3.7'

services:
  backend-service:
    image: backend-service
    deploy:
      replicas: 2
      restart_policy:
        condition: on-failure
    networks:
      - app-network
    ports:
      - "5000:5000"

  api-gateway:
    image: api-gateway
    deploy:
      replicas: 2
      restart_policy:
        condition: on-failure
    networks:
      - app-network
    ports:
      - "8080:8080"
    depends_on:
      - backend-service

networks:
  app-network:
    driver: overlay
```

## 🚀 Step 5: Deploy the Microservices to Docker Swarm
```sh
docker stack deploy -c docker-compose.yml my_microservices
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot6.png)

## 📊 Step 6: Verify the Deployment
```sh
docker stack services my_microservices
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot7.png)

```sh
docker ps
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot8.png)

## 🌐 Step 7: Access the Microservices
Open your browser and go to:
```sh
http://localhost:8080
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot9.png)
You should see: **API Gateway: rajput_tarakk**



## 🔄 Step 8: Scaling the Services
```sh
docker service scale my_microservices_backend-service=5
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot10.png)
```sh
docker stack services my_microservices
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot11.png)

## 🛠 Step 9: Updating the Services
Rebuild and update the backend service:
```sh
docker build -t backend-service ./backend-service
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot12.png)
```sh
docker service update --image backend-service:latest my_microservices_backend-service
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot13.png)

## 🛑 Step 10: Remove the Stack
```sh
docker stack rm my_microservices
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot14.png)
```sh
docker swarm leave --force
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Microservices%20Architecture%20using%20Docker%20Swarm/Screenshot15.png)

---
This guide helps you deploy a microservices architecture using Docker Swarm. 🚀
