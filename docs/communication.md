# Forge – Inter-Service Communication

Forge uses synchronous requests and asynchronous events depending on the communication requirement.

## 1. Communication Methods

### REST

Used between the React frontend and API Gateway.

The API Gateway acts as the single public entry point for frontend requests.

### gRPC

Used for synchronous communication between backend services.

Protocol Buffers define the request and response structures.

Examples:
- API Gateway requests member information from Identity.
- AI/MCP requests task information from Tracker.
- Tracker requests member information from Identity when required.

### Event-Driven Communication

Used for asynchronous updates between services.

NATS or RabbitMQ will be used as the event broker. One will be selected for implementation.

Services publish events when important changes occur. Other services consume these events independently.

### WebSockets

Used for real-time messaging and presence updates between the frontend and Chat Service.

### GitHub Integration

GitHub webhooks notify the AI/MCP Service when relevant pull request events occur.

The Review Agent can then analyse the changes and generate review feedback using Claude.

## 2. Event List

| Event | Publisher | Consumers | Purpose |
|---|---|---|---|
| member.created | Identity | Tracker, Chat | A new member is registered |
| member.updated | Identity | Tracker, Chat | Member information changes |
| member.deactivated | Identity | Tracker, Chat | A member account is deactivated |
| project.created | Tracker | Chat | A new project is created; Chat can initialise its project channel |
| task.created | Tracker | AI/MCP | A new task is created |
| task.updated | Tracker | AI/MCP | Task details change |
| task.handed_off | Tracker | AI/MCP | Task responsibility changes |
| task.submitted_for_review | Tracker | AI/MCP | A task enters review |
| review.completed | AI/MCP | Tracker | An AI code review is completed |

## 3. Event Structure

Each event should contain:

- event_id
- event_type
- timestamp
- source_service
- entity_id
- event_data

Example:

```json
{
  "event_id": "unique-event-id",
  "event_type": "member.updated",
  "timestamp": "2026-11-08T12:00:00Z",
  "source_service": "identity",
  "entity_id": "member-123",
  "event_data": {
    "member_id": "member-123",
    "name": "Updated Name",
    "avatar_url": "profile-image-url"
  }
}
```

## 4. Communication Principles

- Use gRPC when a service needs an immediate response.
- Use events when other services need to react to a change.
- Consumers should handle duplicate events safely.
- Events should not expose unnecessary private information.
- Services should not directly access another service's database.
- Service failures should be isolated wherever possible.
