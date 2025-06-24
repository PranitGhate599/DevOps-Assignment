
# 🧪 DevOps Intern Assignment: NGINX Reverse Proxy + Docker

This project demonstrates a multi-container setup using **Docker Compose** with two backend services — one in **Golang** and another in **Python (Flask)** — both reverse proxied by an **NGINX** container.

All services are accessible via a single port (`localhost:8080`) with routing handled by NGINX based on URL path prefixes.

---

## 📁 Project Structure

```
.
├── docker-compose.yml
├── nginx/
│   ├── nginx.conf
│   └── Dockerfile
├── service_1/
│   ├── main.go
│   ├── go.mod
│   └── Dockerfile
├── service_2/
│   ├── app.py
│   └── Dockerfile
└── README.md
```

---

## 🚀 Services Overview

### 🔧 service_1 (Golang)

- **Port:** 8001  
- **Routes:**
  - `/` → `{"service": "service1"}`
  - `/ping` → Health check: `{"status": "ok", "service": "1"}`
  - `/hello` → Greeting: `{"message": "Hello from Service 1"}`

---

### 🐍 service_2 (Python Flask)

- **Port:** 8002  
- **Routes:**
  - `/` → `{"service": "service2"}`
  - `/ping` → Health check: `{"status": "ok", "service": "2"}`
  - `/hello` → Greeting: `{"message": "Hello from Service 2"}`

---

### 🌐 NGINX Reverse Proxy

- **Port:** 8080 (host)
- **Reverse Routing:**
  - `/service1/` → `http://service1:8001/`
  - `/service2/` → `http://service2:8002/`
- **Features:**
  - Logs each request (with timestamp & status)
  - Runs inside its own container

---

## ⚙️ Setup Instructions

### ✅ Prerequisites

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

### ▶️ Run the System

```bash
docker compose up --build
```

This builds and runs all three containers.

---

## 🌐 Access the Services

Visit the following in your browser:

- [`http://localhost:8080/service1/`](http://localhost:8080/service1/)
- [`http://localhost:8080/service2/`](http://localhost:8080/service2/)

Test optional routes:

- [`http://localhost:8080/service1/ping`](http://localhost:8080/service1/ping)
- [`http://localhost:8080/service2/ping`](http://localhost:8080/service2/ping)
- [`http://localhost:8080/service1/hello`](http://localhost:8080/service1/hello)
- [`http://localhost:8080/service2/hello`](http://localhost:8080/service2/hello)

---

## 🩺 Health Checks

Each service uses a `curl`-based Docker health check:

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:<port>"]
  interval: 10s
  timeout: 5s
  retries: 3
```

---

## 🧠 NGINX Configuration Highlights

File: `nginx/nginx.conf`

```nginx
location /service1/ {
    proxy_pass http://service1:8001/;
}

location /service2/ {
    proxy_pass http://service2:8002/;
}
```

- Logs defined with custom format
- Listens on port 80 inside container (mapped to 8080 on host)

---

## 🧼 Clean Up

To stop and remove containers:

```bash
docker compose down
```

To remove cached builds:

```bash
docker system prune -af
```

---

## 📓 Notes

- Containers communicate over Docker's default **bridge network**.
- NGINX proxies to internal container ports directly.
- You only need **one port open** on your host: `8080`.

---

## 📹 Demo Video

A 5-minute video demonstration has been recorded explaining:
- The project architecture
- Each service's behavior
- How to build and run it
- Sample requests and outputs

> (https://drive.google.com/file/d/1okhP2eKpEbHuQULE9nEtIwZHF7L-SJJE/view?usp=sharing)

---

## 👤 Author

**Pranit Ghate**   
GitHub: [@pranitghate599](https://github.com/pranitghate599)

---

## 📬 Contact

Feel free to reach out for questions or collaboration:
- 📧 Email: pranitghate599@gmail.com
# DevOps-Assignment
