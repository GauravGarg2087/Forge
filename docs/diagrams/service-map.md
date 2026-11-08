# Forge – Service Map

```mermaid
flowchart TD
    U[User / Mentor] --> FE[React Frontend]
    FE -->|REST API| GW[API Gateway]

    GW -->|Authentication| ID[Identity Service - Go]
    GW -->|Projects and Tasks| TR[Tracker Service - Python]
    GW -->|Conversations| CH[Chat Service - JavaScript]

    ID --> IDDB[(Identity Database)]
    TR --> TRDB[(Tracker Database)]
    CH --> CHDB[(Chat Database)]

    TR -->|Member information when required - gRPC| ID

    AI[AI / MCP Service - Java]
    AI -->|gRPC| TR
    AI -->|gRPC| ID

    FE <-->|WebSocket connection via Gateway| CH

    ID -->|Member events| EB[Event Broker - NATS or RabbitMQ]
    TR -->|Project and Task events| EB
    CH -->|Chat events| EB

    EB -->|Member events| TR
    EB -->|Member and Project events| CH
    EB -->|Task events| AI

    GH[GitHub] -->|Pull Request Webhooks| AI
    AI -->|Code review request| CL[Claude API]
    CL -->|Review feedback| AI
    AI -->|Review result| TR

    GH -->|CI Status Updates| GW
```

## Service Responsibilities

- **Identity Service:** Owns member accounts, authentication and roles.
- **Tracker Service:** Owns projects, milestones, tasks, comments and task activity.
- **Chat Service:** Manages project channels, direct messages and real-time communication.
- **AI/MCP Service:** Provides MCP tools, AI agents and Claude-powered code reviews.
- **API Gateway:** Acts as the public entry point and routes frontend requests.
- **Event Broker:** Enables asynchronous communication between services.

## Communication Principles

- Each stateful service owns its own PostgreSQL database.
- Services communicate synchronously through gRPC when an immediate response is required.
- Events are used for asynchronous updates and to maintain local member caches.
- WebSockets provide real-time messaging.
- Services never directly access another service's database.
