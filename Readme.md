# Multi-Service Docker Application with Nginx Reverse Proxy (AWS Deployed)

This project demonstrates a microservices architecture with two backend services (Go and Python) behind an Nginx reverse proxy, all containerized with Docker, and deployed on an **AWS EC2 Ubuntu Instance**.

## Architecture

```
Github → AWS EC2 → Nginx (Port 8080) → Service 1 (Go) | Service 2 (Python)
```

- **Nginx**: Reverse proxy routing requests to backend services
- **Service 1**: Go HTTP service on port 8001
- **Service 2**: Python Flask service on port 8002

## Project Structure

```
.
├── docker-compose.yml
├── nginx/
│   ├── nginx.conf
│   └── Dockerfile
├── service_1/
│   ├── main.go
│   ├── README.md
│   └── Dockerfile
├── service_2/
│   ├── app.py
│   ├── README.md
│   └── Dockerfile
└── README.md
```

## Quick Start (Local)

1. **Clone and navigate to project directory**
2. **Start all services with one command:**

```bash
docker-compose up --build
```

3. **Access services :**

### Locally:
- **Service 1 (Go):**
  - [http://localhost:8080/service1/ping](http://localhost:8080/service1/ping)
  - [http://localhost:8080/service1/hello](http://localhost:8080/service1/hello)

- **Service 2 (Python):**
  - [http://localhost:8080/service2/ping](http://localhost:8080/service2/ping)
  - [http://localhost:8080/service2/hello](http://localhost:8080/service2/hello)

- **Root Endpoint:** [http://localhost:8080/](http://localhost:8080/) (Lists available services)
- **Nginx Health Check:** [http://localhost:8080/health](http://localhost:8080/health)

### On AWS EC2:
- **Service 1 (Go):**
  - [http://18.212.10.67:8080/service1/ping](http://18.212.10.67:8080/service1/ping)
  - [http://18.212.10.67:8080/service1/hello](http://18.212.10.67:8080/service1/hello)

- **Service 2 (Python):**
  - [http://18.212.10.67:8080/service2/ping](http://18.212.10.67:8080/service2/ping)
  - [http://18.212.10.67:8080/service2/hello](http://18.212.10.67:8080/service2/hello)

- **Root Endpoint:** [http://18.212.10.67:8080/](http://18.212.10.67:8080/) (Lists available services)
- **Nginx Health Check:** [http://18.212.10.67:8080/health](http://18.212.10.67:8080/health)


## AWS Deployment Details

- **Cloud Provider:** Amazon Web Services (AWS)
- **Instance Type:** t2.micro
- **AMI:** Ubuntu 24.04 LTS 
- **Public IP:** `18.212.10.67`
- **Security Group:** Port 8080 open to `0.0.0.0/0` for public access

### **Production Access URLs (via AWS EC2):**

| Service / Endpoint     | URL                                                                              |
| ---------------------- | -------------------------------------------------------------------------------- |
| **Nginx Root**         | [http://18.212.10.67:8080/](http://18.212.10.67:8080/)                           |
| **Nginx Health Check** | [http://18.212.10.67:8080/health](http://18.212.10.67:8080/health)               |
| **Service 1 - Go**     | [http://18.212.10.67:8080/service1/ping](http://18.212.10.67:8080/service1/ping) |
| **Service 1 - Go**     | [http://18.212.10.67:8080/service1/hello](http://18.212.10.67:8080/service1/hello)|
| **Service 2 - Python** | [http://18.212.10.67:8080/service2/ping](http://18.212.10.67:8080/service2/ping) |
| **Service 2 - Python** | [http://18.212.10.67:8080/service2/hello](http://18.212.10.67:8080/service2/hello)|

## Features

### Nginx Reverse Proxy

- Path-based routing `/service1/*` and `/service2/*`
- Request logging with timestamp and path
- Health checks at `/health`
- Ready for load balancing

### Service Health Checks

- **Service 1**: `wget` on `/ping`
- **Service 2**: `curl` on `/ping`
- **Nginx**: Custom `/health` endpoint

### Docker Configuration

- Bridge networking for inter-container communication
- Multi-stage Go builds (smallest final image)
- Dependency-managed service startup via Docker Compose

## Monitoring and Logs

```bash
# All services logs
docker-compose logs -f

# Specific service logs
docker-compose logs -f nginx
docker-compose logs -f service1
docker-compose logs -f service2
```

Health Status:

```bash
docker-compose ps
```

## Development

Stop all services:

```bash
docker-compose down
```

Rebuild specific service:

```bash
docker-compose up --build service1
```

Access individual services (bypass nginx):

- [http://localhost:8001/ping](http://localhost:8001/ping)
- [http://localhost:8002/ping](http://localhost:8002/ping)

## Testing

```bash
# Service 1 (Go)
curl http://18.212.10.67:8080/service1/ping
curl http://18.212.10.67:8080/service1/hello

# Service 2 (Python)
curl http://18.212.10.67:8080/service2/ping
curl http://18.212.10.67:8080/service2/hello

# Nginx Health
curl http://18.212.10.67:8080/health
```

## Requirements Met

- Nginx reverse proxy in Docker
- Path-based routing: `/service1`, `/service2`
- Single exposed port: `8080`
- Docker Compose orchestration
- Health checks for Nginx, Go, and Python services
- Logging with timestamp and request path
- Bridge networking (no host network dependency)
- AWS EC2 deployment with public access

## Health Check Summary

| Component     | Endpoint  | Method |
| ------------- | --------- | ------ |
| **Nginx**     | `/health` | HTTP   |
| **Service 1** | `/ping`   | wget   |
| **Service 2** | `/ping`   | curl   |

## AWS EC2 Instance Details

| Field              | Value                             |
| ------------------ | --------------------------------- |
| **Instance Type**  | t2.micro                          |
| **Security Group** | Port 8080 open for 0.0.0.0/0      |

## Author

Anurag Chauhan\
Cloud & DevOps Enthusiast

