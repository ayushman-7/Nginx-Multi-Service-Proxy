# 🔁 Nginx Reverse Proxy with Docker Compose

This project demonstrates a simple microservices architecture using Docker Compose and Nginx as a reverse proxy. It connects two backend services (a Go and a Python app) and routes traffic to them based on URL path prefixes via Nginx.

---

## 🧱 Project Structure

```
.
├── docker-compose.yml
├── nginx/
│   ├── nginx.conf
│   └── Dockerfile
├── service_1/ (Go Service)
│   ├── main.go
│   ├── Dockerfile
│   └── README.md
├── service_2/ (Python Service)
│   ├── app.py
│   ├── Dockerfile
│   ├── requirements.txt
│   └── README.md
└── README.md
```

---

## 🚀 How to Run

1. **Clone the repository**

```bash
git clone https://github.com/your-username/nginx-reverse-proxy-demo.git
cd nginx-reverse-proxy-demo
```

2. **Start the services**

```bash
docker-compose up --build
```

3. **Access the services in your browser or via curl:**

| URL | Description |
|-----|-------------|
| `http://localhost:8080/service1/ping` | Health check from Go service |
| `http://localhost:8080/service1/hello` | Hello message from Go |
| `http://localhost:8080/service2/ping` | Health check from Python service |
| `http://localhost:8080/service2/hello` | Hello message from Python |

---

## 🧠 How Routing Works

Nginx is configured as a reverse proxy. It inspects incoming URLs and forwards them to the appropriate backend:

```nginx
location /service1/ {
    proxy_pass http://service_1:8001/;
    rewrite ^/service1(/.*)$ $1 break;
}

location /service2/ {
    proxy_pass http://service_2:8002/;
    rewrite ^/service2(/.*)$ $1 break;
}
```

So requests like `/service1/ping` are routed internally to `http://service_1:8001/ping`.

---

## 💡 What I Did

- 🐳 Containerized both a Go and Python backend service
- 🔁 Configured Nginx as a reverse proxy with path-based routing
- ⚙️ Connected services with Docker Compose using bridge networking
- 🔍 Added request logging and health checks
- 🚦 Verified end-to-end request flow from browser → Nginx → backend

---

## ❤️ Tech Stack

- Docker
- Docker Compose
- Nginx (Alpine)
- Go 1.22
- Python 3.10
- Flask (Python microservice)

---

## 🩺 Health Checks

Defined in `docker-compose.yml` to ensure services are ready before Nginx starts routing:

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:8001/ping"]
```

Use this to view logs:

```bash
docker-compose logs service_1
docker-compose logs service_2
docker-compose logs nginx
```

---

## 🧪 Bonus

- ✅ Clean, minimal setup with modular Dockerfiles
- ✅ Nginx logs incoming requests with timestamps and paths
- ✅ Reverse proxy handles URL prefix rewriting cleanly

---

## 📦 Final Notes

To shut everything down:

```bash
docker-compose down -v
```

---

## 📬 Author

**Ayushman Tripathi**  
