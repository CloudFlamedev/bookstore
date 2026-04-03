# 📚 PageTurn Bookstore — DevOps Portfolio Project

A cloud-native bookstore application built to demonstrate end-to-end DevOps practices including containerization, CI/CD automation, infrastructure as code, and container orchestration.

---

## 🏗️ Architecture Overview

```
┌─────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Frontend  │────▶│   Book Service  │────▶│                 │
│   (Nginx)   │     │  (Spring Boot)  │     │   PostgreSQL    │
│   Port 80   │     │   Port 8080     │     │   Port 5432     │
└─────────────┘     └─────────────────┘     │                 │
                    ┌─────────────────┐     │                 │
                    │  Order Service  │────▶│                 │
                    │  (Spring Boot)  │     └─────────────────┘
                    │   Port 8081     │
                    └─────────────────┘
```

---

## 🛠️ Tech Stack

### Application
| Layer | Technology |
|---|---|
| Backend | Java 17 + Spring Boot 3 |
| Database | PostgreSQL 15 |
| Frontend | HTML5 + Vanilla JS + Nginx |
| Build Tool | Maven |

### DevOps
| Tool | Purpose |
|---|---|
| **Docker** | Containerization with multi-stage builds |
| **Docker Compose** | Local development orchestration |
| **Terraform** | Infrastructure as Code (AWS EC2) |
| **Jenkins** | CI/CD Pipeline automation |
| **Kubernetes** | Container orchestration (Minikube) |
| **AWS EC2** | Jenkins server (t3.micro, free tier) |
| **Docker Hub** | Container image registry |
| **GitHub** | Source control + webhook triggers |

---

## 📁 Project Structure

```
bookstore/
├── book-service/               # Books REST API
│   ├── src/
│   │   └── main/java/com/bookstore/book_service/
│   │       ├── Book.java
│   │       ├── BookController.java
│   │       ├── BookService.java
│   │       └── BookRepository.java
│   ├── Dockerfile
│   └── pom.xml
│
├── order-service/              # Orders REST API
│   ├── src/
│   │   └── main/java/com/bookstore/order_service/
│   │       ├── Order.java
│   │       ├── OrderController.java
│   │       ├── OrderService.java
│   │       └── OrderRepository.java
│   ├── Dockerfile
│   └── pom.xml
│
├── frontend/                   # Static UI
│   ├── index.html
│   ├── add-book.html
│   ├── orders.html
│   └── Dockerfile
│
├── k8s/                        # Kubernetes manifests
│   ├── book-service-deployment.yaml
│   ├── order-service-deployment.yaml
│   ├── frontend-deployment.yaml
│   ├── postgres-deployment.yaml
│   └── ingress.yaml
│
├── terraform/                  # AWS Infrastructure
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
│
├── Jenkinsfile                 # CI/CD Pipeline
├── docker-compose.yml          # Local dev setup
└── README.md
```

---

## 🚀 Running Locally

### Prerequisites
- Docker and Docker Compose installed
- Java 17 (for local development)

### Start the full stack

```bash
git clone https://github.com/CloudFlamedev/bookstore.git
cd bookstore
docker-compose up --build
```

### Access the services

| Service | URL |
|---|---|
| Frontend | http://localhost |
| Book Service API | http://localhost:8080/api/books |
| Order Service API | http://localhost:8081/api/orders |
| Book Service Health | http://localhost:8080/actuator/health |
| Order Service Health | http://localhost:8081/actuator/health |

---

## ☁️ Infrastructure Setup (Terraform)

Terraform provisions a Jenkins CI/CD server on AWS EC2 (free tier).

### Prerequisites
- AWS CLI configured (`aws configure`)
- Terraform installed
- EC2 Key Pair created

```bash
cd terraform
terraform init
terraform plan -var="key_name=bookstore-key"
terraform apply -var="key_name=bookstore-key"
```

### What gets created
- EC2 `t3.micro` instance (Jenkins server)
- Security Group (ports 22 and 8080)

### Destroy when not in use (saves free tier hours)
```bash
terraform destroy -var="key_name=bookstore-key"
```

---

## 🔄 CI/CD Pipeline (Jenkins)

The `Jenkinsfile` defines a 5-stage pipeline triggered on every GitHub push:

```
Checkout → Build Images → Push to Docker Hub → Deploy to Kubernetes
```

| Stage | What it does |
|---|---|
| Checkout | Clones the GitHub repository |
| Build book-service | Builds Docker image for book service |
| Build order-service | Builds Docker image for order service |
| Build frontend | Builds Docker image for frontend |
| Push to Docker Hub | Pushes all images to registry |
| Deploy to Kubernetes | Applies K8s manifests |

---

## ☸️ Kubernetes Deployment (Minikube)

```bash
# Start Minikube
minikube start

# Deploy all services
kubectl apply -f k8s/

# Check pods
kubectl get pods

# Check services
kubectl get services

# Access via Minikube
minikube service frontend
```

---

## 🐳 Docker Images

| Image | Docker Hub |
|---|---|
| Book Service | `utkrist/book-service:latest` |
| Order Service | `utkrist/order-service:latest` |
| Frontend | `utkrist/bookstore-frontend:latest` |

---

## 📡 API Endpoints

### Book Service (port 8080)
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/books` | Get all books |
| POST | `/api/books` | Add a new book |
| GET | `/actuator/health` | Health check |

### Order Service (port 8081)
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/orders` | Get all orders |
| POST | `/api/orders` | Place an order |
| GET | `/actuator/health` | Health check |

---

## 💡 Key DevOps Concepts Demonstrated

- **Multi-stage Docker builds** — smaller, secure production images
- **Infrastructure as Code** — reproducible AWS infra via Terraform
- **CI/CD automation** — zero-touch deployment on every git push
- **Container orchestration** — K8s rolling updates, health checks, HPA
- **Service discovery** — containers communicate via K8s DNS
- **Persistent storage** — PostgreSQL data survives container restarts

---

## 👨‍💻 Author

**Utkrist Gupta (CloudFlamedev)**
- GitHub: [@CloudFlamedev](https://github.com/CloudFlamedev)
- Role: Systems Development Engineer → DevOps/SRE
- Certifications: AWS Cloud Practitioner | Oracle Cloud Infrastructure GenAI Professional
