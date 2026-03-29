# 🚀 Microservices Automation Platform

**An intelligent microservices control plane with AI-driven automation, orchestration, and monitoring.** This system combines a modular microservices architecture (REST + WebSocket services) with an LLM-powered agent that automates deployment, scaling, health management, and intelligent resource allocation across services.

---

## 📋 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Setup & Installation](#setup--installation)
- [Running Locally](#running-locally)
- [API Endpoints & Automation Workflows](#api-endpoints--automation-workflows)
- [Environment Variables](#environment-variables)
- [Contributing](#contributing)
- [License](#license)

---

## ✨ Features

- **🤖 AI Agent Orchestrator** — LangGraph + LLM (Ollama/OpenAI) powered agent for intelligent service management
- **🎯 Service Registry** — Dynamic service registration and heartbeat-based health monitoring
- **📦 Modular Services** — Fully isolated microservices (REST & WebSocket) with separate databases
- **📊 Real-Time Monitoring** — WebSocket-based metrics collection and live dashboards (Streamlit)
- **💬 Chat Services** — Real-time chat with WebSocket support
- **💳 Payment Processing** — Dedicated payment service with transaction management
- **📁 File Management** — MinIO/S3-compatible file storage with presigned URLs
- **📡 Observability** — OpenTelemetry integration for distributed tracing and metrics
- **🔄 Async Messaging** — RabbitMQ event bus for inter-service communication
- **🐳 Containerized** — Full Docker Compose stack for local development and production

---

## 🛠 Tech Stack

### Core Framework
- **FastAPI** — High-performance async Python web framework
- **SQLAlchemy** — Async ORM for database operations
- **Pydantic** — Data validation and settings management

### AI & Orchestration
- **LangGraph** — Graph-based orchestration and state management
- **Langchain** — LLM abstractions and tool integrations
- **Ollama** — Local LLM runtime (Mistral model)

### Databases & Persistence
- **PostgreSQL** — Relational databases (one per service)
- **Redis** — Caching and sessions
- **MinIO** — S3-compatible object storage

### Messaging & Observability
- **RabbitMQ** — Async event bus
- **OpenTelemetry** — Distributed tracing and metrics collection
- **Prometheus** — Metrics aggregation

### UI & Dashboards
- **Streamlit** — Interactive AI dashboard
- **WebSocket** — Real-time chat and metrics

### DevOps
- **Docker & Docker Compose** — Containerization and orchestration
- **Alembic** — Database migrations

---

## 📁 Project Structure

```
microservices/
├── rest/                          # REST-based services
│   ├── gateway/                   # API Gateway (reverse proxy for REST)
│   ├── product-service/           # Product CRUD operations
│   └── payment-service/           # Payment processing & transactions
├── ws/                            # WebSocket-based services
│   ├── gateway/                   # WebSocket Gateway
│   ├── chat-service/              # Real-time chat rooms
│   └── metrics-service/           # Real-time metrics collection
├── services/
│   ├── agent/                     # 🤖 AI Agent Control Plane
│   │   ├── control_api/           # Service registration & heartbeat API
│   │   ├── registry/              # Service registry & health monitor
│   │   ├── orchestrator/          # LangGraph workflow definitions
│   │   ├── agents/                # Specialized agents (scaling, deployment, etc.)
│   │   ├── ui/                    # Streamlit dashboard
│   │   └── monitoring/            # Metrics collection
│   ├── file-service/              # File upload/storage with MinIO
│   ├── notification-service/      # Email/SMS notifications (event-driven)
│   └── user-service/              # User management & authentication
├── otel-collector.yaml            # OpenTelemetry collector config
├── docker-compose.yml             # Full stack definition
├── .env.example                   # Environment variables template
└── README.md                      # This file
```

### Service Descriptions

| Service | Port | Type | Purpose |
|---------|------|------|---------|
| **REST Gateway** | 8000 | REST | Reverse proxy for `/api/*` routes |
| **Product Service** | 8001 | REST | Product catalog CRUD |
| **Payment Service** | 8002 | REST | Payment transactions & refunds |
| **WS Gateway** | 8010 | WebSocket | Gateway for WebSocket services |
| **Chat Service** | 8011 | WebSocket | Real-time chat rooms |
| **Metrics Service** | 8012 | WebSocket | Live metrics streaming |
| **Agent** | 8020 | REST + Async | AI orchestration & control plane |
| **Dashboard** | 8501 | Streamlit | Interactive AI dashboard |

---

## 🚀 Setup & Installation

### Prerequisites

- **Docker & Docker Compose** (v20.10+)
- **Python 3.11+** (for local development without Docker)
- **Git**

### 1. Clone the Repository

```bash
git clone https://github.com/Kakashi15-pix/microservice.git
cd microservice
```

### 2. Configure Environment Variables

```bash
cp .env.example .env
```

Edit `.env` with your configuration (see [Environment Variables](#environment-variables) section below).

### 3. (Optional) Create Python Virtual Environment

If running locally without Docker:

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r services/agent/requirements.txt
```

---

## 🎮 Running Locally

### Option A: Docker Compose (Recommended)

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down
```

The stack will be available at:
- **REST Gateway** — http://localhost:8000
- **WebSocket Gateway** — ws://localhost:8010
- **Agent API** — http://localhost:8020/docs
- **Streamlit Dashboard** — http://localhost:8501
- **RabbitMQ Management** — http://localhost:15672
- **MinIO Console** — http://localhost:9001

### Option B: Manual Setup (Local Development)

```bash
# Terminal 1: PostgreSQL
docker run --rm -e POSTGRES_PASSWORD=changeme -p 5432:5432 postgres:16-alpine

# Terminal 2: Redis
docker run --rm -p 6379:6379 redis:7-alpine

# Terminal 3: RabbitMQ
docker run --rm -p 5672:5672 -p 15672:15672 rabbitmq:3-management

# Terminal 4: Product Service
cd rest/product-service
pip install -r requirements.txt
uvicorn main:app --port 8001 --reload

# Terminal 5: Payment Service
cd rest/payment-service
pip install -r requirements.txt
uvicorn main:app --port 8002 --reload

# Terminal 6: Agent Service
cd services/agent
pip install -r requirements.txt
uvicorn main:app --port 8020 --reload
```

---

## 🔌 API Endpoints & Automation Workflows

### REST Gateway (`/api/*`)

#### Products API

```bash
# Create a product
curl -X POST http://localhost:8000/api/products \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Laptop Pro",
    "sku": "LP-2024",
    "price": 1299.99,
    "stock": 50
  }'

# List products
curl http://localhost:8000/api/products?skip=0&limit=20&active_only=true

# Get product details
curl http://localhost:8000/api/products/{product_id}

# Update product
curl -X PATCH http://localhost:8000/api/products/{product_id} \
  -H "Content-Type: application/json" \
  -d '{"price": 1199.99, "stock": 45}'

# Delete product
curl -X DELETE http://localhost:8000/api/products/{product_id}
```

#### Payments API

```bash
# Create payment
curl -X POST http://localhost:8000/api/payments \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "uuid",
    "product_id": "uuid",
    "amount": 1299.99
  }'

# List payments
curl 'http://localhost:8000/api/payments?user_id=uuid&skip=0&limit=20'

# Get payment details
curl http://localhost:8000/api/payments/{payment_id}

# Complete payment
curl -X POST http://localhost:8000/api/payments/{payment_id}/complete \
  -d "reference=TXN-123456"

# Refund payment
curl -X POST http://localhost:8000/api/payments/{payment_id}/refund
```

### Agent Control API (`/control/*`)

The agent control API is used by services to register themselves and report health:

```bash
# Register a service with the agent
curl -X POST http://localhost:8020/control/register \
  -H "Content-Type: application/json" \
  -H "X-Control-Key: ${CONTROL_API_KEY}" \
  -d '{
    "name": "product-service",
    "base_url": "http://product-service:8001",
    "health_url": "/health",
    "service_type": "rest",
    "instructions": "Alert if error rate > 5%"
  }'

# Send heartbeat (keep-alive)
curl -X POST http://localhost:8020/control/heartbeat \
  -H "Content-Type: application/json" \
  -H "X-Control-Key: ${CONTROL_API_KEY}" \
  -d '{
    "service_name": "product-service",
    "status": "healthy",
    "metrics": {"error_rate": 0.2, "response_time_ms": 45}
  }'

# List all registered services
curl http://localhost:8020/control/services \
  -H "X-Control-Key: ${CONTROL_API_KEY}"

# Get specific service details
curl http://localhost:8020/control/services/product-service \
  -H "X-Control-Key: ${CONTROL_API_KEY}"

# Update service management instructions
curl -X PATCH http://localhost:8020/control/services/product-service/instructions \
  -H "Content-Type: application/json" \
  -H "X-Control-Key: ${CONTROL_API_KEY}" \
  -d '{"instructions": "Scale to 3 instances if CPU > 80%"}'

# Deregister service
curl -X DELETE http://localhost:8020/control/services/product-service \
  -H "X-Control-Key: ${CONTROL_API_KEY}"
```

### WebSocket Services

#### Chat Service

```javascript
// Connect to chat room
const ws = new WebSocket("ws://localhost:8010/ws/chat/room-123");

// Send message
ws.send(JSON.stringify({
  user_id: "user-uuid",
  message: "Hello everyone!"
}));

// Receive messages
ws.onmessage = (event) => {
  console.log(JSON.parse(event.data));
};
```

#### Metrics Service (Real-time)

```javascript
// Stream metrics
const ws = new WebSocket("ws://localhost:8010/ws/metrics");

ws.onmessage = (event) => {
  const metrics = JSON.parse(event.data);
  console.log(`CPU: ${metrics.cpu}%, Memory: ${metrics.memory}%`);
};
```

### Agent Monitoring & Orchestration

The agent automatically:
- 📍 Discovers registered services via heartbeats
- 🏥 Monitors health and detects failures
- 🤖 Makes intelligent decisions using LLM
- ⚡ Scales services based on load
- 🔄 Triggers rollbacks on failures
- 📧 Sends alerts and notifications

View the Streamlit dashboard at http://localhost:8501 to see the agent's decisions in real-time.

---

## 🔐 Environment Variables

Create a `.env` file based on `.env.example`. Key variables:

### Database & Persistence

```bash
# PostgreSQL connection strings (one per service)
PRODUCT_DATABASE_URL=postgresql+asyncpg://postgres:changeme@product-db:5432/product_db
PAYMENT_DATABASE_URL=postgresql+asyncpg://postgres:changeme@payment-db:5432/payment_db
AGENT_DATABASE_URL=postgresql+asyncpg://postgres:changeme@agent-db:5432/agent_db

REDIS_URL=redis://redis:6379/0
RABBITMQ_URL=amqp://guest:guest@rabbitmq:5672/
```

### Security & Authentication

```bash
SECRET_KEY=your-super-secret-jwt-key-change-this-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
REFRESH_TOKEN_EXPIRE_DAYS=7

# Agent control API key (shared by all services)
CONTROL_API_KEY=change-me-agent-internal-key
```

### AI & LLM Configuration

```bash
OLLAMA_URL=http://ollama:11434/api/generate
OLLAMA_MODEL=mistral
AI_PROVIDER=ollama  # or "openai", "anthropic"
```

### File Storage (MinIO/S3)

```bash
MINIO_ENDPOINT=minio:9000
MINIO_ACCESS_KEY=minioadmin
MINIO_SECRET_KEY=minioadmin
MINIO_BUCKET=uploads
MINIO_SECURE=False
```

### Email/SMS Notifications

```bash
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=your-app-password
EMAIL_FROM_NAME=SaaS Platform

TWILIO_ACCOUNT_SID=your-twilio-account-sid
TWILIO_AUTH_TOKEN=your-twilio-auth-token
TWILIO_PHONE_NUMBER=+1234567890
```

### Service URLs

```bash
PRODUCT_SERVICE_URL=http://product-service:8001
PAYMENT_SERVICE_URL=http://payment-service:8002
CHAT_SERVICE_URL=http://chat-service:8011
METRICS_SERVICE_URL=http://metrics-service:8012
USER_SERVICE_URL=http://user-service:8000
AGENT_SERVICE_URL=http://agent:8020
```

### Observability

```bash
OTEL_EXPORTER_OTLP_ENDPOINT=http://otel-collector:4317
OTEL_SERVICE_NAME=microservices
DEBUG=False
```

See `.env.example` for the complete template with all configuration options.

---

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

1. **Fork the repository** and create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes** and ensure they follow the project's style:
   - Use type hints in Python code
   - Follow PEP 8 conventions
   - Add docstrings to new functions

3. **Test your changes**:
   ```bash
   # Run linting
   pylint services/agent/main.py
   
   # Run tests (if available)
   pytest tests/
   ```

4. **Commit with clear messages**:
   ```bash
   git commit -m "feat: add new service registration endpoint"
   ```

5. **Push and create a Pull Request**:
   ```bash
   git push origin feature/your-feature-name
   ```

### Development Tips

- Each service has its own `requirements.txt` — install with `pip install -r <path>/requirements.txt`
- Use `docker-compose logs <service_name>` to debug individual services
- The agent's control API key is required for all service registration calls (set in `.env`)
- Services should send heartbeats to the agent every 30 seconds for health tracking

---

## 📄 License

This project is licensed under the MIT License — see the LICENSE file for details.

---

## 🔗 Quick Links

- **API Documentation** — http://localhost:8020/docs (Agent API)
- **Dashboard** — http://localhost:8501 (Streamlit)
- **RabbitMQ Console** — http://localhost:15672
- **MinIO Console** — http://localhost:9001
