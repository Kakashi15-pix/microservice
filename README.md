# 🚀 Microservices Automation Platform

An intelligent microservices control plane featuring AI-driven automation, orchestration, monitoring, and management. Modular REST and WebSocket services communicate via event bus with LLM-powered agents handling deployment, scaling, health checks, and resource optimization.

## ✨ Key Features

- **AI Agent Orchestration**: LLM-powered agents (LangGraph) for intelligent service management
- **Modular Microservices**: Independent REST (product, payment) and WebSocket (chat, metrics) services
- **Service Registry & Health Monitoring**: Dynamic registration with heartbeat-based monitoring
- **Real-Time Dashboards**: Interactive Streamlit UI for agent decisions and metrics
- **Event-Driven Architecture**: RabbitMQ for inter-service communication
- **Full Observability**: OpenTelemetry tracing and metrics collection

## 📁 Project Structure

```
microservices/
├── rest/                    # REST services
│   ├── gateway/             # REST API gateway
│   ├── product-service/     # Product management
│   └── payment-service/     # Payment processing
├── ws/                      # WebSocket services
│   ├── gateway/             # WebSocket gateway
│   ├── chat-service/        # Real-time chat
│   └── metrics-service/     # Metrics streaming
├── services/                # Core platform services
│   ├── agent/               # AI control plane (orchestrator, registry, UI)
│   ├── file-service/        # Object storage
│   ├── notification-service/ # Notifications
│   └── user-service/        # Authentication & users
├── docker-compose.yml       # Complete stack orchestration
└── README.md
```

### Services Overview

| Service          | Type     | Purpose                          |
|------------------|----------|----------------------------------|
| REST Gateway     | REST    | API routing                      |
| Product Service  | REST    | Product CRUD                     |
| Payment Service  | REST    | Transactions                     |
| WS Gateway       | WS      | WebSocket routing                |
| Chat Service     | WS      | Real-time messaging              |
| Metrics Service  | WS      | Live metrics                     |
| Agent            | REST/WS | AI orchestration & registry      |
| File Service     | REST    | File storage                     |
| Notification     | REST    | Alerts & messaging               |
| User Service     | REST    | User management                  |

## 🛠 Tech Stack

- **Backend**: FastAPI, SQLAlchemy, Pydantic
- **AI/ML**: LangGraph, LangChain, Ollama
- **Data**: PostgreSQL, Redis, MinIO
- **Messaging**: RabbitMQ
- **Observability**: OpenTelemetry
- **Deployment**: Docker Compose

## 🚀 Quick Start

Use Docker Compose to deploy the full stack locally.

