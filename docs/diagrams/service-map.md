# Forge – Service Map

```mermaid
flowchart TD
    U[User] --> FE[React Frontend]
    FE -->|REST| GW[API Gateway]

    GW --> ID[Identity Service - Go]
    GW --> TR[Tracker Service - Python]
    GW --> CH[Chat Service - JavaScript]
    
    ID --> IDDB[(Identity DB)]
    TR --> TRDB[(Tracker DB)]
    CH --> CHDB[(Chat DB)]

    AI[AI/MCP Service - Java] -->|gRPC| TR
    AI -->|gRPC| ID

    ID <-->|gRPC| TR
    ID <-->|gRPC| CH
    TR <-->|gRPC| CH

    ID -.->|Events| EB[NATS / RabbitMQ]
    TR -.->|Events| EB
    CH -.->|Events| EB

    FE <-->|WebSockets| CH

    GH[GitHub] --> AI
    CL[Claude] --> AI
