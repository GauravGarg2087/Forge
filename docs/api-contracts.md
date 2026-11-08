# Forge – API and Shared Contracts

Services communicate through clearly defined contracts. Protocol Buffers define the gRPC interfaces used for communication between backend services.

## 1. Shared Member Contract

The Identity Service owns the Member entity and provides member information to other services.

The shared Member contract contains:

| Field | Type | Description |
|---|---|---|
| member_id | UUID | Unique member identifier |
| name | String | Member's display name |
| email | String | Member's email address |
| avatar_url | String | Profile image URL |
| updated_at | Timestamp | Last profile update |

### Consumers of the Member Contract

| Consumer | How it uses Member information |
|---|---|
| Tracker Service | Stores member IDs and caches basic profile information |
| Chat Service | Stores member IDs and caches basic profile information |
| API Gateway | Uses Identity to authenticate users and obtain member information |
| AI/MCP Service | Uses member IDs to identify users making requests |

### How Member Information is Shared

- Identity remains the single owner of Member data.
- Tracker and Chat reference members using member_id.
- Both services maintain local caches containing basic member information.
- Identity publishes member.created, member.updated and member.deactivated events.
- Tracker and Chat consume these events to keep their caches updated.
- Services can request the latest member information from Identity through gRPC when required.

## 2. Identity Service gRPC Interface

### GetMember

**Input:**
- member_id

**Output:**
- Member information.

### ValidateToken

**Input:**
- authentication token

**Output:**
- validation status
- member_id
- roles

### GetMemberRoles

**Input:**
- member_id

**Output:**
- list of assigned roles.

## 3. Tracker Service gRPC Interface

### GetProject

**Input:**
- project_id

**Output:**
- project details.

### ListProjects

**Input:**
- member_id

**Output:**
- projects accessible to the member.

### GetTask

**Input:**
- task_id

**Output:**
- task details.

### ListTasks

**Input:**
- project_id
- optional status
- optional assignee_member_id

**Output:**
- list of matching tasks.

### CreateTask

**Input:**
- project_id
- title
- description
- priority
- optional assignee_member_id
- optional due_date

**Output:**
- created task.

### UpdateTask

**Input:**
- task_id
- fields to update.

**Output:**
- updated task.

### AssignTask

**Input:**
- task_id
- member_id

**Output:**
- updated assignment.

### AddComment

**Input:**
- task_id
- content
- author_member_id

**Output:**
- created comment.

### HandoffTask

**Input:**
- task_id
- to_member_id
- message

**Output:**
- handoff details.

## 4. Chat Service gRPC Interface

### GetConversation

**Input:**
- conversation_id

**Output:**
- conversation details and members.

### GetMessages

**Input:**
- conversation_id
- pagination information

**Output:**
- list of messages.

### CreateConversation

**Input:**
- conversation_type
- member_ids
- optional project_id
- optional name

**Output:**
- created conversation.

### AddMember

**Input:**
- conversation_id
- member_id

**Output:**
- updated conversation details.

Real-time message delivery is handled through WebSockets.

## 5. AI/MCP Service

The AI/MCP Service uses the contracts exposed by other services to read and update information.

It does not directly access their databases.

Detailed MCP tool definitions are maintained in `mcp-tools.md`.

## 6. Contract Principles

- Each service owns its own data.
- Shared contracts define the information exchanged between services.
- Member IDs are used to reference members across services.
- Changes to shared contracts must be communicated to their consumers.
- Protocol Buffer definitions will be maintained in a shared contracts directory during implementation.
