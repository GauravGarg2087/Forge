# Forge – Architecture

Forge follows a microservices architecture with four independent backend services, a React frontend and an API Gateway.

## 1. Core Services

**Identity Service (Go)**
- Handles authentication, member profiles, roles and permissions.
- Owns the Member and Role data.

**Tracker Service (Python)**
- Manages projects, milestones, tasks, assignments, comments and activity timelines.
- Owns all project and task data.

**Chat Service (JavaScript/TypeScript)**
- Handles project channels, direct messages and real-time communication.
- Maintains its own chat data and local member information where needed.

**AI/MCP Service (Java)**
- Provides MCP tools and AI agents for task management, code reviews and project summaries.
- Interacts with other services through their APIs.

## 2. Supporting Components

- **Frontend:** React-based interface for users.
- **API Gateway:** Single entry point that handles authentication and routes frontend requests.
- **Databases:** PostgreSQL, with separate databases for Identity, Tracker and Chat.
- **GitHub:** Pull request tracking, CI integration and code reviews.
- **Claude:** AI-powered code review integration.

## 3. Communication

Different communication methods will be used depending on the requirement:

- **REST:** Frontend to API Gateway.
- **gRPC:** Synchronous communication between backend services, using Protocol Buffers.
- **NATS/RabbitMQ:** Asynchronous communication through events.
- **WebSockets:** Real-time communication with the Chat Service.

## 4. Design Principles

- Each service owns its data; databases are never shared directly.
- Services communicate through defined APIs and contracts.
- Failures should remain isolated wherever possible.
- Docker and Docker Compose will be used for local development and deployment.
